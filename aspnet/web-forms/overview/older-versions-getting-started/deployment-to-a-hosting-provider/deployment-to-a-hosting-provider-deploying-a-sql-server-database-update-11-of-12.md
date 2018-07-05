---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
title: "Distribuzione di un'applicazione Web ASP.NET con SQL Server Compact tramite Visual Studio o Visual Web Developer: distribuzione di un aggiornamento di SQL Server Database - 11 su 12 | Microsoft Docs"
author: tdykstra
description: Questa serie di esercitazioni illustra come distribuire un ASP.NET (pubblicazione) progetto di applicazione web che include un database di SQL Server Compact tramite Visual s...
ms.author: aspnetcontent
ms.date: 11/17/2011
ms.assetid: 5e2bb092-cb22-4511-ad0a-22ae12dd99b3
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
msc.type: authoredcontent
ms.openlocfilehash: c744bc54c8ce12820d1e1388ac7ab2db814ff031
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37816631"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-sql-server-database-update---11-of-12"></a><span data-ttu-id="a7dec-103">Distribuzione di un'applicazione Web ASP.NET con SQL Server Compact tramite Visual Studio o Visual Web Developer: distribuzione di un aggiornamento di SQL Server Database - 11 su 12</span><span class="sxs-lookup"><span data-stu-id="a7dec-103">Deploying an ASP.NET Web Application with SQL Server Compact using Visual Studio or Visual Web Developer: Deploying a SQL Server Database Update - 11 of 12</span></span>
====================
<span data-ttu-id="a7dec-104">da [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="a7dec-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="a7dec-105">Download progetto iniziale</span><span class="sxs-lookup"><span data-stu-id="a7dec-105">Download Starter Project</span></span>](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> <span data-ttu-id="a7dec-106">Questa serie di esercitazioni illustra come distribuire un ASP.NET (pubblicazione) progetto di applicazione web che include un database di SQL Server Compact tramite Visual Studio 2012 RC o Visual Studio Express 2012 RC per Web.</span><span class="sxs-lookup"><span data-stu-id="a7dec-106">This series of tutorials shows you how to deploy (publish) an ASP.NET web application project that includes a SQL Server Compact database by using Visual Studio 2012 RC or Visual Studio Express 2012 RC for Web.</span></span> <span data-ttu-id="a7dec-107">Se si installa l'aggiornamento della pubblicazione sul Web, è anche possibile usare Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="a7dec-107">You can also use Visual Studio 2010 if you install the Web Publish Update.</span></span> <span data-ttu-id="a7dec-108">Per un'introduzione alla serie, vedere [la prima esercitazione della serie](deployment-to-a-hosting-provider-introduction-1-of-12.md).</span><span class="sxs-lookup"><span data-stu-id="a7dec-108">For an introduction to the series, see [the first tutorial in the series](deployment-to-a-hosting-provider-introduction-1-of-12.md).</span></span>
> 
> <span data-ttu-id="a7dec-109">Per un'esercitazione che illustra le funzionalità di distribuzione introdotte dopo la versione di Visual Studio 2012 RC, illustra come distribuire le edizioni di SQL Server diverse da SQL Server Compact e illustra come distribuire siti Web di Windows Azure, vedere [distribuzione Web ASP.NET con Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).</span><span class="sxs-lookup"><span data-stu-id="a7dec-109">For a tutorial that shows deployment features introduced after the RC release of Visual Studio 2012, shows how to deploy SQL Server editions other than SQL Server Compact, and shows how to deploy to Windows Azure Web Sites, see [ASP.NET Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="a7dec-110">Panoramica</span><span class="sxs-lookup"><span data-stu-id="a7dec-110">Overview</span></span>

<span data-ttu-id="a7dec-111">Questa esercitazione illustra come distribuire un aggiornamento del database in un database di SQL Server completo.</span><span class="sxs-lookup"><span data-stu-id="a7dec-111">This tutorial shows you how to deploy a database update to a full SQL Server database.</span></span> <span data-ttu-id="a7dec-112">Poiché le migrazioni Code First esegue tutte le operazioni di aggiornamento del database, il processo è quasi identico a quella usata per SQL Server Compact nel [distribuzione di un aggiornamento del Database](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md) esercitazione.</span><span class="sxs-lookup"><span data-stu-id="a7dec-112">Because Code First Migrations does all the work of updating the database, the process is almost identical to what you did for SQL Server Compact in the [Deploying a Database Update](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md) tutorial.</span></span>

<span data-ttu-id="a7dec-113">Promemoria: Se viene visualizzato un messaggio di errore o qualcosa non funziona durante l'esecuzione dell'esercitazione, assicurarsi di controllare la [risoluzione dei problemi pagina](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).</span><span class="sxs-lookup"><span data-stu-id="a7dec-113">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).</span></span>

