---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
title: 'Distribuzione Web ASP.NET tramite Visual Studio: distribuzione di un aggiornamento del codice | Microsoft Docs'
author: tdykstra
description: Questa serie di esercitazioni illustra come distribuire, pubblicare, ASP.NET per App Web di servizio App di Azure o per un provider di hosting di terze parti, di applicazioni web da utilizza...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: c76dbc35-a914-4ee3-919c-4f4d1fa05104
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
msc.type: authoredcontent
ms.openlocfilehash: a3d181f3ec8db74781c550720ba4331bdf6478b3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832092"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-a-code-update"></a><span data-ttu-id="1677a-103">Distribuzione Web ASP.NET tramite Visual Studio: distribuzione di un aggiornamento del codice</span><span class="sxs-lookup"><span data-stu-id="1677a-103">ASP.NET Web Deployment using Visual Studio: Deploying a Code Update</span></span>
====================
<span data-ttu-id="1677a-104">da [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="1677a-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="1677a-105">Download progetto iniziale</span><span class="sxs-lookup"><span data-stu-id="1677a-105">Download Starter Project</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> <span data-ttu-id="1677a-106">Questa serie di esercitazioni illustra come distribuire, pubblicare, ASP.NET per App Web di servizio App di Azure o per un provider di hosting di terze parti, di applicazioni web usando Visual Studio 2012 o Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="1677a-106">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider, by using Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="1677a-107">Per informazioni sulla serie, vedere [la prima esercitazione della serie](introduction.md).</span><span class="sxs-lookup"><span data-stu-id="1677a-107">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="1677a-108">Panoramica</span><span class="sxs-lookup"><span data-stu-id="1677a-108">Overview</span></span>

<span data-ttu-id="1677a-109">Dopo la distribuzione iniziale, continua il lavoro della manutenzione e il sito web di sviluppo e prima di prolungata è opportuno distribuire un aggiornamento.</span><span class="sxs-lookup"><span data-stu-id="1677a-109">After the initial deployment, your work of maintaining and developing your web site continues, and before long you will want to deploy an update.</span></span> <span data-ttu-id="1677a-110">Questa esercitazione illustra il processo di distribuzione di aggiornamenti al codice dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="1677a-110">This tutorial takes you through the process of deploying an update to your application code.</span></span> <span data-ttu-id="1677a-111">L'aggiornamento che implementi e Distribuisci in questa esercitazione non comporta una modifica del database; scoprirai che cos'è diversi sulla distribuzione di una modifica al database nella prossima esercitazione.</span><span class="sxs-lookup"><span data-stu-id="1677a-111">The update that you implement and deploy in this tutorial does not involve a database change; you'll see what's different about deploying a database change in the next tutorial.</span></span>

<span data-ttu-id="1677a-112">Promemoria: Se viene visualizzato un messaggio di errore o qualcosa non funziona durante l'esecuzione dell'esercitazione, assicurarsi di controllare la [risoluzione dei problemi pagina](troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="1677a-112">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](troubleshooting.md).</span></span>

## <a name="make-a-code-change"></a><span data-ttu-id="1677a-113">Modificare il codice</span><span class="sxs-lookup"><span data-stu-id="1677a-113">Make a code change</span></span>

<span data-ttu-id="1677a-114">Un esempio semplice di un aggiornamento all'applicazione, si aggiungeranno al **instructors (insegnanti)** pagina un elenco dei corsi tenuti dall'insegnante selezionato.</span><span class="sxs-lookup"><span data-stu-id="1677a-114">As a simple example of an update to your application, you'll add to the **Instructors** page a list of courses taught by the selected instructor.</span></span>

<span data-ttu-id="1677a-115">Se si esegue la **instructors (insegnanti)** pagina, si noterà che esistono **selezionare** collegamenti nella griglia, ma non esegue alcuna operazione oltre marca il grigio turni di riga in background.</span><span class="sxs-lookup"><span data-stu-id="1677a-115">If you run the **Instructors** page, you'll notice that there are **Select** links in the grid, but they don't do anything other than make the row background turn gray.</span></span>

![Pagina instructors (insegnanti) con la selezione](deploying-a-code-update/_static/image1.png)

<span data-ttu-id="1677a-117">A questo punto si aggiungerà codice che viene eseguito quando il **seleziona** collegamento fa e visualizza un elenco dei corsi tenuti dall'insegnante selezionato.</span><span class="sxs-lookup"><span data-stu-id="1677a-117">Now you'll add code that runs when the **Select** link is clicked and displays a list of courses taught by the selected instructor .</span></span>

1. <span data-ttu-id="1677a-118">Nelle *Instructors.aspx*, aggiungere il markup seguente immediatamente dopo il **ErrorMessageLabel** `Label` controllo:</span><span class="sxs-lookup"><span data-stu-id="1677a-118">In *Instructors.aspx*, add the following markup immediately after the **ErrorMessageLabel** `Label` control:</span></span>

    [!code-aspx[Main](deploying-a-code-update/samples/sample1.aspx)]
2. <span data-ttu-id="1677a-119">Eseguire la pagina e selezionare un insegnante.</span><span class="sxs-lookup"><span data-stu-id="1677a-119">Run the page and select an instructor.</span></span> <span data-ttu-id="1677a-120">Viene visualizzato un elenco dei corsi tenuti da tale insegnante.</span><span class="sxs-lookup"><span data-stu-id="1677a-120">You see a list of courses taught by that instructor.</span></span>

    ![Pagina instructors (insegnanti) con i corsi tenuti](deploying-a-code-update/_static/image2.png)
3. <span data-ttu-id="1677a-122">Chiudere il browser.</span><span class="sxs-lookup"><span data-stu-id="1677a-122">Close the browser.</span></span>

## <a name="deploy-the-code-update-to-the-test-environment"></a><span data-ttu-id="1677a-123">Distribuire l'aggiornamento del codice nell'ambiente di test</span><span class="sxs-lookup"><span data-stu-id="1677a-123">Deploy the code update to the test environment</span></span>

<span data-ttu-id="1677a-124">Prima di utilizzare i profili di pubblicazione per la distribuzione di test, staging e produzione, è necessario modificare le opzioni di pubblicazione del database.</span><span class="sxs-lookup"><span data-stu-id="1677a-124">Before you can use your publish profiles to deploy to test, staging, and production, you need to change database publishing options.</span></span> <span data-ttu-id="1677a-125">È non è più necessario eseguire gli script di distribuzione grant e i dati per il database di appartenenza.</span><span class="sxs-lookup"><span data-stu-id="1677a-125">You no longer need to run the grant and data deployment scripts for the membership database.</span></span>

1. <span data-ttu-id="1677a-126">Aprire il **pubblica sul Web** procedura guidata facendo clic sul progetto ContosoUniversity e facendo clic su **Publish**.</span><span class="sxs-lookup"><span data-stu-id="1677a-126">Open the **Publish Web** wizard by right-clicking the ContosoUniversity project and clicking **Publish**.</span></span>
2. <span data-ttu-id="1677a-127">Fare clic sui **Test** profilare nel **profilo** elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="1677a-127">Click the **Test** profile in the **Profile** drop-down list.</span></span>
3. <span data-ttu-id="1677a-128">Scegliere il **impostazioni** scheda.</span><span class="sxs-lookup"><span data-stu-id="1677a-128">Click the **Settings** tab.</span></span>
4. <span data-ttu-id="1677a-129">Sotto **DefaultConnection** nel **database** sezione, deselezionare il **Aggiorna database** casella di controllo.</span><span class="sxs-lookup"><span data-stu-id="1677a-129">Under **DefaultConnection** in the **Databases** section, clear the **Update database** check box.</span></span>
5. <span data-ttu-id="1677a-130">Fare clic sul **profilo** scheda e quindi fare clic sul **Staging** profilare nel **profilo** elenco a discesa.</span><span class="sxs-lookup"><span data-stu-id="1677a-130">Click the **Profile** tab, and then click the **Staging** profile in the **Profile** drop-down list.</span></span>
6. <span data-ttu-id="1677a-131">Quando viene chiesto se si desidera salvare le modifiche apportate per il **Test** del profilo, fare clic su **Sì**.</span><span class="sxs-lookup"><span data-stu-id="1677a-131">When you are asked if you want to save the changes made to the **Test** profile, click **Yes**.</span></span>
7. <span data-ttu-id="1677a-132">Apportare la stessa modifica nel profilo di gestione temporanea.</span><span class="sxs-lookup"><span data-stu-id="1677a-132">Make the same change in the Staging profile.</span></span>
8. <span data-ttu-id="1677a-133">Ripetere il processo per apportare la stessa modifica nel profilo di produzione.</span><span class="sxs-lookup"><span data-stu-id="1677a-133">Repeat the process to make the same change in the Production profile.</span></span>
9. <span data-ttu-id="1677a-134">Chiudi il **pubblica sul Web** procedura guidata.</span><span class="sxs-lookup"><span data-stu-id="1677a-134">Close the **Publish Web** wizard.</span></span>

<span data-ttu-id="1677a-135">La distribuzione nell'ambiente di test è ora una semplice questione di esecuzione con un clic pubblicare di nuovo.</span><span class="sxs-lookup"><span data-stu-id="1677a-135">Deploying to the test environment is now a simple matter of running one-click publish again.</span></span> <span data-ttu-id="1677a-136">Per rendere più veloce il processo, è possibile usare la **Web-pubblicazione con un clic** sulla barra degli strumenti.</span><span class="sxs-lookup"><span data-stu-id="1677a-136">To make this process quicker, you can use the **Web One Click Publish** toolbar.</span></span>

1. <span data-ttu-id="1677a-137">Nel **View** menu, scegliere **barre degli strumenti** e quindi selezionare **Web-pubblicazione con un clic**.</span><span class="sxs-lookup"><span data-stu-id="1677a-137">In the **View** menu, choose **Toolbars** and then select **Web One Click Publish**.</span></span>

    ![Selecting_One_Click_Publish_toolbar](deploying-a-code-update/_static/image3.png)
2. <span data-ttu-id="1677a-139">Nelle **Esplora soluzioni**, selezionare il progetto ContosoUniversity.</span><span class="sxs-lookup"><span data-stu-id="1677a-139">In **Solution Explorer**, select the ContosoUniversity project.</span></span>
3. <span data-ttu-id="1677a-140">il **Web-pubblicazione con un clic** sulla barra degli strumenti, scegliere il **Test** profilo di pubblicazione e quindi fare clic su **pubblica sul Web** (l'icona con frecce che puntano a sinistra e destra).</span><span class="sxs-lookup"><span data-stu-id="1677a-140">the **Web One Click Publish** toolbar, choose the **Test** publish profile and then click **Publish Web** (the icon with arrows pointing left and right).</span></span>

    ![Web_One_Click_Publish_toolbar](deploying-a-code-update/_static/image4.png)
4. <span data-ttu-id="1677a-142">Visual Studio distribuisce l'applicazione aggiornata e il browser viene aperta automaticamente alla home page.</span><span class="sxs-lookup"><span data-stu-id="1677a-142">Visual Studio deploys the updated application, and the browser automatically opens to the home page.</span></span>
5. <span data-ttu-id="1677a-143">Eseguire la pagina instructors (insegnanti) e selezionare un insegnante per verificare che l'aggiornamento è stato distribuito correttamente.</span><span class="sxs-lookup"><span data-stu-id="1677a-143">Run the Instructors page and select an instructor to verify that the update was successfully deployed.</span></span>

<span data-ttu-id="1677a-144">In genere viene usato anche eseguire test di regressione (vale a dire, il resto del sito per assicurarsi che la nuova modifica non interrompe con le funzionalità esistenti di test).</span><span class="sxs-lookup"><span data-stu-id="1677a-144">You would normally also do regression testing (that is, test the rest of the site to make sure that the new change didn't break any existing functionality).</span></span> <span data-ttu-id="1677a-145">Ma per questa esercitazione è possibile ignorare il passaggio e andare al distribuire l'aggiornamento a gestione temporanea e produzione.</span><span class="sxs-lookup"><span data-stu-id="1677a-145">But for this tutorial you'll skip that step and proceed to deploy the update to staging and production.</span></span>

<span data-ttu-id="1677a-146">Quando si ridistribuisce, distribuzione Web determina automaticamente quali file sono stati modificati e copia solo i file modificati nel server.</span><span class="sxs-lookup"><span data-stu-id="1677a-146">When you redeploy, Web Deploy automatically determines which files have changed and only copies changed files to the server.</span></span> <span data-ttu-id="1677a-147">Per impostazione predefinita, distribuzione Web viene utilizzato Data ultima modifica sui file per determinare quelle che sono stati modificati.</span><span class="sxs-lookup"><span data-stu-id="1677a-147">By default, Web Deploy uses last-changed dates on files to determine which ones have changed.</span></span> <span data-ttu-id="1677a-148">Alcuni sistemi di controllo del codice sorgente cambiano file date anche quando non modifichi il contenuto del file.</span><span class="sxs-lookup"><span data-stu-id="1677a-148">Some source control systems change file dates even when you don't change the file contents.</span></span> <span data-ttu-id="1677a-149">In tal caso, è possibile configurare distribuzione Web per usare checksum dei file per determinare quali file sono stati modificati.</span><span class="sxs-lookup"><span data-stu-id="1677a-149">In that case, you might want to configure Web Deploy to use file checksums to determine which files have changed.</span></span> <span data-ttu-id="1677a-150">Per altre informazioni, vedere [motivo per cui tutti i file di ridistribuzione anche se non hai modificarli?](https://msdn.microsoft.com/library/ee942158.aspx#use_checksum) nelle domande frequenti sulla distribuzione di ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="1677a-150">For more information, see [Why do all of my files get redeployed although I didn't change them?](https://msdn.microsoft.com/library/ee942158.aspx#use_checksum) in the ASP.NET Deployment FAQ.</span></span>

## <a name="take-the-application-offline-during-deployment"></a><span data-ttu-id="1677a-151">Portare offline l'applicazione durante la distribuzione</span><span class="sxs-lookup"><span data-stu-id="1677a-151">Take the application offline during deployment</span></span>

<span data-ttu-id="1677a-152">La modifica che si esegue la distribuzione è ora una semplice modifica a una singola pagina.</span><span class="sxs-lookup"><span data-stu-id="1677a-152">The change you're deploying now is a simple change to a single page.</span></span> <span data-ttu-id="1677a-153">Ma in alcuni casi si distribuiscono modifiche più estese, o si distribuiscono le modifiche di codice e database e il sito potrebbe comportarsi in modo non corretto se un utente richiede una pagina prima del completamento della distribuzione.</span><span class="sxs-lookup"><span data-stu-id="1677a-153">But sometimes you deploy larger changes, or you deploy both code and database changes, and the site might behave incorrectly if a user requests a page before deployment is finished.</span></span> <span data-ttu-id="1677a-154">Per impedire agli utenti di accedere al sito durante la distribuzione è in corso, è possibile usare un *app\_offline.htm* file.</span><span class="sxs-lookup"><span data-stu-id="1677a-154">To prevent users from accessing the site while deployment is in progress, you can use an *app\_offline.htm* file.</span></span> <span data-ttu-id="1677a-155">Quando si inserisce un file denominato *app\_offline.htm* nella cartella radice dell'applicazione, IIS consente di visualizzare automaticamente tale file invece di eseguire l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="1677a-155">When you put a file named *app\_offline.htm* in the root folder of your application, IIS automatically displays that file instead of running your application.</span></span> <span data-ttu-id="1677a-156">Per impedire l'accesso durante la distribuzione, per l'inserimento *app\_offline.htm* nella cartella radice, eseguire il processo di distribuzione e quindi rimuovere *app\_offline.htm* dopo l'esito positivo distribuzione.</span><span class="sxs-lookup"><span data-stu-id="1677a-156">So to prevent access during deployment, you put *app\_offline.htm* in the root folder, run the deployment process, and then remove *app\_offline.htm* after successful deployment.</span></span>

<span data-ttu-id="1677a-157">È possibile configurare distribuzione Web per inserire automaticamente un valore predefinito *app\_offline.htm* all'avvio la distribuzione di file nel server e rimuoverlo al termine.</span><span class="sxs-lookup"><span data-stu-id="1677a-157">You can configure Web Deploy to automatically put a default *app\_offline.htm* file on the server when it starts deploying and remove it when it finishes.</span></span> <span data-ttu-id="1677a-158">A scopo che tutto è necessario effettuare è aggiungere l'elemento XML seguente al file (con estensione pubxml) del profilo di pubblicazione:</span><span class="sxs-lookup"><span data-stu-id="1677a-158">To do that all you have to do is add the following XML element to your publish profile (.pubxml) file:</span></span>

[!code-xml[Main](deploying-a-code-update/samples/sample2.xml)]

<span data-ttu-id="1677a-159">Per questa esercitazione verrà illustrato come creare e usare una classe personalizzata *app\_offline.htm* file.</span><span class="sxs-lookup"><span data-stu-id="1677a-159">For this tutorial you'll see how to create and use a custom *app\_offline.htm* file.</span></span>

<span data-ttu-id="1677a-160">Usando *app\_offline.htm* nel sito di staging non è necessaria, perché non si dispone di utenti accede al sito di gestione temporanea.</span><span class="sxs-lookup"><span data-stu-id="1677a-160">Using *app\_offline.htm* in the staging site isn't required, because you don't have users accessing the staging site.</span></span> <span data-ttu-id="1677a-161">Ma è buona norma utilizzare gestione temporanea per testare tutti gli elementi il modo in cui che si intende distribuire nell'ambiente di produzione.</span><span class="sxs-lookup"><span data-stu-id="1677a-161">But it's a good practice to use staging to test everything the way you plan to deploy in production.</span></span>

### <a name="create-appofflinehtm"></a><span data-ttu-id="1677a-162">Creare app\_offline.htm</span><span class="sxs-lookup"><span data-stu-id="1677a-162">Create app\_offline.htm</span></span>

1. <span data-ttu-id="1677a-163">Nelle **Esplora soluzioni**, fare doppio clic la soluzione e fare clic su **Add**, quindi fare clic su **nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="1677a-163">In **Solution Explorer**, right-click the solution and click **Add**, and then click **New Item**.</span></span>
2. <span data-ttu-id="1677a-164">Creare un **pagina HTML** denominato *app\_offline.htm* (eliminare finale "l" nel *HTML* estensione creato da Visual Studio per impostazione predefinita).</span><span class="sxs-lookup"><span data-stu-id="1677a-164">Create an **HTML Page** named *app\_offline.htm* (delete the final "l" in the *.html* extension that Visual Studio creates by default).</span></span>
3. <span data-ttu-id="1677a-165">Sostituire il markup del modello con il markup seguente:</span><span class="sxs-lookup"><span data-stu-id="1677a-165">Replace the template markup with the following markup:</span></span>

    [!code-html[Main](deploying-a-code-update/samples/sample3.html)]
4. <span data-ttu-id="1677a-166">Salvare e chiudere il file.</span><span class="sxs-lookup"><span data-stu-id="1677a-166">Save and close the file.</span></span>

### <a name="copy-appofflinehtm-to-the-root-folder-of-the-web-site"></a><span data-ttu-id="1677a-167">Copia app\_offline.htm nella cartella radice del sito web</span><span class="sxs-lookup"><span data-stu-id="1677a-167">Copy app\_offline.htm to the root folder of the web site</span></span>

<span data-ttu-id="1677a-168">È possibile usare qualsiasi strumento FTP per copiare i file al sito web.</span><span class="sxs-lookup"><span data-stu-id="1677a-168">You can use any FTP tool to copy files to the web site.</span></span> <span data-ttu-id="1677a-169">[FileZilla](http://filezilla-project.org/) è uno strumento diffuso FTP e viene illustrato nelle schermate.</span><span class="sxs-lookup"><span data-stu-id="1677a-169">[FileZilla](http://filezilla-project.org/) is a popular FTP tool and is shown in the screen shots.</span></span>

<span data-ttu-id="1677a-170">Per usare uno strumento FTP, sono necessarie tre cose: l'URL di FTP, il nome utente e la password.</span><span class="sxs-lookup"><span data-stu-id="1677a-170">To use an FTP tool, you need three things: the FTP URL, the user name, and the password.</span></span>

<span data-ttu-id="1677a-171">L'URL è visualizzato nella pagina dashboard del sito web nel portale di gestione di Azure e il nome utente e password per FTP sono reperibili nel *publishsettings* file scaricato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="1677a-171">The URL is shown on the web site's dashboard page in the Azure Management Portal, and the user name and password for FTP can be found in the *.publishsettings* file that you downloaded earlier.</span></span> <span data-ttu-id="1677a-172">La procedura seguente illustra come ottenere questi valori.</span><span class="sxs-lookup"><span data-stu-id="1677a-172">The following steps show how to get these values.</span></span>

1. <span data-ttu-id="1677a-173">Nel portale di gestione di Azure fare clic su **siti Web** scheda e quindi fare clic sul sito web di staging.</span><span class="sxs-lookup"><span data-stu-id="1677a-173">In the Azure Management Portal, click **Web Sites** tab and then click the staging web site.</span></span>
2. <span data-ttu-id="1677a-174">Nel **Dashboard** pagina, scorrere fino a trovare assegnare un nome host FTP nella **Quick Glance** sezione.</span><span class="sxs-lookup"><span data-stu-id="1677a-174">On the **Dashboard** page, scroll down to find the FTP host name in the **Quick Glance** section.</span></span>

    ![Nome host FTP](deploying-a-code-update/_static/image5.png)
3. <span data-ttu-id="1677a-176">Aprire la gestione temporanea *publishsettings* file in blocco note o un altro editor di testo.</span><span class="sxs-lookup"><span data-stu-id="1677a-176">Open the staging *.publishsettings* file in Notepad or another text editor.</span></span>
4. <span data-ttu-id="1677a-177">Trovare il `publishProfile` (elemento) per il profilo FTP.</span><span class="sxs-lookup"><span data-stu-id="1677a-177">Find the `publishProfile` element for the FTP profile.</span></span>
5. <span data-ttu-id="1677a-178">Copia il `userName` e `userPWD` valori.</span><span class="sxs-lookup"><span data-stu-id="1677a-178">Copy the `userName` and `userPWD` values.</span></span>

    ![Credenziali FTP](deploying-a-code-update/_static/image6.png)
6. <span data-ttu-id="1677a-180">Aprire lo strumento FTP e l'URL di FTP vi accede.</span><span class="sxs-lookup"><span data-stu-id="1677a-180">Open your FTP tool and log on to the FTP URL.</span></span>
7. <span data-ttu-id="1677a-181">Copia *app\_offline.htm* dalla cartella della soluzione per il */site/wwwroot* cartella nel sito di staging.</span><span class="sxs-lookup"><span data-stu-id="1677a-181">Copy *app\_offline.htm* from the solution folder to the */site/wwwroot* folder in the staging site.</span></span>

    ![Copiare app_offline](deploying-a-code-update/_static/image7.png)
8. <span data-ttu-id="1677a-183">Passare all'URL del sito di gestione temporanea.</span><span class="sxs-lookup"><span data-stu-id="1677a-183">Browse to your staging site's URL.</span></span> <span data-ttu-id="1677a-184">Si può osservare che il *app\_offline.htm* viene visualizzata la pagina anziché la home page.</span><span class="sxs-lookup"><span data-stu-id="1677a-184">You see that the *app\_offline.htm* page is now displayed instead of your home page.</span></span>

    ![App_offline. htm nella finestra del browser](deploying-a-code-update/_static/image8.png)

<span data-ttu-id="1677a-186">A questo punto si è pronti distribuire in gestione temporanea.</span><span class="sxs-lookup"><span data-stu-id="1677a-186">You are now ready to deploy to staging.</span></span>

## <a name="deploy-the-code-update-to-staging-and-production"></a><span data-ttu-id="1677a-187">Distribuire l'aggiornamento del codice in gestione temporanea e produzione</span><span class="sxs-lookup"><span data-stu-id="1677a-187">Deploy the code update to staging and production</span></span>

1. <span data-ttu-id="1677a-188">Nel **Web-pubblicazione con un clic** sulla barra degli strumenti, scegliere il **Staging** profilo di pubblicazione e quindi fare clic su **pubblica sul Web**.</span><span class="sxs-lookup"><span data-stu-id="1677a-188">In the **Web One Click Publish** toolbar, choose the **Staging** publish profile and then click **Publish Web**.</span></span>

    <span data-ttu-id="1677a-189">Visual Studio distribuisce l'applicazione aggiornata e verrà aperto il browser alla pagina iniziale del sito.</span><span class="sxs-lookup"><span data-stu-id="1677a-189">Visual Studio deploys the updated application and opens the browser to the site's home page.</span></span> <span data-ttu-id="1677a-190">Il *app\_offline.htm* file viene visualizzato.</span><span class="sxs-lookup"><span data-stu-id="1677a-190">The *app\_offline.htm* file is displayed.</span></span> <span data-ttu-id="1677a-191">Prima di testare per verificare la corretta distribuzione, è necessario rimuovere il *app\_offline.htm* file.</span><span class="sxs-lookup"><span data-stu-id="1677a-191">Before you can test to verify successful deployment, you must remove the *app\_offline.htm* file.</span></span>
2. <span data-ttu-id="1677a-192">Tornare allo strumento FTP ed eliminare **app\_offline.htm** dal sito di staging.</span><span class="sxs-lookup"><span data-stu-id="1677a-192">Return to your FTP tool, and delete **app\_offline.htm** from the staging site.</span></span>
3. <span data-ttu-id="1677a-193">Nel browser, aprire la pagina instructors (insegnanti) nel sito di staging e selezionare un insegnante per verificare che l'aggiornamento è stato distribuito correttamente.</span><span class="sxs-lookup"><span data-stu-id="1677a-193">In the browser, open the Instructors page in the staging site, and select an instructor to verify that the update was successfully deployed.</span></span>
4. <span data-ttu-id="1677a-194">Come è stato fatto per la gestione temporanea, seguire la stessa procedura per la produzione.</span><span class="sxs-lookup"><span data-stu-id="1677a-194">Follow the same procedure for production as you did for staging.</span></span>

<a id="specificfiles"></a>

## <a name="reviewing-changes-and-deploying-specific-files"></a><span data-ttu-id="1677a-195">Verifica delle modifiche e la distribuzione di file specifici</span><span class="sxs-lookup"><span data-stu-id="1677a-195">Reviewing Changes and Deploying Specific Files</span></span>

<span data-ttu-id="1677a-196">Visual Studio 2012 offre inoltre la possibilità di distribuire i singoli file.</span><span class="sxs-lookup"><span data-stu-id="1677a-196">Visual Studio 2012 also gives you the ability to deploy individual files.</span></span> <span data-ttu-id="1677a-197">Per un file selezionato è possibile visualizzare le differenze tra la versione locale e la versione distribuita, distribuire il file nell'ambiente di destinazione o copiare il file dall'ambiente di destinazione per il progetto locale.</span><span class="sxs-lookup"><span data-stu-id="1677a-197">For a selected file you can view differences between the local version and the deployed version, deploy the file to the destination environment, or copy the file from the destination environment to the local project.</span></span> <span data-ttu-id="1677a-198">In questa sezione dell'esercitazione informazioni su come usare queste funzionalità.</span><span class="sxs-lookup"><span data-stu-id="1677a-198">In this section of the tutorial you see how to use these features.</span></span>

### <a name="make-a-change-to-deploy"></a><span data-ttu-id="1677a-199">Apportare una modifica per la distribuzione</span><span class="sxs-lookup"><span data-stu-id="1677a-199">Make a change to deploy</span></span>

1. <span data-ttu-id="1677a-200">Aprire *Content/Site.css*e trovare il blocco per il `body` tag.</span><span class="sxs-lookup"><span data-stu-id="1677a-200">Open *Content/Site.css*, and find the block for the `body` tag.</span></span>
2. <span data-ttu-id="1677a-201">Modificare il valore per `background-color` dal `#fff` a `darkblue`.</span><span class="sxs-lookup"><span data-stu-id="1677a-201">Change the value for `background-color` from `#fff` to `darkblue`.</span></span>

    [!code-css[Main](deploying-a-code-update/samples/sample4.css?highlight=2)]

### <a name="view-the-change-in-the-publish-preview-window"></a><span data-ttu-id="1677a-202">Visualizzare la modifica nella finestra di anteprima pubblica</span><span class="sxs-lookup"><span data-stu-id="1677a-202">View the change in the Publish Preview window</span></span>

<span data-ttu-id="1677a-203">Quando si usa la **pubblica sul Web** procedura guidata per pubblicare il progetto, è possibile visualizzare alle modifiche che si desidera vengano pubblicati dal doppio clic sul file le **anteprima** finestra.</span><span class="sxs-lookup"><span data-stu-id="1677a-203">When you use the **Publish Web** wizard to publish the project, you can see what changes are going to be published by double-clicking the file in the **Preview** window.</span></span>

1. <span data-ttu-id="1677a-204">Fare clic sul progetto ContosoUniversity e fare clic su **pubblica**.</span><span class="sxs-lookup"><span data-stu-id="1677a-204">Right-click the ContosoUniversity project and click **Publish**.</span></span>
2. <span data-ttu-id="1677a-205">Dal **profilo** elenco a discesa, seleziona la **Test** profilo di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="1677a-205">From the **Profile** drop-down list, select the **Test** publish profile.</span></span>
3. <span data-ttu-id="1677a-206">Fare clic su **Preview**, quindi fare clic su **Avvia anteprima**.</span><span class="sxs-lookup"><span data-stu-id="1677a-206">Click **Preview**, and then click **Start Preview**.</span></span>
4. <span data-ttu-id="1677a-207">Nel **Preview** riquadro, fare doppio clic su **CSS**.</span><span class="sxs-lookup"><span data-stu-id="1677a-207">In the **Preview** pane, double-click **Site.css**.</span></span>

    ![Fare doppio clic su CSS](deploying-a-code-update/_static/image9.png)

    <span data-ttu-id="1677a-209">Il **Anteprima modifiche** finestra di dialogo viene visualizzata un'anteprima delle modifiche che verranno distribuiti.</span><span class="sxs-lookup"><span data-stu-id="1677a-209">The **Preview changes** dialog shows a preview of the changes that will be deployed.</span></span>

    ![Anteprima modifiche a CSS](deploying-a-code-update/_static/image10.png)

    <span data-ttu-id="1677a-211">Se si fa doppio clic il *Web. config* file, il **Anteprima modifiche** finestra di dialogo Mostra l'effetto della build le trasformazioni di configurazione e le trasformazioni di profilo di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="1677a-211">If you double-click the *Web.config* file, the **Preview changes** dialog shows the effect of your build configuration transformations and publish profile transformations.</span></span> <span data-ttu-id="1677a-212">A questo punto non è ancora alcuna operazione che comporterebbe la *Web. config* file sul server per modificare, in modo che si desidera non vedere alcuna modifica.</span><span class="sxs-lookup"><span data-stu-id="1677a-212">At this point you have not done anything that would cause the *Web.config* file on the server to change, so you expect to see no changes.</span></span> <span data-ttu-id="1677a-213">Tuttavia, il **Anteprima modifiche** finestra Mostra in modo non corretto due modifiche.</span><span class="sxs-lookup"><span data-stu-id="1677a-213">However, the **Preview changes** window incorrectly shows two changes.</span></span> <span data-ttu-id="1677a-214">Sembra che due elementi XML verranno rimosso.</span><span class="sxs-lookup"><span data-stu-id="1677a-214">It looks like two XML elements will be removed.</span></span> <span data-ttu-id="1677a-215">Questi elementi vengono aggiunti dal processo di pubblicazione quando si seleziona **Esegui migrazioni Code First all'avvio dell'applicazione** per una classe del contesto Code First.</span><span class="sxs-lookup"><span data-stu-id="1677a-215">These elements are added by the publish process when you select **Execute Code First Migrations on application start** for a Code First context class.</span></span> <span data-ttu-id="1677a-216">Il confronto viene eseguito prima che il processo di pubblicazione aggiunge tali elementi, in modo che sembra proprio quello che vengono rimossi anche se essi non verranno rimossi.</span><span class="sxs-lookup"><span data-stu-id="1677a-216">The comparison is done before the publish process adds those elements, so it looks like they are being removed although they will not be removed.</span></span> <span data-ttu-id="1677a-217">Questo errore verrà corretto in una versione futura.</span><span class="sxs-lookup"><span data-stu-id="1677a-217">This error will be corrected in a future release.</span></span>
5. <span data-ttu-id="1677a-218">Fare clic su **Chiudi**.</span><span class="sxs-lookup"><span data-stu-id="1677a-218">Click **Close**.</span></span>
6. <span data-ttu-id="1677a-219">Fare clic su **Pubblica**.</span><span class="sxs-lookup"><span data-stu-id="1677a-219">Click **Publish**.</span></span>
7. <span data-ttu-id="1677a-220">Quando si apre il browser alla home page del sito di Test, premere CTRL+F5 per provocare un aggiornamento hardware per visualizzare l'effetto della modifica CSS.</span><span class="sxs-lookup"><span data-stu-id="1677a-220">When the browser opens to the home page of the Test site, press CTRL+F5 to cause a hard refresh in order to see the effect of the CSS change.</span></span>

    ![Effetto della modifica CSS](deploying-a-code-update/_static/image11.png)
8. <span data-ttu-id="1677a-222">Chiudere il browser.</span><span class="sxs-lookup"><span data-stu-id="1677a-222">Close the browser.</span></span>

### <a name="publish-specific-files-from-solution-explorer"></a><span data-ttu-id="1677a-223">Pubblicare i file specifici da Esplora soluzioni</span><span class="sxs-lookup"><span data-stu-id="1677a-223">Publish specific files from Solution Explorer</span></span>

<span data-ttu-id="1677a-224">Si supponga che non desidera che lo sfondo blu e ripristinare il colore originale.</span><span class="sxs-lookup"><span data-stu-id="1677a-224">Suppose you don't like the blue background and want to revert to the original color.</span></span> <span data-ttu-id="1677a-225">In questa sezione è possibile ripristinare le impostazioni originali mediante la pubblicazione di un file specifico direttamente dal **Esplora soluzioni**.</span><span class="sxs-lookup"><span data-stu-id="1677a-225">In this section you'll restore the original settings by publishing a specific file directly from **Solution Explorer**.</span></span>

1. <span data-ttu-id="1677a-226">Aprire *Content/Site.css* e il ripristino il `background-color` impostando su `#fff`.</span><span class="sxs-lookup"><span data-stu-id="1677a-226">Open *Content/Site.css* and restore the `background-color` setting to `#fff`.</span></span>
2. <span data-ttu-id="1677a-227">Nelle **Esplora soluzioni**, fare doppio clic il *Content/Site.css* file.</span><span class="sxs-lookup"><span data-stu-id="1677a-227">In **Solution Explorer**, right-click the *Content/Site.css* file.</span></span>

    <span data-ttu-id="1677a-228">Menu di scelta rapida mostra che tre opzioni di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="1677a-228">The context menu shows three publish options.</span></span>

    ![Pubblica opzioni da Esplora soluzioni](deploying-a-code-update/_static/image12.png)
3. <span data-ttu-id="1677a-230">Fare clic su **l'anteprima delle modifiche a CSS**.</span><span class="sxs-lookup"><span data-stu-id="1677a-230">Click **Preview changes to Site.css**.</span></span>

    <span data-ttu-id="1677a-231">Verrà visualizzata una finestra per visualizzare le differenze tra il file locale e la versione nell'ambiente di destinazione.</span><span class="sxs-lookup"><span data-stu-id="1677a-231">A window opens to show the differences between the local file and the version of it in the destination environment.</span></span>

    ![Diff-contenuto/Site. CSS](deploying-a-code-update/_static/image13.png)
4. <span data-ttu-id="1677a-233">Nelle **Esplora soluzioni**, fare doppio clic su **Site. CSS** nuovamente e fare clic su **pubblicare CSS**.</span><span class="sxs-lookup"><span data-stu-id="1677a-233">In **Solution Explorer**, right-click **Site.css** again and click **Publish Site.css**.</span></span>

    <span data-ttu-id="1677a-234">Il **attività pubblicazione sul Web** finestra mostra che il file sia stato pubblicato.</span><span class="sxs-lookup"><span data-stu-id="1677a-234">The **Web Publish Activity** window shows that the file has been published.</span></span>

    ![Finestra attività pubblicazione sul Web](deploying-a-code-update/_static/image14.png)
5. <span data-ttu-id="1677a-236">Aprire un browser per il `http://localhost/contosouniversity` URL e quindi premere CTRL+F5 per provocare un disco rigido aggiornare per visualizzare l'effetto del foglio di stile CSS di modifica.</span><span class="sxs-lookup"><span data-stu-id="1677a-236">Open a browser to the `http://localhost/contosouniversity` URL, and then press CTRL+F5 to cause a hard refresh in order to see the effect of the CSS change.</span></span>

    ![Home page con CSS normale](deploying-a-code-update/_static/image15.png)
6. <span data-ttu-id="1677a-238">Chiudere il browser.</span><span class="sxs-lookup"><span data-stu-id="1677a-238">Close the browser.</span></span>

## <a name="summary"></a><span data-ttu-id="1677a-239">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="1677a-239">Summary</span></span>

<span data-ttu-id="1677a-240">Abbiamo visto diversi modi per distribuire un aggiornamento dell'applicazione che non comporta una modifica al database e si è appreso come visualizzare in anteprima le modifiche per verificare che cosa verrà aggiornata è quello previsto.</span><span class="sxs-lookup"><span data-stu-id="1677a-240">You've now seen several ways to deploy an application update that does not involve a database change, and you've seen how to preview the changes to verify that what will be updated is what you expect.</span></span> <span data-ttu-id="1677a-241">La pagina instructors (insegnanti) ora ha un **corsi tenuti** sezione.</span><span class="sxs-lookup"><span data-stu-id="1677a-241">The Instructors page now has a **Courses Taught** section.</span></span>

![Pagina instructors (insegnanti) con i corsi tenuti](deploying-a-code-update/_static/image16.png)

<span data-ttu-id="1677a-243">L'esercitazione successiva illustra come distribuire una modifica al database: si aggiungerà un campo Data di nascita per il database e per la pagina instructors (insegnanti).</span><span class="sxs-lookup"><span data-stu-id="1677a-243">The next tutorial shows you how to deploy a database change: you'll add a birthdate field to the database and to the Instructors page.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="1677a-244">[Precedente](deploying-to-production.md)
> [Successivo](deploying-a-database-update.md)</span><span class="sxs-lookup"><span data-stu-id="1677a-244">[Previous](deploying-to-production.md)
[Next](deploying-a-database-update.md)</span></span>
