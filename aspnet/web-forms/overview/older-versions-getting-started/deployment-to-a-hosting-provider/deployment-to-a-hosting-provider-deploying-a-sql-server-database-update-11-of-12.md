---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
title: "Distribuzione di un'applicazione Web ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: distribuzione di un aggiornamento di SQL Server Database - 11 12 | Documenti Microsoft"
author: tdykstra
description: Questa serie di esercitazioni viene illustrato come distribuire un ASP.NET, pubblicare, progetto di applicazione web che include un database di SQL Server Compact con Visual riceventi...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: 5e2bb092-cb22-4511-ad0a-22ae12dd99b3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
msc.type: authoredcontent
ms.openlocfilehash: d8cc072c631900937f31c8f2637869b53d46cf54
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-sql-server-database-update---11-of-12"></a><span data-ttu-id="d46b8-103">Distribuzione di un'applicazione Web ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: distribuzione di un aggiornamento di SQL Server Database - 11 12</span><span class="sxs-lookup"><span data-stu-id="d46b8-103">Deploying an ASP.NET Web Application with SQL Server Compact using Visual Studio or Visual Web Developer: Deploying a SQL Server Database Update - 11 of 12</span></span>
====================
<span data-ttu-id="d46b8-104">da [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="d46b8-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="d46b8-105">Scaricare il progetto Starter</span><span class="sxs-lookup"><span data-stu-id="d46b8-105">Download Starter Project</span></span>](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> <span data-ttu-id="d46b8-106">Questa serie di esercitazioni viene illustrato come distribuire un ASP.NET, pubblicare, progetto di applicazione web che include un database di SQL Server Compact con Visual Studio 2012 RC o Visual Studio Express 2012 RC per Web.</span><span class="sxs-lookup"><span data-stu-id="d46b8-106">This series of tutorials shows you how to deploy (publish) an ASP.NET web application project that includes a SQL Server Compact database by using Visual Studio 2012 RC or Visual Studio Express 2012 RC for Web.</span></span> <span data-ttu-id="d46b8-107">Se si installa l'aggiornamento di pubblicazione Web, è anche possibile utilizzare Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="d46b8-107">You can also use Visual Studio 2010 if you install the Web Publish Update.</span></span> <span data-ttu-id="d46b8-108">Per un'introduzione alla serie, vedere [la prima esercitazione di serie](deployment-to-a-hosting-provider-introduction-1-of-12.md).</span><span class="sxs-lookup"><span data-stu-id="d46b8-108">For an introduction to the series, see [the first tutorial in the series](deployment-to-a-hosting-provider-introduction-1-of-12.md).</span></span>
> 
> <span data-ttu-id="d46b8-109">Per un'esercitazione che illustra le funzionalità di distribuzione introdotte dopo il rilascio di Visual Studio 2012 RC, viene illustrata l'implementazione di edizioni di SQL Server diverso da SQL Server Compact e viene illustrato come distribuire siti Web di Windows Azure, vedere [distribuzione Web di ASP.NET utilizzo di Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).</span><span class="sxs-lookup"><span data-stu-id="d46b8-109">For a tutorial that shows deployment features introduced after the RC release of Visual Studio 2012, shows how to deploy SQL Server editions other than SQL Server Compact, and shows how to deploy to Windows Azure Web Sites, see [ASP.NET Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="d46b8-110">Panoramica</span><span class="sxs-lookup"><span data-stu-id="d46b8-110">Overview</span></span>

<span data-ttu-id="d46b8-111">In questa esercitazione viene illustrato come distribuire un aggiornamento del database in un database di SQL Server completo.</span><span class="sxs-lookup"><span data-stu-id="d46b8-111">This tutorial shows you how to deploy a database update to a full SQL Server database.</span></span> <span data-ttu-id="d46b8-112">Poiché le migrazioni Code First esegue tutte le operazioni di aggiornamento del database, il processo è quasi identico a quella eseguita per SQL Server Compact nel [un aggiornamento del Database di distribuzione](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md) esercitazione.</span><span class="sxs-lookup"><span data-stu-id="d46b8-112">Because Code First Migrations does all the work of updating the database, the process is almost identical to what you did for SQL Server Compact in the [Deploying a Database Update](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md) tutorial.</span></span>

<span data-ttu-id="d46b8-113">Promemoria: Se viene visualizzato un messaggio di errore o non funzioni man mano che Leggi l'esercitazione, assicurarsi di controllare il [risoluzione dei problemi di pagina](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).</span><span class="sxs-lookup"><span data-stu-id="d46b8-113">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).</span></span>

## <a name="adding-a-new-column-to-a-table"></a><span data-ttu-id="d46b8-114">Aggiunta di una nuova colonna a una tabella</span><span class="sxs-lookup"><span data-stu-id="d46b8-114">Adding a New Column to a Table</span></span>

<span data-ttu-id="d46b8-115">In questa sezione dell'esercitazione che si verrà modificato un database e i corrispondenti alle modifiche al codice, quindi eseguirne il test in Visual Studio in preparazione per la distribuzione per gli ambienti di test e produzione.</span><span class="sxs-lookup"><span data-stu-id="d46b8-115">In this section of the tutorial you'll make a database change and corresponding code changes, then test them in Visual Studio in preparation for deploying them to the test and production environments.</span></span> <span data-ttu-id="d46b8-116">La modifica comporta l'aggiunta di un `OfficeHours` colonna per il `Instructor` entità e le nuove informazioni nella visualizzazione di **i docenti** pagina web.</span><span class="sxs-lookup"><span data-stu-id="d46b8-116">The change involves adding an `OfficeHours` column to the `Instructor` entity and displaying the new information in the **Instructors** web page.</span></span>

<span data-ttu-id="d46b8-117">Nel progetto ContosoUniversity.DAL, aprire *Instructor.cs* e aggiungere la seguente proprietà tra il `HireDate` e `Courses` proprietà:</span><span class="sxs-lookup"><span data-stu-id="d46b8-117">In the ContosoUniversity.DAL project, open *Instructor.cs* and add the following property between the `HireDate` and `Courses` properties:</span></span>

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample1.cs)]

