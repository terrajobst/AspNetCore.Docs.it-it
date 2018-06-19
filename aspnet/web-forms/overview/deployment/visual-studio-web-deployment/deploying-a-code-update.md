---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
title: 'Distribuzione Web ASP.NET utilizzando Visual Studio: distribuzione di un aggiornamento del codice | Documenti Microsoft'
author: tdykstra
description: Questa serie di esercitazioni viene illustrato come distribuire un ASP.NET (pubblica) per App Web di servizio App di Azure o per un provider di hosting di terze parti, di applicazioni web da utilizza...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: c76dbc35-a914-4ee3-919c-4f4d1fa05104
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
msc.type: authoredcontent
ms.openlocfilehash: dd02b5c627fbfbb0034030f4c21207d24f6aabce
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30881334"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-a-code-update"></a><span data-ttu-id="bd7cd-103">Distribuzione Web ASP.NET utilizzando Visual Studio: distribuzione di un aggiornamento del codice</span><span class="sxs-lookup"><span data-stu-id="bd7cd-103">ASP.NET Web Deployment using Visual Studio: Deploying a Code Update</span></span>
====================
<span data-ttu-id="bd7cd-104">da [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="bd7cd-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="bd7cd-105">Scaricare il progetto Starter</span><span class="sxs-lookup"><span data-stu-id="bd7cd-105">Download Starter Project</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> <span data-ttu-id="bd7cd-106">Questa serie di esercitazioni viene illustrato come distribuire un ASP.NET (pubblica) per App Web di servizio App di Azure o per un provider di hosting di terze parti, di applicazioni web utilizzando Visual Studio 2012 o Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-106">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider, by using Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="bd7cd-107">Per informazioni sulla serie, vedere [la prima esercitazione di serie](introduction.md).</span><span class="sxs-lookup"><span data-stu-id="bd7cd-107">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="bd7cd-108">Panoramica</span><span class="sxs-lookup"><span data-stu-id="bd7cd-108">Overview</span></span>

<span data-ttu-id="bd7cd-109">Dopo la distribuzione iniziale, il lavoro di gestione e il sito web di sviluppo continua e poco tempo è possibile distribuire un aggiornamento.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-109">After the initial deployment, your work of maintaining and developing your web site continues, and before long you will want to deploy an update.</span></span> <span data-ttu-id="bd7cd-110">In questa esercitazione illustra il processo di distribuzione di un aggiornamento per il codice dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-110">This tutorial takes you through the process of deploying an update to your application code.</span></span> <span data-ttu-id="bd7cd-111">L'aggiornamento implementare e distribuire in questa esercitazione non comporta la modifica un database. che cos'è diverso sulla distribuzione di una modifica al database nella prossima esercitazione verrà visualizzato.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-111">The update that you implement and deploy in this tutorial does not involve a database change; you'll see what's different about deploying a database change in the next tutorial.</span></span>

<span data-ttu-id="bd7cd-112">Promemoria: Se viene visualizzato un messaggio di errore o non funzioni man mano che Leggi l'esercitazione, assicurarsi di controllare il [risoluzione dei problemi di pagina](troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="bd7cd-112">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](troubleshooting.md).</span></span>

## <a name="make-a-code-change"></a><span data-ttu-id="bd7cd-113">Modificare il codice</span><span class="sxs-lookup"><span data-stu-id="bd7cd-113">Make a code change</span></span>

<span data-ttu-id="bd7cd-114">Un esempio semplice di un aggiornamento per l'applicazione, aggiungerai il **i docenti** pagina un elenco di corsi tenuti da istruttore selezionato.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-114">As a simple example of an update to your application, you'll add to the **Instructors** page a list of courses taught by the selected instructor.</span></span>

<span data-ttu-id="bd7cd-115">Se si esegue il **i docenti** pagina, si noterà che non esistono **selezionare** collegamenti nella griglia, ma non se si diverso da rendere il grigio attiva in background di riga.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-115">If you run the **Instructors** page, you'll notice that there are **Select** links in the grid, but they don't do anything other than make the row background turn gray.</span></span>

![Pagina istruttori con selezione](deploying-a-code-update/_static/image1.png)

<span data-ttu-id="bd7cd-117">Ora si aggiungerà il codice che viene eseguita quando il **selezionare** collegamento viene selezionato e visualizza un elenco di corsi tenuti da istruttore selezionato.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-117">Now you'll add code that runs when the **Select** link is clicked and displays a list of courses taught by the selected instructor .</span></span>

1. <span data-ttu-id="bd7cd-118">In *Instructors.aspx*, aggiungere il markup seguente immediatamente dopo il **ErrorMessageLabel** `Label` controllo:</span><span class="sxs-lookup"><span data-stu-id="bd7cd-118">In *Instructors.aspx*, add the following markup immediately after the **ErrorMessageLabel** `Label` control:</span></span>

    [!code-aspx[Main](deploying-a-code-update/samples/sample1.aspx)]
2. <span data-ttu-id="bd7cd-119">Eseguire la pagina e selezionare un docente.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-119">Run the page and select an instructor.</span></span> <span data-ttu-id="bd7cd-120">Viene visualizzato un elenco di corsi tenuti da tale istruttore.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-120">You see a list of courses taught by that instructor.</span></span>

    ![Pagina istruttori con corsi tenuti](deploying-a-code-update/_static/image2.png)
3. <span data-ttu-id="bd7cd-122">Chiudere il browser.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-122">Close the browser.</span></span>

## <a name="deploy-the-code-update-to-the-test-environment"></a><span data-ttu-id="bd7cd-123">Distribuire l'aggiornamento di codice per l'ambiente di test</span><span class="sxs-lookup"><span data-stu-id="bd7cd-123">Deploy the code update to the test environment</span></span>

<span data-ttu-id="bd7cd-124">È possibile utilizzare i profili di pubblicazione per la distribuzione di test, gestione temporanea e produzione, è necessario modificare le opzioni di pubblicazione di database.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-124">Before you can use your publish profiles to deploy to test, staging, and production, you need to change database publishing options.</span></span> <span data-ttu-id="bd7cd-125">Non è necessario eseguire gli script di distribuzione grant e i dati per il database delle appartenenze.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-125">You no longer need to run the grant and data deployment scripts for the membership database.</span></span>

1. <span data-ttu-id="bd7cd-126">Aprire il **pubblica sul Web** guidata facendo clic su progetto ContosoUniversity e facendo clic su **pubblica**.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-126">Open the **Publish Web** wizard by right-clicking the ContosoUniversity project and clicking **Publish**.</span></span>
2. <span data-ttu-id="bd7cd-127">Fare clic su di **Test** profilo nel **profilo** elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-127">Click the **Test** profile in the **Profile** drop-down list.</span></span>
3. <span data-ttu-id="bd7cd-128">Fare clic su di **impostazioni** scheda.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-128">Click the **Settings** tab.</span></span>
4. <span data-ttu-id="bd7cd-129">In **DefaultConnection** nel **database** sezione, deselezionare il **Aggiorna database** casella di controllo.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-129">Under **DefaultConnection** in the **Databases** section, clear the **Update database** check box.</span></span>
5. <span data-ttu-id="bd7cd-130">Fare clic sul **profilo** scheda e quindi scegliere il **gestione temporanea** profilo nel **profilo** elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-130">Click the **Profile** tab, and then click the **Staging** profile in the **Profile** drop-down list.</span></span>
6. <span data-ttu-id="bd7cd-131">Quando viene chiesto se si desidera salvare le modifiche apportate al **Test** del profilo, fare clic su **Sì**.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-131">When you are asked if you want to save the changes made to the **Test** profile, click **Yes**.</span></span>
7. <span data-ttu-id="bd7cd-132">Apportare la stessa modifica nel profilo di gestione temporanea.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-132">Make the same change in the Staging profile.</span></span>
8. <span data-ttu-id="bd7cd-133">Ripetere il processo per apportare la stessa modifica nel profilo di produzione.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-133">Repeat the process to make the same change in the Production profile.</span></span>
9. <span data-ttu-id="bd7cd-134">Chiudi il **pubblica sul Web** procedura guidata.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-134">Close the **Publish Web** wizard.</span></span>

<span data-ttu-id="bd7cd-135">La distribuzione nell'ambiente di testing è ora sufficiente in esecuzione un solo clic pubblicare di nuovo.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-135">Deploying to the test environment is now a simple matter of running one-click publish again.</span></span> <span data-ttu-id="bd7cd-136">Per rendere più veloce questo processo, è possibile utilizzare il **Web-pubblicazione con un clic** barra degli strumenti.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-136">To make this process quicker, you can use the **Web One Click Publish** toolbar.</span></span>

1. <span data-ttu-id="bd7cd-137">Nel **vista** menu, scegliere **barre degli strumenti** e quindi selezionare **Web-pubblicazione con un clic**.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-137">In the **View** menu, choose **Toolbars** and then select **Web One Click Publish**.</span></span>

    ![Selecting_One_Click_Publish_toolbar](deploying-a-code-update/_static/image3.png)
2. <span data-ttu-id="bd7cd-139">In **Esplora**, selezionare il progetto ContosoUniversity.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-139">In **Solution Explorer**, select the ContosoUniversity project.</span></span>
3. <span data-ttu-id="bd7cd-140">il **Web-pubblicazione con un clic** sulla barra degli strumenti, scegliere il **Test** profilo di pubblicazione e quindi fare clic su **pubblica sul Web** (l'icona con frecce rivolte a sinistra e destra).</span><span class="sxs-lookup"><span data-stu-id="bd7cd-140">the **Web One Click Publish** toolbar, choose the **Test** publish profile and then click **Publish Web** (the icon with arrows pointing left and right).</span></span>

    ![Web_One_Click_Publish_toolbar](deploying-a-code-update/_static/image4.png)
4. <span data-ttu-id="bd7cd-142">Visual Studio distribuisce l'applicazione aggiornata e il browser apre automaticamente alla home page.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-142">Visual Studio deploys the updated application, and the browser automatically opens to the home page.</span></span>
5. <span data-ttu-id="bd7cd-143">Eseguire la pagina istruttori e selezionare un docente per verificare che l'aggiornamento è stato distribuito correttamente.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-143">Run the Instructors page and select an instructor to verify that the update was successfully deployed.</span></span>

<span data-ttu-id="bd7cd-144">È normalmente anche eseguire test di regressione (vale a dire il resto del sito per assicurarsi che la nuova modifica non è stato interrotto le funzionalità esistenti di test).</span><span class="sxs-lookup"><span data-stu-id="bd7cd-144">You would normally also do regression testing (that is, test the rest of the site to make sure that the new change didn't break any existing functionality).</span></span> <span data-ttu-id="bd7cd-145">Ma per questa esercitazione verrà ignorare tale passaggio e continuare a distribuire l'aggiornamento di gestione temporanea e produzione.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-145">But for this tutorial you'll skip that step and proceed to deploy the update to staging and production.</span></span>

<span data-ttu-id="bd7cd-146">Quando si ridistribuisce, solo copie modificare file nel server di distribuzione Web determina automaticamente quali file sono stati modificati.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-146">When you redeploy, Web Deploy automatically determines which files have changed and only copies changed files to the server.</span></span> <span data-ttu-id="bd7cd-147">Per impostazione predefinita, distribuzione Web viene utilizzato Data ultima modifica su file per determinare quali sono stati modificati.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-147">By default, Web Deploy uses last-changed dates on files to determine which ones have changed.</span></span> <span data-ttu-id="bd7cd-148">Alcuni sistemi di controllo di origine modificare il file date anche quando non si modifica il contenuto del file.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-148">Some source control systems change file dates even when you don't change the file contents.</span></span> <span data-ttu-id="bd7cd-149">In tal caso, si potrebbe desidera configurare distribuzione Web per utilizzare i checksum di file per determinare quali file sono stati modificati.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-149">In that case, you might want to configure Web Deploy to use file checksums to determine which files have changed.</span></span> <span data-ttu-id="bd7cd-150">Per ulteriori informazioni, vedere [motivo per cui tutti i file ottenere ridistribuiti anche se non è stata modificarle?](https://msdn.microsoft.com/library/ee942158.aspx#use_checksum) in domande frequenti sulla distribuzione di ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-150">For more information, see [Why do all of my files get redeployed although I didn't change them?](https://msdn.microsoft.com/library/ee942158.aspx#use_checksum) in the ASP.NET Deployment FAQ.</span></span>

## <a name="take-the-application-offline-during-deployment"></a><span data-ttu-id="bd7cd-151">Eseguire l'applicazione durante la distribuzione</span><span class="sxs-lookup"><span data-stu-id="bd7cd-151">Take the application offline during deployment</span></span>

<span data-ttu-id="bd7cd-152">La modifica che si esegue la distribuzione è ora una semplice modifica a una singola pagina.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-152">The change you're deploying now is a simple change to a single page.</span></span> <span data-ttu-id="bd7cd-153">Ma a volte si distribuiscono le modifiche più estese, o di distribuire le modifiche di codice e database e il sito potrebbe comportarsi in modo non corretto se un utente richiede una pagina prima del completamento della distribuzione.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-153">But sometimes you deploy larger changes, or you deploy both code and database changes, and the site might behave incorrectly if a user requests a page before deployment is finished.</span></span> <span data-ttu-id="bd7cd-154">Per impedire agli utenti di accedere al sito, mentre la distribuzione è in corso, è possibile utilizzare un *app\_offline.htm* file.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-154">To prevent users from accessing the site while deployment is in progress, you can use an *app\_offline.htm* file.</span></span> <span data-ttu-id="bd7cd-155">Quando si inserisce un file denominato *app\_offline.htm* nella cartella radice dell'applicazione, IIS viene visualizzato automaticamente tale file anziché eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-155">When you put a file named *app\_offline.htm* in the root folder of your application, IIS automatically displays that file instead of running your application.</span></span> <span data-ttu-id="bd7cd-156">Per impedire l'accesso durante la distribuzione, per l'inserimento *app\_offline.htm* nella cartella radice, eseguire il processo di distribuzione, quindi rimuovere *app\_offline.htm* dopo l'esito positivo distribuzione.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-156">So to prevent access during deployment, you put *app\_offline.htm* in the root folder, run the deployment process, and then remove *app\_offline.htm* after successful deployment.</span></span>

<span data-ttu-id="bd7cd-157">È possibile configurare la distribuzione Web per inserire automaticamente un valore predefinito *app\_offline.htm* file nel server quando viene avviata la distribuzione e rimuoverlo al termine.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-157">You can configure Web Deploy to automatically put a default *app\_offline.htm* file on the server when it starts deploying and remove it when it finishes.</span></span> <span data-ttu-id="bd7cd-158">Per eseguire l'operazione che si dovranno eseguire è aggiungere l'elemento XML seguente nel file di profilo (con estensione pubxml) di pubblicazione:</span><span class="sxs-lookup"><span data-stu-id="bd7cd-158">To do that all you have to do is add the following XML element to your publish profile (.pubxml) file:</span></span>

[!code-xml[Main](deploying-a-code-update/samples/sample2.xml)]

<span data-ttu-id="bd7cd-159">Per questa esercitazione verrà visualizzato come creare e utilizzare un oggetto personalizzato *app\_offline.htm* file.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-159">For this tutorial you'll see how to create and use a custom *app\_offline.htm* file.</span></span>

<span data-ttu-id="bd7cd-160">Utilizzando *app\_offline.htm* nel sito di staging non è necessario, perché non si dispone di utenti che accedono al sito di staging.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-160">Using *app\_offline.htm* in the staging site isn't required, because you don't have users accessing the staging site.</span></span> <span data-ttu-id="bd7cd-161">Ma è consigliabile utilizzare gestione temporanea per testare tutto quello che si prevede di distribuire nell'ambiente di produzione.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-161">But it's a good practice to use staging to test everything the way you plan to deploy in production.</span></span>

### <a name="create-appofflinehtm"></a><span data-ttu-id="bd7cd-162">Creare app\_offline.htm</span><span class="sxs-lookup"><span data-stu-id="bd7cd-162">Create app\_offline.htm</span></span>

1. <span data-ttu-id="bd7cd-163">In **Esplora**, fare doppio clic la soluzione e fare clic su **Aggiungi**, quindi fare clic su **nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-163">In **Solution Explorer**, right-click the solution and click **Add**, and then click **New Item**.</span></span>
2. <span data-ttu-id="bd7cd-164">Creare un **pagina HTML** denominato *app\_offline.htm* (eliminare finale "l" il *HTML* estensione di Visual Studio crea per impostazione predefinita).</span><span class="sxs-lookup"><span data-stu-id="bd7cd-164">Create an **HTML Page** named *app\_offline.htm* (delete the final "l" in the *.html* extension that Visual Studio creates by default).</span></span>
3. <span data-ttu-id="bd7cd-165">Sostituire il markup del modello con il markup seguente:</span><span class="sxs-lookup"><span data-stu-id="bd7cd-165">Replace the template markup with the following markup:</span></span>

    [!code-html[Main](deploying-a-code-update/samples/sample3.html)]
4. <span data-ttu-id="bd7cd-166">Salvare e chiudere il file.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-166">Save and close the file.</span></span>

### <a name="copy-appofflinehtm-to-the-root-folder-of-the-web-site"></a><span data-ttu-id="bd7cd-167">Copia app\_offline.htm nella cartella radice del sito web</span><span class="sxs-lookup"><span data-stu-id="bd7cd-167">Copy app\_offline.htm to the root folder of the web site</span></span>

<span data-ttu-id="bd7cd-168">È possibile utilizzare qualsiasi strumento FTP per copiare i file del sito web.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-168">You can use any FTP tool to copy files to the web site.</span></span> <span data-ttu-id="bd7cd-169">[FileZilla](http://filezilla-project.org/) è uno strumento largamente usato FTP e viene visualizzato nelle schermate.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-169">[FileZilla](http://filezilla-project.org/) is a popular FTP tool and is shown in the screen shots.</span></span>

<span data-ttu-id="bd7cd-170">Per utilizzare uno strumento FTP, sono necessari tre operazioni: l'URL di FTP, il nome utente e la password.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-170">To use an FTP tool, you need three things: the FTP URL, the user name, and the password.</span></span>

<span data-ttu-id="bd7cd-171">L'URL viene visualizzato nella pagina dashboard del sito web nel portale di gestione di Azure e il nome utente e la password per il FTP, vedere il *publishsettings* file scaricato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-171">The URL is shown on the web site's dashboard page in the Azure Management Portal, and the user name and password for FTP can be found in the *.publishsettings* file that you downloaded earlier.</span></span> <span data-ttu-id="bd7cd-172">La procedura seguente viene illustrato come ottenere questi valori.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-172">The following steps show how to get these values.</span></span>

1. <span data-ttu-id="bd7cd-173">Nel portale di gestione di Azure, fare clic su **siti Web** scheda e quindi scegliere il sito web di gestione temporanea.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-173">In the Azure Management Portal, click **Web Sites** tab and then click the staging web site.</span></span>
2. <span data-ttu-id="bd7cd-174">Nel **Dashboard** scorrere fino a trovare il nome host FTP nella pagina di **Quick Glance** sezione.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-174">On the **Dashboard** page, scroll down to find the FTP host name in the **Quick Glance** section.</span></span>

    ![Nome host FTP](deploying-a-code-update/_static/image5.png)
3. <span data-ttu-id="bd7cd-176">Aprire la gestione temporanea *publishsettings* file in blocco note o un altro editor di testo.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-176">Open the staging *.publishsettings* file in Notepad or another text editor.</span></span>
4. <span data-ttu-id="bd7cd-177">Trovare il `publishProfile` elemento per il profilo FTP.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-177">Find the `publishProfile` element for the FTP profile.</span></span>
5. <span data-ttu-id="bd7cd-178">Copia il `userName` e `userPWD` valori.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-178">Copy the `userName` and `userPWD` values.</span></span>

    ![Credenziali FTP](deploying-a-code-update/_static/image6.png)
6. <span data-ttu-id="bd7cd-180">Aprire la strumento FTP e accedere a URL FTP.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-180">Open your FTP tool and log on to the FTP URL.</span></span>
7. <span data-ttu-id="bd7cd-181">Copia *app\_offline.htm* dalla cartella della soluzione per il */sito/wwwroot* cartella nel sito di staging.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-181">Copy *app\_offline.htm* from the solution folder to the */site/wwwroot* folder in the staging site.</span></span>

    ![Copia app_offline](deploying-a-code-update/_static/image7.png)
8. <span data-ttu-id="bd7cd-183">Passare all'URL del sito di gestione temporanea.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-183">Browse to your staging site's URL.</span></span> <span data-ttu-id="bd7cd-184">Vedrai che il *app\_offline.htm* pagina viene visualizzata invece la home page.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-184">You see that the *app\_offline.htm* page is now displayed instead of your home page.</span></span>

    ![App_offline.htm nella finestra del browser](deploying-a-code-update/_static/image8.png)

<span data-ttu-id="bd7cd-186">A questo punto si è pronti distribuire in gestione temporanea.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-186">You are now ready to deploy to staging.</span></span>

## <a name="deploy-the-code-update-to-staging-and-production"></a><span data-ttu-id="bd7cd-187">Distribuire l'aggiornamento del codice in gestione temporanea e produzione</span><span class="sxs-lookup"><span data-stu-id="bd7cd-187">Deploy the code update to staging and production</span></span>

1. <span data-ttu-id="bd7cd-188">Nel **Web-pubblicazione con un clic** sulla barra degli strumenti, scegliere il **gestione temporanea** profilo di pubblicazione e quindi fare clic su **pubblica sul Web**.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-188">In the **Web One Click Publish** toolbar, choose the **Staging** publish profile and then click **Publish Web**.</span></span>

    <span data-ttu-id="bd7cd-189">Visual Studio distribuisce l'applicazione aggiornata e viene aperto il browser alla home page del sito.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-189">Visual Studio deploys the updated application and opens the browser to the site's home page.</span></span> <span data-ttu-id="bd7cd-190">Il *app\_offline.htm* file viene visualizzato.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-190">The *app\_offline.htm* file is displayed.</span></span> <span data-ttu-id="bd7cd-191">Poter eseguire il test per verificare la corretta distribuzione, è necessario rimuovere il *app\_offline.htm* file.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-191">Before you can test to verify successful deployment, you must remove the *app\_offline.htm* file.</span></span>
2. <span data-ttu-id="bd7cd-192">Tornare allo strumento FTP ed eliminare **app\_offline.htm** dal sito di staging.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-192">Return to your FTP tool, and delete **app\_offline.htm** from the staging site.</span></span>
3. <span data-ttu-id="bd7cd-193">Nel browser, aprire la pagina istruttori nel sito di staging e selezionare un docente per verificare che l'aggiornamento è stato distribuito correttamente.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-193">In the browser, open the Instructors page in the staging site, and select an instructor to verify that the update was successfully deployed.</span></span>
4. <span data-ttu-id="bd7cd-194">Seguire la stessa procedura per la produzione utilizzata per la gestione temporanea.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-194">Follow the same procedure for production as you did for staging.</span></span>

<a id="specificfiles"></a>

## <a name="reviewing-changes-and-deploying-specific-files"></a><span data-ttu-id="bd7cd-195">Revisione delle modifiche e la distribuzione di file specifici</span><span class="sxs-lookup"><span data-stu-id="bd7cd-195">Reviewing Changes and Deploying Specific Files</span></span>

<span data-ttu-id="bd7cd-196">Visual Studio 2012 offre inoltre la possibilità di distribuire singoli file.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-196">Visual Studio 2012 also gives you the ability to deploy individual files.</span></span> <span data-ttu-id="bd7cd-197">Per un file selezionato è possibile visualizzare le differenze tra la versione locale e la versione distribuita, distribuire il file nell'ambiente di destinazione o copiare il file dall'ambiente di destinazione del progetto locale.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-197">For a selected file you can view differences between the local version and the deployed version, deploy the file to the destination environment, or copy the file from the destination environment to the local project.</span></span> <span data-ttu-id="bd7cd-198">In questa sezione dell'esercitazione è illustrato come utilizzare queste funzionalità.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-198">In this section of the tutorial you see how to use these features.</span></span>

### <a name="make-a-change-to-deploy"></a><span data-ttu-id="bd7cd-199">Apportare una modifica per la distribuzione</span><span class="sxs-lookup"><span data-stu-id="bd7cd-199">Make a change to deploy</span></span>

1. <span data-ttu-id="bd7cd-200">Aprire *Content/Site.css*e individuare il blocco per il `body` tag.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-200">Open *Content/Site.css*, and find the block for the `body` tag.</span></span>
2. <span data-ttu-id="bd7cd-201">Modificare il valore di `background-color` da `#fff` a `darkblue`.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-201">Change the value for `background-color` from `#fff` to `darkblue`.</span></span>

    [!code-css[Main](deploying-a-code-update/samples/sample4.css?highlight=2)]

### <a name="view-the-change-in-the-publish-preview-window"></a><span data-ttu-id="bd7cd-202">Visualizzare la modifica nella finestra di anteprima pubblica</span><span class="sxs-lookup"><span data-stu-id="bd7cd-202">View the change in the Publish Preview window</span></span>

<span data-ttu-id="bd7cd-203">Quando si utilizza il **pubblica sul Web** procedura guidata per pubblicare il progetto, è possibile vedere alle modifiche che si desidera vengano pubblicati dal doppio clic sul file di **anteprima** finestra.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-203">When you use the **Publish Web** wizard to publish the project, you can see what changes are going to be published by double-clicking the file in the **Preview** window.</span></span>

1. <span data-ttu-id="bd7cd-204">Fare clic sul progetto ContosoUniversity e fare clic su **pubblica**.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-204">Right-click the ContosoUniversity project and click **Publish**.</span></span>
2. <span data-ttu-id="bd7cd-205">Dal **profilo** elenco a discesa, seleziona il **Test** profilo di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-205">From the **Profile** drop-down list, select the **Test** publish profile.</span></span>
3. <span data-ttu-id="bd7cd-206">Fare clic su **anteprima**, quindi fare clic su **avviare Anteprima**.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-206">Click **Preview**, and then click **Start Preview**.</span></span>
4. <span data-ttu-id="bd7cd-207">Nel **anteprima** riquadro, fare doppio clic su **Site.css**.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-207">In the **Preview** pane, double-click **Site.css**.</span></span>

    ![Fare doppio clic su Site](deploying-a-code-update/_static/image9.png)

    <span data-ttu-id="bd7cd-209">Il **Anteprima modifiche** finestra di dialogo viene visualizzata un'anteprima delle modifiche che verranno distribuiti.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-209">The **Preview changes** dialog shows a preview of the changes that will be deployed.</span></span>

    ![Anteprima modifiche a Site](deploying-a-code-update/_static/image10.png)

    <span data-ttu-id="bd7cd-211">Se si fa doppio clic il *Web. config* file, il **Anteprima modifiche** finestra di dialogo Mostra l'effetto della compilazione trasformazioni di configurazione e le trasformazioni di profilo di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-211">If you double-click the *Web.config* file, the **Preview changes** dialog shows the effect of your build configuration transformations and publish profile transformations.</span></span> <span data-ttu-id="bd7cd-212">A questo punto non è stata eseguita alcuna operazione che fa sì che il *Web. config* file sul server per cambiare, pertanto è non prevista alcuna modifica.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-212">At this point you have not done anything that would cause the *Web.config* file on the server to change, so you expect to see no changes.</span></span> <span data-ttu-id="bd7cd-213">Tuttavia, il **Anteprima modifiche** finestra in modo non corretto mostra due modifiche.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-213">However, the **Preview changes** window incorrectly shows two changes.</span></span> <span data-ttu-id="bd7cd-214">Sembra che due elementi XML verranno rimosso.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-214">It looks like two XML elements will be removed.</span></span> <span data-ttu-id="bd7cd-215">Questi elementi vengono aggiunti dal processo di pubblicazione quando si seleziona **eseguire migrazioni Code First all'avvio dell'applicazione** per una classe del contesto Code First.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-215">These elements are added by the publish process when you select **Execute Code First Migrations on application start** for a Code First context class.</span></span> <span data-ttu-id="bd7cd-216">Il confronto viene eseguito prima che il processo di pubblicazione aggiunge gli elementi, in modo che rispecchi come essi vengono rimossi anche se non verranno rimossi.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-216">The comparison is done before the publish process adds those elements, so it looks like they are being removed although they will not be removed.</span></span> <span data-ttu-id="bd7cd-217">Questo errore verrà corretto nelle versioni future.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-217">This error will be corrected in a future release.</span></span>
5. <span data-ttu-id="bd7cd-218">Fare clic su **Chiudi**.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-218">Click **Close**.</span></span>
6. <span data-ttu-id="bd7cd-219">Fare clic su **Pubblica**.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-219">Click **Publish**.</span></span>
7. <span data-ttu-id="bd7cd-220">Quando si apre il browser alla home page del sito di Test, premere CTRL + F5 per provocare un aggiornamento hardware per verificare l'effetto della modifica CSS.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-220">When the browser opens to the home page of the Test site, press CTRL+F5 to cause a hard refresh in order to see the effect of the CSS change.</span></span>

    ![Effetto della modifica CSS](deploying-a-code-update/_static/image11.png)
8. <span data-ttu-id="bd7cd-222">Chiudere il browser.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-222">Close the browser.</span></span>

### <a name="publish-specific-files-from-solution-explorer"></a><span data-ttu-id="bd7cd-223">Pubblicare i file specifici da Esplora soluzioni</span><span class="sxs-lookup"><span data-stu-id="bd7cd-223">Publish specific files from Solution Explorer</span></span>

<span data-ttu-id="bd7cd-224">Si supponga non come sfondo blu e ripristinare il colore originale.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-224">Suppose you don't like the blue background and want to revert to the original color.</span></span> <span data-ttu-id="bd7cd-225">In questa sezione potrai ripristinare le impostazioni originali tramite la pubblicazione di un file specifico direttamente da **Esplora**.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-225">In this section you'll restore the original settings by publishing a specific file directly from **Solution Explorer**.</span></span>

1. <span data-ttu-id="bd7cd-226">Aprire *Content/Site.css* e il ripristino di `background-color` impostando su `#fff`.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-226">Open *Content/Site.css* and restore the `background-color` setting to `#fff`.</span></span>
2. <span data-ttu-id="bd7cd-227">In **Esplora**, fare doppio clic su di *Content/Site.css* file.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-227">In **Solution Explorer**, right-click the *Content/Site.css* file.</span></span>

    <span data-ttu-id="bd7cd-228">Menu di scelta rapida mostra che tre opzioni di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-228">The context menu shows three publish options.</span></span>

    ![Pubblica opzioni da Esplora soluzioni](deploying-a-code-update/_static/image12.png)
3. <span data-ttu-id="bd7cd-230">Fare clic su **Anteprima modifiche a Site**.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-230">Click **Preview changes to Site.css**.</span></span>

    <span data-ttu-id="bd7cd-231">Verrà visualizzata una finestra per visualizzare le differenze tra il file locale e la versione nell'ambiente di destinazione.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-231">A window opens to show the differences between the local file and the version of it in the destination environment.</span></span>

    ![Diff-Content/Site.css](deploying-a-code-update/_static/image13.png)
4. <span data-ttu-id="bd7cd-233">In **Esplora**, fare doppio clic su **Site.css** nuovamente e fare clic su **pubblicare Site**.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-233">In **Solution Explorer**, right-click **Site.css** again and click **Publish Site.css**.</span></span>

    <span data-ttu-id="bd7cd-234">Il **attività di pubblicazione Web** finestra mostra che il file sia stato pubblicato.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-234">The **Web Publish Activity** window shows that the file has been published.</span></span>

    ![Finestra attività di pubblicazione Web](deploying-a-code-update/_static/image14.png)
5. <span data-ttu-id="bd7cd-236">Aprire una finestra del browser il `http://localhost/contosouniversity` URL e quindi premere CTRL + F5 per provocare un disco rigido aggiornare per visualizzare l'effetto del foglio di stile CSS di modifica.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-236">Open a browser to the `http://localhost/contosouniversity` URL, and then press CTRL+F5 to cause a hard refresh in order to see the effect of the CSS change.</span></span>

    ![Home page con CSS normale](deploying-a-code-update/_static/image15.png)
6. <span data-ttu-id="bd7cd-238">Chiudere il browser.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-238">Close the browser.</span></span>

## <a name="summary"></a><span data-ttu-id="bd7cd-239">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="bd7cd-239">Summary</span></span>

<span data-ttu-id="bd7cd-240">Abbiamo visto diversi modi per distribuire un aggiornamento di un'applicazione che non comportano una modifica al database e si è visto come visualizzare in anteprima le modifiche per verificare che cosa verrà aggiornata è quello previsto.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-240">You've now seen several ways to deploy an application update that does not involve a database change, and you've seen how to preview the changes to verify that what will be updated is what you expect.</span></span> <span data-ttu-id="bd7cd-241">La pagina istruttori ora ha un **corsi invece** sezione.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-241">The Instructors page now has a **Courses Taught** section.</span></span>

![Pagina istruttori con corsi tenuti](deploying-a-code-update/_static/image16.png)

<span data-ttu-id="bd7cd-243">L'esercitazione successiva viene illustrato come distribuire una modifica al database: un campo Data di nascita verrà aggiunto al database e alla pagina i docenti.</span><span class="sxs-lookup"><span data-stu-id="bd7cd-243">The next tutorial shows you how to deploy a database change: you'll add a birthdate field to the database and to the Instructors page.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="bd7cd-244">[Precedente](deploying-to-production.md)
> [Successivo](deploying-a-database-update.md)</span><span class="sxs-lookup"><span data-stu-id="bd7cd-244">[Previous](deploying-to-production.md)
[Next](deploying-a-database-update.md)</span></span>
