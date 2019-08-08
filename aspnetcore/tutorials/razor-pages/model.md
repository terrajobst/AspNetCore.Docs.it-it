---
title: Aggiungere un modello a un'app Razor Pages in ASP.NET Core
author: rick-anderson
description: Scoprire come aggiungere classi per la gestione dei film in un database tramite Entity Framework Core (EF Core).
ms.author: riande
ms.date: 07/22/2019
uid: tutorials/razor-pages/model
ms.openlocfilehash: 6132f7b907014b4f57bb9ae0300e00b6ecb23f1a
ms.sourcegitcommit: 2eb605f4f20ac4dd9de6c3b3e3453e108a357a21
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/06/2019
ms.locfileid: "68820072"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="be1b4-103">Aggiungere un modello a un'app Razor Pages in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="be1b4-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

<span data-ttu-id="be1b4-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="be1b4-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="be1b4-105">In questa sezione si aggiungono alcune classi per la gestione di filmati in un database.</span><span class="sxs-lookup"><span data-stu-id="be1b4-105">In this section, classes are added for managing movies in a database.</span></span> <span data-ttu-id="be1b4-106">Queste classi vengono usate con [Entity Framework Core](/ef/core) (EF Core) per lavorare in un database.</span><span class="sxs-lookup"><span data-stu-id="be1b4-106">These classes are used with [Entity Framework Core](/ef/core) (EF Core) to work with a database.</span></span> <span data-ttu-id="be1b4-107">EF Core è un framework ORM (Object-Relational Mapping) che semplifica l'accesso ai dati.</span><span class="sxs-lookup"><span data-stu-id="be1b4-107">EF Core is an object-relational mapping (ORM) framework that simplifies data access.</span></span>

<span data-ttu-id="be1b4-108">Le classi di modello sono dette classi POCO (da "plain-old CLR objects") perché non hanno alcuna dipendenza in EF Core.</span><span class="sxs-lookup"><span data-stu-id="be1b4-108">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="be1b4-109">Definiscono le proprietà dei dati archiviati nel database.</span><span class="sxs-lookup"><span data-stu-id="be1b4-109">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="be1b4-110">Aggiungere un modello di dati</span><span class="sxs-lookup"><span data-stu-id="be1b4-110">Add a data model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="be1b4-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="be1b4-111">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="be1b4-112">Fare clic con il pulsante destro del mouse sul progetto **RazorPagesMovie** > **Aggiungi** > **Nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="be1b4-112">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="be1b4-113">Assegnare il nome *Models* alla cartella.</span><span class="sxs-lookup"><span data-stu-id="be1b4-113">Name the folder *Models*.</span></span>

<span data-ttu-id="be1b4-114">Fare clic con il pulsante destro del mouse sulla cartella *Models*.</span><span class="sxs-lookup"><span data-stu-id="be1b4-114">Right click the *Models* folder.</span></span> <span data-ttu-id="be1b4-115">Selezionare **Aggiungi** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="be1b4-115">Select **Add** > **Class**.</span></span> <span data-ttu-id="be1b4-116">Denominare la classe **Movie**.</span><span class="sxs-lookup"><span data-stu-id="be1b4-116">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="be1b4-117">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="be1b4-117">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="be1b4-118">Aggiungere una cartella denominata *Models*.</span><span class="sxs-lookup"><span data-stu-id="be1b4-118">Add a folder named *Models*.</span></span>
* <span data-ttu-id="be1b4-119">Aggiungere una classe alla cartella *Models* denominata *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="be1b4-119">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="be1b4-120">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="be1b4-120">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="be1b4-121">In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto **RazorPagesMovie**, quindi selezionare **Aggiungi** > **Nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="be1b4-121">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="be1b4-122">Assegnare il nome *Models* alla cartella.</span><span class="sxs-lookup"><span data-stu-id="be1b4-122">Name the folder *Models*.</span></span>
* <span data-ttu-id="be1b4-123">Fare clic con il pulsante destro del mouse sulla cartella *Models* e scegliere **Aggiungi** > **Nuovo file**.</span><span class="sxs-lookup"><span data-stu-id="be1b4-123">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="be1b4-124">Nella finestra di dialogo **Nuovo file**:</span><span class="sxs-lookup"><span data-stu-id="be1b4-124">In the **New File** dialog:</span></span>

  * <span data-ttu-id="be1b4-125">Selezionare **Generale** nel riquadro a sinistra.</span><span class="sxs-lookup"><span data-stu-id="be1b4-125">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="be1b4-126">Selezionare **Classe vuota** nel riquadro centrale.</span><span class="sxs-lookup"><span data-stu-id="be1b4-126">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="be1b4-127">Denominare la classe **Movie** e selezionare **Nuovo**.</span><span class="sxs-lookup"><span data-stu-id="be1b4-127">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

---

<span data-ttu-id="be1b4-128">Compilare il progetto per verificare che non siano presenti errori di compilazione.</span><span class="sxs-lookup"><span data-stu-id="be1b4-128">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="be1b4-129">Eseguire lo scaffolding del modello *Movie*</span><span class="sxs-lookup"><span data-stu-id="be1b4-129">Scaffold the movie model</span></span>

