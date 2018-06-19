---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
title: 'Distribuzione Web ASP.NET utilizzando Visual Studio: aggiornamento di un Database di distribuzione | Documenti Microsoft'
author: tdykstra
description: Questa serie di esercitazioni viene illustrato come distribuire un ASP.NET (pubblica) per App Web di servizio App di Azure o per un provider di hosting di terze parti, di applicazioni web da utilizza...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 9cad0833-486a-4474-a7f3-7715542ec4ce
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
msc.type: authoredcontent
ms.openlocfilehash: 3020cfa19bf12f21c6d42a77ed257595431b4e86
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30892621"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-a-database-update"></a><span data-ttu-id="dfbd6-103">Distribuzione Web ASP.NET utilizzando Visual Studio: aggiornamento di un Database di distribuzione</span><span class="sxs-lookup"><span data-stu-id="dfbd6-103">ASP.NET Web Deployment using Visual Studio: Deploying a Database Update</span></span>
====================
<span data-ttu-id="dfbd6-104">da [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="dfbd6-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="dfbd6-105">Scaricare il progetto Starter</span><span class="sxs-lookup"><span data-stu-id="dfbd6-105">Download Starter Project</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> <span data-ttu-id="dfbd6-106">Questa serie di esercitazioni viene illustrato come distribuire un ASP.NET (pubblica) per App Web di servizio App di Azure o per un provider di hosting di terze parti, di applicazioni web utilizzando Visual Studio 2012 o Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="dfbd6-106">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider, by using Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="dfbd6-107">Per informazioni sulla serie, vedere [la prima esercitazione di serie](introduction.md).</span><span class="sxs-lookup"><span data-stu-id="dfbd6-107">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="dfbd6-108">Panoramica</span><span class="sxs-lookup"><span data-stu-id="dfbd6-108">Overview</span></span>

<span data-ttu-id="dfbd6-109">In questa esercitazione è apportare una modifica al database e le modifiche correlate, verificare le modifiche in Visual Studio, quindi distribuire l'aggiornamento in ambienti di test, gestione temporanea e produzione.</span><span class="sxs-lookup"><span data-stu-id="dfbd6-109">In this tutorial, you make a database change and related code changes, test the changes in Visual Studio, then deploy the update to the test, staging, and production environments.</span></span>

<span data-ttu-id="dfbd6-110">L'esercitazione innanzitutto illustrato come aggiornare un database gestito da migrazioni Code First e in seguito viene illustrato come aggiornare un database tramite il provider dbDacFx.</span><span class="sxs-lookup"><span data-stu-id="dfbd6-110">The tutorial first shows how to update a database that is managed by Code First Migrations, and then later it shows how to update a database by using the dbDacFx provider.</span></span>

<span data-ttu-id="dfbd6-111">Promemoria: Se viene visualizzato un messaggio di errore o non funzioni man mano che Leggi l'esercitazione, assicurarsi di controllare il [risoluzione dei problemi di pagina](troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="dfbd6-111">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](troubleshooting.md).</span></span>

## <a name="deploy-a-database-update-by-using-code-first-migrations"></a><span data-ttu-id="dfbd6-112">Distribuire un aggiornamento del database utilizzando migrazioni Code First</span><span class="sxs-lookup"><span data-stu-id="dfbd6-112">Deploy a database update by using Code First Migrations</span></span>

<span data-ttu-id="dfbd6-113">In questa sezione, si aggiunge una colonna di data di nascita per il `Person` classe di base per il `Student` e `Instructor` entità.</span><span class="sxs-lookup"><span data-stu-id="dfbd6-113">In this section, you add a birth date column to the `Person` base class for the `Student` and `Instructor` entities.</span></span> <span data-ttu-id="dfbd6-114">È quindi necessario aggiornare la pagina che visualizza i dati di istruttore in modo da visualizzare la nuova colonna.</span><span class="sxs-lookup"><span data-stu-id="dfbd6-114">Then you update the page that displays instructor data so that it displays the new column.</span></span> <span data-ttu-id="dfbd6-115">Infine, le modifiche vengono distribuite a test, gestione temporanea e produzione.</span><span class="sxs-lookup"><span data-stu-id="dfbd6-115">Finally, you deploy the changes to test, staging, and production.</span></span>

### <a name="add-a-column-to-a-table-in-the-application-database"></a><span data-ttu-id="dfbd6-116">Aggiungere una colonna a una tabella nel database dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="dfbd6-116">Add a column to a table in the application database</span></span>

1. <span data-ttu-id="dfbd6-117">Nel *ContosoUniversity.DAL* progetto, aprire *Person* e aggiungere la proprietà seguente alla fine del `Person` classe (dovrebbero essere presenti due parentesi graffe riportata di seguito):</span><span class="sxs-lookup"><span data-stu-id="dfbd6-117">In the *ContosoUniversity.DAL* project, open *Person.cs* and add the following property at the end of the `Person` class (there should be two closing curly braces following it):</span></span>

    [!code-csharp[Main](deploying-a-database-update/samples/sample1.cs)]

    <span data-ttu-id="dfbd6-118">Aggiornare quindi il `Seed` metodo in modo che fornisce un valore per la nuova colonna.</span><span class="sxs-lookup"><span data-stu-id="dfbd6-118">Next, update the `Seed` method so that it provides a value for the new column.</span></span> <span data-ttu-id="dfbd6-119">Aprire *Migrations\Configuration.cs* e sostituire il blocco di codice che inizia `var instructors = new List<Instructor>` con blocco di codice seguente, che include informazioni sulla data di nascita:</span><span class="sxs-lookup"><span data-stu-id="dfbd6-119">Open *Migrations\Configuration.cs* and replace the code block that begins `var instructors = new List<Instructor>` with the following code block which includes birth date information:</span></span>

    [!code-csharp[Main](deploying-a-database-update/samples/sample2.cs)]
2. <span data-ttu-id="dfbd6-120">Compilare la soluzione e quindi aprire il **Package Manager Console** finestra.</span><span class="sxs-lookup"><span data-stu-id="dfbd6-120">Build the solution, and then open the **Package Manager Console** window.</span></span> <span data-ttu-id="dfbd6-121">Assicurarsi che ContosoUniversity.DAL sia ancora selezionato come il **progetto predefinito**.</span><span class="sxs-lookup"><span data-stu-id="dfbd6-121">Make sure that ContosoUniversity.DAL is still selected as the **Default project**.</span></span>
3. <span data-ttu-id="dfbd6-122">Nel **Package Manager Console** selezionare **ContosoUniversity.DAL** come il **progetto predefinito**, quindi immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="dfbd6-122">In the **Package Manager Console** window, select **ContosoUniversity.DAL** as the **Default project**, and then enter the following command:</span></span>

    [!code-powershell[Main](deploying-a-database-update/samples/sample3.ps1)]

    <span data-ttu-id="dfbd6-123">Al termine di questo comando, Visual Studio apre il file di classe che definisce la nuova `DbMIgration` (classe) e il `Up` metodo è possibile visualizzare il codice che crea la nuova colonna.</span><span class="sxs-lookup"><span data-stu-id="dfbd6-123">When this command finishes, Visual Studio opens the class file that defines the new `DbMIgration` class, and in the `Up` method you can see the code that creates the new column.</span></span> <span data-ttu-id="dfbd6-124">Il `Up` metodo consente di creare la colonna quando si implementa la modifica e `Down` metodo elimina la colonna quando si esegue il rollback della modifica.</span><span class="sxs-lookup"><span data-stu-id="dfbd6-124">The `Up` method creates the column when you are implementing the change, and the `Down` method deletes the column when you are rolling back the change.</span></span>

    ![AddBirthDate_migration_code](deploying-a-database-update/_static/image1.png)
4. <span data-ttu-id="dfbd6-126">Compilare la soluzione e quindi immettere il comando seguente nel **Package Manager Console** finestra (assicurarsi che sia ancora selezionato il progetto ContosoUniversity.DAL):</span><span class="sxs-lookup"><span data-stu-id="dfbd6-126">Build the solution, and then enter the following command in the **Package Manager Console** window (make sure the ContosoUniversity.DAL project is still selected):</span></span>

    [!code-powershell[Main](deploying-a-database-update/samples/sample4.ps1)]

    <span data-ttu-id="dfbd6-127">Entity Framework viene eseguito il `Up` (metodo) e quindi viene eseguito il `Seed` metodo.</span><span class="sxs-lookup"><span data-stu-id="dfbd6-127">The Entity Framework runs the `Up` method and then runs the `Seed` method.</span></span>

### <a name="display-the-new-column-in-the-instructors-page"></a><span data-ttu-id="dfbd6-128">Visualizzare la nuova colonna nella pagina i docenti</span><span class="sxs-lookup"><span data-stu-id="dfbd6-128">Display the new column in the Instructors page</span></span>

1. <span data-ttu-id="dfbd6-129">Nel progetto ContosoUniversity, aprire *Instructors.aspx* e aggiungere un nuovo modello di campo per visualizzare la data di nascita.</span><span class="sxs-lookup"><span data-stu-id="dfbd6-129">In the ContosoUniversity project, open *Instructors.aspx* and add a new template field to display the birth date.</span></span> <span data-ttu-id="dfbd6-130">Aggiungere il progetto tra quelle per l'assegnazione di office e di data di assunzione:</span><span class="sxs-lookup"><span data-stu-id="dfbd6-130">Add it between the ones for hire date and office assignment:</span></span>

    [!code-aspx[Main](deploying-a-database-update/samples/sample5.aspx?highlight=9-17)]

    <span data-ttu-id="dfbd6-131">(Se il rientro di codice ottiene sincronizzato, è possibile premere CTRL-K e CTRL + D per riformattare automaticamente il file.)</span><span class="sxs-lookup"><span data-stu-id="dfbd6-131">(If code indentation gets out of sync, you can press CTRL-K and then CTRL-D to automatically reformat the file.)</span></span>
2. <span data-ttu-id="dfbd6-132">Eseguire l'applicazione e fare clic su di **i docenti** collegamento.</span><span class="sxs-lookup"><span data-stu-id="dfbd6-132">Run the application and click the **Instructors** link.</span></span>

    <span data-ttu-id="dfbd6-133">Quando il caricamento della pagina, vedrai che contiene il nuovo campo Data di nascita.</span><span class="sxs-lookup"><span data-stu-id="dfbd6-133">When the page loads, you see that it has the new birth date field.</span></span>

    ![Pagina istruttori con data di nascita](deploying-a-database-update/_static/image2.png)
3. <span data-ttu-id="dfbd6-135">Chiudere il browser.</span><span class="sxs-lookup"><span data-stu-id="dfbd6-135">Close the browser.</span></span>

### <a name="deploy-the-database-update"></a><span data-ttu-id="dfbd6-136">Distribuire l'aggiornamento del database</span><span class="sxs-lookup"><span data-stu-id="dfbd6-136">Deploy the database update</span></span>

1. <span data-ttu-id="dfbd6-137">In **Esplora** selezionare il progetto ContosoUniversity.</span><span class="sxs-lookup"><span data-stu-id="dfbd6-137">In **Solution Explorer** select the ContosoUniversity project.</span></span>
2. <span data-ttu-id="dfbd6-138">Nel **Web-pubblicazione con un clic** sulla barra degli strumenti, fare clic su di **Test** profilo di pubblicazione e quindi fare clic su **pubblica sul Web**.</span><span class="sxs-lookup"><span data-stu-id="dfbd6-138">In the **Web One Click Publish** toolbar, click the **Test** publish profile, and then click **Publish Web**.</span></span> <span data-ttu-id="dfbd6-139">(Se la barra degli strumenti è disabilitato, selezionare il progetto ContosoUniversity in **Esplora**.)</span><span class="sxs-lookup"><span data-stu-id="dfbd6-139">(If the toolbar is disabled, select the ContosoUniversity project in **Solution Explorer**.)</span></span>

    <span data-ttu-id="dfbd6-140">Visual Studio distribuisce l'applicazione aggiornata e il browser viene visualizzata la pagina home.</span><span class="sxs-lookup"><span data-stu-id="dfbd6-140">Visual Studio deploys the updated application, and the browser opens to the home page.</span></span>
3. <span data-ttu-id="dfbd6-141">Eseguire il **i docenti** pagina per verificare che l'aggiornamento è stato distribuito correttamente.</span><span class="sxs-lookup"><span data-stu-id="dfbd6-141">Run the **Instructors** page to verify that the update was successfully deployed.</span></span>

    <span data-ttu-id="dfbd6-142">Quando l'applicazione tenta di accedere al database per questa pagina, il primo codice aggiorna lo schema del database e viene eseguito il `Seed` metodo.</span><span class="sxs-lookup"><span data-stu-id="dfbd6-142">When the application tries to access the database for this page, Code First updates the database schema and runs the `Seed` method.</span></span> <span data-ttu-id="dfbd6-143">Quando viene visualizzata la pagina, viene visualizzato il previsto **data di nascita** colonna delle date in essa contenuti.</span><span class="sxs-lookup"><span data-stu-id="dfbd6-143">When the page displays, you see the expected **Birth Date** column with dates in it.</span></span>
4. <span data-ttu-id="dfbd6-144">Nel **Web-pubblicazione con un clic** sulla barra degli strumenti, fare clic su di **gestione temporanea** profilo di pubblicazione e quindi fare clic su **pubblica sul Web**.</span><span class="sxs-lookup"><span data-stu-id="dfbd6-144">In the **Web One Click Publish** toolbar, click the **Staging** publish profile, and then click **Publish Web**.</span></span>
5. <span data-ttu-id="dfbd6-145">Eseguire il **i docenti** pagina di gestione temporanea per verificare che l'aggiornamento è stato distribuito correttamente.</span><span class="sxs-lookup"><span data-stu-id="dfbd6-145">Run the **Instructors** page in staging to verify that the update was successfully deployed.</span></span>
6. <span data-ttu-id="dfbd6-146">Nel **Web-pubblicazione con un clic** sulla barra degli strumenti, fare clic su di **produzione** profilo di pubblicazione e quindi fare clic su **pubblica sul Web**.</span><span class="sxs-lookup"><span data-stu-id="dfbd6-146">In the **Web One Click Publish** toolbar, click the **Production** publish profile, and then click **Publish Web**.</span></span>
7. <span data-ttu-id="dfbd6-147">Eseguire il **i docenti** pagina nell'ambiente di produzione per verificare che l'aggiornamento è stato distribuito correttamente.</span><span class="sxs-lookup"><span data-stu-id="dfbd6-147">Run the **Instructors** page in production to verify that the update was successfully deployed.</span></span>

    <span data-ttu-id="dfbd6-148">Per un aggiornamento dell'applicazione di produzione reali che include una modifica al database anche in genere si procederà quindi l'applicazione durante la distribuzione tramite *app\_offline.htm*, come visto nell'esercitazione precedente.</span><span class="sxs-lookup"><span data-stu-id="dfbd6-148">For a real production application update that includes a database change you would also typically take the application offline during deployment by using *app\_offline.htm*, as you saw in the previous tutorial.</span></span>

## <a name="deploy-a-database-update-by-using-the-dbdacfx-provider"></a><span data-ttu-id="dfbd6-149">Distribuire un aggiornamento del database tramite il provider dbDacFx</span><span class="sxs-lookup"><span data-stu-id="dfbd6-149">Deploy a database update by using the dbDacFx provider</span></span>

<span data-ttu-id="dfbd6-150">In questa sezione, si aggiunge un *commenti* colonna per il *utente* tabella nel database delle appartenenze e creare una pagina che consente di visualizzare e modificare commenti per ogni utente.</span><span class="sxs-lookup"><span data-stu-id="dfbd6-150">In this section, you add a *Comments* column to the *User* table in the membership database and create a page that lets you display and edit comments for each user.</span></span> <span data-ttu-id="dfbd6-151">Distribuire le modifiche a test, gestione temporanea e produzione.</span><span class="sxs-lookup"><span data-stu-id="dfbd6-151">Then you deploy the changes to test, staging, and production.</span></span>

### <a name="add-a-column-to-a-table-in-the-membership-database"></a><span data-ttu-id="dfbd6-152">Aggiungere una colonna a una tabella nel database delle appartenenze</span><span class="sxs-lookup"><span data-stu-id="dfbd6-152">Add a column to a table in the membership database</span></span>

1. <span data-ttu-id="dfbd6-153">In Visual Studio, aprire **Esplora oggetti di SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="dfbd6-153">In Visual Studio, open **SQL Server Object Explorer**.</span></span>
2. <span data-ttu-id="dfbd6-154">Espandere **(localdb) \v11.0**, espandere **database**, espandere **aspnet ContosoUniversity** (non **aspnet-ContosoUniversity-Prod**) e quindi espandere **tabelle**.</span><span class="sxs-lookup"><span data-stu-id="dfbd6-154">Expand **(localdb)\v11.0**, expand **Databases**, expand **aspnet-ContosoUniversity** (not **aspnet-ContosoUniversity-Prod**) and then expand **Tables**.</span></span>

    <span data-ttu-id="dfbd6-155">Se non viene visualizzato **(localdb) \v11.0** sotto il **SQL Server** nodo, fare doppio clic su di **SQL Server** nodo e fare clic su **aggiungere SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="dfbd6-155">If you don't see **(localdb)\v11.0** under the **SQL Server** node, right-click the **SQL Server** node and click **Add SQL Server**.</span></span> <span data-ttu-id="dfbd6-156">Nel **Connetti al Server** la finestra di dialogo immettere *(localdb) \v11.0* come il **nome del Server**, quindi fare clic su **Connetti**.</span><span class="sxs-lookup"><span data-stu-id="dfbd6-156">In the **Connect to Server** dialog box enter *(localdb)\v11.0* as the **Server name**, and then click **Connect**.</span></span>

    <span data-ttu-id="dfbd6-157">Se non viene visualizzato **aspnet ContosoUniversity**, eseguire il progetto e accedere con il *admin* credenziali (password è *devpwd*), quindi aggiornare il  **Esplora oggetti di SQL Server** finestra.</span><span class="sxs-lookup"><span data-stu-id="dfbd6-157">If you don't see **aspnet-ContosoUniversity**, run the project and log in using the *admin* credentials (password is *devpwd*), and then refresh the **SQL Server Object Explorer** window.</span></span>
3. <span data-ttu-id="dfbd6-158">Fare doppio clic su di **utenti** tabella e quindi fare clic su **Visualizza finestra di progettazione**.</span><span class="sxs-lookup"><span data-stu-id="dfbd6-158">Right-click the **Users** table, and then click **View Designer**.</span></span>

    ![Finestra di progettazione sillaba SSOX](deploying-a-database-update/_static/image3.png)
4. <span data-ttu-id="dfbd6-160">Nella finestra di progettazione, aggiungere un *commenti* colonna e renderlo *nvarchar (128)* e ammette valori null, quindi fare clic su **aggiornamento**.</span><span class="sxs-lookup"><span data-stu-id="dfbd6-160">In the designer, add a *Comments* column and make it *nvarchar(128)* and nullable, and then click **Update**.</span></span>

    ![Aggiunta di commenti colonna](deploying-a-database-update/_static/image4.png)
5. <span data-ttu-id="dfbd6-162">Nel **Anteprima aggiornamenti Database** fare clic su **aggiornamento Database**.</span><span class="sxs-lookup"><span data-stu-id="dfbd6-162">In the **Preview Database Updates** box, click **Update Database**.</span></span>

    ![Anteprima aggiornamenti Database](deploying-a-database-update/_static/image5.png)

### <a name="create-a-page-to-display-and-edit-the-new-column"></a><span data-ttu-id="dfbd6-164">Creare una pagina per visualizzare e modificare la nuova colonna</span><span class="sxs-lookup"><span data-stu-id="dfbd6-164">Create a page to display and edit the new column</span></span>

1. <span data-ttu-id="dfbd6-165">In **Esplora**, fare doppio clic su di **Account** cartella nel progetto ContosoUniversity, fare clic su **Aggiungi**e quindi fare clic su **nuovo elemento** .</span><span class="sxs-lookup"><span data-stu-id="dfbd6-165">In **Solution Explorer**, right-click the **Account** folder in the ContosoUniversity project, click **Add**, and then click **New Item**.</span></span>
2. <span data-ttu-id="dfbd6-166">Creare un nuovo **pagina Web Form utilizzando Master** e denominarlo *UserInfo.aspx*.</span><span class="sxs-lookup"><span data-stu-id="dfbd6-166">Create a new **Web Form Using Master Page** and name it *UserInfo.aspx*.</span></span> <span data-ttu-id="dfbd6-167">Accettare il valore predefinito *Site. master* file della pagina master.</span><span class="sxs-lookup"><span data-stu-id="dfbd6-167">Accept the default *Site.Master* file as the master page.</span></span>
3. <span data-ttu-id="dfbd6-168">Copiare il seguente markup nel `MainContent` `Content` elemento (l'ultimo di 3 `Content` elementi):</span><span class="sxs-lookup"><span data-stu-id="dfbd6-168">Copy the following markup into the `MainContent` `Content` element (the last of the 3 `Content` elements):</span></span>

    [!code-aspx[Main](deploying-a-database-update/samples/sample6.aspx)]
4. <span data-ttu-id="dfbd6-169">Fare doppio clic su di *UserInfo.aspx* pagina e fare clic su **Visualizza nel Browser**.</span><span class="sxs-lookup"><span data-stu-id="dfbd6-169">Right-click the *UserInfo.aspx* page and click **View in Browser**.</span></span>
5. <span data-ttu-id="dfbd6-170">Accedere con il *admin* le credenziali dell'utente (password *devpwd*) e aggiungere alcuni commenti a un utente per verificare il corretto funzionamento della pagina.</span><span class="sxs-lookup"><span data-stu-id="dfbd6-170">Log in with your *admin* user credentials (password is *devpwd*) and add some comments to a user to verify that the page works correctly.</span></span>

    ![Pagina informazioni utente](deploying-a-database-update/_static/image6.png)
6. <span data-ttu-id="dfbd6-172">Chiudere il browser.</span><span class="sxs-lookup"><span data-stu-id="dfbd6-172">Close the browser.</span></span>

## <a name="deploy-the-database-update"></a><span data-ttu-id="dfbd6-173">Distribuire l'aggiornamento del database</span><span class="sxs-lookup"><span data-stu-id="dfbd6-173">Deploy the database update</span></span>

<span data-ttu-id="dfbd6-174">Per distribuire tramite il provider dbDacFx, è sufficiente selezionare il **Aggiorna database** opzione nel profilo di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="dfbd6-174">To deploy by using the dbDacFx provider, you just need to select the **Update database** option in the publish profile.</span></span> <span data-ttu-id="dfbd6-175">Tuttavia, per la distribuzione iniziale quando si utilizza questa opzione è inoltre configurato alcuni script SQL aggiuntive da eseguire: quelli presenti nel profilo, è necessario evitare che venga eseguita di nuovo.</span><span class="sxs-lookup"><span data-stu-id="dfbd6-175">However, for the initial deployment when you used this option you also configured some additional SQL scripts to run: those are still in the profile and you'll have to prevent them from running again.</span></span>

1. <span data-ttu-id="dfbd6-176">Aprire il **pubblica sul Web** guidata facendo clic su progetto ContosoUniversity e facendo clic su **pubblica**.</span><span class="sxs-lookup"><span data-stu-id="dfbd6-176">Open the **Publish Web** wizard by right-clicking the ContosoUniversity project and clicking **Publish**.</span></span>
2. <span data-ttu-id="dfbd6-177">Selezionare il **Test** profilo.</span><span class="sxs-lookup"><span data-stu-id="dfbd6-177">Select the **Test** profile.</span></span>
3. <span data-ttu-id="dfbd6-178">Fare clic su di **impostazioni** scheda.</span><span class="sxs-lookup"><span data-stu-id="dfbd6-178">Click the **Settings** tab.</span></span>
4. <span data-ttu-id="dfbd6-179">In **DefaultConnection**selezionare **Aggiorna database**.</span><span class="sxs-lookup"><span data-stu-id="dfbd6-179">Under **DefaultConnection**, select **Update database**.</span></span>
5. <span data-ttu-id="dfbd6-180">Disabilitare gli script aggiuntivi configurati per essere eseguiti per la distribuzione iniziale:</span><span class="sxs-lookup"><span data-stu-id="dfbd6-180">Disable the additional scripts that you configured to run for the initial deployment:</span></span>

    1. <span data-ttu-id="dfbd6-181">Fare clic su **configurare gli aggiornamenti di database**.</span><span class="sxs-lookup"><span data-stu-id="dfbd6-181">Click **Configure database updates**.</span></span>
    2. <span data-ttu-id="dfbd6-182">Nel **Configura Aggiornamenti Database** la finestra di dialogo, deselezionare le caselle di controllo accanto a *Grant.sql* e *aspnet-data-dev.sql*.</span><span class="sxs-lookup"><span data-stu-id="dfbd6-182">In the **Configure Database Updates** dialog box, clear the check boxes next to *Grant.sql* and *aspnet-data-dev.sql*.</span></span>
    3. <span data-ttu-id="dfbd6-183">Fare clic su **Chiudi**.</span><span class="sxs-lookup"><span data-stu-id="dfbd6-183">Click **Close**.</span></span>
6. <span data-ttu-id="dfbd6-184">Fare clic su di **anteprima** scheda.</span><span class="sxs-lookup"><span data-stu-id="dfbd6-184">Click the **Preview** tab.</span></span>
7. <span data-ttu-id="dfbd6-185">In **database** e a destra del **DefaultConnection**, fare clic su di **database di anteprima** collegamento.</span><span class="sxs-lookup"><span data-stu-id="dfbd6-185">Under **Databases** and to the right of **DefaultConnection**, click the **Preview database** link.</span></span>

    ![Anteprima di database](deploying-a-database-update/_static/image7.png)

    <span data-ttu-id="dfbd6-187">La finestra di anteprima mostra lo script che verrà eseguito nel database di destinazione affinché corrisponda allo schema del database di origine schema del database.</span><span class="sxs-lookup"><span data-stu-id="dfbd6-187">The preview window shows the script that will be run in the destination database to make that database schema match the schema of the source database.</span></span> <span data-ttu-id="dfbd6-188">Lo script include un comando ALTER TABLE che aggiunge la nuova colonna.</span><span class="sxs-lookup"><span data-stu-id="dfbd6-188">The script includes an ALTER TABLE command that adds the new column.</span></span>
8. <span data-ttu-id="dfbd6-189">Chiudi il **anteprima di Database** la finestra di dialogo e quindi fare clic su **pubblica**.</span><span class="sxs-lookup"><span data-stu-id="dfbd6-189">Close the **Database Preview** dialog box, and then click **Publish**.</span></span>

    <span data-ttu-id="dfbd6-190">Visual Studio distribuisce l'applicazione aggiornata e il browser viene visualizzata la pagina home.</span><span class="sxs-lookup"><span data-stu-id="dfbd6-190">Visual Studio deploys the updated application, and the browser opens to the home page.</span></span>
9. <span data-ttu-id="dfbd6-191">Eseguire la pagina UserInfo (aggiungere *Account/UserInfo.aspx* per l'URL della home page) per verificare che l'aggiornamento è stato distribuito correttamente.</span><span class="sxs-lookup"><span data-stu-id="dfbd6-191">Run the UserInfo page (add *Account/UserInfo.aspx* to the home page URL) to verify that the update was successfully deployed.</span></span> <span data-ttu-id="dfbd6-192">È necessario accedere immettendo *admin* e *devpwd*.</span><span class="sxs-lookup"><span data-stu-id="dfbd6-192">You'll have to log in by entering *admin* and *devpwd*.</span></span>

    <span data-ttu-id="dfbd6-193">Per impostazione predefinita non vengono distribuiti i dati nelle tabelle e si non è stato configurato uno script di distribuzione di dati per l'esecuzione, in modo da non individuare il commento è stato aggiunto in fase di sviluppo.</span><span class="sxs-lookup"><span data-stu-id="dfbd6-193">Data in tables is not deployed by default, and you didn't configure a data deployment script to run, so you won't find the comment that you added in development.</span></span> <span data-ttu-id="dfbd6-194">È possibile aggiungere un nuovo commento a questo punto di gestione temporanea per verificare che la modifica è stata distribuita il database e corretto funzionamento della pagina.</span><span class="sxs-lookup"><span data-stu-id="dfbd6-194">You can add a new comment now in staging to verify that the change was deployed to the database and the page works correctly.</span></span>
10. <span data-ttu-id="dfbd6-195">Seguire la stessa procedura per la distribuzione di gestione temporanea e produzione.</span><span class="sxs-lookup"><span data-stu-id="dfbd6-195">Follow the same procedure to deploy to staging and production.</span></span>

    <span data-ttu-id="dfbd6-196">Non dimenticare di disattivare gli script aggiuntivi.</span><span class="sxs-lookup"><span data-stu-id="dfbd6-196">Don't forget to disable the extra scripts.</span></span> <span data-ttu-id="dfbd6-197">L'unica differenza rispetto al profilo di Test è che si disabiliterà solo uno script di gestione temporanea e produzione profili perché sono stati configurati per l'esecuzione solo *aspnet-op-data.sql*.</span><span class="sxs-lookup"><span data-stu-id="dfbd6-197">The only difference compared to the Test profile is that you will disable only one script in the Staging and Production profiles because they were configured to run only *aspnet-prod-data.sql*.</span></span>

    <span data-ttu-id="dfbd6-198">Le credenziali per la gestione temporanea e produzione sono prodpwd e amministrazione.</span><span class="sxs-lookup"><span data-stu-id="dfbd6-198">The credentials for staging and production are admin and prodpwd.</span></span>

    <span data-ttu-id="dfbd6-199">Per un aggiornamento dell'applicazione di produzione reali che include una modifica al database anche in genere si procederà quindi l'applicazione durante la distribuzione caricando *app\_offline.htm* prima della pubblicazione e l'eliminazione in seguito, come specificato nella [esercitazione precedente](deploying-a-code-update.md).</span><span class="sxs-lookup"><span data-stu-id="dfbd6-199">For a real production application update that includes a database change you would also typically take the application offline during deployment by uploading *app\_offline.htm* before publishing and deleting it afterward, as you saw in [the previous tutorial](deploying-a-code-update.md).</span></span>

## <a name="summary"></a><span data-ttu-id="dfbd6-200">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="dfbd6-200">Summary</span></span>

<span data-ttu-id="dfbd6-201">Aggiornamento di un'applicazione che include una modifica di database utilizzando migrazioni Code First sia il provider dbDacFx distribuiti a questo punto.</span><span class="sxs-lookup"><span data-stu-id="dfbd6-201">You've now deployed an application update that included a database change using both Code First Migrations and the dbDacFx provider.</span></span>

![Pagina istruttori con data di nascita](deploying-a-database-update/_static/image8.png)

![Pagina informazioni utente](deploying-a-database-update/_static/image9.png)

<span data-ttu-id="dfbd6-204">L'esercitazione successiva viene illustrato come eseguire le distribuzioni tramite la riga di comando.</span><span class="sxs-lookup"><span data-stu-id="dfbd6-204">The next tutorial shows you how to execute deployments by using the command line.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="dfbd6-205">[Precedente](deploying-a-code-update.md)
> [Successivo](command-line-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="dfbd6-205">[Previous](deploying-a-code-update.md)
[Next](command-line-deployment.md)</span></span>
