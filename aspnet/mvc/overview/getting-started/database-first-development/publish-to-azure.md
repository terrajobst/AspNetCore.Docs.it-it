---
uid: mvc/overview/getting-started/database-first-development/publish-to-azure
title: Sito MVC Database prima di pubblicare in Azure | Documenti Microsoft
author: tfitzmac
description: Usa lo Scaffolding di ASP.NET MVC ed Entity Framework, è possibile creare un'applicazione web che fornisce un'interfaccia a un database esistente. Questa esercitazione seri...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/22/2014
ms.topic: article
ms.assetid: 7131f1c1-cef3-4396-ab44-ed4519676546
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/publish-to-azure
msc.type: authoredcontent
ms.openlocfilehash: 839bbceba6f0e098303facd40dbb1496bd449ba3
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869959"
---
<a name="publish-mvc-database-first-site-to-azure"></a><span data-ttu-id="36423-104">Sito MVC Database prima di pubblicare in Azure</span><span class="sxs-lookup"><span data-stu-id="36423-104">Publish MVC Database First site to Azure</span></span>
====================
<span data-ttu-id="36423-105">da [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="36423-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="36423-106">Usa lo Scaffolding di ASP.NET MVC ed Entity Framework, è possibile creare un'applicazione web che fornisce un'interfaccia a un database esistente.</span><span class="sxs-lookup"><span data-stu-id="36423-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="36423-107">Questa serie di esercitazioni viene illustrato come automaticamente generare il codice che consente agli utenti di visualizzare, modificare, creare ed eliminare dati che si trovano in una tabella di database.</span><span class="sxs-lookup"><span data-stu-id="36423-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="36423-108">Il codice generato corrisponde alle colonne nella tabella di database.</span><span class="sxs-lookup"><span data-stu-id="36423-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="36423-109">Questa parte della serie è incentrata sulla pubblicazione di app web e il database in Azure.</span><span class="sxs-lookup"><span data-stu-id="36423-109">This part of the series focuses on publishing the web app and database to Azure.</span></span> <span data-ttu-id="36423-110">È possibile leggere questo argomento per ulteriori informazioni sulla pubblicazione di un'applicazione web e il database, ma in realtà eseguire i passaggi che deve iniziare all'inizio dell'esercitazione.</span><span class="sxs-lookup"><span data-stu-id="36423-110">You can read this topic to learn about publishing a web app and database, but to actually perform the steps you must start at the beginning of the tutorial.</span></span> <span data-ttu-id="36423-111">Vedere [Introduzione](setting-up-database.md).</span><span class="sxs-lookup"><span data-stu-id="36423-111">See [Getting Started](setting-up-database.md).</span></span>


## <a name="deploy-your-web-app-on-azure"></a><span data-ttu-id="36423-112">Distribuire l'app web in Azure</span><span class="sxs-lookup"><span data-stu-id="36423-112">Deploy your web app on Azure</span></span>

<span data-ttu-id="36423-113">È necessario un account Azure per completare questa esercitazione:</span><span class="sxs-lookup"><span data-stu-id="36423-113">You need an Azure account to complete this tutorial:</span></span>

- <span data-ttu-id="36423-114">È possibile [aprire un account Azure, gratuitamente](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) -si ottiene crediti è possibile utilizzare per provare i servizi di Azure a pagamento e anche dopo l'uso massimo è possibile mantenere l'account e utilizzare senza servizi di Azure.</span><span class="sxs-lookup"><span data-stu-id="36423-114">You can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
- <span data-ttu-id="36423-115">È possibile [attivare i benefici per sottoscrittori MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) -Your sottoscrizione MSDN offre crediti ogni mese in cui è possibile utilizzare per i servizi di Azure a pagamento.</span><span class="sxs-lookup"><span data-stu-id="36423-115">You can [activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

<span data-ttu-id="36423-116">Per pubblicare l'app web, fare clic sul progetto e selezionare **pubblica**.</span><span class="sxs-lookup"><span data-stu-id="36423-116">To publish your web app, right-click the project and select **Publish**.</span></span>

![pubblicazione del sito](publish-to-azure/_static/image1.png)

<span data-ttu-id="36423-118">Selezionare i siti Web di Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="36423-118">Select Microsoft Azure Websites.</span></span>

![Selezionare Azure](publish-to-azure/_static/image2.png)

<span data-ttu-id="36423-120">Se non è connesso a Azure, fornire le credenziali dell'account Azure.</span><span class="sxs-lookup"><span data-stu-id="36423-120">If you are not signed in to Azure, provide your Azure account credentials.</span></span> <span data-ttu-id="36423-121">Quindi, scegliere Nuovo per creare una nuova app web.</span><span class="sxs-lookup"><span data-stu-id="36423-121">Then, select New to create a new web app.</span></span>

![nuovo sito](publish-to-azure/_static/image3.png)

<span data-ttu-id="36423-123">Creare un nome univoco per l'app web.</span><span class="sxs-lookup"><span data-stu-id="36423-123">Create a unique name for your web app.</span></span> <span data-ttu-id="36423-124">Si conosceranno che il nome è univoco se viene visualizzato un segno di spunta verde a destra del campo name.</span><span class="sxs-lookup"><span data-stu-id="36423-124">You will know the name is unique if you see a green check mark to the right of the name field.</span></span> <span data-ttu-id="36423-125">Selezionare un'area per l'app web.</span><span class="sxs-lookup"><span data-stu-id="36423-125">Select a region for your web app.</span></span> <span data-ttu-id="36423-126">Selezionare **Crea nuovo server** per il database e fornire un nome utente e una password per questo nuovo server di database.</span><span class="sxs-lookup"><span data-stu-id="36423-126">Select **Create new server** for the database, and provide a user name and password for this new database server.</span></span> <span data-ttu-id="36423-127">Al termine, fare clic su **crea**.</span><span class="sxs-lookup"><span data-stu-id="36423-127">When finished, click **Create**.</span></span>

![Crea sito](publish-to-azure/_static/image4.png)

<span data-ttu-id="36423-129">I valori di connessione sono ora impostati.</span><span class="sxs-lookup"><span data-stu-id="36423-129">Your connection values are now all set.</span></span> <span data-ttu-id="36423-130">È possibile lasciare invariati i questi valori.</span><span class="sxs-lookup"><span data-stu-id="36423-130">You can leave these values unchanged.</span></span>

![connection](publish-to-azure/_static/image5.png)

<span data-ttu-id="36423-132">Scegliere **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="36423-132">Click **Next**.</span></span>

<span data-ttu-id="36423-133">Per le impostazioni, si noti che due connessioni di database vengono specificati - ApplicationDBContext e ContosoUniversityDataEntities.</span><span class="sxs-lookup"><span data-stu-id="36423-133">For Settings, notice that two database connections are specified - ApplicationDBContext and ContosoUniversityDataEntities.</span></span> <span data-ttu-id="36423-134">ApplicationDBContext è la connessione per le tabelle di account utente.</span><span class="sxs-lookup"><span data-stu-id="36423-134">ApplicationDBContext is the connection for user account tables.</span></span> <span data-ttu-id="36423-135">Questi valori vengono visualizzate solo le stringhe di connessione per i database.</span><span class="sxs-lookup"><span data-stu-id="36423-135">These values only show the connection strings for the databases.</span></span> <span data-ttu-id="36423-136">Ciò significa che questi database verranno pubblicati quando si pubblica il sito.</span><span class="sxs-lookup"><span data-stu-id="36423-136">It does not mean that these databases will be published when you publish your site.</span></span> <span data-ttu-id="36423-137">Dopo aver completato la pubblicazione dell'app web verrà pubblicato il progetto di database.</span><span class="sxs-lookup"><span data-stu-id="36423-137">You will publish your database project after you have finished publishing the web app.</span></span>

![](publish-to-azure/_static/image6.png)

<span data-ttu-id="36423-138">I puntini di sospensione (...) accanto alla connessione al database Mostra i dettagli della stringa di connessione.</span><span class="sxs-lookup"><span data-stu-id="36423-138">The ellipsis (...) next to the database connection shows you the details of the connection string.</span></span> <span data-ttu-id="36423-139">Fare clic sui puntini di sospensione accanto a ContosoUniversityDataEntities.</span><span class="sxs-lookup"><span data-stu-id="36423-139">Click the ellipsis next to ContosoUniversityDataEntities.</span></span>

![](publish-to-azure/_static/image7.png)

<span data-ttu-id="36423-140">Annotare il nome del server di database e del database.</span><span class="sxs-lookup"><span data-stu-id="36423-140">Note the name of the database server and the database.</span></span> <span data-ttu-id="36423-141">Il nome del server viene generato casualmente.</span><span class="sxs-lookup"><span data-stu-id="36423-141">The server name is randomly generated.</span></span> <span data-ttu-id="36423-142">Il nome del database è semplice il nome del sito con  **\_db** aggiunto.</span><span class="sxs-lookup"><span data-stu-id="36423-142">The database name is simple the name of your site with **\_db** appended.</span></span> <span data-ttu-id="36423-143">È necessario che entrambi i nomi in un secondo momento durante la pubblicazione del database.</span><span class="sxs-lookup"><span data-stu-id="36423-143">You will need both names later when you publish your database.</span></span>

<span data-ttu-id="36423-144">Fare clic su **OK** per chiudere la finestra di stringa di connessione di database.</span><span class="sxs-lookup"><span data-stu-id="36423-144">Click **OK** to close the database connection string window.</span></span>

<span data-ttu-id="36423-145">Nella finestra di pubblica sul Web, fare clic su **Avanti** per visualizzare l'anteprima.</span><span class="sxs-lookup"><span data-stu-id="36423-145">In the Publish Web window, click **Next** to see the preview.</span></span>

![](publish-to-azure/_static/image8.png)

<span data-ttu-id="36423-146">È possibile scegliere di avviare l'anteprima per visualizzare un elenco di file da pubblicare.</span><span class="sxs-lookup"><span data-stu-id="36423-146">You can click Start Preview to see a list of the files to publish.</span></span> <span data-ttu-id="36423-147">Poiché questa è la prima volta che questo sito è pubblicato, l'elenco è ogni file rilevanti nel progetto.</span><span class="sxs-lookup"><span data-stu-id="36423-147">Since this is the first time you have published this site, the list is every relevant file in the project.</span></span>

<span data-ttu-id="36423-148">Fare clic su **Pubblica**.</span><span class="sxs-lookup"><span data-stu-id="36423-148">Click **Publish**.</span></span>

<span data-ttu-id="36423-149">Il riquadro di Output verrà visualizzato il risultato della pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="36423-149">The Output pane will display the result of your publication.</span></span>

![](publish-to-azure/_static/image9.png)

<span data-ttu-id="36423-150">Dopo la pubblicazione, il sito è immedialely avviata in un web browser.</span><span class="sxs-lookup"><span data-stu-id="36423-150">After publication, the site is immedialely launched in a web browser.</span></span> <span data-ttu-id="36423-151">Il sito è stato distribuito ed è possibile registrare un nuovo utente per il sito. Tuttavia, le tabelle nel progetto ContosoUniversityData non sono ancora state pubblicate.</span><span class="sxs-lookup"><span data-stu-id="36423-151">Your site has been deployed and you can register a new user to the site; however, your tables in the ContosoUniversityData project have not yet been published.</span></span> <span data-ttu-id="36423-152">Se si fa clic sull'elenco di studenti collegamento si riceverà un errore.</span><span class="sxs-lookup"><span data-stu-id="36423-152">If you click on the List of students link you will receive an error.</span></span>

## <a name="publish-database-to-sql-azure"></a><span data-ttu-id="36423-153">Pubblicare un database a SQL Azure</span><span class="sxs-lookup"><span data-stu-id="36423-153">Publish database to SQL Azure</span></span>

<span data-ttu-id="36423-154">Prima di pubblicare il database, è necessario assicurarsi che il computer locale può connettersi al server di database.</span><span class="sxs-lookup"><span data-stu-id="36423-154">Before publishing your database, you must make sure your local computer can connect to the database server.</span></span> <span data-ttu-id="36423-155">Il firewall per il server di database limita le macchine possono connettersi al database.</span><span class="sxs-lookup"><span data-stu-id="36423-155">The firewall for your database server restricts which machines can connect to the database.</span></span> <span data-ttu-id="36423-156">È necessario aggiungere l'indirizzo IP del computer agli indirizzi IP consentiti per il firewall.</span><span class="sxs-lookup"><span data-stu-id="36423-156">You need to add the IP address of your computer to the allowed IP addresses for the firewall.</span></span>

<span data-ttu-id="36423-157">Accedere al proprio account Azure tramite il portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="36423-157">Login to your Azure account through the Azure portal.</span></span>

<span data-ttu-id="36423-158">Selezionare il nuovo database e selezionare **Gestisci**.</span><span class="sxs-lookup"><span data-stu-id="36423-158">Select your new database and select **Manage**.</span></span>

![Gestire](publish-to-azure/_static/image10.png)

<span data-ttu-id="36423-160">È necessario configurare il server di database per consentire le connessioni dal computer.</span><span class="sxs-lookup"><span data-stu-id="36423-160">You must configure your database server to allow connections from your computer.</span></span> <span data-ttu-id="36423-161">Quando si seleziona gestione, viene chiesto di aggiungere l'indirizzo IP corrente, come se il server di database.</span><span class="sxs-lookup"><span data-stu-id="36423-161">When you select Manage, you are asked to add the current IP address as permitted to the database server.</span></span> <span data-ttu-id="36423-162">Selezionare Sì.</span><span class="sxs-lookup"><span data-stu-id="36423-162">Select Yes.</span></span>

![aggiungere l'indirizzo ip](publish-to-azure/_static/image11.png)

<span data-ttu-id="36423-164">È probabile che l'indirizzo IP che è stato aggiunto nel passaggio precedente non è il solo indirizzo IP che è necessario configurare per le connessioni.</span><span class="sxs-lookup"><span data-stu-id="36423-164">There is a chance that the IP address you added in the previous step is not the only IP address you need to configure for connections.</span></span> <span data-ttu-id="36423-165">È possibile tentare di eseguire l'accesso al database per vedere se le connessioni sono state configurate correttamente.</span><span class="sxs-lookup"><span data-stu-id="36423-165">You can attempt to login to the database to see if the connections have been properly set up.</span></span> <span data-ttu-id="36423-166">Specificare la password creata in precedenza e l'utente.</span><span class="sxs-lookup"><span data-stu-id="36423-166">Provide the user and password you created earlier.</span></span>

![Accesso](publish-to-azure/_static/image12.png)

<span data-ttu-id="36423-168">Se si riceve un messaggio di errore, è necessario aggiungere un altro indirizzo IP.</span><span class="sxs-lookup"><span data-stu-id="36423-168">If you receive an error message, you need to add another IP address.</span></span> <span data-ttu-id="36423-169">Fare clic sul messaggio di errore per visualizzare ulteriori dettagli sull'errore.</span><span class="sxs-lookup"><span data-stu-id="36423-169">Click the error message to see more details about error.</span></span> <span data-ttu-id="36423-170">Nei dettagli verrà visualizzato l'indirizzo IP che si desidera aggiungere.</span><span class="sxs-lookup"><span data-stu-id="36423-170">In the details you will see the IP address that you need to add.</span></span> <span data-ttu-id="36423-171">Si noti l'indirizzo IP.</span><span class="sxs-lookup"><span data-stu-id="36423-171">Note this IP address.</span></span>

![non è consentito](publish-to-azure/_static/image13.png)

<span data-ttu-id="36423-173">Chiudere questa finestra di dialogo di accesso e tornare al portale di Azure.</span><span class="sxs-lookup"><span data-stu-id="36423-173">Close this login window, and return to the Azure portal.</span></span>

<span data-ttu-id="36423-174">Passare al Dashboard per il database.</span><span class="sxs-lookup"><span data-stu-id="36423-174">Navigate to the Dashboard for your database.</span></span> <span data-ttu-id="36423-175">Fare clic su **gestione indirizzi IP consentiti**.</span><span class="sxs-lookup"><span data-stu-id="36423-175">Click **Manage allowed IP addresses**.</span></span>

![gestire gli indirizzi ip](publish-to-azure/_static/image14.png)

<span data-ttu-id="36423-177">A questo punto, è necessario aggiungere l'indirizzo IP dal messaggio di errore.</span><span class="sxs-lookup"><span data-stu-id="36423-177">You must now add the IP address from the error message.</span></span> <span data-ttu-id="36423-178">L'intervallo di indirizzi IP consentiti per includere il messaggio di errore con quello di modificare o aggiungere tale indirizzo IP come una voce separata.</span><span class="sxs-lookup"><span data-stu-id="36423-178">Either change the range of allowed IP addresses to include the one from the error message, or add that IP address as a separate entry.</span></span>

![Aggiungere nuovo indirizzo](publish-to-azure/_static/image15.png)

<span data-ttu-id="36423-180">Salvare le modifiche agli indirizzi IP.</span><span class="sxs-lookup"><span data-stu-id="36423-180">Save the change to allowed IP addresses.</span></span>

<span data-ttu-id="36423-181">Fare clic su Gestisci e provare a effettuare l'accesso al database.</span><span class="sxs-lookup"><span data-stu-id="36423-181">Click Manage, and try logging in again to the database.</span></span> <span data-ttu-id="36423-182">Potrebbe essere necessario attendere alcuni minuti prima che gli indirizzi IP consentiti siano configurati correttamente per il firewall.</span><span class="sxs-lookup"><span data-stu-id="36423-182">You may need to wait a few minutes before the allowed IP addresses are correctly configured for the firewall.</span></span> <span data-ttu-id="36423-183">Quando è possibile registrare correttamente nel database, avere impostato la connessione al database.</span><span class="sxs-lookup"><span data-stu-id="36423-183">When you can successfully log in the database, you have finished setting up your connection to the database.</span></span>

<span data-ttu-id="36423-184">È possibile lasciare aperta questa finestra di gestione perché si verificherà il risultato della distribuzione di database a breve.</span><span class="sxs-lookup"><span data-stu-id="36423-184">You can leave this management window open because you will check the result of your database deployment shortly.</span></span>

<span data-ttu-id="36423-185">Tornare al progetto di database.</span><span class="sxs-lookup"><span data-stu-id="36423-185">Return to your database project.</span></span> <span data-ttu-id="36423-186">Fare clic sul progetto e selezionare **pubblica**.</span><span class="sxs-lookup"><span data-stu-id="36423-186">Right-click the project and select **Publish**.</span></span>

![pubblicare database](publish-to-azure/_static/image16.png)

<span data-ttu-id="36423-188">Nella finestra di Database di pubblicazione, selezionare **modifica**.</span><span class="sxs-lookup"><span data-stu-id="36423-188">In the Publish Database window, select **Edit**.</span></span>

![modifica](publish-to-azure/_static/image17.png)

<span data-ttu-id="36423-190">Specificare il nome del server di database e le credenziali di autenticazione per il server.</span><span class="sxs-lookup"><span data-stu-id="36423-190">Provide the name of the database server and your authentication credentials for the server.</span></span> <span data-ttu-id="36423-191">Dopo aver fornito le credenziali, selezionare il database creato dall'elenco dei database disponibili.</span><span class="sxs-lookup"><span data-stu-id="36423-191">After providing the credentials, select the database you created from the list of available databases.</span></span> <span data-ttu-id="36423-192">Per impostazione predefinita, Visual Studio imposta il nome del campo di database per il nome del progetto che potrebbe non essere lo stesso nome di database che è stato creato.</span><span class="sxs-lookup"><span data-stu-id="36423-192">By default, Visual Studio sets the name of the database field to the name of your project which might not be the same as the database you created.</span></span>

![](publish-to-azure/_static/image18.png)

<span data-ttu-id="36423-193">Fare clic su OK.</span><span class="sxs-lookup"><span data-stu-id="36423-193">Click OK.</span></span>

<span data-ttu-id="36423-194">Probabilmente è possibile salvare questo profilo, quindi è possibile pubblicare gli aggiornamenti in futuro senza dover immettere nuovamente tutte le informazioni di connessione.</span><span class="sxs-lookup"><span data-stu-id="36423-194">You will probably want to save this profile so you can publish updates in the future without re-entering all of the connection information.</span></span> <span data-ttu-id="36423-195">Selezionare **Crea profilo**.</span><span class="sxs-lookup"><span data-stu-id="36423-195">Select **Create Profile**.</span></span>

![Salva profilo](publish-to-azure/_static/image19.png)

<span data-ttu-id="36423-197">Crea un file di progetto denominato **ContosoUniversityData.publish.xml**.</span><span class="sxs-lookup"><span data-stu-id="36423-197">It will create a file in your project named **ContosoUniversityData.publish.xml**.</span></span> <span data-ttu-id="36423-198">Alla successiva che si desidera pubblicare il database in Azure, caricare semplicemente tale profilo.</span><span class="sxs-lookup"><span data-stu-id="36423-198">The next time you want to publish the database to Azure, simply load that profile.</span></span>

<span data-ttu-id="36423-199">A questo punto, fare clic su **pubblica** per creare il database in Azure.</span><span class="sxs-lookup"><span data-stu-id="36423-199">Now, click **Publish** to create the database on Azure.</span></span>

![publish](publish-to-azure/_static/image20.png)

<span data-ttu-id="36423-201">Dopo aver eseguito per un periodo di tempo, vengono visualizzati i risultati di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="36423-201">After running for a while, the publishing results are displayed.</span></span>

![results](publish-to-azure/_static/image21.png)

<span data-ttu-id="36423-203">A questo punto, è possibile tornare al portale di gestione per il database.</span><span class="sxs-lookup"><span data-stu-id="36423-203">Now, you can go back to the management portal for your database.</span></span> <span data-ttu-id="36423-204">Aggiornare la visualizzazione di progettazione e osservare che le tabelle con dati precompilate 3 sono state distribuite.</span><span class="sxs-lookup"><span data-stu-id="36423-204">Refresh the design view, and notice the 3 tables with pre-filled data have been deployed.</span></span>

![nuove tabelle](publish-to-azure/_static/image22.png)

<span data-ttu-id="36423-206">A questo punto si è pronti per testare l'app web che viene distribuito in Azure.</span><span class="sxs-lookup"><span data-stu-id="36423-206">Now you are ready to test the web app that is deployed to Azure.</span></span> <span data-ttu-id="36423-207">Passare all'app web in Azure (ad esempio http://contosositeexample.azurewebsites.net/).</span><span class="sxs-lookup"><span data-stu-id="36423-207">Navigate to the web app on Azure (such as http://contosositeexample.azurewebsites.net/).</span></span> <span data-ttu-id="36423-208">Fare clic sul collegamento per l'elenco di studenti e dovrebbe essere la visualizzazione dell'indice per studenti.</span><span class="sxs-lookup"><span data-stu-id="36423-208">Click the link for List of students and you should see the index view for students.</span></span>

![visualizzazione](publish-to-azure/_static/image23.png)

<span data-ttu-id="36423-210">In alcuni casi, il database e la connessione è necessario un po' di tempo per essere configurato correttamente.</span><span class="sxs-lookup"><span data-stu-id="36423-210">Occasionally, the database and connection need a little time to be properly configured.</span></span> <span data-ttu-id="36423-211">Se si riceve un errore, attendere qualche minuto e riprovare.</span><span class="sxs-lookup"><span data-stu-id="36423-211">If you receive an error, wait a few minutes and then try again.</span></span>

## <a name="conclusion"></a><span data-ttu-id="36423-212">Conclusione</span><span class="sxs-lookup"><span data-stu-id="36423-212">Conclusion</span></span>

<span data-ttu-id="36423-213">Questa serie fornito un semplice esempio di come generare codice da un database esistente che consente agli utenti di modificare, aggiornare, creare ed eliminare dati.</span><span class="sxs-lookup"><span data-stu-id="36423-213">This series provided a simple example of how to generate code from an existing database that enables users to edit, update, create and delete data.</span></span> <span data-ttu-id="36423-214">Consente di creare il progetto ASP.NET MVC 5, Entity Framework e lo Scaffolding di ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="36423-214">It used ASP.NET MVC 5, Entity Framework and ASP.NET Scaffolding to create the project.</span></span>

<span data-ttu-id="36423-215">Per un esempio introduttivo dello sviluppo Code First, vedere [Introduzione a ASP.NET MVC 5](../introduction/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="36423-215">For an introductory example of Code First development, see [Getting Started with ASP.NET MVC 5](../introduction/getting-started.md).</span></span>

<span data-ttu-id="36423-216">Per un esempio più avanzato, vedere [creazione di un modello di dati di Entity Framework per applicazioni ASP.NET MVC 4](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="36423-216">For a more advanced example, see [Creating an Entity Framework Data Model for an ASP.NET MVC 4 App](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span> <span data-ttu-id="36423-217">Si noti che l'API DbContext utilizzabili per lavorare con i dati nel primo Database è identico API utilizzata per l'utilizzo di dati di Code First.</span><span class="sxs-lookup"><span data-stu-id="36423-217">Note that the DbContext API that you use for working with data in Database First is the same as the API you use for working with data in Code First.</span></span> <span data-ttu-id="36423-218">Anche se si prevede di utilizzare Database First, è possibile imparare a gestire scenari più complessi, ad esempio la lettura e aggiornamento dei dati correlati, gestione dei conflitti di concorrenza, e così via da un'esercitazione Code First.</span><span class="sxs-lookup"><span data-stu-id="36423-218">Even if you intend to use Database First, you can learn how to handle more complex scenarios such as reading and updating related data, handling concurrency conflicts, and so forth from a Code First tutorial.</span></span> <span data-ttu-id="36423-219">L'unica differenza è in modalità di creazione di database, classe del contesto e le classi di entità.</span><span class="sxs-lookup"><span data-stu-id="36423-219">The only difference is in how the database, context class, and entity classes are created.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="36423-220">Precedente</span><span class="sxs-lookup"><span data-stu-id="36423-220">Previous</span></span>](enhancing-data-validation.md)