<span data-ttu-id="d46b8-118">Aggiornare la classe di inizializzatore in modo che la nuova colonna con dati di test esegue il seeding.</span><span class="sxs-lookup"><span data-stu-id="d46b8-118">Update the initializer class so that it seeds the new column with test data.</span></span> <span data-ttu-id="d46b8-119">Aprire *Migrations\Configuration.cs* e sostituire il blocco di codice che inizia `var instructors = new List<Instructor>` con blocco di codice seguente, che include la nuova colonna:</span><span class="sxs-lookup"><span data-stu-id="d46b8-119">Open *Migrations\Configuration.cs* and replace the code block that begins `var instructors = new List<Instructor>` with the following code block which includes the new column:</span></span>

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample2.cs)]

<span data-ttu-id="d46b8-120">Nel progetto ContosoUniversity, aprire *Instructors.aspx* e aggiungere un nuovo campo di modello per l'orario di ufficio appena prima della chiusura `</Columns>` tag nel primo `GridView` controllo:</span><span class="sxs-lookup"><span data-stu-id="d46b8-120">In the ContosoUniversity project, open *Instructors.aspx* and add a new template field for office hours just before the closing `</Columns>` tag in the first `GridView` control:</span></span>

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample3.aspx)]

<span data-ttu-id="d46b8-121">Compilare la soluzione.</span><span class="sxs-lookup"><span data-stu-id="d46b8-121">Build the solution.</span></span>

<span data-ttu-id="d46b8-122">Aprire il **Package Manager Console** finestra e selezionare ContosoUniversity.DAL come il **progetto predefinito**.</span><span class="sxs-lookup"><span data-stu-id="d46b8-122">Open the **Package Manager Console** window, and select ContosoUniversity.DAL as the **Default project**.</span></span>

<span data-ttu-id="d46b8-123">Immettere i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="d46b8-123">Enter the following commands:</span></span>

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample4.ps1)]