<span data-ttu-id="be1b4-130">In questa sezione viene eseguito lo scaffolding del modello *Movie*.</span><span class="sxs-lookup"><span data-stu-id="be1b4-130">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="be1b4-131">Lo strumento di scaffolding crea quindi le pagine per le operazioni CRUD (creazione, lettura, aggiornamento ed eliminazione) per il modello *Movie*.</span><span class="sxs-lookup"><span data-stu-id="be1b4-131">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="be1b4-132">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="be1b4-132">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="be1b4-133">Creare una cartella *Pages/Movies*:</span><span class="sxs-lookup"><span data-stu-id="be1b4-133">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="be1b4-134">Fare clic con il pulsante destro del mouse sulla cartella *Pages* > **Aggiungi** > **Nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="be1b4-134">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="be1b4-135">Assegnare il nome *Movies* alla cartella</span><span class="sxs-lookup"><span data-stu-id="be1b4-135">Name the folder *Movies*</span></span>

<span data-ttu-id="be1b4-136">Fare clic con il pulsante destro del mouse sulla cartella *Pages/Movies* > **Aggiungi** > **Nuovo elemento di scaffolding**.</span><span class="sxs-lookup"><span data-stu-id="be1b4-136">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Immagine relativa alle istruzioni precedenti.](model/_static/sca.png)

<span data-ttu-id="be1b4-138">Nella finestra di dialogo **Aggiungi scaffolding** selezionare **Razor che usano Entity Framework (CRUD)** > **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="be1b4-138">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Immagine relativa alle istruzioni precedenti.](model/_static/add_scaffold.png)

<span data-ttu-id="be1b4-140">Completare la finestra di dialogo **Pagine Razor che usano Entity Framework (CRUD)** :</span><span class="sxs-lookup"><span data-stu-id="be1b4-140">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="be1b4-141">Nel menu a discesa **Classe modello** selezionare **Movie (RazorPagesMovie.Models)** .</span><span class="sxs-lookup"><span data-stu-id="be1b4-141">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="be1b4-142">Nella riga **Classe contesto di dati** selezionare il segno più **+** e modificare il nome generato da RazorPagesMovie.**Models**.RazorPagesMovieContext a RazorPagesMovie.**Data**.RazorPagesMovieContext.</span><span class="sxs-lookup"><span data-stu-id="be1b4-142">In the **Data context class** row, select the **+** (plus) sign and change the generated name from RazorPagesMovie.**Models**.RazorPagesMovieContext to RazorPagesMovie.**Data**.RazorPagesMovieContext.</span></span> <span data-ttu-id="be1b4-143">[Questa modifica](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) non è obbligatoria.</span><span class="sxs-lookup"><span data-stu-id="be1b4-143">[This change](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) is not required.</span></span> <span data-ttu-id="be1b4-144">Crea la classe del contesto di database con lo spazio dei nomi corretto.</span><span class="sxs-lookup"><span data-stu-id="be1b4-144">It creates the database context class with the correct namespace.</span></span>
* <span data-ttu-id="be1b4-145">Selezionare **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="be1b4-145">Select **Add**.</span></span>

![Immagine relativa alle istruzioni precedenti.](model/_static/3/arp.png)

<span data-ttu-id="be1b4-147">Il file *appsettings.json* è stato aggiornato con la stringa di connessione usata per connettersi a un database locale.</span><span class="sxs-lookup"><span data-stu-id="be1b4-147">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="be1b4-148">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="be1b4-148">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="be1b4-149">Aprire una finestra di comando nella directory del progetto (la directory che contiene i file *Program.cs*, *Startup.cs* e *csproj*).</span><span class="sxs-lookup"><span data-stu-id="be1b4-149">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="be1b4-150">Installare lo strumento di scaffolding:</span><span class="sxs-lookup"><span data-stu-id="be1b4-150">Install the scaffolding tool:</span></span>

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator --version 3.0.0-*
   ```

* <span data-ttu-id="be1b4-151">**Per Windows**: Eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="be1b4-151">**For Windows**: Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="be1b4-152">**Per macOS e Linux**: Eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="be1b4-152">**For macOS and Linux**: Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="be1b4-153">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="be1b4-153">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="be1b4-154">Aprire una finestra di comando nella directory del progetto (la directory che contiene i file *Program.cs*, *Startup.cs* e *csproj*).</span><span class="sxs-lookup"><span data-stu-id="be1b4-154">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="be1b4-155">Installare lo strumento di scaffolding:</span><span class="sxs-lookup"><span data-stu-id="be1b4-155">Install the scaffolding tool:</span></span>

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator --version 3.0.0-*
   ```

* <span data-ttu-id="be1b4-156">Eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="be1b4-156">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

---

<span data-ttu-id="be1b4-157">Il processo di scaffolding crea e aggiorna i file seguenti:</span><span class="sxs-lookup"><span data-stu-id="be1b4-157">The scaffold process creates and updates the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="be1b4-158">File creati</span><span class="sxs-lookup"><span data-stu-id="be1b4-158">Files created</span></span>

