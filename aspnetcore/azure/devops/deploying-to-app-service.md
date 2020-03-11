---
title: Distribuire un'app nel servizio App - DevOps con ASP.NET Core e Azure
author: CamSoper
description: Distribuire un'app ASP.NET Core in servizio App di Azure, il primo passaggio per DevOps con ASP.NET Core e Azure.
ms.author: casoper
ms.custom: mvc, seodec18
ms.date: 10/24/2018
uid: azure/devops/deploy-to-app-service
ms.openlocfilehash: df41f296e9c4e1eff6e31d45b29ec30ee1e20cf4
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78657745"
---
# <a name="deploy-an-app-to-app-service"></a><span data-ttu-id="779df-103">Distribuire un'app in servizio App</span><span class="sxs-lookup"><span data-stu-id="779df-103">Deploy an app to App Service</span></span>

<span data-ttu-id="779df-104">[App Azure servizio](/azure/app-service/) è la piattaforma di hosting Web di Azure.</span><span class="sxs-lookup"><span data-stu-id="779df-104">[Azure App Service](/azure/app-service/) is Azure's web hosting platform.</span></span> <span data-ttu-id="779df-105">Distribuzione di un'app web in servizio App di Azure può essere eseguita manualmente o mediante un processo automatizzato.</span><span class="sxs-lookup"><span data-stu-id="779df-105">Deploying a web app to Azure App Service can be done manually or by an automated process.</span></span> <span data-ttu-id="779df-106">In questa sezione della Guida vengono illustrati i metodi di distribuzione che possono essere attivati manualmente o tramite script utilizzando la riga di comando oppure attivata manualmente tramite Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="779df-106">This section of the guide discusses deployment methods that can be triggered manually or by script using the command line, or triggered manually using Visual Studio.</span></span>

<span data-ttu-id="779df-107">In questa sezione, è possibile eseguire le attività seguenti:</span><span class="sxs-lookup"><span data-stu-id="779df-107">In this section, you'll accomplish the following tasks:</span></span>

* <span data-ttu-id="779df-108">Scaricare e compilare l'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="779df-108">Download and build the sample app.</span></span>
* <span data-ttu-id="779df-109">Creare un'App Web di Azure App Service usando Azure Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="779df-109">Create an Azure App Service Web App using the Azure Cloud Shell.</span></span>
* <span data-ttu-id="779df-110">Distribuire l'app di esempio in Azure tramite Git.</span><span class="sxs-lookup"><span data-stu-id="779df-110">Deploy the sample app to Azure using Git.</span></span>
* <span data-ttu-id="779df-111">Distribuire una modifica all'app usando Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="779df-111">Deploy a change to the app using Visual Studio.</span></span>
* <span data-ttu-id="779df-112">Aggiungere uno slot di staging all'app web.</span><span class="sxs-lookup"><span data-stu-id="779df-112">Add a staging slot to the web app.</span></span>
* <span data-ttu-id="779df-113">Distribuire un aggiornamento allo slot di staging.</span><span class="sxs-lookup"><span data-stu-id="779df-113">Deploy an update to the staging slot.</span></span>
* <span data-ttu-id="779df-114">Scambiare gli slot di staging e produzione.</span><span class="sxs-lookup"><span data-stu-id="779df-114">Swap the staging and production slots.</span></span>

## <a name="download-and-test-the-app"></a><span data-ttu-id="779df-115">Scaricare e testare l'app</span><span class="sxs-lookup"><span data-stu-id="779df-115">Download and test the app</span></span>

