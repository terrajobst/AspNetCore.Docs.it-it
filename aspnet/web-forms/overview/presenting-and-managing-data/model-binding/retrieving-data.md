---
uid: web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
title: Il recupero e visualizzazione dei dati con modello di associazione e web form | Microsoft Docs
author: Rick-Anderson
description: Questa serie di esercitazioni illustra aspetti di base dell'uso di associazione di modelli con un progetto di Web Form ASP.NET. Associazione di modelli consente l'interazione dei dati più linee rette-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 9f24fb82-c7ac-48da-b8e2-51b3da17e365
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
msc.type: authoredcontent
ms.openlocfilehash: c53c27f4852eab9813bd917315111e7cd3b04953
ms.sourcegitcommit: 184ba5b44d1c393076015510ac842b77bc9d4d93
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/18/2019
ms.locfileid: "54396285"
---
<a name="retrieving-and-displaying-data-with-model-binding-and-web-forms"></a><span data-ttu-id="bf0fb-104">Il recupero e visualizzazione dei dati con l'associazione di modelli e web form</span><span class="sxs-lookup"><span data-stu-id="bf0fb-104">Retrieving and displaying data with model binding and web forms</span></span>
====================

> <span data-ttu-id="bf0fb-105">Questa serie di esercitazioni illustra aspetti di base dell'uso di associazione di modelli con un progetto di Web Form ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-105">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="bf0fb-106">Associazione di modelli consente l'interazione dei dati più semplice rispetto a gestione dati di oggetti di origine (ad esempio ObjectDataSource o SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="bf0fb-106">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="bf0fb-107">Questa serie inizia con materiale introduttivo e sposta i concetti più avanzati nelle esercitazioni successive.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-107">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
>  <span data-ttu-id="bf0fb-108">Il modello di associazione del modello funziona con qualsiasi tecnologia di accesso ai dati.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-108">The model binding pattern works with any data access technology.</span></span> <span data-ttu-id="bf0fb-109">In questa esercitazione si userà Entity Framework, ma è possibile usare la tecnologia di accesso ai dati che è più nota all'utente.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-109">In this tutorial, you will use Entity Framework, but you could use the data access technology that is most familiar to you.</span></span> <span data-ttu-id="bf0fb-110">Da un controllo server associato a dati, ad esempio un controllo GridView, ListView, DetailsView e FormView, si specificano i nomi dei metodi da usare per la selezione, l'aggiornamento, eliminazione e creazione di dati.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-110">From a data-bound server control, such as a GridView, ListView, DetailsView, or FormView control, you specify the names of the methods to use for selecting, updating, deleting, and creating data.</span></span> <span data-ttu-id="bf0fb-111">In questa esercitazione, si specificherà un valore per la proprietà SelectMethod.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-111">In this tutorial, you will specify a value for the SelectMethod.</span></span> 
> 
> <span data-ttu-id="bf0fb-112">All'interno di tale metodo, per fornire la logica per recuperare i dati.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-112">Within that method, you provide the logic for retrieving the data.</span></span> <span data-ttu-id="bf0fb-113">Nella prossima esercitazione, si imposteranno i valori per UpdateMethod e InsertMethod e DeleteMethod.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-113">In the next tutorial, you will set values for UpdateMethod, DeleteMethod and InsertMethod.</span></span>
>
> <span data-ttu-id="bf0fb-114">È possibile [scaricare](https://go.microsoft.com/fwlink/?LinkId=286116) il progetto completo in C# o Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-114">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or Visual Basic.</span></span> <span data-ttu-id="bf0fb-115">Il codice scaricabile funziona con Visual Studio 2012 e versioni successive.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-115">The downloadable code works with Visual Studio 2012 and later.</span></span> <span data-ttu-id="bf0fb-116">Usa il modello di Visual Studio 2012, che è leggermente diverso rispetto al modello di Visual Studio 2017 illustrato in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-116">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2017 template shown in this tutorial.</span></span>
> 
> <span data-ttu-id="bf0fb-117">In questa esercitazione si esegue l'applicazione in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-117">In the tutorial you run the application in Visual Studio.</span></span> <span data-ttu-id="bf0fb-118">È anche possibile distribuire l'applicazione in un provider di hosting e renderlo disponibile tramite internet.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-118">You can also deploy the application to a hosting provider and make it available over the internet.</span></span> <span data-ttu-id="bf0fb-119">Microsoft offre l'hosting web gratuito per fino a 10 siti web in un</span><span class="sxs-lookup"><span data-stu-id="bf0fb-119">Microsoft offers free web hosting for up to 10 web sites in a</span></span>  
>  <span data-ttu-id="bf0fb-120">[account Azure gratuito](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).</span><span class="sxs-lookup"><span data-stu-id="bf0fb-120">[free Azure trial account](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).</span></span> <span data-ttu-id="bf0fb-121">Per informazioni su come distribuire un progetto web Visual Studio per App Web di servizio App di Azure, vedere la [distribuzione Web ASP.NET tramite Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md) serie.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-121">For information about how to deploy a Visual Studio web project to Azure App Service Web Apps, see the [ASP.NET Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md) series.</span></span> <span data-ttu-id="bf0fb-122">Tale esercitazione illustra anche come usare le migrazioni di Entity Framework Code First per distribuire il database di SQL Server in Database SQL di Azure.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-122">That tutorial also shows how to use Entity Framework Code First Migrations to deploy your SQL Server database to Azure SQL Database.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="bf0fb-123">Versioni del software utilizzate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="bf0fb-123">Software versions used in the tutorial</span></span>
> 
> - <span data-ttu-id="bf0fb-124">Microsoft Visual Studio 2017 or Microsoft Visual Studio Community 2017</span><span class="sxs-lookup"><span data-stu-id="bf0fb-124">Microsoft Visual Studio 2017 or Microsoft Visual Studio Community 2017</span></span>
>   
> <span data-ttu-id="bf0fb-125">Questa esercitazione funziona anche con Visual Studio 2012 e Visual Studio 2013, ma esistono alcune differenze nel modello di progetto e dell'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-125">This tutorial also works with Visual Studio 2012 and Visual Studio 2013, but there are some differences in the user interface and project template.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="bf0fb-126">Scopo dell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="bf0fb-126">What you'll build</span></span>

<span data-ttu-id="bf0fb-127">In questa esercitazione, sarà:</span><span class="sxs-lookup"><span data-stu-id="bf0fb-127">In this tutorial, you'll:</span></span>

* <span data-ttu-id="bf0fb-128">Creare oggetti dati che riflettono un'università con studenti iscritti a corsi</span><span class="sxs-lookup"><span data-stu-id="bf0fb-128">Build data objects that reflect a university with students enrolled in courses</span></span>
* <span data-ttu-id="bf0fb-129">Creare tabelle di database dagli oggetti</span><span class="sxs-lookup"><span data-stu-id="bf0fb-129">Build database tables from the objects</span></span>
* <span data-ttu-id="bf0fb-130">Popolare il database con dati di test</span><span class="sxs-lookup"><span data-stu-id="bf0fb-130">Populate the database with test data</span></span>
* <span data-ttu-id="bf0fb-131">Visualizzare i dati in un web form</span><span class="sxs-lookup"><span data-stu-id="bf0fb-131">Display data in a web form</span></span>

## <a name="create-the-project"></a><span data-ttu-id="bf0fb-132">Creare il progetto</span><span class="sxs-lookup"><span data-stu-id="bf0fb-132">Create the project</span></span>

1. <span data-ttu-id="bf0fb-133">In Visual Studio 2017, creare un **applicazione Web ASP.NET (.NET Framework)** progetto chiamato **ContosoUniversityModelBinding**.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-133">In Visual Studio 2017, create a **ASP.NET Web Application (.NET Framework)** project called **ContosoUniversityModelBinding**.</span></span>

   ![Crea progetto](retrieving-data/_static/image19.png)

2. <span data-ttu-id="bf0fb-135">Scegliere **OK**.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-135">Select **OK**.</span></span> <span data-ttu-id="bf0fb-136">Viene visualizzata la finestra di dialogo per selezionare un modello.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-136">The dialog box to select a template appears.</span></span>

   ![Selezionare web form](retrieving-data/_static/image3.png)

3. <span data-ttu-id="bf0fb-138">Selezionare il **Web Form** modello.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-138">Select the **Web Forms** template.</span></span> 

4. <span data-ttu-id="bf0fb-139">Se necessario, modificare l'autenticazione **singoli account utente di**.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-139">If necessary, change the authentication to **Individual User Accounts**.</span></span> 

5. <span data-ttu-id="bf0fb-140">Selezionare **OK** per creare il progetto.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-140">Select **OK** to create the project.</span></span>

## <a name="modify-site-appearance"></a><span data-ttu-id="bf0fb-141">Modificare l'aspetto del sito</span><span class="sxs-lookup"><span data-stu-id="bf0fb-141">Modify site appearance</span></span>

   <span data-ttu-id="bf0fb-142">Apportare alcune modifiche per personalizzare l'aspetto del sito.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-142">Make a few changes to customize site appearance.</span></span> 
   
   1. <span data-ttu-id="bf0fb-143">Aprire il file Site. master.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-143">Open the Site.Master file.</span></span>
   
   2. <span data-ttu-id="bf0fb-144">Modificare il titolo da visualizzare **Contoso University** e non **My ASP.NET Application**.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-144">Change the title to display **Contoso University** and not **My ASP.NET Application**.</span></span>

      [!code-aspx-csharp[Main](retrieving-data/samples/sample1.aspx?highlight=1)]

   3. <span data-ttu-id="bf0fb-145">Modificare il testo dell'intestazione dal **nome dell'applicazione** al **Contoso University**.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-145">Change the header text from **Application name** to **Contoso University**.</span></span>

      [!code-aspx-csharp[Main](retrieving-data/samples/sample2.aspx?highlight=7)]

   4. <span data-ttu-id="bf0fb-146">Modificare i collegamenti di intestazione di navigazione per le colonne appropriate del sito.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-146">Change the navigation header links to site appropriate ones.</span></span> 
   
      <span data-ttu-id="bf0fb-147">Rimuovere i collegamenti per **sulle** e **Contact** e, invece, crea un collegamento a una **studenti** pagina, che verrà creato.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-147">Remove the links for **About** and **Contact** and, instead, link to a **Students** page, which you will create.</span></span>

      [!code-aspx-csharp[Main](retrieving-data/samples/sample3.aspx)]

   5. <span data-ttu-id="bf0fb-148">Salvare Site. master.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-148">Save Site.Master.</span></span>

## <a name="add-a-web-form-to-display-student-data"></a><span data-ttu-id="bf0fb-149">Aggiungere un web form per visualizzare i dati degli studenti</span><span class="sxs-lookup"><span data-stu-id="bf0fb-149">Add a web form to display student data</span></span>

   1. <span data-ttu-id="bf0fb-150">Nelle **Esplora soluzioni**, fare clic sul progetto, selezionare **Add** e quindi **nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-150">In **Solution Explorer**, right-click your project, select **Add** and then **New Item**.</span></span> 
   
   2. <span data-ttu-id="bf0fb-151">Nel **Aggiungi nuovo elemento** finestra di dialogo, seleziona la **Web Form con pagina Master** modello e denominarlo **Students.aspx**.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-151">In the **Add New Item** dialog box, select the **Web Form with Master Page** template and name it **Students.aspx**.</span></span>

      ![pagina Crea](retrieving-data/_static/image5.png)

   3. <span data-ttu-id="bf0fb-153">Selezionare **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-153">Select **Add**.</span></span>
   
   4. <span data-ttu-id="bf0fb-154">Per la pagina master del web form, selezionare **Site. master**.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-154">For the web form's master page, select **Site.Master**.</span></span>
   
   5. <span data-ttu-id="bf0fb-155">Scegliere **OK**.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-155">Select **OK**.</span></span>
   

## <a name="add-the-data-model"></a><span data-ttu-id="bf0fb-156">Aggiungere il modello di dati</span><span class="sxs-lookup"><span data-stu-id="bf0fb-156">Add the data model</span></span>

<span data-ttu-id="bf0fb-157">Nel **modelli** cartella, aggiungere una classe denominata **UniversityModels.cs**.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-157">In the **Models** folder, add a class named **UniversityModels.cs**.</span></span>

   1. <span data-ttu-id="bf0fb-158">Fare doppio clic su **modelli**, selezionare **Add**e quindi **nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-158">Right-click **Models**, select **Add**, and then **New Item**.</span></span> <span data-ttu-id="bf0fb-159">Verrà visualizzata la finestra di dialogo **Aggiungi nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-159">The **Add New Item** dialog box appears.</span></span>

   2. <span data-ttu-id="bf0fb-160">Nel menu di spostamento a sinistra, selezionare **codice**, quindi **classe**.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-160">From the left navigation menu, select **Code**, then **Class**.</span></span>

      ![creare una classe di modello](retrieving-data/_static/image20.png)

   3. <span data-ttu-id="bf0fb-162">Denominare la classe **UniversityModels.cs** e selezionare **Add**.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-162">Name the class **UniversityModels.cs** and select **Add**.</span></span>

      <span data-ttu-id="bf0fb-163">In questo file, definire le `SchoolContext`, `Student`, `Enrollment`, e `Course` classi come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="bf0fb-163">In this file, define the `SchoolContext`, `Student`, `Enrollment`, and `Course` classes as follows:</span></span>

      [!code-csharp[Main](retrieving-data/samples/sample4.cs)]

      <span data-ttu-id="bf0fb-164">Il `SchoolContext` deriva dalla classe `DbContext`, che gestisce la connessione al database e modifica dei dati.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-164">The `SchoolContext` class derives from `DbContext`, which manages the database connection and changes in the data.</span></span>

      <span data-ttu-id="bf0fb-165">Nel `Student` classe, si noti che gli attributi applicati al `FirstName`, `LastName`, e `Year` proprietà.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-165">In the `Student` class, notice the attributes applied to the `FirstName`, `LastName`, and `Year` properties.</span></span> <span data-ttu-id="bf0fb-166">Questa esercitazione Usa questi attributi per la convalida dei dati.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-166">This tutorial uses these attributes for data validation.</span></span> <span data-ttu-id="bf0fb-167">Per semplificare il codice, queste proprietà sono contrassegnate con attributi di convalida dei dati.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-167">To simplify the code, only these properties are marked with data-validation attributes.</span></span> <span data-ttu-id="bf0fb-168">In un progetto reale, gli attributi di convalida viene applicato a tutte le proprietà che necessitano di convalida.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-168">In a real project, you would apply validation attributes to all properties needing validation.</span></span>

   4. <span data-ttu-id="bf0fb-169">Save UniversityModels.cs.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-169">Save UniversityModels.cs.</span></span>

## <a name="set-up-the-database-based-on-classes"></a><span data-ttu-id="bf0fb-170">Configurare il database basato su classi</span><span class="sxs-lookup"><span data-stu-id="bf0fb-170">Set up the database based on classes</span></span>

<span data-ttu-id="bf0fb-171">Questa esercitazione viene usato [migrazioni Code First](https://docs.microsoft.com/en-us/ef/ef6/modeling/code-first/migrations/) per creare oggetti e tabelle di database.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-171">This tutorial uses [Code First Migrations](https://docs.microsoft.com/en-us/ef/ef6/modeling/code-first/migrations/) to create objects and database tables.</span></span> <span data-ttu-id="bf0fb-172">Queste tabelle archiviano informazioni agli studenti e i rispettivi corsi.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-172">These tables store information about the students and their courses.</span></span>

   1. <span data-ttu-id="bf0fb-173">Selezionare **strumenti di** > **Gestione pacchetti NuGet** > **la Console di Gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-173">Select **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>

   2. <span data-ttu-id="bf0fb-174">Nelle **Console di gestione pacchetti**, eseguire questo comando:</span><span class="sxs-lookup"><span data-stu-id="bf0fb-174">In **Package Manager Console**, run this command:</span></span>  
      `enable-migrations -ContextTypeName ContosoUniversityModelBinding.Models.SchoolContext`

      <span data-ttu-id="bf0fb-175">Se il comando viene completato correttamente, viene visualizzato un messaggio che indica che le migrazioni sono state abilitate.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-175">If the command completes successfully, a message stating migrations have been enabled appears.</span></span>

      ![abilitare migrazioni](retrieving-data/_static/image8.png)

      <span data-ttu-id="bf0fb-177">Si noti che un file denominato *Configuration.cs* è stato creato.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-177">Notice that a file named *Configuration.cs* has been created.</span></span> <span data-ttu-id="bf0fb-178">Il `Configuration` classe ha un `Seed` metodo, che è possibile popolare anticipatamente tabelle del database con dati di test.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-178">The `Configuration` class has a `Seed` method, which can pre-populate the database tables with test data.</span></span>

## <a name="pre-populate-the-database"></a><span data-ttu-id="bf0fb-179">Pre-popolare il database</span><span class="sxs-lookup"><span data-stu-id="bf0fb-179">Pre-populate the database</span></span>

   1. <span data-ttu-id="bf0fb-180">Aprire Configuration.cs.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-180">Open Configuration.cs.</span></span>
   
   2. <span data-ttu-id="bf0fb-181">Aggiungere al metodo `Seed` il seguente codice.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-181">Add the following code to the `Seed` method.</span></span> <span data-ttu-id="bf0fb-182">Aggiungere anche un `using` istruzione per il `ContosoUniversityModelBinding. Models` dello spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-182">Also, add a `using` statement for the `ContosoUniversityModelBinding. Models` namespace.</span></span>

      [!code-csharp[Main](retrieving-data/samples/sample5.cs)]

   3. <span data-ttu-id="bf0fb-183">Salvare Configuration.cs.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-183">Save Configuration.cs.</span></span>

   4. <span data-ttu-id="bf0fb-184">Nella Console di gestione pacchetti eseguire il comando **aggiungere la migrazione iniziale**.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-184">In the Package Manager Console, run the command **add-migration initial**.</span></span>

   5. <span data-ttu-id="bf0fb-185">Eseguire il comando **update-database**.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-185">Run the command **update-database**.</span></span>

      <span data-ttu-id="bf0fb-186">Se si riceve un'eccezione quando si esegue questo comando, il `StudentID` e `CourseID` valori potrebbero essere diversi dal `Seed` i valori del metodo.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-186">If you receive an exception when running this command, the `StudentID` and `CourseID` values might be different from the `Seed` method values.</span></span> <span data-ttu-id="bf0fb-187">Aprire le tabelle di database e i valori esistenti per trovare `StudentID` e `CourseID`.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-187">Open those database tables and find existing values for `StudentID` and `CourseID`.</span></span> <span data-ttu-id="bf0fb-188">Aggiungere tali valori per il codice per il seeding di `Enrollments` tabella.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-188">Add those values to the code for seeding the `Enrollments` table.</span></span>

## <a name="add-a-gridview-control"></a><span data-ttu-id="bf0fb-189">Aggiungere un controllo GridView</span><span class="sxs-lookup"><span data-stu-id="bf0fb-189">Add a GridView control</span></span>

<span data-ttu-id="bf0fb-190">Con dati popolati del database, ora possibile recuperare tali dati e visualizzarli.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-190">With populated database data, you're now ready to retrieve that data and display it.</span></span> 

1. <span data-ttu-id="bf0fb-191">Aprire Students.aspx.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-191">Open Students.aspx.</span></span>

2. <span data-ttu-id="bf0fb-192">Individuare il `MainContent` segnaposto.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-192">Locate the `MainContent` placeholder.</span></span> <span data-ttu-id="bf0fb-193">All'interno di tale segnaposto, aggiungere un **GridView** controllo che include il codice.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-193">Within that placeholder, add a **GridView** control that includes this code.</span></span>

   [!code-aspx-csharp[Main](retrieving-data/samples/sample6.aspx)]

   <span data-ttu-id="bf0fb-194">Note importanti:</span><span class="sxs-lookup"><span data-stu-id="bf0fb-194">Things to note:</span></span>
   * <span data-ttu-id="bf0fb-195">Si noti che il valore impostato per il `SelectMethod` proprietà nell'elemento GridView.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-195">Notice the value set for the `SelectMethod` property in the GridView element.</span></span> <span data-ttu-id="bf0fb-196">Questo valore specifica il metodo utilizzato per recuperare i dati di GridView, che creato nel passaggio successivo.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-196">This value specifies the method used to retrieve GridView data, which you create in the next step.</span></span> 
   
   * <span data-ttu-id="bf0fb-197">Il `ItemType` è impostata sul `Student` classe creata in precedenza.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-197">The `ItemType` property is set to the `Student` class created earlier.</span></span> <span data-ttu-id="bf0fb-198">Questa impostazione consente di fare riferimento alle proprietà di classe nel markup.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-198">This setting allows you to reference class properties in the markup.</span></span> <span data-ttu-id="bf0fb-199">Ad esempio, il `Student` classe dispone di una raccolta denominata `Enrollments`.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-199">For example, the `Student` class has a collection named `Enrollments`.</span></span> <span data-ttu-id="bf0fb-200">È possibile usare `Item.Enrollments` per recuperare tale insieme e quindi usare [sintassi LINQ](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) per recuperare ogni studente del registrati somma i crediti.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-200">You can use `Item.Enrollments` to retrieve that collection and then use [LINQ syntax](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) to retrieve each student's enrolled credits sum.</span></span>
   
3. <span data-ttu-id="bf0fb-201">Salvare Students.aspx.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-201">Save Students.aspx.</span></span>

## <a name="add-code-to-retrieve-data"></a><span data-ttu-id="bf0fb-202">Aggiungere il codice per recuperare i dati</span><span class="sxs-lookup"><span data-stu-id="bf0fb-202">Add code to retrieve data</span></span>

   <span data-ttu-id="bf0fb-203">Nel file code-behind Students.aspx, aggiungere il metodo specificato per il `SelectMethod` valore.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-203">In the Students.aspx code-behind file, add the method specified for the `SelectMethod` value.</span></span> 
   
   1. <span data-ttu-id="bf0fb-204">Aprire Students.aspx.cs.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-204">Open Students.aspx.cs.</span></span>
   
   2. <span data-ttu-id="bf0fb-205">Aggiungere `using` istruzioni per il `ContosoUniversityModelBinding. Models` e `System.Data.Entity` gli spazi dei nomi.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-205">Add `using` statements for the `ContosoUniversityModelBinding. Models` and `System.Data.Entity` namespaces.</span></span>

      [!code-csharp[Main](retrieving-data/samples/sample7.cs)]

   3. <span data-ttu-id="bf0fb-206">Aggiungere il metodo specificato per `SelectMethod`:</span><span class="sxs-lookup"><span data-stu-id="bf0fb-206">Add the method you specified for `SelectMethod`:</span></span>

      [!code-csharp[Main](retrieving-data/samples/sample8.cs)]

      <span data-ttu-id="bf0fb-207">Il `Include` clausola consente di migliorare le prestazioni delle query ma non è obbligatorio.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-207">The `Include` clause improves query performance but isn't required.</span></span> <span data-ttu-id="bf0fb-208">Senza il `Include` clausola, i dati verrà recuperato utilizzando [ *caricamento lazy*](https://en.wikipedia.org/wiki/Lazy_loading), che prevede l'invio di una query separata per il database ogni volta che correlati vengono recuperati i dati.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-208">Without the `Include` clause, the data is retrieved using [*lazy loading*](https://en.wikipedia.org/wiki/Lazy_loading), which involves sending a separate query to the database each time related data is retrieved.</span></span> <span data-ttu-id="bf0fb-209">Con il `Include` clausola, i dati verrà recuperato utilizzando *caricamento eager*, ovvero un'unica query al database recupera tutti i dati correlati.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-209">With the `Include` clause, data is retrieved using *eager loading*, which means a single database query retrieves all related data.</span></span> <span data-ttu-id="bf0fb-210">Se i dati correlati non viene usati, il caricamento eager è meno efficiente perché vengono recuperati più dati.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-210">If related data isn't used, eager loading is less efficient because more data is retrieved.</span></span> <span data-ttu-id="bf0fb-211">Tuttavia, in questo caso, il caricamento eager offre prestazioni ottimali perché i dati correlati vengono visualizzati per ogni record.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-211">However, in this case, eager loading gives you the best performance because the related data is displayed for each record.</span></span>

      <span data-ttu-id="bf0fb-212">Per altre informazioni sulle considerazioni sulle prestazioni quando si caricano i dati correlati, vedere la **esplicito il caricamento di dati correlati Lazy ed Eager** sezione il [la lettura di dati correlati con Entity Framework in ASP.NET Applicazione MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) articolo.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-212">For more information about performance considerations when loading related data, see the **Lazy, Eager, and Explicit Loading of Related Data** section in the [Reading Related Data with the Entity Framework in an ASP.NET MVC Application](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) article.</span></span>

      <span data-ttu-id="bf0fb-213">Per impostazione predefinita, i dati vengono ordinati i valori della proprietà contrassegnata come chiave.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-213">By default, the data is sorted by the values of the property marked as the key.</span></span> <span data-ttu-id="bf0fb-214">È possibile aggiungere un `OrderBy` clausola per specificare un valore di ordinamento diversi.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-214">You can add an `OrderBy` clause to specify a different sort value.</span></span> <span data-ttu-id="bf0fb-215">In questo esempio, il valore predefinito `StudentID` proprietà viene utilizzata per l'ordinamento.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-215">In this example, the default `StudentID` property is used for sorting.</span></span> <span data-ttu-id="bf0fb-216">Nel [ordinamento, Paging e filtro dei dati](sorting-paging-and-filtering-data.md) articolo, l'utente è abilitato per selezionare una colonna per l'ordinamento.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-216">In the [Sorting, Paging, and Filtering Data](sorting-paging-and-filtering-data.md) article, the user is enabled to select a column for sorting.</span></span>
 
   4. <span data-ttu-id="bf0fb-217">Salvare Students.aspx.cs.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-217">Save Students.aspx.cs.</span></span>

## <a name="run-your-application"></a><span data-ttu-id="bf0fb-218">Eseguire l'applicazione</span><span class="sxs-lookup"><span data-stu-id="bf0fb-218">Run your application</span></span> 

<span data-ttu-id="bf0fb-219">Eseguire l'applicazione web (**F5**) e passare alle **studenti** pagina, che verrà visualizzato quanto segue:</span><span class="sxs-lookup"><span data-stu-id="bf0fb-219">Run your web application (**F5**) and navigate to the **Students** page, which displays the following:</span></span>

   ![visualizzare i dati](retrieving-data/_static/image16.png)

## <a name="automatic-generation-of-model-binding-methods"></a><span data-ttu-id="bf0fb-221">Generazione automatica di metodi di associazione di modelli</span><span class="sxs-lookup"><span data-stu-id="bf0fb-221">Automatic generation of model binding methods</span></span>

<span data-ttu-id="bf0fb-222">Quando questa serie di esercitazioni, è possibile copiare semplicemente il codice dell'esercitazione al progetto.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-222">When working through this tutorial series, you can simply copy the code from the tutorial to your project.</span></span> <span data-ttu-id="bf0fb-223">Tuttavia, uno svantaggio di questo approccio è che si potrebbe non viene a conoscenza della funzionalità fornite da Visual Studio per generare automaticamente codice per metodi di associazione di modelli.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-223">However, one disadvantage of this approach is that you may not become aware of the feature provided by Visual Studio to automatically generate code for model binding methods.</span></span> <span data-ttu-id="bf0fb-224">Quando si lavora su progetti, generazione automatica di codice può risparmiare tempo e la Guida è possibile ottenere un'idea di come implementare un'operazione.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-224">When working on your own projects, automatic code generation can save you time and help you gain a sense of how to implement an operation.</span></span> <span data-ttu-id="bf0fb-225">In questa sezione viene descritta la funzionalità di generazione automatica di codice.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-225">This section describes the automatic code generation feature.</span></span> <span data-ttu-id="bf0fb-226">Questa sezione è solo informativo e non contiene alcun codice che è necessario implementare nel progetto.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-226">This section is only informational and does not contain any code you need to implement in your project.</span></span> 

<span data-ttu-id="bf0fb-227">Quando si imposta un valore per il `SelectMethod`, `UpdateMethod`, `InsertMethod`, o `DeleteMethod` le proprietà nel codice di markup, è possibile selezionare il **creare nuovo metodo** opzione.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-227">When setting a value for the `SelectMethod`, `UpdateMethod`, `InsertMethod`, or `DeleteMethod` properties in the markup code, you can select the **Create New Method** option.</span></span>

![creare un metodo](retrieving-data/_static/image18.png)

<span data-ttu-id="bf0fb-229">Visual Studio non solo crea un metodo nel code-behind con la firma appropriata, ma genera anche codice di implementazione per eseguire l'operazione.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-229">Visual Studio not only creates a method in the code-behind with the proper signature, but also generates implementation code to perform the operation.</span></span> <span data-ttu-id="bf0fb-230">Se si imposta innanzitutto il `ItemType` proprietà prima di usare la generazione automatica di codice delle funzionalità, il codice generato viene utilizzato dai tipi per le operazioni.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-230">If you first set the `ItemType` property before using the automatic code generation feature, the generated code uses that type for the operations.</span></span> <span data-ttu-id="bf0fb-231">Ad esempio, quando si impostano le `UpdateMethod` proprietà, il codice seguente viene generato automaticamente:</span><span class="sxs-lookup"><span data-stu-id="bf0fb-231">For example, when setting the `UpdateMethod` property, the following code is automatically generated:</span></span>

[!code-csharp[Main](retrieving-data/samples/sample9.cs)]

<span data-ttu-id="bf0fb-232">Anche in questo caso, questo codice non deve essere aggiunto al progetto.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-232">Again, this code doesn't need to be added to your project.</span></span> <span data-ttu-id="bf0fb-233">Nella prossima esercitazione, verrà implementato metodi per l'aggiornamento, eliminazione e aggiunta di nuovi dati.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-233">In the next tutorial, you'll implement methods for updating, deleting, and adding new data.</span></span>

## <a name="summary"></a><span data-ttu-id="bf0fb-234">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="bf0fb-234">Summary</span></span>

<span data-ttu-id="bf0fb-235">In questa esercitazione si create classi di modello di dati e generata un database da tali classi.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-235">In this tutorial, you created data model classes and generated a database from those classes.</span></span> <span data-ttu-id="bf0fb-236">Le tabelle del database è stato compilato con i dati di test.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-236">You filled the database tables with test data.</span></span> <span data-ttu-id="bf0fb-237">È utilizzata l'associazione di modelli per recuperare dati dal database e quindi visualizzati i dati in un controllo GridView.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-237">You used model binding to retrieve data from the database, and then displayed the data in a GridView.</span></span>

<span data-ttu-id="bf0fb-238">Entro i prossimi [esercitazione](updating-deleting-and-creating-data.md) in questa serie, verrà abilitata l'aggiornamento, eliminazione e creazione di dati.</span><span class="sxs-lookup"><span data-stu-id="bf0fb-238">In the next [tutorial](updating-deleting-and-creating-data.md) in this series, you'll enable updating, deleting, and creating data.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="bf0fb-239">avanti</span><span class="sxs-lookup"><span data-stu-id="bf0fb-239">Next</span></span>](updating-deleting-and-creating-data.md)