* <span data-ttu-id="be1b4-159">*Pages/Movies*: Crea, Elimina, Dettagli, Modifica e Indice.</span><span class="sxs-lookup"><span data-stu-id="be1b4-159">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="be1b4-160">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="be1b4-160">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="file-updated"></a><span data-ttu-id="be1b4-161">File aggiornato</span><span class="sxs-lookup"><span data-stu-id="be1b4-161">File updated</span></span>

* <span data-ttu-id="be1b4-162">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="be1b4-162">*Startup.cs*</span></span>

<span data-ttu-id="be1b4-163">I file creati e aggiornati sono illustrati nella sezione successiva.</span><span class="sxs-lookup"><span data-stu-id="be1b4-163">The created and updated files are explained in the next section.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="be1b4-164">Migrazione iniziale</span><span class="sxs-lookup"><span data-stu-id="be1b4-164">Initial migration</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="be1b4-165">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="be1b4-165">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="be1b4-166">In questa sezione viene usata la Console di Gestione pacchetti (PMC) per:</span><span class="sxs-lookup"><span data-stu-id="be1b4-166">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="be1b4-167">Aggiungere una migrazione iniziale.</span><span class="sxs-lookup"><span data-stu-id="be1b4-167">Add an initial migration.</span></span>
* <span data-ttu-id="be1b4-168">Aggiornare il database con la migrazione iniziale.</span><span class="sxs-lookup"><span data-stu-id="be1b4-168">Update the database with the initial migration.</span></span>

<span data-ttu-id="be1b4-169">Dal menu **Strumenti** selezionare **Gestione pacchetti NuGet** >  **Console di Gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="be1b4-169">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Menu della Console di Gestione pacchetti](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="be1b4-171">Nella Console di Gestione pacchetti immettere i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="be1b4-171">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration InitialCreate
Update-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="be1b4-172">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="be1b4-172">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="be1b4-173">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="be1b4-173">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---

<span data-ttu-id="be1b4-174">I comandi precedenti generano l'avviso seguente: "No type was specified for the decimal column 'Price' on entity type 'Movie'. (Nessun tipo specificato per la colonna decimale 'Price' nel tipo di entità 'Movie').</span><span class="sxs-lookup"><span data-stu-id="be1b4-174">The preceding commands generate the following warning: "No type was specified for the decimal column 'Price' on entity type 'Movie'.</span></span> <span data-ttu-id="be1b4-175">This will cause values to be silently truncated if they do not fit in the default precision and scale. (I valori saranno quindi automaticamente troncati se non rispettano la precisione e la scala predefinite).</span><span class="sxs-lookup"><span data-stu-id="be1b4-175">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span></span> <span data-ttu-id="be1b4-176">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'. (Specificare in modo esplicito il tipo di colonna di SQL Server che può supportare tutti i valori usando 'HasColumnType()')"</span><span class="sxs-lookup"><span data-stu-id="be1b4-176">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'."</span></span>

<span data-ttu-id="be1b4-177">È possibile ignorare tale avviso. Verrà risolto in un'esercitazione successiva.</span><span class="sxs-lookup"><span data-stu-id="be1b4-177">You can ignore that warning, it will be fixed in a later tutorial.</span></span>

<span data-ttu-id="be1b4-178">Il comando `ef migrations add InitialCreate` genera un codice per creare lo schema del database iniziale.</span><span class="sxs-lookup"><span data-stu-id="be1b4-178">The `ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span> <span data-ttu-id="be1b4-179">Lo schema è basato sul modello specificato in `DbContext` (nel file *RazorPagesMovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="be1b4-179">The schema is based on the model specified in the `DbContext` (In the *RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="be1b4-180">L'argomento `InitialCreate` viene usato per denominare le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="be1b4-180">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="be1b4-181">È possibile usare qualsiasi nome, ma per convenzione viene selezionato un nome che descrive la migrazione.</span><span class="sxs-lookup"><span data-stu-id="be1b4-181">Any name can be used, but by convention a name is selected that describes the migration.</span></span>

<span data-ttu-id="be1b4-182">Il comando `ef database update` esegue il metodo `Up` nel file *Migrations/\<time-stamp>_InitialCreate.cs*.</span><span class="sxs-lookup"><span data-stu-id="be1b4-182">The `ef database update` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="be1b4-183">Il metodo `Up` crea il database.</span><span class="sxs-lookup"><span data-stu-id="be1b4-183">The `Up` method creates the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="be1b4-184">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="be1b4-184">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="be1b4-185">Esaminare il contesto registrato con l'inserimento di dipendenze</span><span class="sxs-lookup"><span data-stu-id="be1b4-185">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="be1b4-186">ASP.NET Core viene compilato tramite [dependency injection](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="be1b4-186">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="be1b4-187">I servizi, ad esempio il contesto di database di Entity Framework Core, vengono registrati tramite dependency injection durante l'avvio dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="be1b4-187">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="be1b4-188">Questi servizi vengono quindi offerti ai componenti per cui sono necessari (ad esempio Razor Pages) tramite i parametri del costruttore.</span><span class="sxs-lookup"><span data-stu-id="be1b4-188">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="be1b4-189">Più avanti nell'esercitazione viene illustrato il codice del costruttore che ottiene un'istanza del contesto di database.</span><span class="sxs-lookup"><span data-stu-id="be1b4-189">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="be1b4-190">Lo strumento di scaffolding ha creato automaticamente un contesto del database e lo ha registrato per la dependency injection.</span><span class="sxs-lookup"><span data-stu-id="be1b4-190">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="be1b4-191">Esaminare il metodo `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="be1b4-191">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="be1b4-192">La riga evidenziata è stata aggiunta dallo scaffolder:</span><span class="sxs-lookup"><span data-stu-id="be1b4-192">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_ConfigureServices&highlight=5-6)]