<span data-ttu-id="779df-116">L'app usata in questa guida è un'app ASP.NET Core predefinita, un [lettore di feed semplice](https://github.com/Azure-Samples/simple-feed-reader/).</span><span class="sxs-lookup"><span data-stu-id="779df-116">The app used in this guide is a pre-built ASP.NET Core app, [Simple Feed Reader](https://github.com/Azure-Samples/simple-feed-reader/).</span></span> <span data-ttu-id="779df-117">Si tratta di un'app Razor Pages che usa l'API `Microsoft.SyndicationFeed.ReaderWriter` per recuperare un feed RSS/Atom e visualizzare le voci di notizie in un elenco.</span><span class="sxs-lookup"><span data-stu-id="779df-117">It's a Razor Pages app that uses the `Microsoft.SyndicationFeed.ReaderWriter` API to retrieve an RSS/Atom feed and display the news items in a list.</span></span>

<span data-ttu-id="779df-118">È possibile esaminare il codice, ma è importante comprendere che non ci sono particolari sull'app.</span><span class="sxs-lookup"><span data-stu-id="779df-118">Feel free to review the code, but it's important to understand that there's nothing special about this app.</span></span> <span data-ttu-id="779df-119">È sufficiente una semplice app ASP.NET Core a scopo illustrativo.</span><span class="sxs-lookup"><span data-stu-id="779df-119">It's just a simple ASP.NET Core app for illustrative purposes.</span></span>

<span data-ttu-id="779df-120">Da una shell dei comandi, scaricare il codice, compilare il progetto ed eseguirlo come indicato di seguito.</span><span class="sxs-lookup"><span data-stu-id="779df-120">From a command shell, download the code, build the project, and run it as follows.</span></span>

> <span data-ttu-id="779df-121">*Nota: gli utenti Linux/macOS devono apportare le modifiche appropriate per i percorsi, ad esempio usando la barra (`/`) anziché la barra rovesciata (`\`).*</span><span class="sxs-lookup"><span data-stu-id="779df-121">*Note: Linux/macOS users should make appropriate changes for paths, e.g., using forward slash (`/`) rather than back slash (`\`).*</span></span>

1. <span data-ttu-id="779df-122">Clonare il codice in una cartella nel computer locale.</span><span class="sxs-lookup"><span data-stu-id="779df-122">Clone the code to a folder on your local machine.</span></span>

    ```console
    git clone https://github.com/Azure-Samples/simple-feed-reader/
    ```

2. <span data-ttu-id="779df-123">Modificare la cartella di lavoro nella cartella *Simple-Feed-Reader* creata.</span><span class="sxs-lookup"><span data-stu-id="779df-123">Change your working folder to the *simple-feed-reader* folder that was created.</span></span>

    ```console
    cd .\simple-feed-reader\SimpleFeedReader
    ```

3. <span data-ttu-id="779df-124">Ripristinare i pacchetti e compilare la soluzione.</span><span class="sxs-lookup"><span data-stu-id="779df-124">Restore the packages, and build the solution.</span></span>

    ```dotnetcli
    dotnet build
    ```

4. <span data-ttu-id="779df-125">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="779df-125">Run the app.</span></span>

    ```dotnetcli
    dotnet run
    ```

    ![Il comando dotnet run ha esito positivo](./media/deploying-to-app-service/dotnet-run.png)

5. <span data-ttu-id="779df-127">Aprire un browser e passare a `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="779df-127">Open a browser and navigate to `http://localhost:5000`.</span></span> <span data-ttu-id="779df-128">L'app consente di digitare o incollare un URL del feed di diffusione e visualizzare un elenco delle notizie.</span><span class="sxs-lookup"><span data-stu-id="779df-128">The app allows you to type or paste a syndication feed URL and view a list of news items.</span></span>

     ![L'app Visualizza il contenuto di un feed RSS](./media/deploying-to-app-service/app-in-browser.png)

6. <span data-ttu-id="779df-130">Una volta soddisfatta la corretta esecuzione dell'app, chiuderla premendo **Ctrl**+**C** nella shell dei comandi.</span><span class="sxs-lookup"><span data-stu-id="779df-130">Once you're satisfied the app is working correctly, shut it down by pressing **Ctrl**+**C** in the command shell.</span></span>

## <a name="create-the-azure-app-service-web-app"></a><span data-ttu-id="779df-131">Creare l'App Web di servizio App di Azure</span><span class="sxs-lookup"><span data-stu-id="779df-131">Create the Azure App Service Web App</span></span>

<span data-ttu-id="779df-132">Per distribuire l'app, è necessario creare un' [app Web](/azure/app-service/app-service-web-overview)del servizio app.</span><span class="sxs-lookup"><span data-stu-id="779df-132">To deploy the app, you'll need to create an App Service [Web App](/azure/app-service/app-service-web-overview).</span></span> <span data-ttu-id="779df-133">Dopo la creazione dell'App Web, verrà distribuita a esso dal computer locale tramite Git.</span><span class="sxs-lookup"><span data-stu-id="779df-133">After creation of the Web App, you'll deploy to it from your local machine using Git.</span></span>

1. <span data-ttu-id="779df-134">Accedere ad [Azure Cloud Shell](https://shell.azure.com/bash).</span><span class="sxs-lookup"><span data-stu-id="779df-134">Sign in to the [Azure Cloud Shell](https://shell.azure.com/bash).</span></span> <span data-ttu-id="779df-135">Nota: Quando si accede per la prima volta, Cloud Shell chiede di creare un account di archiviazione per i file di configurazione.</span><span class="sxs-lookup"><span data-stu-id="779df-135">Note: When you sign in for the first time, Cloud Shell prompts to create a storage account for configuration files.</span></span> <span data-ttu-id="779df-136">Accettare le impostazioni predefinite oppure specificare un nome univoco.</span><span class="sxs-lookup"><span data-stu-id="779df-136">Accept the defaults or provide a unique name.</span></span>

2. <span data-ttu-id="779df-137">Usare Cloud Shell per i passaggi seguenti.</span><span class="sxs-lookup"><span data-stu-id="779df-137">Use the Cloud Shell for the following steps.</span></span>

    <span data-ttu-id="779df-138">a.</span><span class="sxs-lookup"><span data-stu-id="779df-138">a.</span></span> <span data-ttu-id="779df-139">Dichiarare una variabile per archiviare il nome dell'app web.</span><span class="sxs-lookup"><span data-stu-id="779df-139">Declare a variable to store your web app's name.</span></span> <span data-ttu-id="779df-140">Il nome deve essere univoco da usare nell'URL predefinito.</span><span class="sxs-lookup"><span data-stu-id="779df-140">The name must be unique to be used in the default URL.</span></span> <span data-ttu-id="779df-141">L'uso della funzione `$RANDOM` bash per costruire il nome garantisce l'univocità e produce il formato `webappname99999`.</span><span class="sxs-lookup"><span data-stu-id="779df-141">Using the `$RANDOM` Bash function to construct the name guarantees uniqueness and results in the format `webappname99999`.</span></span>

    ```console
    webappname=mywebapp$RANDOM
    ```

    <span data-ttu-id="779df-142">b.</span><span class="sxs-lookup"><span data-stu-id="779df-142">b.</span></span> <span data-ttu-id="779df-143">Creare un gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="779df-143">Create a resource group.</span></span> <span data-ttu-id="779df-144">Gruppi di risorse forniscono un mezzo per aggregare le risorse di Azure da gestire come gruppo.</span><span class="sxs-lookup"><span data-stu-id="779df-144">Resource groups provide a means to aggregate Azure resources to be managed as a group.</span></span>

    ```azure-cli
    az group create --location centralus --name AzureTutorial
    ```

    <span data-ttu-id="779df-145">Il comando `az` richiama l'interfaccia della riga di comando di [Azure](/cli/azure/).</span><span class="sxs-lookup"><span data-stu-id="779df-145">The `az` command invokes the [Azure CLI](/cli/azure/).</span></span> <span data-ttu-id="779df-146">L'interfaccia della riga di comando può essere eseguito in locale, ma l'utilizzo in Cloud Shell consente di risparmiare tempo e configurazione.</span><span class="sxs-lookup"><span data-stu-id="779df-146">The CLI can be run locally, but using it in the Cloud Shell saves time and configuration.</span></span>

    <span data-ttu-id="779df-147">c.</span><span class="sxs-lookup"><span data-stu-id="779df-147">c.</span></span> <span data-ttu-id="779df-148">Creare un piano di servizio App nel livello S1.</span><span class="sxs-lookup"><span data-stu-id="779df-148">Create an App Service plan in the S1 tier.</span></span> <span data-ttu-id="779df-149">Un piano di servizio App è un raggruppamento di App web che condividono lo stesso livello di prezzo.</span><span class="sxs-lookup"><span data-stu-id="779df-149">An App Service plan is a grouping of web apps that share the same pricing tier.</span></span> <span data-ttu-id="779df-150">Il piano S1 non è disponibile, ma è obbligatorio per la funzionalità degli slot di staging.</span><span class="sxs-lookup"><span data-stu-id="779df-150">The S1 tier isn't free, but it's required for the staging slots feature.</span></span>

    ```azure-cli
    az appservice plan create --name $webappname --resource-group AzureTutorial --sku S1
    ```

    <span data-ttu-id="779df-151">d.</span><span class="sxs-lookup"><span data-stu-id="779df-151">d.</span></span> <span data-ttu-id="779df-152">Creare la risorsa dell'app web usando il piano di servizio App nello stesso gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="779df-152">Create the web app resource using the App Service plan in the same resource group.</span></span>

    ```azure-cli
    az webapp create --name $webappname --resource-group AzureTutorial --plan $webappname
    ```

    <span data-ttu-id="779df-153">e.</span><span class="sxs-lookup"><span data-stu-id="779df-153">e.</span></span> <span data-ttu-id="779df-154">Impostare le credenziali di distribuzione.</span><span class="sxs-lookup"><span data-stu-id="779df-154">Set the deployment credentials.</span></span> <span data-ttu-id="779df-155">Queste credenziali per la distribuzione si applicano a tutte le app web nella sottoscrizione.</span><span class="sxs-lookup"><span data-stu-id="779df-155">These deployment credentials apply to all the web apps in your subscription.</span></span> <span data-ttu-id="779df-156">Non usare caratteri speciali nel nome utente.</span><span class="sxs-lookup"><span data-stu-id="779df-156">Don't use special characters in the user name.</span></span>

    ```azure-cli
    az webapp deployment user set --user-name REPLACE_WITH_USER_NAME --password REPLACE_WITH_PASSWORD
    ```

    <span data-ttu-id="779df-157">f.</span><span class="sxs-lookup"><span data-stu-id="779df-157">f.</span></span> <span data-ttu-id="779df-158">Configurare l'app Web per accettare le distribuzioni da git locale e visualizzare l' *URL di distribuzione git*.</span><span class="sxs-lookup"><span data-stu-id="779df-158">Configure the web app to accept deployments from local Git and display the *Git deployment URL*.</span></span> <span data-ttu-id="779df-159">**Si noti questo URL per il riferimento in un secondo momento**.</span><span class="sxs-lookup"><span data-stu-id="779df-159">**Note this URL for reference later**.</span></span>

    ```azure-cli
    echo Git deployment URL: $(az webapp deployment source config-local-git --name $webappname --resource-group AzureTutorial --query url --output tsv)
    ```

    <span data-ttu-id="779df-160">g.</span><span class="sxs-lookup"><span data-stu-id="779df-160">g.</span></span> <span data-ttu-id="779df-161">Visualizzare l' *URL dell'app Web*.</span><span class="sxs-lookup"><span data-stu-id="779df-161">Display the *web app URL*.</span></span> <span data-ttu-id="779df-162">Passare a questo URL per visualizzare l'app web vuota.</span><span class="sxs-lookup"><span data-stu-id="779df-162">Browse to this URL to see the blank web app.</span></span> <span data-ttu-id="779df-163">**Si noti questo URL per il riferimento in un secondo momento**.</span><span class="sxs-lookup"><span data-stu-id="779df-163">**Note this URL for reference later**.</span></span>

    ```console
    echo Web app URL: http://$webappname.azurewebsites.net
    ```

3. <span data-ttu-id="779df-164">Usando una shell dei comandi nel computer locale, passare alla cartella del progetto dell'app Web, ad esempio `.\simple-feed-reader\SimpleFeedReader`.</span><span class="sxs-lookup"><span data-stu-id="779df-164">Using a command shell on your local machine, navigate to the web app's project folder (for example, `.\simple-feed-reader\SimpleFeedReader`).</span></span> <span data-ttu-id="779df-165">Eseguire i comandi seguenti per configurare Git per effettuare il push all'URL di distribuzione:</span><span class="sxs-lookup"><span data-stu-id="779df-165">Execute the following commands to set up Git to push to the deployment URL:</span></span>

    <span data-ttu-id="779df-166">a.</span><span class="sxs-lookup"><span data-stu-id="779df-166">a.</span></span> <span data-ttu-id="779df-167">Aggiungere l'URL remoto nel repository locale.</span><span class="sxs-lookup"><span data-stu-id="779df-167">Add the remote URL to the local repository.</span></span>

    ```console
    git remote add azure-prod GIT_DEPLOYMENT_URL
    ```

    <span data-ttu-id="779df-168">b.</span><span class="sxs-lookup"><span data-stu-id="779df-168">b.</span></span> <span data-ttu-id="779df-169">Eseguire il push del ramo *Master* locale nel ramo *Master* del Remote di *Azure-prod* .</span><span class="sxs-lookup"><span data-stu-id="779df-169">Push the local *master* branch to the *azure-prod* remote's *master* branch.</span></span>

    ```console
    git push azure-prod master
    ```

    <span data-ttu-id="779df-170">Verrà richiesto per le credenziali di distribuzione creata in precedenza.</span><span class="sxs-lookup"><span data-stu-id="779df-170">You'll be prompted for the deployment credentials you created earlier.</span></span> <span data-ttu-id="779df-171">Osservare l'output nella shell dei comandi.</span><span class="sxs-lookup"><span data-stu-id="779df-171">Observe the output in the command shell.</span></span> <span data-ttu-id="779df-172">Azure crea l'app ASP.NET Core in remoto.</span><span class="sxs-lookup"><span data-stu-id="779df-172">Azure builds the ASP.NET Core app remotely.</span></span>

4. <span data-ttu-id="779df-173">In un browser passare all'URL dell' *app Web* e notare che l'app è stata compilata e distribuita.</span><span class="sxs-lookup"><span data-stu-id="779df-173">In a browser, navigate to the *Web app URL* and note the app has been built and deployed.</span></span> <span data-ttu-id="779df-174">È possibile eseguire il commit di altre modifiche nel repository git locale con `git commit`.</span><span class="sxs-lookup"><span data-stu-id="779df-174">Additional changes can be committed to the local Git repository with `git commit`.</span></span> <span data-ttu-id="779df-175">Queste modifiche vengono inserite in Azure con il comando `git push` precedente.</span><span class="sxs-lookup"><span data-stu-id="779df-175">These changes are pushed to Azure with the preceding `git push` command.</span></span>

## <a name="deployment-with-visual-studio"></a><span data-ttu-id="779df-176">Distribuzione con Visual Studio</span><span class="sxs-lookup"><span data-stu-id="779df-176">Deployment with Visual Studio</span></span>

> <span data-ttu-id="779df-177">*Nota: questa sezione si applica solo a Windows. Gli utenti di Linux e macOS devono apportare le modifiche descritte nel passaggio 2 seguente. Salvare il file ed eseguire il commit della modifica nel repository locale con `git commit`. Infine, eseguire il push della modifica con `git push`, come nella prima sezione.*</span><span class="sxs-lookup"><span data-stu-id="779df-177">*Note: This section applies to Windows only. Linux and macOS users should make the change described in step 2 below. Save the file, and commit the change to the local repository with `git commit`. Finally, push the change with `git push`, as in the first section.*</span></span>

<span data-ttu-id="779df-178">L'app è già stata distribuita dalla shell dei comandi.</span><span class="sxs-lookup"><span data-stu-id="779df-178">The app has already been deployed from the command shell.</span></span> <span data-ttu-id="779df-179">Utilizziamo gli strumenti integrati di Visual Studio per distribuire un aggiornamento all'app.</span><span class="sxs-lookup"><span data-stu-id="779df-179">Let's use Visual Studio's integrated tools to deploy an update to the app.</span></span> <span data-ttu-id="779df-180">Dietro le quinte, Visual Studio esegue la stessa operazione come la riga di comando degli strumenti, ma all'interno dell'interfaccia utente familiare di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="779df-180">Behind the scenes, Visual Studio accomplishes the same thing as the command line tooling, but within Visual Studio's familiar UI.</span></span>

1. <span data-ttu-id="779df-181">Aprire *SimpleFeedReader. sln* in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="779df-181">Open *SimpleFeedReader.sln* in Visual Studio.</span></span>
2. <span data-ttu-id="779df-182">In Esplora soluzioni aprire *Pages\Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="779df-182">In Solution Explorer, open *Pages\Index.cshtml*.</span></span> <span data-ttu-id="779df-183">Cambiare `<h2>Simple Feed Reader</h2>` in `<h2>Simple Feed Reader - V2</h2>`.</span><span class="sxs-lookup"><span data-stu-id="779df-183">Change `<h2>Simple Feed Reader</h2>` to `<h2>Simple Feed Reader - V2</h2>`.</span></span>
3. <span data-ttu-id="779df-184">Premere **Ctrl**+**MAIUSC**+**B** per compilare l'app.</span><span class="sxs-lookup"><span data-stu-id="779df-184">Press **Ctrl**+**Shift**+**B** to build the app.</span></span>
4. <span data-ttu-id="779df-185">In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto e scegliere **pubblica**.</span><span class="sxs-lookup"><span data-stu-id="779df-185">In Solution Explorer, right-click on the project and click **Publish**.</span></span>

    ![Screenshot che Mostra pulsante destro del mouse, pubblicazione](./media/deploying-to-app-service/publish.png)
5. <span data-ttu-id="779df-187">Visual Studio possa creare una nuova risorsa di servizio App, ma questo aggiornamento verrà pubblicato tramite la distribuzione esistente.</span><span class="sxs-lookup"><span data-stu-id="779df-187">Visual Studio can create a new App Service resource, but this update will be published over the existing deployment.</span></span> <span data-ttu-id="779df-188">Nella finestra di dialogo selezionare **una destinazione di pubblicazione** selezionare **servizio app** dall'elenco a sinistra e quindi selezionare **Seleziona esistente**.</span><span class="sxs-lookup"><span data-stu-id="779df-188">In the **Pick a publish target** dialog, select **App Service** from the list on the left, and then select **Select Existing**.</span></span> <span data-ttu-id="779df-189">Fare clic su **Pubblica**.</span><span class="sxs-lookup"><span data-stu-id="779df-189">Click **Publish**.</span></span>
6. <span data-ttu-id="779df-190">Nella finestra di dialogo **servizio app** confermare che l'account Microsoft o dell'organizzazione usato per creare la sottoscrizione di Azure sia visualizzato in alto a destra.</span><span class="sxs-lookup"><span data-stu-id="779df-190">In the **App Service** dialog, confirm that the Microsoft or Organizational account used to create your Azure subscription is displayed in the upper right.</span></span> <span data-ttu-id="779df-191">In caso contrario, fare clic sul menu a discesa e aggiungerlo.</span><span class="sxs-lookup"><span data-stu-id="779df-191">If it's not, click the drop-down and add it.</span></span>
7. <span data-ttu-id="779df-192">Verificare che sia selezionata la **sottoscrizione** di Azure corretta.</span><span class="sxs-lookup"><span data-stu-id="779df-192">Confirm that the correct Azure **Subscription** is selected.</span></span> <span data-ttu-id="779df-193">Per **Visualizza**, selezionare **gruppo di risorse**.</span><span class="sxs-lookup"><span data-stu-id="779df-193">For **View**, select **Resource Group**.</span></span> <span data-ttu-id="779df-194">Espandere il gruppo di risorse **AzureTutorial** , quindi selezionare l'app Web esistente.</span><span class="sxs-lookup"><span data-stu-id="779df-194">Expand the **AzureTutorial** resource group and then select the existing web app.</span></span> <span data-ttu-id="779df-195">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="779df-195">Click **OK**.</span></span>

    ![Finestra di dialogo di screenshot che illustra la pubblicazione del servizio App](./media/deploying-to-app-service/publish-dialog.png)

<span data-ttu-id="779df-197">Visual Studio compila e distribuisce l'app in Azure.</span><span class="sxs-lookup"><span data-stu-id="779df-197">Visual Studio builds and deploys the app to Azure.</span></span> <span data-ttu-id="779df-198">Passare all'URL dell'app web.</span><span class="sxs-lookup"><span data-stu-id="779df-198">Browse to the web app URL.</span></span> <span data-ttu-id="779df-199">Verificare che la modifica dell'elemento `<h2>` sia attiva.</span><span class="sxs-lookup"><span data-stu-id="779df-199">Validate that the `<h2>` element modification is live.</span></span>

![L'app con la modifica del titolo](./media/deploying-to-app-service/app-v2.png)

## <a name="deployment-slots"></a><span data-ttu-id="779df-201">Slot di distribuzione</span><span class="sxs-lookup"><span data-stu-id="779df-201">Deployment slots</span></span>

<span data-ttu-id="779df-202">Gli slot di distribuzione supportano la gestione temporanea delle modifiche senza conseguenze per le app in esecuzione nell'ambiente di produzione.</span><span class="sxs-lookup"><span data-stu-id="779df-202">Deployment slots support the staging of changes without impacting the app running in production.</span></span> <span data-ttu-id="779df-203">Dopo aver convalidata da un team di garanzia di qualità, la versione dell'app per le fasi di produzione e di slot di staging è possibile scambiare.</span><span class="sxs-lookup"><span data-stu-id="779df-203">Once the staged version of the app is validated by a quality assurance team, the production and staging slots can be swapped.</span></span> <span data-ttu-id="779df-204">L'app in gestione temporanea viene promosso alla produzione in questo modo.</span><span class="sxs-lookup"><span data-stu-id="779df-204">The app in staging is promoted to production in this manner.</span></span> <span data-ttu-id="779df-205">La procedura seguente crea uno slot di staging, distribuisce alcune modifiche a esso e scambia lo slot di staging alla produzione dopo la verifica.</span><span class="sxs-lookup"><span data-stu-id="779df-205">The following steps create a staging slot, deploy some changes to it, and swap the staging slot with production after verification.</span></span>

1. <span data-ttu-id="779df-206">Accedere al [Azure cloud Shell](https://shell.azure.com/bash), se non è già stato effettuato l'accesso.</span><span class="sxs-lookup"><span data-stu-id="779df-206">Sign in to the [Azure Cloud Shell](https://shell.azure.com/bash), if not already signed in.</span></span>
2. <span data-ttu-id="779df-207">Creare lo slot di staging.</span><span class="sxs-lookup"><span data-stu-id="779df-207">Create the staging slot.</span></span>

    <span data-ttu-id="779df-208">a.</span><span class="sxs-lookup"><span data-stu-id="779df-208">a.</span></span> <span data-ttu-id="779df-209">Creare uno slot di distribuzione con il nome *staging*.</span><span class="sxs-lookup"><span data-stu-id="779df-209">Create a deployment slot with the name *staging*.</span></span>

    ```azure-cli
    az webapp deployment slot create --name $webappname --resource-group AzureTutorial --slot staging
    ```

    <span data-ttu-id="779df-210">b.</span><span class="sxs-lookup"><span data-stu-id="779df-210">b.</span></span> <span data-ttu-id="779df-211">Configurare lo slot di staging per usare la distribuzione da git locale e ottenere l'URL di distribuzione di **gestione temporanea** .</span><span class="sxs-lookup"><span data-stu-id="779df-211">Configure the staging slot to use deployment from local Git and get the **staging** deployment URL.</span></span> <span data-ttu-id="779df-212">**Si noti questo URL per il riferimento in un secondo momento**.</span><span class="sxs-lookup"><span data-stu-id="779df-212">**Note this URL for reference later**.</span></span>

    ```azure-cli
    echo Git deployment URL for staging: $(az webapp deployment source config-local-git --name $webappname --resource-group AzureTutorial --slot staging --query url --output tsv)
    ```

    <span data-ttu-id="779df-213">c.</span><span class="sxs-lookup"><span data-stu-id="779df-213">c.</span></span> <span data-ttu-id="779df-214">Visualizzazione URL dello slot di staging.</span><span class="sxs-lookup"><span data-stu-id="779df-214">Display the staging slot's URL.</span></span> <span data-ttu-id="779df-215">Passare all'URL per visualizzare lo slot di staging vuoto.</span><span class="sxs-lookup"><span data-stu-id="779df-215">Browse to the URL to see the empty staging slot.</span></span> <span data-ttu-id="779df-216">**Si noti questo URL per il riferimento in un secondo momento**.</span><span class="sxs-lookup"><span data-stu-id="779df-216">**Note this URL for reference later**.</span></span>

    ```console
    echo Staging web app URL: http://$webappname-staging.azurewebsites.net
    ```

3. <span data-ttu-id="779df-217">In un editor di testo o in Visual Studio modificare di nuovo *pages/index. cshtml* in modo che l'elemento `<h2>` legga `<h2>Simple Feed Reader - V3</h2>` e salvi il file.</span><span class="sxs-lookup"><span data-stu-id="779df-217">In a text editor or Visual Studio, modify *Pages/Index.cshtml* again so that the `<h2>` element reads `<h2>Simple Feed Reader - V3</h2>` and save the file.</span></span>

4. <span data-ttu-id="779df-218">Eseguire il commit del file nel repository git locale, usando la pagina **modifiche** nella scheda *Team Explorer* di Visual Studio oppure immettendo quanto segue usando la shell dei comandi del computer locale:</span><span class="sxs-lookup"><span data-stu-id="779df-218">Commit the file to the local Git repository, using either the **Changes** page in Visual Studio's *Team Explorer* tab, or by entering the following using the local machine's command shell:</span></span>

    ```console
    git commit -a -m "upgraded to V3"
    ```

5. <span data-ttu-id="779df-219">Utilizzando shell dei comandi del computer locale, aggiungere l'URL di distribuzione di staging come un Git remoto e il push il commit delle modifiche:</span><span class="sxs-lookup"><span data-stu-id="779df-219">Using the local machine's command shell, add the staging deployment URL as a Git remote and push the committed changes:</span></span>

    <span data-ttu-id="779df-220">a.</span><span class="sxs-lookup"><span data-stu-id="779df-220">a.</span></span> <span data-ttu-id="779df-221">Aggiungere l'URL remoto per la gestione temporanea per il repository Git locale.</span><span class="sxs-lookup"><span data-stu-id="779df-221">Add the remote URL for staging to the local Git repository.</span></span>

    ```console
    git remote add azure-staging <Git_staging_deployment_URL>
    ```

    <span data-ttu-id="779df-222">b.</span><span class="sxs-lookup"><span data-stu-id="779df-222">b.</span></span> <span data-ttu-id="779df-223">Eseguire il push del ramo *Master* locale nel ramo *Master* del remoto di *gestione temporanea di Azure* .</span><span class="sxs-lookup"><span data-stu-id="779df-223">Push the local *master* branch to the *azure-staging* remote's *master* branch.</span></span>

    ```console
    git push azure-staging master
    ```

    <span data-ttu-id="779df-224">Attendere che Azure crea e distribuisce l'app.</span><span class="sxs-lookup"><span data-stu-id="779df-224">Wait while Azure builds and deploys the app.</span></span>

6. <span data-ttu-id="779df-225">Per verificare che sia stato distribuito V3 allo slot di staging, aprire due finestre del browser.</span><span class="sxs-lookup"><span data-stu-id="779df-225">To verify that V3 has been deployed to the staging slot, open two browser windows.</span></span> <span data-ttu-id="779df-226">In una finestra, passare all'URL dell'app web originale.</span><span class="sxs-lookup"><span data-stu-id="779df-226">In one window, navigate to the original web app URL.</span></span> <span data-ttu-id="779df-227">In altra finestra, passare all'URL di app web di staging.</span><span class="sxs-lookup"><span data-stu-id="779df-227">In the other window, navigate to the staging web app URL.</span></span> <span data-ttu-id="779df-228">L'URL di produzione serve V2 dell'app.</span><span class="sxs-lookup"><span data-stu-id="779df-228">The production URL serves V2 of the app.</span></span> <span data-ttu-id="779df-229">L'URL di gestione temporanea serve V3 dell'app.</span><span class="sxs-lookup"><span data-stu-id="779df-229">The staging URL serves V3 of the app.</span></span>

    ![Schermata di confronto tra le finestre del browser](./media/deploying-to-app-service/ready-to-swap.png)

7. <span data-ttu-id="779df-231">In Cloud Shell, trasferire lo slot di gestione temporanea verificata/preparata-up nell'ambiente di produzione.</span><span class="sxs-lookup"><span data-stu-id="779df-231">In the Cloud Shell, swap the verified/warmed-up staging slot into production.</span></span>

    ```azure-cli
    az webapp deployment slot swap --name $webappname --resource-group AzureTutorial --slot staging
    ```

8. <span data-ttu-id="779df-232">Verificare che lo scambio si è verificato aggiornando le finestre del due browser.</span><span class="sxs-lookup"><span data-stu-id="779df-232">Verify that the swap occurred by refreshing the two browser windows.</span></span>

    ![Confronto tra le finestre del browser al termine dello scambio](./media/deploying-to-app-service/swapped.png)

## <a name="summary"></a><span data-ttu-id="779df-234">Summary</span><span class="sxs-lookup"><span data-stu-id="779df-234">Summary</span></span>

<span data-ttu-id="779df-235">In questa sezione sono state completate le attività seguenti:</span><span class="sxs-lookup"><span data-stu-id="779df-235">In this section, the following tasks were completed:</span></span>

* <span data-ttu-id="779df-236">Scaricata e compilata l'app di esempio.</span><span class="sxs-lookup"><span data-stu-id="779df-236">Downloaded and built the sample app.</span></span>
* <span data-ttu-id="779df-237">Creare un'App Web di Azure App Service usando Azure Cloud Shell.</span><span class="sxs-lookup"><span data-stu-id="779df-237">Created an Azure App Service Web App using the Azure Cloud Shell.</span></span>
* <span data-ttu-id="779df-238">Distribuire l'app di esempio in Azure con Git.</span><span class="sxs-lookup"><span data-stu-id="779df-238">Deployed the sample app to Azure using Git.</span></span>
* <span data-ttu-id="779df-239">Distribuire una modifica all'app usando Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="779df-239">Deployed a change to the app using Visual Studio.</span></span>
* <span data-ttu-id="779df-240">Aggiungere uno slot di staging all'app web.</span><span class="sxs-lookup"><span data-stu-id="779df-240">Added a staging slot to the web app.</span></span>
* <span data-ttu-id="779df-241">Distribuzione di un aggiornamento allo slot di staging.</span><span class="sxs-lookup"><span data-stu-id="779df-241">Deployed an update to the staging slot.</span></span>
* <span data-ttu-id="779df-242">Scambiare gli slot di staging e produzione.</span><span class="sxs-lookup"><span data-stu-id="779df-242">Swapped the staging and production slots.</span></span>

<span data-ttu-id="779df-243">Nella sezione successiva, si apprenderà come creare una pipeline di DevOps con le pipeline di Azure.</span><span class="sxs-lookup"><span data-stu-id="779df-243">In the next section, you'll learn how to build a DevOps pipeline with Azure Pipelines.</span></span>

## <a name="additional-reading"></a><span data-ttu-id="779df-244">Informazioni aggiuntive</span><span class="sxs-lookup"><span data-stu-id="779df-244">Additional reading</span></span>

* [<span data-ttu-id="779df-245">Panoramica di App Web</span><span class="sxs-lookup"><span data-stu-id="779df-245">Web Apps overview</span></span>](/azure/app-service/app-service-web-overview)
* [<span data-ttu-id="779df-246">Creare un'app Web .NET Core e database SQL nel servizio app Azure</span><span class="sxs-lookup"><span data-stu-id="779df-246">Build a .NET Core and SQL Database web app in Azure App Service</span></span>](/azure/app-service/app-service-web-tutorial-dotnetcore-sqldb)
* [<span data-ttu-id="779df-247">Configurare le credenziali di distribuzione per il servizio app Azure</span><span class="sxs-lookup"><span data-stu-id="779df-247">Configure deployment credentials for Azure App Service</span></span>](/azure/app-service/app-service-deployment-credentials)
* [<span data-ttu-id="779df-248">Configurare gli ambienti di gestione temporanea nel Servizio app di Azure</span><span class="sxs-lookup"><span data-stu-id="779df-248">Set up staging environments in Azure App Service</span></span>](/azure/app-service/web-sites-staged-publishing)