## <a name="adding-a-new-column-to-a-table"></a><span data-ttu-id="a7dec-114">Aggiunta di una nuova colonna a una tabella</span><span class="sxs-lookup"><span data-stu-id="a7dec-114">Adding a New Column to a Table</span></span>

<span data-ttu-id="a7dec-115">In questa sezione dell'esercitazione si apporteranno corrispondenti modifiche al codice, quindi eseguirne il test in Visual Studio in preparazione per la loro distribuzione agli ambienti di test e produzione e un database di modifica.</span><span class="sxs-lookup"><span data-stu-id="a7dec-115">In this section of the tutorial you'll make a database change and corresponding code changes, then test them in Visual Studio in preparation for deploying them to the test and production environments.</span></span> <span data-ttu-id="a7dec-116">La modifica comporta l'aggiunta di un `OfficeHours` colonna per il `Instructor` entità e le nuove informazioni nella visualizzazione di **instructors (insegnanti)** pagina web.</span><span class="sxs-lookup"><span data-stu-id="a7dec-116">The change involves adding an `OfficeHours` column to the `Instructor` entity and displaying the new information in the **Instructors** web page.</span></span>

<span data-ttu-id="a7dec-117">Nel progetto ContosoUniversity.DAL, aprire *Instructor.cs* e aggiungere la proprietà seguente tra i `HireDate` e `Courses` proprietà:</span><span class="sxs-lookup"><span data-stu-id="a7dec-117">In the ContosoUniversity.DAL project, open *Instructor.cs* and add the following property between the `HireDate` and `Courses` properties:</span></span>

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample1.cs)]

<span data-ttu-id="a7dec-118">Aggiornare la classe inizializzatore in modo che esegue il seeding la nuova colonna con dati di test.</span><span class="sxs-lookup"><span data-stu-id="a7dec-118">Update the initializer class so that it seeds the new column with test data.</span></span> <span data-ttu-id="a7dec-119">Aprire *Migrations\Configuration.cs* e sostituire il blocco di codice che inizia `var instructors = new List<Instructor>` con blocco di codice seguente che include la nuova colonna:</span><span class="sxs-lookup"><span data-stu-id="a7dec-119">Open *Migrations\Configuration.cs* and replace the code block that begins `var instructors = new List<Instructor>` with the following code block which includes the new column:</span></span>

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample2.cs)]

<span data-ttu-id="a7dec-120">Nel progetto ContosoUniversity, aprire *Instructors.aspx* e aggiungere un nuovo campo modello per le ore di office subito prima del tag `</Columns>` tag nel primo `GridView` controllo:</span><span class="sxs-lookup"><span data-stu-id="a7dec-120">In the ContosoUniversity project, open *Instructors.aspx* and add a new template field for office hours just before the closing `</Columns>` tag in the first `GridView` control:</span></span>

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample3.aspx)]

<span data-ttu-id="a7dec-121">Compilare la soluzione.</span><span class="sxs-lookup"><span data-stu-id="a7dec-121">Build the solution.</span></span>

<span data-ttu-id="a7dec-122">Aprire il **Console di gestione pacchetti** finestra e ContosoUniversity.DAL selezionare come il **progetto predefinito**.</span><span class="sxs-lookup"><span data-stu-id="a7dec-122">Open the **Package Manager Console** window, and select ContosoUniversity.DAL as the **Default project**.</span></span>

