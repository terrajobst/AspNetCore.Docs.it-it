---
uid: web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment
title: 'Distribuzione Web ASP.NET tramite Visual Studio: distribuzione da riga di comando | Microsoft Docs'
author: tdykstra
description: Questa serie di esercitazioni illustra come distribuire, pubblicare, ASP.NET per App Web di servizio App di Azure o per un provider di hosting di terze parti, di applicazioni web da utilizza...
ms.author: aspnetcontent
ms.date: 02/15/2013
ms.assetid: 82b8dea0-f062-4ee4-8784-3ffa30fbb1ca
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment
msc.type: authoredcontent
ms.openlocfilehash: f079beabccd253ff13c19d3192181ddbdf00b3bf
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37830967"
---
<a name="aspnet-web-deployment-using-visual-studio-command-line-deployment"></a><span data-ttu-id="f8c54-103">Distribuzione Web ASP.NET tramite Visual Studio: distribuzione da riga di comando</span><span class="sxs-lookup"><span data-stu-id="f8c54-103">ASP.NET Web Deployment using Visual Studio: Command Line Deployment</span></span>
====================
<span data-ttu-id="f8c54-104">da [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="f8c54-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="f8c54-105">Download progetto iniziale</span><span class="sxs-lookup"><span data-stu-id="f8c54-105">Download Starter Project</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> <span data-ttu-id="f8c54-106">Questa serie di esercitazioni illustra come distribuire, pubblicare, ASP.NET per App Web di servizio App di Azure o per un provider di hosting di terze parti, di applicazioni web usando Visual Studio 2012 o Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="f8c54-106">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider, by using Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="f8c54-107">Per informazioni sulla serie, vedere [la prima esercitazione della serie](introduction.md).</span><span class="sxs-lookup"><span data-stu-id="f8c54-107">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="f8c54-108">Panoramica</span><span class="sxs-lookup"><span data-stu-id="f8c54-108">Overview</span></span>

<span data-ttu-id="f8c54-109">Questa esercitazione illustra come richiamare il web di Visual Studio pipeline dalla riga di comando di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="f8c54-109">This tutorial shows you how to invoke the Visual Studio web publish pipeline from the command line.</span></span> <span data-ttu-id="f8c54-110">Ciò è utile per scenari in cui si desidera [automatizzare il processo di distribuzione](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) anziché farlo manualmente in Visual Studio, in genere usando una [origine sistema di controllo della versione codice](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md).</span><span class="sxs-lookup"><span data-stu-id="f8c54-110">This is useful for scenarios where you want to [automate the deployment process](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) instead of doing it manually in Visual Studio, typically by using a [source code version control system](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md).</span></span>

## <a name="make-a-change-to-deploy"></a><span data-ttu-id="f8c54-111">Apportare una modifica per la distribuzione</span><span class="sxs-lookup"><span data-stu-id="f8c54-111">Make a change to deploy</span></span>

<span data-ttu-id="f8c54-112">Attualmente, la pagina About vengono visualizzati il codice del modello.</span><span class="sxs-lookup"><span data-stu-id="f8c54-112">Currently the About page displays the template code.</span></span>

![Pagina About con il codice del modello](command-line-deployment/_static/image1.png)

<span data-ttu-id="f8c54-114">Che sarà sostituito con il codice che visualizza un riepilogo della registrazione per studenti.</span><span class="sxs-lookup"><span data-stu-id="f8c54-114">You'll replace that with code that displays a summary of student enrollment.</span></span>

<span data-ttu-id="f8c54-115">Aprire il *About* pagina, eliminare tutti i commenti all'interno di `MainContent` `Content` elemento e inserire il markup seguente al suo posto:</span><span class="sxs-lookup"><span data-stu-id="f8c54-115">Open the *About.aspx* page, delete all of the markup inside the `MainContent` `Content` element, and insert the following markup in its place:</span></span>

[!code-aspx[Main](command-line-deployment/samples/sample1.aspx)]

<span data-ttu-id="f8c54-116">Eseguire il progetto e selezionare il **sulle** pagina.</span><span class="sxs-lookup"><span data-stu-id="f8c54-116">Run the project and select the **About** page.</span></span>

![Pagina About (Informazioni)](command-line-deployment/_static/image2.png)

## <a name="deploy-to-test-by-using-the-command-line"></a><span data-ttu-id="f8c54-118">Distribuire Test usando la riga di comando</span><span class="sxs-lookup"><span data-stu-id="f8c54-118">Deploy to Test by using the command line</span></span>

<span data-ttu-id="f8c54-119">Non distribuisce un'altra modifica di database, in tal caso Disabilita dbDacFx database distribuzione per il database aspnet-ContosoUniversity.</span><span class="sxs-lookup"><span data-stu-id="f8c54-119">You won't be deploying another database change, so disable dbDacFx database deployment for the aspnet-ContosoUniversity database.</span></span> <span data-ttu-id="f8c54-120">Aprire il **pubblica sul Web** guidato e in ognuno dei tre profili, non crittografati di pubblicazione il **aggiornare il Database** casella di controllo la **impostazioni** scheda.</span><span class="sxs-lookup"><span data-stu-id="f8c54-120">Open the **Publish Web** wizard, and in each of the three publish profiles, clear the **Update Database** check box on the **Settings** tab.</span></span>

<span data-ttu-id="f8c54-121">Nella pagina iniziale di Windows 8, cercare **prompt dei comandi per gli sviluppatori per VS2012**.</span><span class="sxs-lookup"><span data-stu-id="f8c54-121">In the Windows 8 Start page, search for **Developer Command Prompt for VS2012**.</span></span>

<span data-ttu-id="f8c54-122">Fare doppio clic sull'icona **prompt dei comandi per gli sviluppatori per VS2012** e fare clic su **Esegui come amministratore**.</span><span class="sxs-lookup"><span data-stu-id="f8c54-122">Right-click the icon for **Developer Command Prompt for VS2012** and click **Run as administrator**.</span></span>

<span data-ttu-id="f8c54-123">Immettere il comando seguente al prompt dei comandi, sostituendo il percorso del file di soluzione con il percorso al file della soluzione:</span><span class="sxs-lookup"><span data-stu-id="f8c54-123">Enter the following command at the command prompt, replacing the path to the solution file with the path to your solution file:</span></span>

[!code-console[Main](command-line-deployment/samples/sample2.cmd)]

<span data-ttu-id="f8c54-124">MSBuild compila la soluzione e la distribuisce nell'ambiente di test.</span><span class="sxs-lookup"><span data-stu-id="f8c54-124">MSBuild builds the solution and deploys it to the test environment.</span></span>

![Output della riga di comando](command-line-deployment/_static/image3.png)

<span data-ttu-id="f8c54-126">Aprire un browser e passare a `http://localhost/ContosoUniversity`, quindi scegliere il **sulle** pagina per verificare che la distribuzione è riuscita.</span><span class="sxs-lookup"><span data-stu-id="f8c54-126">Open a browser and go to `http://localhost/ContosoUniversity`, then click the **About** page to verify that the deployment was successful.</span></span>

<span data-ttu-id="f8c54-127">Se è stata creata di studenti nel test, si noterà una pagina vuota sotto la **studente corpo statistiche** intestazione.</span><span class="sxs-lookup"><span data-stu-id="f8c54-127">If you haven't created any students in test, you'll see an empty page under the **Student Body Statistics** heading.</span></span> <span data-ttu-id="f8c54-128">Passare al **studenti** pagina, fare clic su **aggiungere studente**e aggiungere alcuni studenti e quindi tornare al **sulle** pagina per visualizzare le statistiche degli studenti.</span><span class="sxs-lookup"><span data-stu-id="f8c54-128">Go to the **Students** page, click **Add Student**, and add some students, and then return to the **About** page to see student statistics.</span></span>

![Informazioni sulla pagina in ambiente di Test](command-line-deployment/_static/image4.png)

## <a name="key-command-line-options"></a><span data-ttu-id="f8c54-130">Opzioni della riga di comando chiave</span><span class="sxs-lookup"><span data-stu-id="f8c54-130">Key command line options</span></span>

<span data-ttu-id="f8c54-131">Il comando immesso passato il percorso del file di soluzione e due proprietà di MSBuild:</span><span class="sxs-lookup"><span data-stu-id="f8c54-131">The command that you entered passed the solution file path and two properties to MSBuild:</span></span>

[!code-console[Main](command-line-deployment/samples/sample3.cmd)]

### <a name="deploying-the-solution-versus-deploying-individual-projects"></a><span data-ttu-id="f8c54-132">La distribuzione della soluzione rispetto alla distribuzione di progetti singoli</span><span class="sxs-lookup"><span data-stu-id="f8c54-132">Deploying the solution versus deploying individual projects</span></span>

<span data-ttu-id="f8c54-133">Se si specifica il file della soluzione, tutti i progetti della soluzione da compilare.</span><span class="sxs-lookup"><span data-stu-id="f8c54-133">Specifying the solution file causes all projects in the solution to be built.</span></span> <span data-ttu-id="f8c54-134">Se si hanno più progetti web nella soluzione, si applica il comportamento di MSBuild seguente:</span><span class="sxs-lookup"><span data-stu-id="f8c54-134">If you have multiple web projects in the solution, the following MSBuild behavior applies:</span></span>

- <span data-ttu-id="f8c54-135">Le proprietà che specificano nella riga di comando vengono passate a ogni progetto.</span><span class="sxs-lookup"><span data-stu-id="f8c54-135">The properties that you specify on the command line are passed to every project.</span></span> <span data-ttu-id="f8c54-136">Pertanto, ciascun progetto web deve avere un profilo di pubblicazione con il nome specificato.</span><span class="sxs-lookup"><span data-stu-id="f8c54-136">Therefore, each web project must have a publish profile with the name that you specify.</span></span> <span data-ttu-id="f8c54-137">Se si specifica `/p:PublishProfile=Test`, ciascun progetto web deve avere un profilo di pubblicazione denominato *Test*.</span><span class="sxs-lookup"><span data-stu-id="f8c54-137">If you specify `/p:PublishProfile=Test`, each web project must have a publish profile named *Test*.</span></span>
- <span data-ttu-id="f8c54-138">È possibile pubblicare un progetto è stato quando non viene compilato anche un altro.</span><span class="sxs-lookup"><span data-stu-id="f8c54-138">You might successfully publish one project when another one doesn't even build.</span></span> <span data-ttu-id="f8c54-139">Per altre informazioni, vedere il thread di stackoverflow [si verifica un errore di MSBuild con due pacchetti](http://stackoverflow.com/questions/14226451/msbuild-fails-with-two-packages).</span><span class="sxs-lookup"><span data-stu-id="f8c54-139">For more information, see the stackoverflow thread [MSBuild fails with two packages](http://stackoverflow.com/questions/14226451/msbuild-fails-with-two-packages).</span></span>

<span data-ttu-id="f8c54-140">Se si specifica un singolo progetto invece di una soluzione, è necessario aggiungere un parametro che specifica la versione di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f8c54-140">If you specify an individual project instead of a solution, you have to add a parameter that specifies the Visual Studio version.</span></span> <span data-ttu-id="f8c54-141">Se si usa Visual Studio 2012 è possibile che la riga di comando sarà simile all'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="f8c54-141">If you are using Visual Studio 2012 the command line would be similar to the following example:</span></span>

[!code-console[Main](command-line-deployment/samples/sample4.cmd?highlight=1)]

<span data-ttu-id="f8c54-142">Il numero di versione per Visual Studio 2010 è 10,0.</span><span class="sxs-lookup"><span data-stu-id="f8c54-142">The version number for Visual Studio 2010 is 10.0.</span></span> <span data-ttu-id="f8c54-143">Per altre informazioni, vedere [compatibilità dei progetti di Visual Studio e VisualStudioVersion](http://sedodream.com/2012/08/19/VisualStudioProjectCompatabilityAndVisualStudioVersion.aspx) sul blog di Sayed Hashimi.</span><span class="sxs-lookup"><span data-stu-id="f8c54-143">For more information, see [Visual Studio project compatability and VisualStudioVersion](http://sedodream.com/2012/08/19/VisualStudioProjectCompatabilityAndVisualStudioVersion.aspx) on Sayed Hashimi's blog.</span></span>

### <a name="specifying-the-publish-profile"></a><span data-ttu-id="f8c54-144">Specifica il profilo di pubblicazione</span><span class="sxs-lookup"><span data-stu-id="f8c54-144">Specifying the publish profile</span></span>

<span data-ttu-id="f8c54-145">È possibile specificare il profilo di pubblicazione con nome o il percorso completo per il *pubxml* file, come illustrato nell'esempio seguente:</span><span class="sxs-lookup"><span data-stu-id="f8c54-145">You can specify the publish profile by name or by the full path to the *.pubxml* file, as shown in the following example:</span></span>

[!code-console[Main](command-line-deployment/samples/sample5.cmd?highlight=1)]

### <a name="web-publish-methods-supported-for-command-line-publishing"></a><span data-ttu-id="f8c54-146">Metodi supportati per la pubblicazione della riga di comando di pubblicazione sul Web</span><span class="sxs-lookup"><span data-stu-id="f8c54-146">Web publish methods supported for command-line publishing</span></span>

<span data-ttu-id="f8c54-147">Tre metodi di pubblicazione sono supportati per la pubblicazione della riga di comando:</span><span class="sxs-lookup"><span data-stu-id="f8c54-147">Three publish methods are supported for command line publishing:</span></span>

- <span data-ttu-id="f8c54-148">`MSDeploy` -Pubblicazione con distribuzione Web.</span><span class="sxs-lookup"><span data-stu-id="f8c54-148">`MSDeploy` - Publish by using Web Deploy.</span></span>
- <span data-ttu-id="f8c54-149">`Package` -Pubblicazione mediante la creazione di un pacchetto di distribuzione Web.</span><span class="sxs-lookup"><span data-stu-id="f8c54-149">`Package` - Publish by creating a Web Deploy Package.</span></span> <span data-ttu-id="f8c54-150">È necessario installare separatamente il pacchetto del comando di MSBuild che lo crea.</span><span class="sxs-lookup"><span data-stu-id="f8c54-150">You have to install the package separately from the MSBuild command that creates it.</span></span>
- <span data-ttu-id="f8c54-151">`FileSystem` -Pubblicazione copiando i file in una cartella specificata.</span><span class="sxs-lookup"><span data-stu-id="f8c54-151">`FileSystem` - Publish by copying files to a specified folder.</span></span>

### <a name="specifying-the-build-configuration-and-platform"></a><span data-ttu-id="f8c54-152">Specifica la piattaforma e configurazione della build</span><span class="sxs-lookup"><span data-stu-id="f8c54-152">Specifying the build configuration and platform</span></span>

<span data-ttu-id="f8c54-153">La configurazione della build e la piattaforma deve essere impostate in Visual Studio o dalla riga di comando.</span><span class="sxs-lookup"><span data-stu-id="f8c54-153">The build configuration and platform must be set in Visual Studio or on the command line.</span></span> <span data-ttu-id="f8c54-154">I profili di pubblicazione includono proprietà che sono denominate `LastUsedBuildConfiguration` e `LastUsedPlatform`, ma non è possibile impostare queste proprietà per determinare come viene compilato il progetto.</span><span class="sxs-lookup"><span data-stu-id="f8c54-154">The publish profiles include properties that are named `LastUsedBuildConfiguration` and `LastUsedPlatform`, but you can't set these properties in order to determine how the project is built.</span></span> <span data-ttu-id="f8c54-155">Per altre informazioni, vedere [MSBuild: come impostare la proprietà di configurazione](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) sul blog di Sayed Hashimi.</span><span class="sxs-lookup"><span data-stu-id="f8c54-155">For more information, see [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) on Sayed Hashimi's blog.</span></span>

## <a name="deploy-to-staging"></a><span data-ttu-id="f8c54-156">Distribuire in gestione temporanea</span><span class="sxs-lookup"><span data-stu-id="f8c54-156">Deploy to staging</span></span>

<span data-ttu-id="f8c54-157">Per distribuire in Azure, è necessario aggiungere la password nella riga di comando.</span><span class="sxs-lookup"><span data-stu-id="f8c54-157">To deploy to Azure, you must add the password to the command line.</span></span> <span data-ttu-id="f8c54-158">Se è stata salvata la password nel profilo di pubblicazione in Visual Studio, è stato archiviato in forma crittografata nel il *. pubxml* file.</span><span class="sxs-lookup"><span data-stu-id="f8c54-158">If you saved the password in the publish profile in Visual Studio, it was stored in encrypted form in the your *.pubxml.user* file.</span></span> <span data-ttu-id="f8c54-159">Tale file non è accessibile da MSBuild quando si esegue una distribuzione da riga di comando, pertanto è necessario passare la password in un parametro della riga di comando.</span><span class="sxs-lookup"><span data-stu-id="f8c54-159">That file is not accessed by MSBuild when you do a command line deployment, so you have to pass in the password in a command line parameter.</span></span>

1. <span data-ttu-id="f8c54-160">Copiare la password necessari dal *publishsettings* file scaricato in precedenza per il sito web di staging.</span><span class="sxs-lookup"><span data-stu-id="f8c54-160">Copy the password that you need from the *.publishsettings* file that you downloaded earlier for the staging web site.</span></span> <span data-ttu-id="f8c54-161">La password è il valore della `userPWD` attributo per la distribuzione Web `publishProfile` elemento.</span><span class="sxs-lookup"><span data-stu-id="f8c54-161">The password is the value of the `userPWD` attribute for the Web Deploy `publishProfile` element.</span></span>

    ![Password di distribuzione Web](command-line-deployment/_static/image5.png)
2. <span data-ttu-id="f8c54-163">Nella pagina iniziale di Windows 8, cercare **prompt dei comandi per gli sviluppatori per VS2012**e fare clic sull'icona per aprire il prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="f8c54-163">In the Windows 8 Start page, search for **Developer Command Prompt for VS2012**, and click the icon to open the command prompt.</span></span> <span data-ttu-id="f8c54-164">(Non è necessario aprirlo come amministratore in questo momento perché non distribuzione in IIS nel computer locale.)</span><span class="sxs-lookup"><span data-stu-id="f8c54-164">(You don't have to open it as administrator this time because you aren't deploying to IIS on the local computer.)</span></span>
3. <span data-ttu-id="f8c54-165">Immettere il comando seguente al prompt dei comandi, sostituendo il percorso del file di soluzione con il percorso di file della soluzione e la password con la password:</span><span class="sxs-lookup"><span data-stu-id="f8c54-165">Enter the following command at the command prompt, replacing the path to the solution file with the path to your solution file and the password with your password:</span></span>

    [!code-console[Main](command-line-deployment/samples/sample6.cmd)]

    <span data-ttu-id="f8c54-166">Si noti che questa riga di comando include un parametro aggiuntivo: `/p:AllowUntrustedCertificate=true`.</span><span class="sxs-lookup"><span data-stu-id="f8c54-166">Notice that this command line includes an extra parameter: `/p:AllowUntrustedCertificate=true`.</span></span> <span data-ttu-id="f8c54-167">Come viene scritto in questa esercitazione, il `AllowUntrustedCertificate` deve essere impostata quando pubblica in Azure dalla riga di comando.</span><span class="sxs-lookup"><span data-stu-id="f8c54-167">As this tutorial is being written, the `AllowUntrustedCertificate` property must be set when you publish to Azure from the command line.</span></span> <span data-ttu-id="f8c54-168">Quando viene rilasciata la correzione per questo bug, non sarà necessario tale parametro.</span><span class="sxs-lookup"><span data-stu-id="f8c54-168">When the fix for this bug is released, you won't need that parameter.</span></span>
4. <span data-ttu-id="f8c54-169">Aprire un browser e passare all'URL del sito di gestione temporanea e quindi scegliere il **sulle** pagina per verificare che la distribuzione è riuscita.</span><span class="sxs-lookup"><span data-stu-id="f8c54-169">Open a browser and go to the URL of your staging site, and then click the **About** page to verify that the deployment was successful.</span></span>

    <span data-ttu-id="f8c54-170">Come illustrato in precedenza per l'ambiente di test, potrebbe essere necessario creare alcuni studenti per visualizzare le statistiche sul **sulle** pagina.</span><span class="sxs-lookup"><span data-stu-id="f8c54-170">As you saw earlier for the test environment, you might have to create some students to see statistics on the **About** page.</span></span>

## <a name="deploy-to-production"></a><span data-ttu-id="f8c54-171">Distribuire nell'ambiente di produzione</span><span class="sxs-lookup"><span data-stu-id="f8c54-171">Deploy to production</span></span>

<span data-ttu-id="f8c54-172">Il processo per la distribuzione nell'ambiente di produzione è simile al processo per la gestione temporanea.</span><span class="sxs-lookup"><span data-stu-id="f8c54-172">The process for deploying to production is similar to the process for staging.</span></span>

1. <span data-ttu-id="f8c54-173">Copiare la password necessari dal *publishsettings* file scaricato in precedenza per il sito web di produzione.</span><span class="sxs-lookup"><span data-stu-id="f8c54-173">Copy the password that you need from the *.publishsettings* file that you downloaded earlier for the production web site.</span></span>
2. <span data-ttu-id="f8c54-174">Aprire **prompt dei comandi per gli sviluppatori per VS2012**.</span><span class="sxs-lookup"><span data-stu-id="f8c54-174">Open **Developer Command Prompt for VS2012**.</span></span>
3. <span data-ttu-id="f8c54-175">Immettere il comando seguente al prompt dei comandi, sostituendo il percorso del file di soluzione con il percorso di file della soluzione e la password con la password:</span><span class="sxs-lookup"><span data-stu-id="f8c54-175">Enter the following command at the command prompt, replacing the path to the solution file with the path to your solution file and the password with your password:</span></span>

    [!code-console[Main](command-line-deployment/samples/sample7.cmd)]

    <span data-ttu-id="f8c54-176">Per un sito di produzione reale, se si è verificato anche una modifica al database, si usa in genere per copiare il *app\_offline.htm* al sito prima della distribuzione di file ed eliminarlo al termine della distribuzione.</span><span class="sxs-lookup"><span data-stu-id="f8c54-176">For a real production site, if there was also a database change, you would typically copy the *app\_offline.htm* file to the site before deployment and delete it after successful deployment.</span></span>
4. <span data-ttu-id="f8c54-177">Aprire un browser e passare all'URL del sito di gestione temporanea e quindi scegliere il **sulle** pagina per verificare che la distribuzione è riuscita.</span><span class="sxs-lookup"><span data-stu-id="f8c54-177">Open a browser and go to the URL of your staging site, and then click the **About** page to verify that the deployment was successful.</span></span>

## <a name="summary"></a><span data-ttu-id="f8c54-178">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="f8c54-178">Summary</span></span>

<span data-ttu-id="f8c54-179">È stato distribuito un aggiornamento dell'applicazione tramite la riga di comando.</span><span class="sxs-lookup"><span data-stu-id="f8c54-179">You have now deployed an application update by using the command line.</span></span>

![Informazioni sulla pagina in ambiente di Test](command-line-deployment/_static/image6.png)

<span data-ttu-id="f8c54-181">Nella prossima esercitazione, verrà visualizzato un esempio di come estendere il web della pipeline di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="f8c54-181">In the next tutorial, you will see an example of how to extend the web publish pipeline.</span></span> <span data-ttu-id="f8c54-182">L'esempio illustra come distribuire i file che non sono inclusi nel progetto.</span><span class="sxs-lookup"><span data-stu-id="f8c54-182">The example will show you how to deploy files that are not included in the project.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f8c54-183">[Precedente](deploying-a-database-update.md)
> [Successivo](deploying-extra-files.md)</span><span class="sxs-lookup"><span data-stu-id="f8c54-183">[Previous](deploying-a-database-update.md)
[Next](deploying-extra-files.md)</span></span>
