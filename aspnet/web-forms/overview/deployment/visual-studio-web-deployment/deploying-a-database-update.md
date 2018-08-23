---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
title: 'Distribuzione Web ASP.NET tramite Visual Studio: distribuzione di un aggiornamento di Database | Microsoft Docs'
author: tdykstra
description: Questa serie di esercitazioni illustra come distribuire, pubblicare, ASP.NET per App Web di servizio App di Azure o per un provider di hosting di terze parti, di applicazioni web da utilizza...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: 9cad0833-486a-4474-a7f3-7715542ec4ce
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
msc.type: authoredcontent
ms.openlocfilehash: 5c9b0c71e2e0d35645e975e9adb7086e65bcf4c3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838663"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-a-database-update"></a><span data-ttu-id="7a3dc-103">Distribuzione Web ASP.NET tramite Visual Studio: distribuzione di un aggiornamento del Database</span><span class="sxs-lookup"><span data-stu-id="7a3dc-103">ASP.NET Web Deployment using Visual Studio: Deploying a Database Update</span></span>
====================
<span data-ttu-id="7a3dc-104">da [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="7a3dc-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="7a3dc-105">Download progetto iniziale</span><span class="sxs-lookup"><span data-stu-id="7a3dc-105">Download Starter Project</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> <span data-ttu-id="7a3dc-106">Questa serie di esercitazioni illustra come distribuire, pubblicare, ASP.NET per App Web di servizio App di Azure o per un provider di hosting di terze parti, di applicazioni web usando Visual Studio 2012 o Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="7a3dc-106">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider, by using Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="7a3dc-107">Per informazioni sulla serie, vedere [la prima esercitazione della serie](introduction.md).</span><span class="sxs-lookup"><span data-stu-id="7a3dc-107">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="7a3dc-108">Panoramica</span><span class="sxs-lookup"><span data-stu-id="7a3dc-108">Overview</span></span>

<span data-ttu-id="7a3dc-109">In questa esercitazione, si apportano una modifica al database e le modifiche al codice correlato, testare le modifiche in Visual Studio, quindi distribuire l'aggiornamento agli ambienti di test, staging e produzione.</span><span class="sxs-lookup"><span data-stu-id="7a3dc-109">In this tutorial, you make a database change and related code changes, test the changes in Visual Studio, then deploy the update to the test, staging, and production environments.</span></span>

<span data-ttu-id="7a3dc-110">L'esercitazione illustra innanzitutto la procedura per aggiornare un database gestito da migrazioni Code First e versioni successive viene illustrato come aggiornare un database tramite il provider dbDacFx.</span><span class="sxs-lookup"><span data-stu-id="7a3dc-110">The tutorial first shows how to update a database that is managed by Code First Migrations, and then later it shows how to update a database by using the dbDacFx provider.</span></span>

<span data-ttu-id="7a3dc-111">Promemoria: Se viene visualizzato un messaggio di errore o qualcosa non funziona durante l'esecuzione dell'esercitazione, assicurarsi di controllare la [risoluzione dei problemi pagina](troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="7a3dc-111">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](troubleshooting.md).</span></span>

## <a name="deploy-a-database-update-by-using-code-first-migrations"></a><span data-ttu-id="7a3dc-112">Distribuire un aggiornamento del database usando migrazioni Code First</span><span class="sxs-lookup"><span data-stu-id="7a3dc-112">Deploy a database update by using Code First Migrations</span></span>

<span data-ttu-id="7a3dc-113">In questa sezione si aggiunge una colonna di date di nascita per il `Person` classe di base per il `Student` e `Instructor` entità.</span><span class="sxs-lookup"><span data-stu-id="7a3dc-113">In this section, you add a birth date column to the `Person` base class for the `Student` and `Instructor` entities.</span></span> <span data-ttu-id="7a3dc-114">Quindi necessario aggiornare la pagina che visualizza i dati di instructor in modo da visualizzare la nuova colonna.</span><span class="sxs-lookup"><span data-stu-id="7a3dc-114">Then you update the page that displays instructor data so that it displays the new column.</span></span> <span data-ttu-id="7a3dc-115">Infine, distribuire le modifiche al test, staging e produzione.</span><span class="sxs-lookup"><span data-stu-id="7a3dc-115">Finally, you deploy the changes to test, staging, and production.</span></span>

### <a name="add-a-column-to-a-table-in-the-application-database"></a><span data-ttu-id="7a3dc-116">Aggiungere una colonna a una tabella nel database dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="7a3dc-116">Add a column to a table in the application database</span></span>

1. <span data-ttu-id="7a3dc-117">Nel *ContosoUniversity.DAL* progetto aprire *Person.cs* e aggiungere la proprietà seguente alla fine del `Person` classe (dovrebbero essere presenti due parentesi graffe riportata di seguito):</span><span class="sxs-lookup"><span data-stu-id="7a3dc-117">In the *ContosoUniversity.DAL* project, open *Person.cs* and add the following property at the end of the `Person` class (there should be two closing curly braces following it):</span></span>

    [!code-csharp[Main](deploying-a-database-update/samples/sample1.cs)]

    <span data-ttu-id="7a3dc-118">A questo punto, aggiornare il `Seed` metodo in modo che l'it conferisce un valore per la nuova colonna.</span><span class="sxs-lookup"><span data-stu-id="7a3dc-118">Next, update the `Seed` method so that it provides a value for the new column.</span></span> <span data-ttu-id="7a3dc-119">Aprire *Migrations\Configuration.cs* e sostituire il blocco di codice che inizia `var instructors = new List<Instructor>` con blocco di codice seguente che include informazioni sulla data di nascita:</span><span class="sxs-lookup"><span data-stu-id="7a3dc-119">Open *Migrations\Configuration.cs* and replace the code block that begins `var instructors = new List<Instructor>` with the following code block which includes birth date information:</span></span>

    [!code-csharp[Main](deploying-a-database-update/samples/sample2.cs)]
2. <span data-ttu-id="7a3dc-120">Compilare la soluzione e quindi aprire il **Console di gestione pacchetti** finestra.</span><span class="sxs-lookup"><span data-stu-id="7a3dc-120">Build the solution, and then open the **Package Manager Console** window.</span></span> <span data-ttu-id="7a3dc-121">Assicurarsi che ContosoUniversity.DAL sia ancora selezionato come le **progetto predefinito**.</span><span class="sxs-lookup"><span data-stu-id="7a3dc-121">Make sure that ContosoUniversity.DAL is still selected as the **Default project**.</span></span>
3. <span data-ttu-id="7a3dc-122">Nel **Console di gestione pacchetti** finestra, seleziona **ContosoUniversity.DAL** come il **progetto predefinito**, quindi immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="7a3dc-122">In the **Package Manager Console** window, select **ContosoUniversity.DAL** as the **Default project**, and then enter the following command:</span></span>

    [!code-powershell[Main](deploying-a-database-update/samples/sample3.ps1)]

    <span data-ttu-id="7a3dc-123">Al termine di questo comando, Visual Studio apre il file di classe che definisce la nuova `DbMIgration` (classe) e nel `Up` metodo è possibile visualizzare il codice che crea la nuova colonna.</span><span class="sxs-lookup"><span data-stu-id="7a3dc-123">When this command finishes, Visual Studio opens the class file that defines the new `DbMIgration` class, and in the `Up` method you can see the code that creates the new column.</span></span> <span data-ttu-id="7a3dc-124">Il `Up` metodo consente di creare la colonna quando si implementa la modifica e il `Down` metodo elimina la colonna quando si esegue il rollback della modifica.</span><span class="sxs-lookup"><span data-stu-id="7a3dc-124">The `Up` method creates the column when you are implementing the change, and the `Down` method deletes the column when you are rolling back the change.</span></span>

    ![AddBirthDate_migration_code](deploying-a-database-update/_static/image1.png)
4. <span data-ttu-id="7a3dc-126">Compilare la soluzione e quindi immettere il comando seguente nel **Console di gestione pacchetti** finestra (assicurarsi che il progetto ContosoUniversity.DAL sia ancora selezionato):</span><span class="sxs-lookup"><span data-stu-id="7a3dc-126">Build the solution, and then enter the following command in the **Package Manager Console** window (make sure the ContosoUniversity.DAL project is still selected):</span></span>

    [!code-powershell[Main](deploying-a-database-update/samples/sample4.ps1)]

    <span data-ttu-id="7a3dc-127">Entity Framework esegue il `Up` metodo e quindi esegue il `Seed` (metodo).</span><span class="sxs-lookup"><span data-stu-id="7a3dc-127">The Entity Framework runs the `Up` method and then runs the `Seed` method.</span></span>

### <a name="display-the-new-column-in-the-instructors-page"></a><span data-ttu-id="7a3dc-128">Visualizzare la nuova colonna nella pagina instructors (insegnanti)</span><span class="sxs-lookup"><span data-stu-id="7a3dc-128">Display the new column in the Instructors page</span></span>

1. <span data-ttu-id="7a3dc-129">Nel progetto ContosoUniversity, aprire *Instructors.aspx* e aggiungere un nuovo modello di campo per visualizzare la data di nascita.</span><span class="sxs-lookup"><span data-stu-id="7a3dc-129">In the ContosoUniversity project, open *Instructors.aspx* and add a new template field to display the birth date.</span></span> <span data-ttu-id="7a3dc-130">Aggiungerlo tra quelle per l'assegnazione di office e data di assunzione:</span><span class="sxs-lookup"><span data-stu-id="7a3dc-130">Add it between the ones for hire date and office assignment:</span></span>

    [!code-aspx[Main](deploying-a-database-update/samples/sample5.aspx?highlight=9-17)]

    <span data-ttu-id="7a3dc-131">(Se rientro del codice viene sincronizzato, è possibile premere CTRL-K e CTRL + D per riformattare automaticamente il file.)</span><span class="sxs-lookup"><span data-stu-id="7a3dc-131">(If code indentation gets out of sync, you can press CTRL-K and then CTRL-D to automatically reformat the file.)</span></span>
2. <span data-ttu-id="7a3dc-132">Eseguire l'applicazione, quindi scegliere il **instructors (insegnanti)** collegamento.</span><span class="sxs-lookup"><span data-stu-id="7a3dc-132">Run the application and click the **Instructors** link.</span></span>

    <span data-ttu-id="7a3dc-133">Quando la pagina viene caricata, si noterà che che contiene il nuovo campo di data di nascita.</span><span class="sxs-lookup"><span data-stu-id="7a3dc-133">When the page loads, you see that it has the new birth date field.</span></span>

    ![Pagina instructors (insegnanti) con data di nascita](deploying-a-database-update/_static/image2.png)
3. <span data-ttu-id="7a3dc-135">Chiudere il browser.</span><span class="sxs-lookup"><span data-stu-id="7a3dc-135">Close the browser.</span></span>

### <a name="deploy-the-database-update"></a><span data-ttu-id="7a3dc-136">Distribuire l'aggiornamento del database</span><span class="sxs-lookup"><span data-stu-id="7a3dc-136">Deploy the database update</span></span>

1. <span data-ttu-id="7a3dc-137">Nelle **Esplora soluzioni** selezionare il progetto ContosoUniversity.</span><span class="sxs-lookup"><span data-stu-id="7a3dc-137">In **Solution Explorer** select the ContosoUniversity project.</span></span>
2. <span data-ttu-id="7a3dc-138">Nel **Web-pubblicazione con un clic** sulla barra degli strumenti, fare clic sui **Test** profilo di pubblicazione e quindi fare clic su **pubblica sul Web**.</span><span class="sxs-lookup"><span data-stu-id="7a3dc-138">In the **Web One Click Publish** toolbar, click the **Test** publish profile, and then click **Publish Web**.</span></span> <span data-ttu-id="7a3dc-139">(Se la barra degli strumenti è disabilitata, selezionare il progetto ContosoUniversity **Esplora soluzioni**.)</span><span class="sxs-lookup"><span data-stu-id="7a3dc-139">(If the toolbar is disabled, select the ContosoUniversity project in **Solution Explorer**.)</span></span>

    <span data-ttu-id="7a3dc-140">Visual Studio distribuisce l'applicazione aggiornata e nel browser verrà aperta la home page.</span><span class="sxs-lookup"><span data-stu-id="7a3dc-140">Visual Studio deploys the updated application, and the browser opens to the home page.</span></span>
3. <span data-ttu-id="7a3dc-141">Eseguire la **instructors (insegnanti)** pagina per verificare che l'aggiornamento è stato distribuito correttamente.</span><span class="sxs-lookup"><span data-stu-id="7a3dc-141">Run the **Instructors** page to verify that the update was successfully deployed.</span></span>

    <span data-ttu-id="7a3dc-142">Quando l'applicazione prova ad accedere al database per questa pagina, Code First Aggiorna lo schema del database ed esegue il `Seed` (metodo).</span><span class="sxs-lookup"><span data-stu-id="7a3dc-142">When the application tries to access the database for this page, Code First updates the database schema and runs the `Seed` method.</span></span> <span data-ttu-id="7a3dc-143">Quando viene visualizzata la pagina, noterete che quella prevista **data di nascita** colonna delle date in esso.</span><span class="sxs-lookup"><span data-stu-id="7a3dc-143">When the page displays, you see the expected **Birth Date** column with dates in it.</span></span>
4. <span data-ttu-id="7a3dc-144">Nel **Web-pubblicazione con un clic** sulla barra degli strumenti, fare clic sui **Staging** profilo di pubblicazione e quindi fare clic su **pubblica sul Web**.</span><span class="sxs-lookup"><span data-stu-id="7a3dc-144">In the **Web One Click Publish** toolbar, click the **Staging** publish profile, and then click **Publish Web**.</span></span>
5. <span data-ttu-id="7a3dc-145">Eseguire la **instructors (insegnanti)** pagina in gestione temporanea per verificare che l'aggiornamento è stato distribuito correttamente.</span><span class="sxs-lookup"><span data-stu-id="7a3dc-145">Run the **Instructors** page in staging to verify that the update was successfully deployed.</span></span>
6. <span data-ttu-id="7a3dc-146">Nel **Web-pubblicazione con un clic** sulla barra degli strumenti, fare clic sui **produzione** profilo di pubblicazione e quindi fare clic su **pubblica sul Web**.</span><span class="sxs-lookup"><span data-stu-id="7a3dc-146">In the **Web One Click Publish** toolbar, click the **Production** publish profile, and then click **Publish Web**.</span></span>
7. <span data-ttu-id="7a3dc-147">Eseguire la **instructors (insegnanti)** pagina nell'ambiente di produzione per verificare che l'aggiornamento è stato distribuito correttamente.</span><span class="sxs-lookup"><span data-stu-id="7a3dc-147">Run the **Instructors** page in production to verify that the update was successfully deployed.</span></span>

    <span data-ttu-id="7a3dc-148">Per un aggiornamento dell'applicazione di produzione reali che include una modifica al database anche in genere si procederà quindi l'applicazione in modalità offline durante la distribuzione usando *app\_offline.htm*, come illustrato nell'esercitazione precedente.</span><span class="sxs-lookup"><span data-stu-id="7a3dc-148">For a real production application update that includes a database change you would also typically take the application offline during deployment by using *app\_offline.htm*, as you saw in the previous tutorial.</span></span>

## <a name="deploy-a-database-update-by-using-the-dbdacfx-provider"></a><span data-ttu-id="7a3dc-149">Distribuire un aggiornamento del database tramite il provider dbDacFx</span><span class="sxs-lookup"><span data-stu-id="7a3dc-149">Deploy a database update by using the dbDacFx provider</span></span>

<span data-ttu-id="7a3dc-150">In questa sezione aggiunge un *commenti* colonna il *utente* tabella nel database delle appartenenze e creare una pagina che consente di visualizzare e modificare i commenti per ogni utente.</span><span class="sxs-lookup"><span data-stu-id="7a3dc-150">In this section, you add a *Comments* column to the *User* table in the membership database and create a page that lets you display and edit comments for each user.</span></span> <span data-ttu-id="7a3dc-151">Quindi distribuire le modifiche al test, staging e produzione.</span><span class="sxs-lookup"><span data-stu-id="7a3dc-151">Then you deploy the changes to test, staging, and production.</span></span>

### <a name="add-a-column-to-a-table-in-the-membership-database"></a><span data-ttu-id="7a3dc-152">Aggiungere una colonna a una tabella nel database delle appartenenze</span><span class="sxs-lookup"><span data-stu-id="7a3dc-152">Add a column to a table in the membership database</span></span>

1. <span data-ttu-id="7a3dc-153">In Visual Studio, aprire **Esplora oggetti di SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="7a3dc-153">In Visual Studio, open **SQL Server Object Explorer**.</span></span>
2. <span data-ttu-id="7a3dc-154">Espandere **(localdb) \v11.0**, espandere **database**, espandere **aspnet-ContosoUniversity** (non **aspnet-ContosoUniversity-produzione**) e infine **tabelle**.</span><span class="sxs-lookup"><span data-stu-id="7a3dc-154">Expand **(localdb)\v11.0**, expand **Databases**, expand **aspnet-ContosoUniversity** (not **aspnet-ContosoUniversity-Prod**) and then expand **Tables**.</span></span>

    <span data-ttu-id="7a3dc-155">Se non viene visualizzata **\v11.0 (localdb)** sotto il **SQL Server** nodo, fare doppio clic il **SQL Server** nodo e fare clic su **Aggiungi SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="7a3dc-155">If you don't see **(localdb)\v11.0** under the **SQL Server** node, right-click the **SQL Server** node and click **Add SQL Server**.</span></span> <span data-ttu-id="7a3dc-156">Nel **Connetti al Server** finestra di dialogo immettere *\v11.0 (localdb)* come il **nome del Server**e quindi fare clic su **Connect**.</span><span class="sxs-lookup"><span data-stu-id="7a3dc-156">In the **Connect to Server** dialog box enter *(localdb)\v11.0* as the **Server name**, and then click **Connect**.</span></span>

    <span data-ttu-id="7a3dc-157">Se non viene visualizzato **aspnet-ContosoUniversity**, eseguire il progetto e accedere con il *admin* credenziali (password viene *devpwd*) e quindi aggiornare il  **Esplora oggetti di SQL Server** finestra.</span><span class="sxs-lookup"><span data-stu-id="7a3dc-157">If you don't see **aspnet-ContosoUniversity**, run the project and log in using the *admin* credentials (password is *devpwd*), and then refresh the **SQL Server Object Explorer** window.</span></span>
3. <span data-ttu-id="7a3dc-158">Fare doppio clic il **gli utenti** tabella e quindi fare clic su **Progettazione viste**.</span><span class="sxs-lookup"><span data-stu-id="7a3dc-158">Right-click the **Users** table, and then click **View Designer**.</span></span>

    ![Progettazione viste SSOX](deploying-a-database-update/_static/image3.png)
4. <span data-ttu-id="7a3dc-160">Nella finestra di progettazione, aggiungere un *commenti* colonna e renderlo *nvarchar (128)* e ammette valori null, quindi fare clic su **Update**.</span><span class="sxs-lookup"><span data-stu-id="7a3dc-160">In the designer, add a *Comments* column and make it *nvarchar(128)* and nullable, and then click **Update**.</span></span>

    ![Aggiunta di commenti colonna](deploying-a-database-update/_static/image4.png)
5. <span data-ttu-id="7a3dc-162">Nel **Anteprima aggiornamenti Database** fare clic su **Update Database**.</span><span class="sxs-lookup"><span data-stu-id="7a3dc-162">In the **Preview Database Updates** box, click **Update Database**.</span></span>

    ![Anteprima aggiornamenti Database](deploying-a-database-update/_static/image5.png)

### <a name="create-a-page-to-display-and-edit-the-new-column"></a><span data-ttu-id="7a3dc-164">Creare una pagina per visualizzare e modificare la nuova colonna</span><span class="sxs-lookup"><span data-stu-id="7a3dc-164">Create a page to display and edit the new column</span></span>

1. <span data-ttu-id="7a3dc-165">In **Esplora soluzioni**, fare doppio clic sul **Account** cartella nel progetto ContosoUniversity, fare clic su **Add**, quindi fare clic su **nuovo elemento** .</span><span class="sxs-lookup"><span data-stu-id="7a3dc-165">In **Solution Explorer**, right-click the **Account** folder in the ContosoUniversity project, click **Add**, and then click **New Item**.</span></span>
2. <span data-ttu-id="7a3dc-166">Creare una nuova **della pagina Master utilizzando di Web Form** e denominarlo *UserInfo.aspx*.</span><span class="sxs-lookup"><span data-stu-id="7a3dc-166">Create a new **Web Form Using Master Page** and name it *UserInfo.aspx*.</span></span> <span data-ttu-id="7a3dc-167">Accettare il valore predefinito *Site. master* file come pagina master.</span><span class="sxs-lookup"><span data-stu-id="7a3dc-167">Accept the default *Site.Master* file as the master page.</span></span>
3. <span data-ttu-id="7a3dc-168">Copiare il seguente markup nel `MainContent` `Content` elemento (l'ultimo di 3 `Content` elementi):</span><span class="sxs-lookup"><span data-stu-id="7a3dc-168">Copy the following markup into the `MainContent` `Content` element (the last of the 3 `Content` elements):</span></span>

    [!code-aspx[Main](deploying-a-database-update/samples/sample6.aspx)]
4. <span data-ttu-id="7a3dc-169">Fare doppio clic il *UserInfo.aspx* e fare clic su **Visualizza nel Browser**.</span><span class="sxs-lookup"><span data-stu-id="7a3dc-169">Right-click the *UserInfo.aspx* page and click **View in Browser**.</span></span>
5. <span data-ttu-id="7a3dc-170">Accedi con il *admin* le credenziali dell'utente (password viene *devpwd*) e aggiungere alcuni commenti a un utente per verificare il corretto funzionamento della pagina.</span><span class="sxs-lookup"><span data-stu-id="7a3dc-170">Log in with your *admin* user credentials (password is *devpwd*) and add some comments to a user to verify that the page works correctly.</span></span>

    ![Pagina informazioni utente](deploying-a-database-update/_static/image6.png)
6. <span data-ttu-id="7a3dc-172">Chiudere il browser.</span><span class="sxs-lookup"><span data-stu-id="7a3dc-172">Close the browser.</span></span>

## <a name="deploy-the-database-update"></a><span data-ttu-id="7a3dc-173">Distribuire l'aggiornamento del database</span><span class="sxs-lookup"><span data-stu-id="7a3dc-173">Deploy the database update</span></span>

<span data-ttu-id="7a3dc-174">Per distribuire usando il provider dbDacFx, è sufficiente selezionare la **aggiornare il database** opzione nel profilo di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="7a3dc-174">To deploy by using the dbDacFx provider, you just need to select the **Update database** option in the publish profile.</span></span> <span data-ttu-id="7a3dc-175">Tuttavia, per la distribuzione iniziale quando è stata utilizzata questa opzione inoltre configurato alcuni altri script SQL per eseguire: sono ancora nel profilo e sarà necessario impedirne l'esecuzione anche in questo caso.</span><span class="sxs-lookup"><span data-stu-id="7a3dc-175">However, for the initial deployment when you used this option you also configured some additional SQL scripts to run: those are still in the profile and you'll have to prevent them from running again.</span></span>

1. <span data-ttu-id="7a3dc-176">Aprire il **pubblica sul Web** procedura guidata facendo clic sul progetto ContosoUniversity e facendo clic su **Publish**.</span><span class="sxs-lookup"><span data-stu-id="7a3dc-176">Open the **Publish Web** wizard by right-clicking the ContosoUniversity project and clicking **Publish**.</span></span>
2. <span data-ttu-id="7a3dc-177">Selezionare il **Test** profilo.</span><span class="sxs-lookup"><span data-stu-id="7a3dc-177">Select the **Test** profile.</span></span>
3. <span data-ttu-id="7a3dc-178">Scegliere il **impostazioni** scheda.</span><span class="sxs-lookup"><span data-stu-id="7a3dc-178">Click the **Settings** tab.</span></span>
4. <span data-ttu-id="7a3dc-179">Sotto **DefaultConnection**, selezionare **Aggiorna database**.</span><span class="sxs-lookup"><span data-stu-id="7a3dc-179">Under **DefaultConnection**, select **Update database**.</span></span>
5. <span data-ttu-id="7a3dc-180">Disabilitare gli script aggiuntivi che è configurato per l'esecuzione per la distribuzione iniziale:</span><span class="sxs-lookup"><span data-stu-id="7a3dc-180">Disable the additional scripts that you configured to run for the initial deployment:</span></span>

    1. <span data-ttu-id="7a3dc-181">Fare clic su **Configura Aggiornamenti database**.</span><span class="sxs-lookup"><span data-stu-id="7a3dc-181">Click **Configure database updates**.</span></span>
    2. <span data-ttu-id="7a3dc-182">Nel **configurare gli aggiornamenti del Database** della finestra di dialogo deselezionare le caselle di controllo accanto a *Grant.sql* e *aspnet-data-dev.sql*.</span><span class="sxs-lookup"><span data-stu-id="7a3dc-182">In the **Configure Database Updates** dialog box, clear the check boxes next to *Grant.sql* and *aspnet-data-dev.sql*.</span></span>
    3. <span data-ttu-id="7a3dc-183">Fare clic su **Chiudi**.</span><span class="sxs-lookup"><span data-stu-id="7a3dc-183">Click **Close**.</span></span>
6. <span data-ttu-id="7a3dc-184">Scegliere il **Preview** scheda.</span><span class="sxs-lookup"><span data-stu-id="7a3dc-184">Click the **Preview** tab.</span></span>
7. <span data-ttu-id="7a3dc-185">Sotto **database** e a destra di **DefaultConnection**, fare clic sui **anteprima database** collegamento.</span><span class="sxs-lookup"><span data-stu-id="7a3dc-185">Under **Databases** and to the right of **DefaultConnection**, click the **Preview database** link.</span></span>

    ![Database (anteprima)](deploying-a-database-update/_static/image7.png)

    <span data-ttu-id="7a3dc-187">La finestra di anteprima mostra lo script che verrà eseguito nel database di destinazione per rendere tale schema del database corrispondono allo schema del database di origine.</span><span class="sxs-lookup"><span data-stu-id="7a3dc-187">The preview window shows the script that will be run in the destination database to make that database schema match the schema of the source database.</span></span> <span data-ttu-id="7a3dc-188">Lo script include un comando ALTER TABLE che aggiunge la nuova colonna.</span><span class="sxs-lookup"><span data-stu-id="7a3dc-188">The script includes an ALTER TABLE command that adds the new column.</span></span>
8. <span data-ttu-id="7a3dc-189">Chiudi il **Database (anteprima)** finestra di dialogo e quindi fare clic su **Publish**.</span><span class="sxs-lookup"><span data-stu-id="7a3dc-189">Close the **Database Preview** dialog box, and then click **Publish**.</span></span>

    <span data-ttu-id="7a3dc-190">Visual Studio distribuisce l'applicazione aggiornata e nel browser verrà aperta la home page.</span><span class="sxs-lookup"><span data-stu-id="7a3dc-190">Visual Studio deploys the updated application, and the browser opens to the home page.</span></span>
9. <span data-ttu-id="7a3dc-191">Eseguire la pagina della UserInfo (aggiungere *Account/UserInfo.aspx* a URL della home page) per verificare che l'aggiornamento è stato distribuito correttamente.</span><span class="sxs-lookup"><span data-stu-id="7a3dc-191">Run the UserInfo page (add *Account/UserInfo.aspx* to the home page URL) to verify that the update was successfully deployed.</span></span> <span data-ttu-id="7a3dc-192">È possibile accedere immettendo *admin* e *devpwd*.</span><span class="sxs-lookup"><span data-stu-id="7a3dc-192">You'll have to log in by entering *admin* and *devpwd*.</span></span>

    <span data-ttu-id="7a3dc-193">Per impostazione predefinita non vengono distribuiti i dati nelle tabelle e non sono state configurate uno script di distribuzione dei dati per l'esecuzione, in modo che non sono disponibili il commento aggiunto in fase di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="7a3dc-193">Data in tables is not deployed by default, and you didn't configure a data deployment script to run, so you won't find the comment that you added in development.</span></span> <span data-ttu-id="7a3dc-194">È possibile aggiungere un nuovo commento a questo punto nella gestione temporanea per verificare che la modifica è stata distribuita nel database e la pagina funziona correttamente.</span><span class="sxs-lookup"><span data-stu-id="7a3dc-194">You can add a new comment now in staging to verify that the change was deployed to the database and the page works correctly.</span></span>
10. <span data-ttu-id="7a3dc-195">Seguire la stessa procedura per distribuire in gestione temporanea e produzione.</span><span class="sxs-lookup"><span data-stu-id="7a3dc-195">Follow the same procedure to deploy to staging and production.</span></span>

    <span data-ttu-id="7a3dc-196">Non dimenticare di disabilitare gli script aggiuntivi.</span><span class="sxs-lookup"><span data-stu-id="7a3dc-196">Don't forget to disable the extra scripts.</span></span> <span data-ttu-id="7a3dc-197">L'unica differenza rispetto al profilo di Test è che si disabiliterà un solo script nel processo di Staging e produzione profili perché sono stati configurati per l'esecuzione solo *aspnet-prod-data.sql*.</span><span class="sxs-lookup"><span data-stu-id="7a3dc-197">The only difference compared to the Test profile is that you will disable only one script in the Staging and Production profiles because they were configured to run only *aspnet-prod-data.sql*.</span></span>

    <span data-ttu-id="7a3dc-198">Le credenziali per lo staging e produzione sono prodpwd e amministratore.</span><span class="sxs-lookup"><span data-stu-id="7a3dc-198">The credentials for staging and production are admin and prodpwd.</span></span>

    <span data-ttu-id="7a3dc-199">Per un aggiornamento dell'applicazione di produzione reali che include una modifica al database è in genere anche sarebbe portare offline l'applicazione durante la distribuzione caricando *app\_offline.htm* prima della pubblicazione e l'eliminazione in seguito, come illustrato nelle [nell'esercitazione precedente](deploying-a-code-update.md).</span><span class="sxs-lookup"><span data-stu-id="7a3dc-199">For a real production application update that includes a database change you would also typically take the application offline during deployment by uploading *app\_offline.htm* before publishing and deleting it afterward, as you saw in [the previous tutorial](deploying-a-code-update.md).</span></span>

## <a name="summary"></a><span data-ttu-id="7a3dc-200">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="7a3dc-200">Summary</span></span>

<span data-ttu-id="7a3dc-201">A questo punto è stata distribuita un aggiornamento dell'applicazione che una modifica al database tramite le migrazioni Code First e il provider dbDacFx incluso.</span><span class="sxs-lookup"><span data-stu-id="7a3dc-201">You've now deployed an application update that included a database change using both Code First Migrations and the dbDacFx provider.</span></span>

![Pagina instructors (insegnanti) con data di nascita](deploying-a-database-update/_static/image8.png)

![Pagina informazioni utente](deploying-a-database-update/_static/image9.png)

<span data-ttu-id="7a3dc-204">L'esercitazione successiva illustra come eseguire distribuzioni usando la riga di comando.</span><span class="sxs-lookup"><span data-stu-id="7a3dc-204">The next tutorial shows you how to execute deployments by using the command line.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="7a3dc-205">[Precedente](deploying-a-code-update.md)
> [Successivo](command-line-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="7a3dc-205">[Previous](deploying-a-code-update.md)
[Next](command-line-deployment.md)</span></span>