<span data-ttu-id="a7dec-123">Immettere i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="a7dec-123">Enter the following commands:</span></span>

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample4.ps1)]

<span data-ttu-id="a7dec-124">Eseguire l'applicazione e selezionare il **instructors (insegnanti)** pagina.</span><span class="sxs-lookup"><span data-stu-id="a7dec-124">Run the application and select the **Instructors** page.</span></span> <span data-ttu-id="a7dec-125">La pagina richiede un po' più tempo del solito caricare, perché Entity Framework ricrea il database ed esegue il seeding con dati di test.</span><span class="sxs-lookup"><span data-stu-id="a7dec-125">The page takes a little longer than usual to load, because the Entity Framework re-creates the database and seeds it with test data.</span></span>

<span data-ttu-id="a7dec-126">[![Instructors_page_with_office_hours](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="a7dec-126">[![Instructors_page_with_office_hours](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image1.png)</span></span>

## <a name="deploying-the-database-update-to-the-test-environment"></a><span data-ttu-id="a7dec-127">Distribuzione di aggiornamento del Database nell'ambiente di Test</span><span class="sxs-lookup"><span data-stu-id="a7dec-127">Deploying the Database Update to the Test Environment</span></span>

<span data-ttu-id="a7dec-128">Quando si usa migrazioni Code First, il metodo per la distribuzione di una modifica al database di SQL Server è identico a quello di SQL Server Compact.</span><span class="sxs-lookup"><span data-stu-id="a7dec-128">When you use Code First Migrations, the method for deploying a database change to SQL Server is the same as for SQL Server Compact.</span></span> <span data-ttu-id="a7dec-129">Tuttavia, è necessario modificare il Test profilo di pubblicazione è ancora stato configurato per eseguire la migrazione da SQL Server Compact a SQL Server.</span><span class="sxs-lookup"><span data-stu-id="a7dec-129">However, you have to change the Test publish profile because it is still set up to migrate from SQL Server Compact to SQL Server.</span></span>

<span data-ttu-id="a7dec-130">Il primo passaggio consiste nel rimuovere le trasformazioni di stringa di connessione che è stato creato nell'esercitazione precedente.</span><span class="sxs-lookup"><span data-stu-id="a7dec-130">The first step is to remove the connection string transformations that you created in the previous tutorial.</span></span> <span data-ttu-id="a7dec-131">Questi non sono più necessarie poiché non è possibile specificare le trasformazioni di stringa di connessione nel profilo di pubblicazione, come accadeva prima della configurazione di **pubblicazione/creazione pacchetto SQL** scheda per la migrazione a SQL Server.</span><span class="sxs-lookup"><span data-stu-id="a7dec-131">These are no longer needed because you'll specify connection string transformations in the publish profile, as you did before you configured the **Package/Publish SQL** tab for migration to SQL Server.</span></span>

<span data-ttu-id="a7dec-132">Aprire il *Web.Test.config* del file e rimuovere il `connectionStrings` elemento.</span><span class="sxs-lookup"><span data-stu-id="a7dec-132">Open the *Web.Test.config* file and remove the `connectionStrings` element.</span></span> <span data-ttu-id="a7dec-133">L'unica trasformazione rimanente nel *Web.Test.config* file è per il `Environment` valore nel `appSettings` elemento.</span><span class="sxs-lookup"><span data-stu-id="a7dec-133">The only remaining transformation in the *Web.Test.config* file is for the `Environment` value in the `appSettings` element.</span></span>

<span data-ttu-id="a7dec-134">A questo punto è possibile aggiornare il profilo di pubblicazione e pubblicazione nell'ambiente di test.</span><span class="sxs-lookup"><span data-stu-id="a7dec-134">Now you can update the publish profile and publish to the test environment.</span></span>

<span data-ttu-id="a7dec-135">Aprire il **pubblica sul Web** procedura guidata e quindi passare al **profilo** scheda.</span><span class="sxs-lookup"><span data-stu-id="a7dec-135">Open the **Publish Web** wizard, and then switch to the **Profile** tab.</span></span>

<span data-ttu-id="a7dec-136">Selezionare il **Test** profilo di pubblicazione.</span><span class="sxs-lookup"><span data-stu-id="a7dec-136">Select the **Test** publish profile.</span></span>

<span data-ttu-id="a7dec-137">Selezionare il **impostazioni** scheda.</span><span class="sxs-lookup"><span data-stu-id="a7dec-137">Select the **Settings** tab.</span></span>

<span data-ttu-id="a7dec-138">Fare clic su **Abilita il nuovo database di pubblicazione miglioramenti**.</span><span class="sxs-lookup"><span data-stu-id="a7dec-138">Click **enable the new database publishing improvements**.</span></span>

<span data-ttu-id="a7dec-139">Nella casella stringa di connessione per **SchoolContext**, immettere lo stesso valore usato nel *Web.Test.config* file trasformazione nell'esercitazione precedente:</span><span class="sxs-lookup"><span data-stu-id="a7dec-139">In the connection string box for **SchoolContext**, enter the same value that you used in the *Web.Test.config* transformation file in the previous tutorial:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample5.cmd)]

<span data-ttu-id="a7dec-140">Selezionare **Esegui migrazioni Code First (inizia all'avvio dell'applicazione)**.</span><span class="sxs-lookup"><span data-stu-id="a7dec-140">Select **Execute Code First Migrations (runs on application start)**.</span></span> <span data-ttu-id="a7dec-141">(Nella versione di Visual Studio, la casella di controllo potrebbe essere etichettata **applicare le migrazioni Code First**.)</span><span class="sxs-lookup"><span data-stu-id="a7dec-141">(In your version of Visual Studio, the check box might be labeled **Apply Code First Migrations**.)</span></span>

<span data-ttu-id="a7dec-142">Nella casella stringa di connessione per **DefaultConnection**, immettere lo stesso valore usato nel *Web.Test.config* file trasformazione nell'esercitazione precedente:</span><span class="sxs-lookup"><span data-stu-id="a7dec-142">In the connection string box for **DefaultConnection**, enter the same value that you used in the *Web.Test.config* transformation file in the previous tutorial:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample6.cmd)]

<span data-ttu-id="a7dec-143">Lasciare **aggiornare il database** deselezionata.</span><span class="sxs-lookup"><span data-stu-id="a7dec-143">Leave **Update database** cleared.</span></span>

<span data-ttu-id="a7dec-144">Fare clic su **Pubblica**.</span><span class="sxs-lookup"><span data-stu-id="a7dec-144">Click **Publish**.</span></span>

<span data-ttu-id="a7dec-145">Visual Studio consente di distribuire le modifiche al codice nell'ambiente di test e apre il browser alla home page di Contoso University.</span><span class="sxs-lookup"><span data-stu-id="a7dec-145">Visual Studio deploys the code changes to the test environment and opens the browser to the Contoso University home page.</span></span>

<span data-ttu-id="a7dec-146">Selezionare la pagina instructors (insegnanti).</span><span class="sxs-lookup"><span data-stu-id="a7dec-146">Select the Instructors page.</span></span>

<span data-ttu-id="a7dec-147">Quando l'applicazione viene eseguita questa pagina, tenta di accedere al database.</span><span class="sxs-lookup"><span data-stu-id="a7dec-147">When the application runs this page, it tries to access the database.</span></span> <span data-ttu-id="a7dec-148">Migrazioni Code First controlla se il database corrente e rileva che la migrazione di più recente non è ancora stata applicata.</span><span class="sxs-lookup"><span data-stu-id="a7dec-148">Code First Migrations checks if the database is current, and finds that the latest migration has not been applied yet.</span></span> <span data-ttu-id="a7dec-149">Migrazioni Code First si applica la migrazione di più recente, viene eseguito il `Seed` (metodo) e quindi la pagina viene eseguito normalmente.</span><span class="sxs-lookup"><span data-stu-id="a7dec-149">Code First Migrations applies the latest migration, runs the `Seed` method, and then the page runs normally.</span></span> <span data-ttu-id="a7dec-150">Visualizzare la nuova colonna orario di ufficio con dati di seeding.</span><span class="sxs-lookup"><span data-stu-id="a7dec-150">You see the new Office Hours column with the seeded data.</span></span>

<span data-ttu-id="a7dec-151">[![Instructors_page_with_OfficeHours_Test](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="a7dec-151">[![Instructors_page_with_OfficeHours_Test](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image3.png)</span></span>

## <a name="deploying-the-database-update-to-the-production-environment"></a><span data-ttu-id="a7dec-152">Distribuzione di aggiornamento del Database nell'ambiente di produzione</span><span class="sxs-lookup"><span data-stu-id="a7dec-152">Deploying the Database Update to the Production Environment</span></span>

<span data-ttu-id="a7dec-153">È necessario modificare anche il profilo di pubblicazione per l'ambiente di produzione.</span><span class="sxs-lookup"><span data-stu-id="a7dec-153">You have to change the publish profile for the production environment also.</span></span> <span data-ttu-id="a7dec-154">In questo caso si sarà rimuovere il profilo esistente e crearne una nuova importando un file con estensione publishsettings aggiornato.</span><span class="sxs-lookup"><span data-stu-id="a7dec-154">In this case you'll remove the existing profile and create a new one by importing an updated .publishsettings file.</span></span> <span data-ttu-id="a7dec-155">Il file aggiornato includerà la stringa di connessione per il database di SQL Server nel Cytanium.</span><span class="sxs-lookup"><span data-stu-id="a7dec-155">The updated file will include the connection string for the SQL Server database at Cytanium.</span></span>

<span data-ttu-id="a7dec-156">Come si è visto durante la distribuzione nell'ambiente di test, le trasformazioni di stringa di connessione in non sono più necessari i *Web.Production.config* file di trasformazione.</span><span class="sxs-lookup"><span data-stu-id="a7dec-156">As you saw when you deployed to the test environment, you no longer need connection string transforms in the *Web.Production.config* transformation file.</span></span> <span data-ttu-id="a7dec-157">Apertura di file e rimuovere il `connectionStrings` elemento.</span><span class="sxs-lookup"><span data-stu-id="a7dec-157">Open that file and remove the `connectionStrings` element.</span></span> <span data-ttu-id="a7dec-158">Le trasformazioni rimanenti sono per la `Environment` valore il `appSettings` elemento e il `location` elemento che limita l'accesso ai report di errore Elmah.</span><span class="sxs-lookup"><span data-stu-id="a7dec-158">The remaining transformations are for the `Environment` value in the `appSettings` element and the `location` element that restricts access to Elmah error reports.</span></span>

<span data-ttu-id="a7dec-159">Prima di creare un nuovo profilo di pubblicazione per la produzione, scaricare un file con estensione publishsettings aggiornato allo stesso modo è stato fatto in precedenza nella [distribuzione nell'ambiente di produzione](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) esercitazione.</span><span class="sxs-lookup"><span data-stu-id="a7dec-159">Before you create a new publish profile for production, download an updated .publishsettings file the same way you did earlier in the [Deploying to the Production Environment](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) tutorial.</span></span> <span data-ttu-id="a7dec-160">(Nel Pannello di controllo Cytanium, fare clic su **siti Web**, quindi fare clic sui **contosouniversity.com** sito Web.</span><span class="sxs-lookup"><span data-stu-id="a7dec-160">(In the Cytanium control panel, click **Web Sites**, and then click the **contosouniversity.com** website.</span></span> <span data-ttu-id="a7dec-161">Selezionare il **pubblicazione sul Web** scheda e quindi fare clic su **Download Publishing Profile per il sito web**.) Il motivo che si esegue il processo consiste nello scegliere la stringa di connessione di database nel file con estensione publishsettings.</span><span class="sxs-lookup"><span data-stu-id="a7dec-161">Select the **Web Publishing** tab, and then click **Download Publishing Profile for this web site**.) The reason you are doing this is to pick up the database connection string in the .publishsettings file.</span></span> <span data-ttu-id="a7dec-162">La stringa di connessione non era disponibile la prima volta che hai scaricato il file, poiché si sta ancora utilizzando SQL Server Compact e ancora non veniva creato il database di SQL Server in Cytanium.</span><span class="sxs-lookup"><span data-stu-id="a7dec-162">The connection string wasn't available the first time you downloaded the file, because you were still using SQL Server Compact and hadn't created the SQL Server database at Cytanium yet.</span></span>

<span data-ttu-id="a7dec-163">A questo punto è possibile aggiornare il profilo di pubblicazione e pubblicazione nell'ambiente di produzione.</span><span class="sxs-lookup"><span data-stu-id="a7dec-163">Now you can update the publish profile and publish to the production environment.</span></span>

<span data-ttu-id="a7dec-164">Aprire il **pubblica sul Web** procedura guidata e quindi passare al **profilo** scheda.</span><span class="sxs-lookup"><span data-stu-id="a7dec-164">Open the **Publish Web** wizard, and then switch to the **Profile** tab.</span></span>

<span data-ttu-id="a7dec-165">Fare clic su **gestire i profili**e quindi eliminare il profilo di produzione.</span><span class="sxs-lookup"><span data-stu-id="a7dec-165">Click **Manage Profiles**, and then delete the Production profile.</span></span>

<span data-ttu-id="a7dec-166">Chiudi il **pubblica sul Web** guidata consente di salvare questa modifica.</span><span class="sxs-lookup"><span data-stu-id="a7dec-166">Close the **Publish Web** wizard to save this change.</span></span>

<span data-ttu-id="a7dec-167">Aprire il **pubblica sul Web** Creazione guidata nuovo e quindi fare clic su **importazione**.</span><span class="sxs-lookup"><span data-stu-id="a7dec-167">Open the **Publish Web** wizard again, and then click **Import**.</span></span>

<span data-ttu-id="a7dec-168">Nel **Connection** scheda, modificare **URL di destinazione** sul valore appropriato se si usa un URL temporaneo.</span><span class="sxs-lookup"><span data-stu-id="a7dec-168">On the **Connection** tab, change **Destination URL** to the appropriate value if you are using a temporary URL.</span></span>

<span data-ttu-id="a7dec-169">Scegliere **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="a7dec-169">Click **Next**.</span></span>

<span data-ttu-id="a7dec-170">Nel **le impostazioni** scheda, fare clic su **Abilita il nuovo database di pubblicazione miglioramenti**.</span><span class="sxs-lookup"><span data-stu-id="a7dec-170">On the **Settings** tab, click **enable the new database publishing improvements**.</span></span>

<span data-ttu-id="a7dec-171">Nell'elenco di riepilogo a discesa stringa di connessione per **SchoolContext**, selezionare la stringa di connessione Cytanium.</span><span class="sxs-lookup"><span data-stu-id="a7dec-171">In the connection string drop-down list for **SchoolContext**, select the Cytanium connection string.</span></span>

![Selecting_Cytanium_connection_string](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image5.png)

<span data-ttu-id="a7dec-173">Selezionare **migrazioni Code First Execute (inizia all'avvio dell'applicazione)**.</span><span class="sxs-lookup"><span data-stu-id="a7dec-173">Select **Execute Code First migrations (runs on application start)**.</span></span>

<span data-ttu-id="a7dec-174">Nell'elenco di riepilogo a discesa stringa di connessione per **DefaultConnection**, selezionare la stringa di connessione Cytanium.</span><span class="sxs-lookup"><span data-stu-id="a7dec-174">In the connection string drop-down list for **DefaultConnection**, select the Cytanium connection string.</span></span>

<span data-ttu-id="a7dec-175">Selezionare il **profilo** scheda, fare clic su **Gestione profili**e rinomina il profilo da "contosouniversity.com - distribuzione Web" a "Production".</span><span class="sxs-lookup"><span data-stu-id="a7dec-175">Select the **Profile** tab, click **Manage Profiles**, and rename the profile from "contosouniversity.com - Web Deploy" to "Production".</span></span>

<span data-ttu-id="a7dec-176">Chiudere il profilo di pubblicazione per salvare le modifiche, quindi aprirlo nuovamente.</span><span class="sxs-lookup"><span data-stu-id="a7dec-176">Close the publish profile to save the change, then open it again.</span></span>

<span data-ttu-id="a7dec-177">Fare clic su **Pubblica**.</span><span class="sxs-lookup"><span data-stu-id="a7dec-177">Click **Publish**.</span></span> <span data-ttu-id="a7dec-178">(Per siti Web di produzione reali, copiare *app\_offline.htm* alla produzione e put che nella cartella del progetto prima della pubblicazione, quindi di rimuovere una volta completata la distribuzione.)</span><span class="sxs-lookup"><span data-stu-id="a7dec-178">(For a real production website, you would copy *app\_offline.htm* to production and put it in your project folder before publishing, then remove it when deployment is complete.)</span></span>

<span data-ttu-id="a7dec-179">Visual Studio consente di distribuire le modifiche al codice nell'ambiente di test e apre il browser alla home page di Contoso University.</span><span class="sxs-lookup"><span data-stu-id="a7dec-179">Visual Studio deploys the code changes to the test environment and opens the browser to the Contoso University home page.</span></span>

<span data-ttu-id="a7dec-180">Selezionare la pagina instructors (insegnanti).</span><span class="sxs-lookup"><span data-stu-id="a7dec-180">Select the Instructors page.</span></span>

<span data-ttu-id="a7dec-181">Migrazioni Code First aggiorna il database allo stesso modo di che quanto accadeva in ambiente di Test.</span><span class="sxs-lookup"><span data-stu-id="a7dec-181">Code First Migrations updates the database the same way it did in the Test environment.</span></span> <span data-ttu-id="a7dec-182">Visualizzare la nuova colonna orario di ufficio con dati di seeding.</span><span class="sxs-lookup"><span data-stu-id="a7dec-182">You see the new Office Hours column with the seeded data.</span></span>

![Instructors_page_with_OfficeHours_Prod](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image6.png)

<span data-ttu-id="a7dec-184">Ora aver distribuito un aggiornamento dell'applicazione che include una modifica di database, usando un database di SQL Server.</span><span class="sxs-lookup"><span data-stu-id="a7dec-184">You have now successfully deployed an application update that included a database change, using a SQL Server database.</span></span>

## <a name="more-information"></a><span data-ttu-id="a7dec-185">Altre informazioni</span><span class="sxs-lookup"><span data-stu-id="a7dec-185">More Information</span></span>

<span data-ttu-id="a7dec-186">In questo passaggio si completa questa serie di esercitazioni sulla distribuzione di un'applicazione web ASP.NET in un provider di hosting di terze parti.</span><span class="sxs-lookup"><span data-stu-id="a7dec-186">This completes this series of tutorials on deploying an ASP.NET web application to a third-party hosting provider.</span></span> <span data-ttu-id="a7dec-187">Per altre informazioni su uno qualsiasi degli argomenti trattati in queste esercitazioni, vedere la [mappa del contenuto ASP.NET distribuzione](https://msdn.microsoft.com/library/bb386521(v=vs.110).aspx) nel sito web MSDN.</span><span class="sxs-lookup"><span data-stu-id="a7dec-187">For more information about any of the topics covered in these tutorials, see the [ASP.NET Deployment Content Map](https://msdn.microsoft.com/library/bb386521(v=vs.110).aspx) on the MSDN web site.</span></span>

## <a name="acknowledgements"></a><span data-ttu-id="a7dec-188">Riconoscimenti</span><span class="sxs-lookup"><span data-stu-id="a7dec-188">Acknowledgements</span></span>

<span data-ttu-id="a7dec-189">Vorrei ringraziare i seguenti persone che hanno apportato contributi significativi per il contenuto di questa serie di esercitazioni:</span><span class="sxs-lookup"><span data-stu-id="a7dec-189">I would like to thank the following people who made significant contributions to the content of this tutorial series:</span></span>

- [<span data-ttu-id="a7dec-190">Alberto Poblacion, MVP &amp; MCT, Spagna</span><span class="sxs-lookup"><span data-stu-id="a7dec-190">Alberto Poblacion, MVP &amp; MCT, Spain</span></span>](https://mvp.support.microsoft.com/profile/Alberto)
- <span data-ttu-id="a7dec-191">Jarod Ferguson, sviluppo della piattaforma dati MVP, Stati Uniti</span><span class="sxs-lookup"><span data-stu-id="a7dec-191">Jarod Ferguson, Data Platform Development MVP, United States</span></span>
- <span data-ttu-id="a7dec-192">Harsh Mittal, Microsoft</span><span class="sxs-lookup"><span data-stu-id="a7dec-192">Harsh Mittal, Microsoft</span></span>
- [<span data-ttu-id="a7dec-193">Kristina Olson, Microsoft</span><span class="sxs-lookup"><span data-stu-id="a7dec-193">Kristina Olson, Microsoft</span></span>](https://blogs.iis.net/krolson/default.aspx)
- [<span data-ttu-id="a7dec-194">Mike Pope, Microsoft</span><span class="sxs-lookup"><span data-stu-id="a7dec-194">Mike Pope, Microsoft</span></span>](http://www.mikepope.com/blog/DisplayBlog.aspx)
- <span data-ttu-id="a7dec-195">Mohit Srivastava, Microsoft</span><span class="sxs-lookup"><span data-stu-id="a7dec-195">Mohit Srivastava, Microsoft</span></span>
- [<span data-ttu-id="a7dec-196">Raffaele Rialdi (Italia)</span><span class="sxs-lookup"><span data-stu-id="a7dec-196">Raffaele Rialdi, Italy</span></span>](http://www.iamraf.net/)
- [<span data-ttu-id="a7dec-197">Rick Anderson, Microsoft</span><span class="sxs-lookup"><span data-stu-id="a7dec-197">Rick Anderson, Microsoft</span></span>](https://blogs.msdn.com/b/rickandy/)
- <span data-ttu-id="a7dec-198">[Microsoft, sayed Hashimi](http://sedodream.com/default.aspx)(twitter: [ @sayedihashimi ](http://twitter.com/sayedihashimi))</span><span class="sxs-lookup"><span data-stu-id="a7dec-198">[Sayed Hashimi, Microsoft](http://sedodream.com/default.aspx)(twitter: [@sayedihashimi](http://twitter.com/sayedihashimi))</span></span>
- <span data-ttu-id="a7dec-199">[Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](http://twitter.com/shanselman))</span><span class="sxs-lookup"><span data-stu-id="a7dec-199">[Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [@shanselman](http://twitter.com/shanselman))</span></span>
- <span data-ttu-id="a7dec-200">[Microsoft, Scott Hunter](https://blogs.msdn.com/b/scothu/) (twitter: [ @coolcsh ](http://twitter.com/coolcsh))</span><span class="sxs-lookup"><span data-stu-id="a7dec-200">[Scott Hunter, Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [@coolcsh](http://twitter.com/coolcsh))</span></span>
- [<span data-ttu-id="a7dec-201">Srđan Božović, Serbia e Montenegro</span><span class="sxs-lookup"><span data-stu-id="a7dec-201">Srđan Božović, Serbia</span></span>](http://msforge.net/blogs/zmajcek/)
- <span data-ttu-id="a7dec-202">[Vishal Joshi, Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [ @vishalrjoshi ](http://twitter.com/vishalrjoshi))</span><span class="sxs-lookup"><span data-stu-id="a7dec-202">[Vishal Joshi, Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [@vishalrjoshi](http://twitter.com/vishalrjoshi))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a7dec-203">[Precedente](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
> [Successivo](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)</span><span class="sxs-lookup"><span data-stu-id="a7dec-203">[Previous](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
[Next](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)</span></span>