<span data-ttu-id="be1b4-193">`RazorPagesMovieContext` coordina la funzionalità di EF Core (Create, Read, Update, Delete e così via) per il modello `Movie`.</span><span class="sxs-lookup"><span data-stu-id="be1b4-193">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="be1b4-194">Il contesto dei dati (`RazorPagesMovieContext`) è derivato da [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="be1b4-194">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="be1b4-195">Il contesto dei dati specifica le entità incluse nel modello di dati.</span><span class="sxs-lookup"><span data-stu-id="be1b4-195">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="be1b4-196">il codice precedente crea una proprietà [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) per il set di entità.</span><span class="sxs-lookup"><span data-stu-id="be1b4-196">The preceding code creates a [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="be1b4-197">Nella terminologia di Entity Framework, un set di entità corrisponde in genere alla tabella di un database.</span><span class="sxs-lookup"><span data-stu-id="be1b4-197">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="be1b4-198">Un'entità corrisponde a una riga nella tabella.</span><span class="sxs-lookup"><span data-stu-id="be1b4-198">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="be1b4-199">Il nome della stringa di connessione viene passato al contesto chiamando un metodo in un oggetto [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions).</span><span class="sxs-lookup"><span data-stu-id="be1b4-199">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="be1b4-200">Per lo sviluppo locale, il [sistema di configurazione di ASP.NET Core](xref:fundamentals/configuration/index) legge la stringa di connessione dal file *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="be1b4-200">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="be1b4-201">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="be1b4-201">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="be1b4-202">Esaminare il metodo `Up`.</span><span class="sxs-lookup"><span data-stu-id="be1b4-202">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="be1b4-203">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="be1b4-203">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="be1b4-204">Esaminare il metodo `Up`.</span><span class="sxs-lookup"><span data-stu-id="be1b4-204">Examine the `Up` method.</span></span>

---

<span data-ttu-id="be1b4-205">Il comando `Add-Migration` genera un codice per creare lo schema del database iniziale.</span><span class="sxs-lookup"><span data-stu-id="be1b4-205">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="be1b4-206">Lo schema è basato sul modello specificato in `RazorPagesMovieContext` (nel file *Data/RazorPagesMovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="be1b4-206">The schema is based on the model specified in the `RazorPagesMovieContext` (In the *Data/RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="be1b4-207">L'argomento `Initial` viene usato per denominare le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="be1b4-207">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="be1b4-208">È possibile usare qualsiasi nome, ma per convenzione viene usato un nome che descrive la migrazione.</span><span class="sxs-lookup"><span data-stu-id="be1b4-208">Any name can be used, but by convention a name that describes the migration is used.</span></span> <span data-ttu-id="be1b4-209">Per altre informazioni, vedere <xref:data/ef-mvc/migrations>.</span><span class="sxs-lookup"><span data-stu-id="be1b4-209">For more information, see <xref:data/ef-mvc/migrations>.</span></span>

<span data-ttu-id="be1b4-210">Il comando `Update-Database` esegue il metodo `Up` nel file *Migrations/{time-stamp}_InitialCreate.cs*, che crea il database.</span><span class="sxs-lookup"><span data-stu-id="be1b4-210">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="be1b4-211">Eseguire il test dell'app</span><span class="sxs-lookup"><span data-stu-id="be1b4-211">Test the app</span></span>

* <span data-ttu-id="be1b4-212">Eseguire l'app e accodare `/Movies` all'URL nel browser (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="be1b4-212">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="be1b4-213">Se viene visualizzato l'errore:</span><span class="sxs-lookup"><span data-stu-id="be1b4-213">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="be1b4-214">Non è stato eseguita la [migrazione](#pmc).</span><span class="sxs-lookup"><span data-stu-id="be1b4-214">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="be1b4-215">Eseguire il test del collegamento **Crea**.</span><span class="sxs-lookup"><span data-stu-id="be1b4-215">Test the **Create** link.</span></span>

  ![Pagina Crea](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="be1b4-217">Potrebbe non essere possibile immettere virgole decimali nel campo `Price`.</span><span class="sxs-lookup"><span data-stu-id="be1b4-217">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="be1b4-218">Per supportare la [convalida jQuery](https://jqueryvalidation.org/) per impostazioni locali diverse dall'inglese che usano la virgola (",") come separatore decimale e per formati di data diversi da quello dell'inglese (Stati Uniti), è necessario localizzare l'app.</span><span class="sxs-lookup"><span data-stu-id="be1b4-218">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="be1b4-219">Per istruzioni sulla localizzazione, vedere [questo problema su GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="be1b4-219">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="be1b4-220">Eseguire il test dei collegamenti **Modifica**, **Dettagli** e **Elimina**.</span><span class="sxs-lookup"><span data-stu-id="be1b4-220">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="be1b4-221">L'esercitazione successiva illustra i file creati tramite scaffolding.</span><span class="sxs-lookup"><span data-stu-id="be1b4-221">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="be1b4-222">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="be1b4-222">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="be1b4-223">[Precedente: Introduzione](xref:tutorials/razor-pages/razor-pages-start)
> [Avanti: Pagine Razor create tramite scaffolding](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="be1b4-223">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end

<!--  ::: moniker previous version   -->
::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="be1b4-224">In questa sezione si aggiungono alcune classi per la gestione di filmati in un database.</span><span class="sxs-lookup"><span data-stu-id="be1b4-224">In this section, classes are added for managing movies in a database.</span></span> <span data-ttu-id="be1b4-225">Queste classi vengono usate con [Entity Framework Core](/ef/core) (EF Core) per lavorare in un database.</span><span class="sxs-lookup"><span data-stu-id="be1b4-225">These classes are used with [Entity Framework Core](/ef/core) (EF Core) to work with a database.</span></span> <span data-ttu-id="be1b4-226">EF Core è un framework object-relational mapping (ORM) che semplifica il codice di accesso ai dati.</span><span class="sxs-lookup"><span data-stu-id="be1b4-226">EF Core is an object-relational mapping (ORM) framework that simplifies data access code.</span></span>

<span data-ttu-id="be1b4-227">Le classi di modello sono dette classi POCO (da "plain-old CLR objects") perché non hanno alcuna dipendenza in EF Core.</span><span class="sxs-lookup"><span data-stu-id="be1b4-227">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="be1b4-228">Definiscono le proprietà dei dati archiviati nel database.</span><span class="sxs-lookup"><span data-stu-id="be1b4-228">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="be1b4-229">Aggiungere un modello di dati</span><span class="sxs-lookup"><span data-stu-id="be1b4-229">Add a data model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="be1b4-230">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="be1b4-230">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="be1b4-231">Fare clic con il pulsante destro del mouse sul progetto **RazorPagesMovie** > **Aggiungi** > **Nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="be1b4-231">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="be1b4-232">Assegnare il nome *Models* alla cartella.</span><span class="sxs-lookup"><span data-stu-id="be1b4-232">Name the folder *Models*.</span></span>

<span data-ttu-id="be1b4-233">Fare clic con il pulsante destro del mouse sulla cartella *Models*.</span><span class="sxs-lookup"><span data-stu-id="be1b4-233">Right click the *Models* folder.</span></span> <span data-ttu-id="be1b4-234">Selezionare **Aggiungi** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="be1b4-234">Select **Add** > **Class**.</span></span> <span data-ttu-id="be1b4-235">Denominare la classe **Movie**.</span><span class="sxs-lookup"><span data-stu-id="be1b4-235">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="be1b4-236">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="be1b4-236">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="be1b4-237">Aggiungere una cartella denominata *Models*.</span><span class="sxs-lookup"><span data-stu-id="be1b4-237">Add a folder named *Models*.</span></span>
* <span data-ttu-id="be1b4-238">Aggiungere una classe alla cartella *Models* denominata *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="be1b4-238">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="be1b4-239">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="be1b4-239">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="be1b4-240">In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto **RazorPagesMovie**, quindi selezionare **Aggiungi** > **Nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="be1b4-240">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="be1b4-241">Assegnare il nome *Models* alla cartella.</span><span class="sxs-lookup"><span data-stu-id="be1b4-241">Name the folder *Models*.</span></span>
* <span data-ttu-id="be1b4-242">Fare clic con il pulsante destro del mouse sulla cartella *Models* e scegliere **Aggiungi** > **Nuovo file**.</span><span class="sxs-lookup"><span data-stu-id="be1b4-242">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="be1b4-243">Nella finestra di dialogo **Nuovo file**:</span><span class="sxs-lookup"><span data-stu-id="be1b4-243">In the **New File** dialog:</span></span>

  * <span data-ttu-id="be1b4-244">Selezionare **Generale** nel riquadro a sinistra.</span><span class="sxs-lookup"><span data-stu-id="be1b4-244">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="be1b4-245">Selezionare **Classe vuota** nel riquadro centrale.</span><span class="sxs-lookup"><span data-stu-id="be1b4-245">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="be1b4-246">Denominare la classe **Movie** e selezionare **Nuovo**.</span><span class="sxs-lookup"><span data-stu-id="be1b4-246">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

---

<span data-ttu-id="be1b4-247">Compilare il progetto per verificare che non siano presenti errori di compilazione.</span><span class="sxs-lookup"><span data-stu-id="be1b4-247">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="be1b4-248">Eseguire lo scaffolding del modello *Movie*</span><span class="sxs-lookup"><span data-stu-id="be1b4-248">Scaffold the movie model</span></span>

<span data-ttu-id="be1b4-249">In questa sezione viene eseguito lo scaffolding del modello *Movie*.</span><span class="sxs-lookup"><span data-stu-id="be1b4-249">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="be1b4-250">Lo strumento di scaffolding crea quindi le pagine per le operazioni CRUD (creazione, lettura, aggiornamento ed eliminazione) per il modello *Movie*.</span><span class="sxs-lookup"><span data-stu-id="be1b4-250">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="be1b4-251">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="be1b4-251">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="be1b4-252">Creare una cartella *Pages/Movies*:</span><span class="sxs-lookup"><span data-stu-id="be1b4-252">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="be1b4-253">Fare clic con il pulsante destro del mouse sulla cartella *Pages* > **Aggiungi** > **Nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="be1b4-253">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="be1b4-254">Assegnare il nome *Movies* alla cartella</span><span class="sxs-lookup"><span data-stu-id="be1b4-254">Name the folder *Movies*</span></span>

<span data-ttu-id="be1b4-255">Fare clic con il pulsante destro del mouse sulla cartella *Pages/Movies* > **Aggiungi** > **Nuovo elemento di scaffolding**.</span><span class="sxs-lookup"><span data-stu-id="be1b4-255">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Immagine relativa alle istruzioni precedenti.](model/_static/sca.png)

<span data-ttu-id="be1b4-257">Nella finestra di dialogo **Aggiungi scaffolding** selezionare **Razor che usano Entity Framework (CRUD)** > **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="be1b4-257">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Immagine relativa alle istruzioni precedenti.](model/_static/add_scaffold.png)

<span data-ttu-id="be1b4-259">Completare la finestra di dialogo **Pagine Razor che usano Entity Framework (CRUD)** :</span><span class="sxs-lookup"><span data-stu-id="be1b4-259">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>
<!-- In the next section, change 
(plus) sign and accept the generated name 
to use Data, it should not use models. That will make the namespace the same for the VS version and the CLI version
-->

* <span data-ttu-id="be1b4-260">Nel menu a discesa **Classe modello** selezionare **Movie (RazorPagesMovie.Models)** .</span><span class="sxs-lookup"><span data-stu-id="be1b4-260">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="be1b4-261">Nella riga **Classe contesto di dati** selezionare il segno più **+** e accettare il nome generato **RazorPagesMovie.Models.RazorPagesMovieContext**.</span><span class="sxs-lookup"><span data-stu-id="be1b4-261">In the **Data context class** row, select the **+** (plus) sign and accept the generated name **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="be1b4-262">Selezionare **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="be1b4-262">Select **Add**.</span></span>

![Immagine relativa alle istruzioni precedenti.](model/_static/arp.png)

<span data-ttu-id="be1b4-264">Il file *appsettings.json* è stato aggiornato con la stringa di connessione usata per connettersi a un database locale.</span><span class="sxs-lookup"><span data-stu-id="be1b4-264">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="be1b4-265">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="be1b4-265">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="be1b4-266">Aprire una finestra di comando nella directory del progetto (la directory che contiene i file *Program.cs*, *Startup.cs* e *csproj*).</span><span class="sxs-lookup"><span data-stu-id="be1b4-266">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="be1b4-267">Installare lo strumento di scaffolding:</span><span class="sxs-lookup"><span data-stu-id="be1b4-267">Install the scaffolding tool:</span></span>

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="be1b4-268">**Per Windows**: Eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="be1b4-268">**For Windows**: Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="be1b4-269">**Per macOS e Linux**: Eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="be1b4-269">**For macOS and Linux**: Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="be1b4-270">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="be1b4-270">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="be1b4-271">Aprire una finestra di comando nella directory del progetto (la directory che contiene i file *Program.cs*, *Startup.cs* e *csproj*).</span><span class="sxs-lookup"><span data-stu-id="be1b4-271">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="be1b4-272">Installare lo strumento di scaffolding:</span><span class="sxs-lookup"><span data-stu-id="be1b4-272">Install the scaffolding tool:</span></span>

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="be1b4-273">Eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="be1b4-273">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

---

<span data-ttu-id="be1b4-274">Il processo di scaffolding crea e aggiorna i file seguenti:</span><span class="sxs-lookup"><span data-stu-id="be1b4-274">The scaffold process creates and updates the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="be1b4-275">File creati</span><span class="sxs-lookup"><span data-stu-id="be1b4-275">Files created</span></span>

* <span data-ttu-id="be1b4-276">*Pages/Movies*: Crea, Elimina, Dettagli, Modifica e Indice.</span><span class="sxs-lookup"><span data-stu-id="be1b4-276">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="be1b4-277">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="be1b4-277">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="file-updated"></a><span data-ttu-id="be1b4-278">File aggiornato</span><span class="sxs-lookup"><span data-stu-id="be1b4-278">File updated</span></span>

* <span data-ttu-id="be1b4-279">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="be1b4-279">*Startup.cs*</span></span>

<span data-ttu-id="be1b4-280">I file creati e aggiornati sono illustrati nella sezione successiva.</span><span class="sxs-lookup"><span data-stu-id="be1b4-280">The created and updated files are explained in the next section.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="be1b4-281">Migrazione iniziale</span><span class="sxs-lookup"><span data-stu-id="be1b4-281">Initial migration</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="be1b4-282">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="be1b4-282">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="be1b4-283">In questa sezione viene usata la Console di Gestione pacchetti (PMC) per:</span><span class="sxs-lookup"><span data-stu-id="be1b4-283">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="be1b4-284">Aggiungere una migrazione iniziale.</span><span class="sxs-lookup"><span data-stu-id="be1b4-284">Add an initial migration.</span></span>
* <span data-ttu-id="be1b4-285">Aggiornare il database con la migrazione iniziale.</span><span class="sxs-lookup"><span data-stu-id="be1b4-285">Update the database with the initial migration.</span></span>

<span data-ttu-id="be1b4-286">Dal menu **Strumenti** selezionare **Gestione pacchetti NuGet** >  **Console di Gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="be1b4-286">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Menu della Console di Gestione pacchetti](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="be1b4-288">Nella Console di Gestione pacchetti immettere i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="be1b4-288">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration Initial
Update-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="be1b4-289">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="be1b4-289">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="be1b4-290">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="be1b4-290">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---

<span data-ttu-id="be1b4-291">I comandi precedenti generano l'avviso seguente: "No type was specified for the decimal column 'Price' on entity type 'Movie'. (Nessun tipo specificato per la colonna decimale 'Price' nel tipo di entità 'Movie').</span><span class="sxs-lookup"><span data-stu-id="be1b4-291">The preceding commands generate the following warning: "No type was specified for the decimal column 'Price' on entity type 'Movie'.</span></span> <span data-ttu-id="be1b4-292">This will cause values to be silently truncated if they do not fit in the default precision and scale. (I valori saranno quindi automaticamente troncati se non rispettano la precisione e la scala predefinite).</span><span class="sxs-lookup"><span data-stu-id="be1b4-292">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span></span> <span data-ttu-id="be1b4-293">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'. (Specificare in modo esplicito il tipo di colonna di SQL Server che può supportare tutti i valori usando 'HasColumnType()')"</span><span class="sxs-lookup"><span data-stu-id="be1b4-293">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'."</span></span>

<span data-ttu-id="be1b4-294">È possibile ignorare tale avviso. Verrà risolto in un'esercitazione successiva.</span><span class="sxs-lookup"><span data-stu-id="be1b4-294">You can ignore that warning, it will be fixed in a later tutorial.</span></span>

<span data-ttu-id="be1b4-295">Il comando `ef migrations add InitialCreate` genera un codice per creare lo schema del database iniziale.</span><span class="sxs-lookup"><span data-stu-id="be1b4-295">The `ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span> <span data-ttu-id="be1b4-296">Lo schema è basato sul modello specificato in `DbContext` (nel file *RazorPagesMovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="be1b4-296">The schema is based on the model specified in the `DbContext` (In the *RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="be1b4-297">L'argomento `InitialCreate` viene usato per denominare le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="be1b4-297">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="be1b4-298">È possibile usare qualsiasi nome, ma per convenzione viene selezionato un nome che descrive la migrazione.</span><span class="sxs-lookup"><span data-stu-id="be1b4-298">Any name can be used, but by convention a name is selected that describes the migration.</span></span>

<span data-ttu-id="be1b4-299">Il comando `ef database update` esegue il metodo `Up` nel file *Migrations/\<time-stamp>_InitialCreate.cs*.</span><span class="sxs-lookup"><span data-stu-id="be1b4-299">The `ef database update` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="be1b4-300">Il metodo `Up` crea il database.</span><span class="sxs-lookup"><span data-stu-id="be1b4-300">The `Up` method creates the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="be1b4-301">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="be1b4-301">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="be1b4-302">Esaminare il contesto registrato con l'inserimento di dipendenze</span><span class="sxs-lookup"><span data-stu-id="be1b4-302">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="be1b4-303">ASP.NET Core viene compilato tramite [dependency injection](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="be1b4-303">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="be1b4-304">I servizi, ad esempio il contesto di database di Entity Framework Core, vengono registrati tramite dependency injection durante l'avvio dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="be1b4-304">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="be1b4-305">Questi servizi vengono quindi offerti ai componenti per cui sono necessari (ad esempio Razor Pages) tramite i parametri del costruttore.</span><span class="sxs-lookup"><span data-stu-id="be1b4-305">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="be1b4-306">Più avanti nell'esercitazione viene illustrato il codice del costruttore che ottiene un'istanza del contesto di database.</span><span class="sxs-lookup"><span data-stu-id="be1b4-306">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="be1b4-307">Lo strumento di scaffolding ha creato automaticamente un contesto del database e lo ha registrato per la dependency injection.</span><span class="sxs-lookup"><span data-stu-id="be1b4-307">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="be1b4-308">Esaminare il metodo `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="be1b4-308">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="be1b4-309">La riga evidenziata è stata aggiunta dallo scaffolder:</span><span class="sxs-lookup"><span data-stu-id="be1b4-309">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

<span data-ttu-id="be1b4-310">`RazorPagesMovieContext` coordina la funzionalità di EF Core (Create, Read, Update, Delete e così via) per il modello `Movie`.</span><span class="sxs-lookup"><span data-stu-id="be1b4-310">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="be1b4-311">Il contesto dei dati (`RazorPagesMovieContext`) è derivato da [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="be1b4-311">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="be1b4-312">Il contesto dei dati specifica le entità incluse nel modello di dati.</span><span class="sxs-lookup"><span data-stu-id="be1b4-312">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="be1b4-313">il codice precedente crea una proprietà [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) per il set di entità.</span><span class="sxs-lookup"><span data-stu-id="be1b4-313">The preceding code creates a [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="be1b4-314">Nella terminologia di Entity Framework, un set di entità corrisponde in genere alla tabella di un database.</span><span class="sxs-lookup"><span data-stu-id="be1b4-314">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="be1b4-315">Un'entità corrisponde a una riga nella tabella.</span><span class="sxs-lookup"><span data-stu-id="be1b4-315">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="be1b4-316">Il nome della stringa di connessione viene passato al contesto chiamando un metodo in un oggetto [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions).</span><span class="sxs-lookup"><span data-stu-id="be1b4-316">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="be1b4-317">Per lo sviluppo locale, il [sistema di configurazione di ASP.NET Core](xref:fundamentals/configuration/index) legge la stringa di connessione dal file *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="be1b4-317">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="be1b4-318">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="be1b4-318">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="be1b4-319">Esaminare il metodo `Up`.</span><span class="sxs-lookup"><span data-stu-id="be1b4-319">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="be1b4-320">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="be1b4-320">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="be1b4-321">Esaminare il metodo `Up`.</span><span class="sxs-lookup"><span data-stu-id="be1b4-321">Examine the `Up` method.</span></span>

---

<span data-ttu-id="be1b4-322">Il comando `Add-Migration` genera un codice per creare lo schema del database iniziale.</span><span class="sxs-lookup"><span data-stu-id="be1b4-322">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="be1b4-323">Lo schema è basato sul modello specificato in `RazorPagesMovieContext` (nel file *Data/RazorPagesMovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="be1b4-323">The schema is based on the model specified in the `RazorPagesMovieContext` (In the *Data/RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="be1b4-324">L'argomento `Initial` viene usato per denominare le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="be1b4-324">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="be1b4-325">È possibile usare qualsiasi nome, ma per convenzione viene usato un nome che descrive la migrazione.</span><span class="sxs-lookup"><span data-stu-id="be1b4-325">Any name can be used, but by convention a name that describes the migration is used.</span></span> <span data-ttu-id="be1b4-326">Per altre informazioni, vedere <xref:data/ef-mvc/migrations>.</span><span class="sxs-lookup"><span data-stu-id="be1b4-326">For more information, see <xref:data/ef-mvc/migrations>.</span></span>

<span data-ttu-id="be1b4-327">Il comando `Update-Database` esegue il metodo `Up` nel file *Migrations/{time-stamp}_InitialCreate.cs*, che crea il database.</span><span class="sxs-lookup"><span data-stu-id="be1b4-327">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="be1b4-328">Eseguire il test dell'app</span><span class="sxs-lookup"><span data-stu-id="be1b4-328">Test the app</span></span>

* <span data-ttu-id="be1b4-329">Eseguire l'app e accodare `/Movies` all'URL nel browser (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="be1b4-329">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="be1b4-330">Se viene visualizzato l'errore:</span><span class="sxs-lookup"><span data-stu-id="be1b4-330">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="be1b4-331">Non è stato eseguita la [migrazione](#pmc).</span><span class="sxs-lookup"><span data-stu-id="be1b4-331">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="be1b4-332">Eseguire il test del collegamento **Crea**.</span><span class="sxs-lookup"><span data-stu-id="be1b4-332">Test the **Create** link.</span></span>

  ![Pagina Crea](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="be1b4-334">Potrebbe non essere possibile immettere virgole decimali nel campo `Price`.</span><span class="sxs-lookup"><span data-stu-id="be1b4-334">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="be1b4-335">Per supportare la [convalida jQuery](https://jqueryvalidation.org/) per impostazioni locali diverse dall'inglese che usano la virgola (",") come separatore decimale e per formati di data diversi da quello dell'inglese (Stati Uniti), è necessario localizzare l'app.</span><span class="sxs-lookup"><span data-stu-id="be1b4-335">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="be1b4-336">Per istruzioni sulla localizzazione, vedere [questo problema su GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="be1b4-336">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="be1b4-337">Eseguire il test dei collegamenti **Modifica**, **Dettagli** e **Elimina**.</span><span class="sxs-lookup"><span data-stu-id="be1b4-337">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="be1b4-338">L'esercitazione successiva illustra i file creati tramite scaffolding.</span><span class="sxs-lookup"><span data-stu-id="be1b4-338">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="be1b4-339">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="be1b4-339">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="be1b4-340">[Precedente: Introduzione](xref:tutorials/razor-pages/razor-pages-start)
> [Avanti: Pagine Razor create tramite scaffolding](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="be1b4-340">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end