<span data-ttu-id="d46b8-124">Eseguire l'applicazione e selezionare il **i docenti** pagina.</span><span class="sxs-lookup"><span data-stu-id="d46b8-124">Run the application and select the **Instructors** page.</span></span> <span data-ttu-id="d46b8-125">La pagina richiede un po' più tempo del solito da caricare, poiché Entity Framework consente di ricreare il database e si esegue il seeding con dati di test.</span><span class="sxs-lookup"><span data-stu-id="d46b8-125">The page takes a little longer than usual to load, because the Entity Framework re-creates the database and seeds it with test data.</span></span>

<span data-ttu-id="d46b8-126">[![Instructors_page_with_office_hours](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="d46b8-126">[![Instructors_page_with_office_hours](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image1.png)</span></span>

## <a name="deploying-the-database-update-to-the-test-environment"></a><span data-ttu-id="d46b8-127">L'aggiornamento del Database di distribuzione nell'ambiente di testing</span><span class="sxs-lookup"><span data-stu-id="d46b8-127">Deploying the Database Update to the Test Environment</span></span>

<span data-ttu-id="d46b8-128">Quando si utilizza migrazioni Code First, il metodo per la distribuzione di una modifica al database di SQL Server è la stessa di SQL Server Compact.</span><span class="sxs-lookup"><span data-stu-id="d46b8-128">When you use Code First Migrations, the method for deploying a database change to SQL Server is the same as for SQL Server Compact.</span></span> <span data-ttu-id="d46b8-129">Tuttavia, è necessario modificare il Test di profilo di pubblicazione, perché è ancora stato impostato per la migrazione da SQL Server Compact a SQL Server.</span><span class="sxs-lookup"><span data-stu-id="d46b8-129">However, you have to change the Test publish profile because it is still set up to migrate from SQL Server Compact to SQL Server.</span></span>

<span data-ttu-id="d46b8-130">Il primo passaggio consiste nel rimuovere le trasformazioni di stringa di connessione che è stato creato nell'esercitazione precedente.</span><span class="sxs-lookup"><span data-stu-id="d46b8-130">The first step is to remove the connection string transformations that you created in the previous tutorial.</span></span> <span data-ttu-id="d46b8-131">Questi non sono più necessarie poiché è necessario specificare le trasformazioni di stringa di connessione nel profilo di pubblicazione, seguendo la prima è stato configurato il **pubblicazione/creazione pacchetto SQL** scheda per la migrazione a SQL Server.</span><span class="sxs-lookup"><span data-stu-id="d46b8-131">These are no longer needed because you'll specify connection string transformations in the publish profile, as you did before you configured the **Package/Publish SQL** tab for migration to SQL Server.</span></span>

<span data-ttu-id="d46b8-132">Aprire il *Web.Test.config* file e rimuovere il `connectionStrings` elemento.</span><span class="sxs-lookup"><span data-stu-id="d46b8-132">Open the *Web.Test.config* file and remove the `connectionStrings` element.</span></span> <span data-ttu-id="d46b8-133">La trasformazione rimanente sola nel *Web.Test.config* file è per il `Environment` valore il `appSettings` elemento.</span><span class="sxs-lookup"><span data-stu-id="d46b8-133">The only remaining transformation in the *Web.Test.config* file is for the `Environment` value in the `appSettings` element.</span></span>

<span data-ttu-id="d46b8-134">È ora possibile aggiornare il profilo di pubblicazione e la pubblicazione nell'ambiente di testing.</span><span class="sxs-lookup"><span data-stu-id="d46b8-134">Now you can update the publish profile and publish to the test environment.</span></span>

<span data-ttu-id="d46b8-135">Aprire il **pubblica sul Web** procedura guidata, quindi passi al **profilo** scheda.</span><span class="sxs-lookup"><span data-stu-id="d46b8-135">Open the **Publish Web** wizard, and then switch to the **Profile** tab.</span></span>

<span data-ttu-id="d46b8-136">Selezionare il **Test** profilo di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="d46b8-136">Select the **Test** publish profile.</span></span>

<span data-ttu-id="d46b8-137">Selezionare il **impostazioni** scheda.</span><span class="sxs-lookup"><span data-stu-id="d46b8-137">Select the **Settings** tab.</span></span>

<span data-ttu-id="d46b8-138">Fare clic su **attivare il nuovo database di pubblicazione miglioramenti**.</span><span class="sxs-lookup"><span data-stu-id="d46b8-138">Click **enable the new database publishing improvements**.</span></span>

<span data-ttu-id="d46b8-139">Nella casella stringa di connessione per **SchoolContext**, immettere lo stesso valore utilizzato nel *Web.Test.config* file di trasformazione nell'esercitazione precedente:</span><span class="sxs-lookup"><span data-stu-id="d46b8-139">In the connection string box for **SchoolContext**, enter the same value that you used in the *Web.Test.config* transformation file in the previous tutorial:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample5.cmd)]

<span data-ttu-id="d46b8-140">Selezionare **eseguire migrazioni Code First (esecuzione all'avvio dell'applicazione)**.</span><span class="sxs-lookup"><span data-stu-id="d46b8-140">Select **Execute Code First Migrations (runs on application start)**.</span></span> <span data-ttu-id="d46b8-141">(Nella versione di Visual Studio, la casella di controllo potrebbe essere denominata **applicare le migrazioni Code First**.)</span><span class="sxs-lookup"><span data-stu-id="d46b8-141">(In your version of Visual Studio, the check box might be labeled **Apply Code First Migrations**.)</span></span>

<span data-ttu-id="d46b8-142">Nella casella stringa di connessione per **DefaultConnection**, immettere lo stesso valore utilizzato nel *Web.Test.config* file di trasformazione nell'esercitazione precedente:</span><span class="sxs-lookup"><span data-stu-id="d46b8-142">In the connection string box for **DefaultConnection**, enter the same value that you used in the *Web.Test.config* transformation file in the previous tutorial:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample6.cmd)]

<span data-ttu-id="d46b8-143">Lasciare **Aggiorna database** deselezionata.</span><span class="sxs-lookup"><span data-stu-id="d46b8-143">Leave **Update database** cleared.</span></span>

<span data-ttu-id="d46b8-144">Fare clic su **Pubblica**.</span><span class="sxs-lookup"><span data-stu-id="d46b8-144">Click **Publish**.</span></span>

<span data-ttu-id="d46b8-145">Visual Studio consente di distribuire le modifiche al codice nell'ambiente di testing e apre il browser alla home page di Contoso University.</span><span class="sxs-lookup"><span data-stu-id="d46b8-145">Visual Studio deploys the code changes to the test environment and opens the browser to the Contoso University home page.</span></span>

<span data-ttu-id="d46b8-146">Selezionare la pagina istruttori.</span><span class="sxs-lookup"><span data-stu-id="d46b8-146">Select the Instructors page.</span></span>

<span data-ttu-id="d46b8-147">Quando l'applicazione viene eseguita questa pagina, tenta di accedere al database.</span><span class="sxs-lookup"><span data-stu-id="d46b8-147">When the application runs this page, it tries to access the database.</span></span> <span data-ttu-id="d46b8-148">Migrazioni Code First controlla se il database corrente e rileva che la migrazione di più recente non è ancora stata applicata.</span><span class="sxs-lookup"><span data-stu-id="d46b8-148">Code First Migrations checks if the database is current, and finds that the latest migration has not been applied yet.</span></span> <span data-ttu-id="d46b8-149">Migrazioni Code First si applica la migrazione di più recente, viene eseguito il `Seed` (metodo), quindi selezionare la pagina viene eseguito normalmente.</span><span class="sxs-lookup"><span data-stu-id="d46b8-149">Code First Migrations applies the latest migration, runs the `Seed` method, and then the page runs normally.</span></span> <span data-ttu-id="d46b8-150">Visualizzare la nuova colonna di ore di Office con i dati di seeding.</span><span class="sxs-lookup"><span data-stu-id="d46b8-150">You see the new Office Hours column with the seeded data.</span></span>

<span data-ttu-id="d46b8-151">[![Instructors_page_with_OfficeHours_Test](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="d46b8-151">[![Instructors_page_with_OfficeHours_Test](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image3.png)</span></span>

## <a name="deploying-the-database-update-to-the-production-environment"></a><span data-ttu-id="d46b8-152">L'aggiornamento del Database di distribuzione nell'ambiente di produzione</span><span class="sxs-lookup"><span data-stu-id="d46b8-152">Deploying the Database Update to the Production Environment</span></span>

<span data-ttu-id="d46b8-153">È necessario modificare anche il profilo di pubblicazione per l'ambiente di produzione.</span><span class="sxs-lookup"><span data-stu-id="d46b8-153">You have to change the publish profile for the production environment also.</span></span> <span data-ttu-id="d46b8-154">In questo caso si sarà rimuovere un profilo esistente e crearne una nuova importando un file con estensione publishsettings aggiornato.</span><span class="sxs-lookup"><span data-stu-id="d46b8-154">In this case you'll remove the existing profile and create a new one by importing an updated .publishsettings file.</span></span> <span data-ttu-id="d46b8-155">Il file aggiornato includerà la stringa di connessione per il database di SQL Server nel Cytanium.</span><span class="sxs-lookup"><span data-stu-id="d46b8-155">The updated file will include the connection string for the SQL Server database at Cytanium.</span></span>

<span data-ttu-id="d46b8-156">Come specificato al momento della distribuzione nell'ambiente di testing, non è più necessario trasformazioni di stringa di connessione di *Web.Production.config* file di trasformazione.</span><span class="sxs-lookup"><span data-stu-id="d46b8-156">As you saw when you deployed to the test environment, you no longer need connection string transforms in the *Web.Production.config* transformation file.</span></span> <span data-ttu-id="d46b8-157">Apertura di file e rimuovere il `connectionStrings` elemento.</span><span class="sxs-lookup"><span data-stu-id="d46b8-157">Open that file and remove the `connectionStrings` element.</span></span> <span data-ttu-id="d46b8-158">Le trasformazioni rimanenti sono per la `Environment` valore il `appSettings` elemento e il `location` elemento che limita l'accesso ai report di errore Elmah.</span><span class="sxs-lookup"><span data-stu-id="d46b8-158">The remaining transformations are for the `Environment` value in the `appSettings` element and the `location` element that restricts access to Elmah error reports.</span></span>

<span data-ttu-id="d46b8-159">Prima di creare un nuovo profilo di pubblicazione per la produzione, scaricare un file con estensione publishsettings aggiornato allo stesso modo è stato fatto in precedenza il [distribuzione nell'ambiente di produzione](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) esercitazione.</span><span class="sxs-lookup"><span data-stu-id="d46b8-159">Before you create a new publish profile for production, download an updated .publishsettings file the same way you did earlier in the [Deploying to the Production Environment](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) tutorial.</span></span> <span data-ttu-id="d46b8-160">(Nel Pannello di controllo Cytanium, fare clic su **siti Web**, quindi fare clic su di **contosouniversity.com** sito Web.</span><span class="sxs-lookup"><span data-stu-id="d46b8-160">(In the Cytanium control panel, click **Web Sites**, and then click the **contosouniversity.com** website.</span></span> <span data-ttu-id="d46b8-161">Selezionare il **pubblicazione sul Web** scheda e quindi fare clic su **scaricare il profilo di pubblicazione per il sito web**.) Il motivo che questo tipo di procedura è prelevare la stringa di connessione di database nel file con estensione publishsettings.</span><span class="sxs-lookup"><span data-stu-id="d46b8-161">Select the **Web Publishing** tab, and then click **Download Publishing Profile for this web site**.) The reason you are doing this is to pick up the database connection string in the .publishsettings file.</span></span> <span data-ttu-id="d46b8-162">La stringa di connessione non è disponibile la prima volta che è stato scaricato il file, perché si sta ancora utilizzando SQL Server Compact e ancora non veniva creato il database di SQL Server in Cytanium.</span><span class="sxs-lookup"><span data-stu-id="d46b8-162">The connection string wasn't available the first time you downloaded the file, because you were still using SQL Server Compact and hadn't created the SQL Server database at Cytanium yet.</span></span>

<span data-ttu-id="d46b8-163">È ora possibile aggiornare il profilo di pubblicazione e la pubblicazione nell'ambiente di produzione.</span><span class="sxs-lookup"><span data-stu-id="d46b8-163">Now you can update the publish profile and publish to the production environment.</span></span>

<span data-ttu-id="d46b8-164">Aprire il **pubblica sul Web** procedura guidata, quindi passi al **profilo** scheda.</span><span class="sxs-lookup"><span data-stu-id="d46b8-164">Open the **Publish Web** wizard, and then switch to the **Profile** tab.</span></span>

<span data-ttu-id="d46b8-165">Fare clic su **Gestione profili**e quindi eliminare il profilo di produzione.</span><span class="sxs-lookup"><span data-stu-id="d46b8-165">Click **Manage Profiles**, and then delete the Production profile.</span></span>

<span data-ttu-id="d46b8-166">Chiudi il **pubblica sul Web** procedura guidata per salvare questa modifica.</span><span class="sxs-lookup"><span data-stu-id="d46b8-166">Close the **Publish Web** wizard to save this change.</span></span>

<span data-ttu-id="d46b8-167">Aprire il **pubblica sul Web** procedura guidata e quindi fare clic su **importazione**.</span><span class="sxs-lookup"><span data-stu-id="d46b8-167">Open the **Publish Web** wizard again, and then click **Import**.</span></span>

<span data-ttu-id="d46b8-168">Nel **connessione** modificare **URL di destinazione** sul valore appropriato se si utilizza un URL temporaneo.</span><span class="sxs-lookup"><span data-stu-id="d46b8-168">On the **Connection** tab, change **Destination URL** to the appropriate value if you are using a temporary URL.</span></span>

<span data-ttu-id="d46b8-169">Scegliere **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="d46b8-169">Click **Next**.</span></span>

<span data-ttu-id="d46b8-170">Nel **impostazioni** scheda, fare clic su **attivare il nuovo database di pubblicazione miglioramenti**.</span><span class="sxs-lookup"><span data-stu-id="d46b8-170">On the **Settings** tab, click **enable the new database publishing improvements**.</span></span>

<span data-ttu-id="d46b8-171">Nell'elenco di riepilogo a discesa stringhe di connessione per **SchoolContext**, selezionare la stringa di connessione Cytanium.</span><span class="sxs-lookup"><span data-stu-id="d46b8-171">In the connection string drop-down list for **SchoolContext**, select the Cytanium connection string.</span></span>

![Selecting_Cytanium_connection_string](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image5.png)

<span data-ttu-id="d46b8-173">Selezionare **migrazioni eseguire Code First (esecuzione all'avvio dell'applicazione)**.</span><span class="sxs-lookup"><span data-stu-id="d46b8-173">Select **Execute Code First migrations (runs on application start)**.</span></span>

<span data-ttu-id="d46b8-174">Nell'elenco di riepilogo a discesa stringhe di connessione per **DefaultConnection**, selezionare la stringa di connessione Cytanium.</span><span class="sxs-lookup"><span data-stu-id="d46b8-174">In the connection string drop-down list for **DefaultConnection**, select the Cytanium connection string.</span></span>

<span data-ttu-id="d46b8-175">Selezionare il **profilo** scheda, fare clic su **Gestione profili**e rinomina il profilo da "contosouniversity.com - distribuzione Web" a "Produzione".</span><span class="sxs-lookup"><span data-stu-id="d46b8-175">Select the **Profile** tab, click **Manage Profiles**, and rename the profile from "contosouniversity.com - Web Deploy" to "Production".</span></span>

<span data-ttu-id="d46b8-176">Chiudere il profilo di pubblicazione per salvare le modifiche, quindi aprirlo nuovamente.</span><span class="sxs-lookup"><span data-stu-id="d46b8-176">Close the publish profile to save the change, then open it again.</span></span>

<span data-ttu-id="d46b8-177">Fare clic su **Pubblica**.</span><span class="sxs-lookup"><span data-stu-id="d46b8-177">Click **Publish**.</span></span> <span data-ttu-id="d46b8-178">(Per un sito Web di produzione reali, è necessario copiare *app\_offline.htm* alla produzione e put nella cartella del progetto prima della pubblicazione, quindi rimuoverlo quando la distribuzione è stata completata.)</span><span class="sxs-lookup"><span data-stu-id="d46b8-178">(For a real production website, you would copy *app\_offline.htm* to production and put it in your project folder before publishing, then remove it when deployment is complete.)</span></span>

<span data-ttu-id="d46b8-179">Visual Studio consente di distribuire le modifiche al codice nell'ambiente di testing e apre il browser alla home page di Contoso University.</span><span class="sxs-lookup"><span data-stu-id="d46b8-179">Visual Studio deploys the code changes to the test environment and opens the browser to the Contoso University home page.</span></span>

<span data-ttu-id="d46b8-180">Selezionare la pagina istruttori.</span><span class="sxs-lookup"><span data-stu-id="d46b8-180">Select the Instructors page.</span></span>

<span data-ttu-id="d46b8-181">Migrazioni Code First aggiorna il database stesso modo nell'ambiente di Test.</span><span class="sxs-lookup"><span data-stu-id="d46b8-181">Code First Migrations updates the database the same way it did in the Test environment.</span></span> <span data-ttu-id="d46b8-182">Visualizzare la nuova colonna di ore di Office con i dati di seeding.</span><span class="sxs-lookup"><span data-stu-id="d46b8-182">You see the new Office Hours column with the seeded data.</span></span>

![Instructors_page_with_OfficeHours_Prod](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image6.png)

<span data-ttu-id="d46b8-184">Ora aver distribuito un aggiornamento di un'applicazione che modifica un database, incluso l'utilizzo di un database di SQL Server.</span><span class="sxs-lookup"><span data-stu-id="d46b8-184">You have now successfully deployed an application update that included a database change, using a SQL Server database.</span></span>

## <a name="more-information"></a><span data-ttu-id="d46b8-185">Altre informazioni</span><span class="sxs-lookup"><span data-stu-id="d46b8-185">More Information</span></span>

<span data-ttu-id="d46b8-186">In questo passaggio si completa questa serie di esercitazioni sulla distribuzione di un'applicazione web ASP.NET in un provider di hosting di terze parti.</span><span class="sxs-lookup"><span data-stu-id="d46b8-186">This completes this series of tutorials on deploying an ASP.NET web application to a third-party hosting provider.</span></span> <span data-ttu-id="d46b8-187">Per ulteriori informazioni sugli argomenti trattati in queste esercitazioni, vedere il [mappa del contenuto di distribuzione di ASP.NET](https://msdn.microsoft.com/library/bb386521(v=vs.110).aspx) nel sito web MSDN.</span><span class="sxs-lookup"><span data-stu-id="d46b8-187">For more information about any of the topics covered in these tutorials, see the [ASP.NET Deployment Content Map](https://msdn.microsoft.com/library/bb386521(v=vs.110).aspx) on the MSDN web site.</span></span>

## <a name="acknowledgements"></a><span data-ttu-id="d46b8-188">Riconoscimenti</span><span class="sxs-lookup"><span data-stu-id="d46b8-188">Acknowledgements</span></span>

<span data-ttu-id="d46b8-189">Come è possibile grazie seguenti persone che hanno apportato contributi significativi per il contenuto di questa serie di esercitazioni:</span><span class="sxs-lookup"><span data-stu-id="d46b8-189">I would like to thank the following people who made significant contributions to the content of this tutorial series:</span></span>

- [<span data-ttu-id="d46b8-190">Alberto Poblacion, MVP &amp; MCT, (Spagna)</span><span class="sxs-lookup"><span data-stu-id="d46b8-190">Alberto Poblacion, MVP &amp; MCT, Spain</span></span>](https://mvp.support.microsoft.com/profile/Alberto)
- <span data-ttu-id="d46b8-191">Stati Uniti Jarod Ferguson, MVP di sviluppo della piattaforma di dati,</span><span class="sxs-lookup"><span data-stu-id="d46b8-191">Jarod Ferguson, Data Platform Development MVP, United States</span></span>
- <span data-ttu-id="d46b8-192">Harsh Mittal, Microsoft</span><span class="sxs-lookup"><span data-stu-id="d46b8-192">Harsh Mittal, Microsoft</span></span>
- [<span data-ttu-id="d46b8-193">Kristina Olson, Microsoft</span><span class="sxs-lookup"><span data-stu-id="d46b8-193">Kristina Olson, Microsoft</span></span>](https://blogs.iis.net/krolson/default.aspx)
- [<span data-ttu-id="d46b8-194">Mike Pope, Microsoft</span><span class="sxs-lookup"><span data-stu-id="d46b8-194">Mike Pope, Microsoft</span></span>](http://www.mikepope.com/blog/DisplayBlog.aspx)
- <span data-ttu-id="d46b8-195">Mohit Srivastava, Microsoft</span><span class="sxs-lookup"><span data-stu-id="d46b8-195">Mohit Srivastava, Microsoft</span></span>
- [<span data-ttu-id="d46b8-196">Raffaele Rialdi (Italia)</span><span class="sxs-lookup"><span data-stu-id="d46b8-196">Raffaele Rialdi, Italy</span></span>](http://www.iamraf.net/)
- [<span data-ttu-id="d46b8-197">Rick Anderson, Microsoft</span><span class="sxs-lookup"><span data-stu-id="d46b8-197">Rick Anderson, Microsoft</span></span>](https://blogs.msdn.com/b/rickandy/)
- <span data-ttu-id="d46b8-198">[Microsoft, Hashimi sayed](http://sedodream.com/default.aspx)(twitter: [ @sayedihashimi ](http://twitter.com/sayedihashimi))</span><span class="sxs-lookup"><span data-stu-id="d46b8-198">[Sayed Hashimi, Microsoft](http://sedodream.com/default.aspx)(twitter: [@sayedihashimi](http://twitter.com/sayedihashimi))</span></span>
- <span data-ttu-id="d46b8-199">[Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](http://twitter.com/shanselman))</span><span class="sxs-lookup"><span data-stu-id="d46b8-199">[Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [@shanselman](http://twitter.com/shanselman))</span></span>
- <span data-ttu-id="d46b8-200">[Microsoft, Scott Hunter](https://blogs.msdn.com/b/scothu/) (twitter: [ @coolcsh ](http://twitter.com/coolcsh))</span><span class="sxs-lookup"><span data-stu-id="d46b8-200">[Scott Hunter, Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [@coolcsh](http://twitter.com/coolcsh))</span></span>
- [<span data-ttu-id="d46b8-201">Srđan Božović, Serbia</span><span class="sxs-lookup"><span data-stu-id="d46b8-201">Srđan Božović, Serbia</span></span>](http://msforge.net/blogs/zmajcek/)
- <span data-ttu-id="d46b8-202">[Microsoft, Vishal Joshi](http://vishaljoshi.blogspot.com/) (twitter: [ @vishalrjoshi ](http://twitter.com/vishalrjoshi))</span><span class="sxs-lookup"><span data-stu-id="d46b8-202">[Vishal Joshi, Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [@vishalrjoshi](http://twitter.com/vishalrjoshi))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d46b8-203">[Precedente](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
> [Successivo](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)</span><span class="sxs-lookup"><span data-stu-id="d46b8-203">[Previous](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
[Next](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)</span></span>
