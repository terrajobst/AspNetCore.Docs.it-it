---
title: Aggiungere un modello a un'app Razor Pages in ASP.NET Core
author: rick-anderson
description: Scoprire come aggiungere classi per la gestione dei film in un database tramite Entity Framework Core (EF Core).
ms.author: riande
ms.date: 07/22/2019
uid: tutorials/razor-pages/model
ms.openlocfilehash: 39e2a38e0b91b7dbecf05c084ca0be5e312dcb0d
ms.sourcegitcommit: 2719c70cd15a430479ab4007ff3e197fbf5dfee0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/09/2019
ms.locfileid: "68862876"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="a6831-103">Aggiungere un modello a un'app Razor Pages in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a6831-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

<span data-ttu-id="a6831-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a6831-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="a6831-105">In questa sezione si aggiungono alcune classi per la gestione di filmati in un database.</span><span class="sxs-lookup"><span data-stu-id="a6831-105">In this section, classes are added for managing movies in a database.</span></span> <span data-ttu-id="a6831-106">Queste classi vengono usate con [Entity Framework Core](/ef/core) (EF Core) per lavorare in un database.</span><span class="sxs-lookup"><span data-stu-id="a6831-106">These classes are used with [Entity Framework Core](/ef/core) (EF Core) to work with a database.</span></span> <span data-ttu-id="a6831-107">EF Core è un framework ORM (Object-Relational Mapping) che semplifica l'accesso ai dati.</span><span class="sxs-lookup"><span data-stu-id="a6831-107">EF Core is an object-relational mapping (ORM) framework that simplifies data access.</span></span>

<span data-ttu-id="a6831-108">Le classi di modello sono dette classi POCO (da "plain-old CLR objects") perché non hanno alcuna dipendenza in EF Core.</span><span class="sxs-lookup"><span data-stu-id="a6831-108">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="a6831-109">Definiscono le proprietà dei dati archiviati nel database.</span><span class="sxs-lookup"><span data-stu-id="a6831-109">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="a6831-110">Aggiungere un modello di dati</span><span class="sxs-lookup"><span data-stu-id="a6831-110">Add a data model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a6831-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a6831-111">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a6831-112">Fare clic con il pulsante destro del mouse sul progetto **RazorPagesMovie** > **Aggiungi** > **Nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="a6831-112">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="a6831-113">Assegnare il nome *Models* alla cartella.</span><span class="sxs-lookup"><span data-stu-id="a6831-113">Name the folder *Models*.</span></span>

<span data-ttu-id="a6831-114">Fare clic con il pulsante destro del mouse sulla cartella *Models*.</span><span class="sxs-lookup"><span data-stu-id="a6831-114">Right click the *Models* folder.</span></span> <span data-ttu-id="a6831-115">Selezionare **Aggiungi** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="a6831-115">Select **Add** > **Class**.</span></span> <span data-ttu-id="a6831-116">Denominare la classe **Movie**.</span><span class="sxs-lookup"><span data-stu-id="a6831-116">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a6831-117">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a6831-117">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="a6831-118">Aggiungere una cartella denominata *Models*.</span><span class="sxs-lookup"><span data-stu-id="a6831-118">Add a folder named *Models*.</span></span>
* <span data-ttu-id="a6831-119">Aggiungere una classe alla cartella *Models* denominata *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="a6831-119">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="a6831-120">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="a6831-120">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="a6831-121">In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto **RazorPagesMovie**, quindi selezionare **Aggiungi** > **Nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="a6831-121">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="a6831-122">Assegnare il nome *Models* alla cartella.</span><span class="sxs-lookup"><span data-stu-id="a6831-122">Name the folder *Models*.</span></span>
* <span data-ttu-id="a6831-123">Fare clic con il pulsante destro del mouse sulla cartella *Models* e scegliere **Aggiungi** > **Nuovo file**.</span><span class="sxs-lookup"><span data-stu-id="a6831-123">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="a6831-124">Nella finestra di dialogo **Nuovo file**:</span><span class="sxs-lookup"><span data-stu-id="a6831-124">In the **New File** dialog:</span></span>

  * <span data-ttu-id="a6831-125">Selezionare **Generale** nel riquadro a sinistra.</span><span class="sxs-lookup"><span data-stu-id="a6831-125">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="a6831-126">Selezionare **Classe vuota** nel riquadro centrale.</span><span class="sxs-lookup"><span data-stu-id="a6831-126">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="a6831-127">Denominare la classe **Movie** e selezionare **Nuovo**.</span><span class="sxs-lookup"><span data-stu-id="a6831-127">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

---

<span data-ttu-id="a6831-128">Compilare il progetto per verificare che non siano presenti errori di compilazione.</span><span class="sxs-lookup"><span data-stu-id="a6831-128">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="a6831-129">Eseguire lo scaffolding del modello *Movie*</span><span class="sxs-lookup"><span data-stu-id="a6831-129">Scaffold the movie model</span></span>

<span data-ttu-id="a6831-130">In questa sezione viene eseguito lo scaffolding del modello *Movie*.</span><span class="sxs-lookup"><span data-stu-id="a6831-130">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="a6831-131">Lo strumento di scaffolding crea quindi le pagine per le operazioni CRUD (creazione, lettura, aggiornamento ed eliminazione) per il modello *Movie*.</span><span class="sxs-lookup"><span data-stu-id="a6831-131">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a6831-132">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a6831-132">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a6831-133">Creare una cartella *Pages/Movies*:</span><span class="sxs-lookup"><span data-stu-id="a6831-133">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="a6831-134">Fare clic con il pulsante destro del mouse sulla cartella *Pages* > **Aggiungi** > **Nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="a6831-134">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="a6831-135">Assegnare il nome *Movies* alla cartella</span><span class="sxs-lookup"><span data-stu-id="a6831-135">Name the folder *Movies*</span></span>

<span data-ttu-id="a6831-136">Fare clic con il pulsante destro del mouse sulla cartella *Pages/Movies* > **Aggiungi** > **Nuovo elemento di scaffolding**.</span><span class="sxs-lookup"><span data-stu-id="a6831-136">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Immagine relativa alle istruzioni precedenti.](model/_static/sca.png)

<span data-ttu-id="a6831-138">Nella finestra di dialogo **Aggiungi scaffolding** selezionare **Razor che usano Entity Framework (CRUD)** > **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="a6831-138">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Immagine relativa alle istruzioni precedenti.](model/_static/add_scaffold.png)

<span data-ttu-id="a6831-140">Completare la finestra di dialogo **Pagine Razor che usano Entity Framework (CRUD)** :</span><span class="sxs-lookup"><span data-stu-id="a6831-140">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="a6831-141">Nel menu a discesa **Classe modello** selezionare **Movie (RazorPagesMovie.Models)** .</span><span class="sxs-lookup"><span data-stu-id="a6831-141">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="a6831-142">Nella riga **Classe contesto di dati** selezionare il segno più **+** e modificare il nome generato da RazorPagesMovie.**Models**.RazorPagesMovieContext a RazorPagesMovie.**Data**.RazorPagesMovieContext.</span><span class="sxs-lookup"><span data-stu-id="a6831-142">In the **Data context class** row, select the **+** (plus) sign and change the generated name from RazorPagesMovie.**Models**.RazorPagesMovieContext to RazorPagesMovie.**Data**.RazorPagesMovieContext.</span></span> <span data-ttu-id="a6831-143">[Questa modifica](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) non è obbligatoria.</span><span class="sxs-lookup"><span data-stu-id="a6831-143">[This change](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) is not required.</span></span> <span data-ttu-id="a6831-144">Crea la classe del contesto di database con lo spazio dei nomi corretto.</span><span class="sxs-lookup"><span data-stu-id="a6831-144">It creates the database context class with the correct namespace.</span></span>
* <span data-ttu-id="a6831-145">Selezionare **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="a6831-145">Select **Add**.</span></span>

![Immagine relativa alle istruzioni precedenti.](model/_static/3/arp.png)

<span data-ttu-id="a6831-147">Il file *appsettings.json* è stato aggiornato con la stringa di connessione usata per connettersi a un database locale.</span><span class="sxs-lookup"><span data-stu-id="a6831-147">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a6831-148">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a6831-148">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="a6831-149">Aprire una finestra di comando nella directory del progetto (la directory che contiene i file *Program.cs*, *Startup.cs* e *csproj*).</span><span class="sxs-lookup"><span data-stu-id="a6831-149">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="a6831-150">Installare lo strumento di scaffolding:</span><span class="sxs-lookup"><span data-stu-id="a6831-150">Install the scaffolding tool:</span></span>

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator --version 3.0.0-*
   ```

* <span data-ttu-id="a6831-151">**Per Windows**: Eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="a6831-151">**For Windows**: Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="a6831-152">**Per macOS e Linux**: Eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="a6831-152">**For macOS and Linux**: Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="a6831-153">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="a6831-153">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="a6831-154">Aprire una finestra di comando nella directory del progetto (la directory che contiene i file *Program.cs*, *Startup.cs* e *csproj*).</span><span class="sxs-lookup"><span data-stu-id="a6831-154">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="a6831-155">Installare lo strumento di scaffolding:</span><span class="sxs-lookup"><span data-stu-id="a6831-155">Install the scaffolding tool:</span></span>

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator --version 3.0.0-*
   ```

* <span data-ttu-id="a6831-156">Eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="a6831-156">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

---

### <a name="files-created"></a><span data-ttu-id="a6831-157">File creati</span><span class="sxs-lookup"><span data-stu-id="a6831-157">Files created</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a6831-158">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a6831-158">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a6831-159">Il processo di scaffolding crea e aggiorna i file seguenti:</span><span class="sxs-lookup"><span data-stu-id="a6831-159">The scaffold process creates and updates the following files:</span></span>

* <span data-ttu-id="a6831-160">*Pages/Movies*: Crea, Elimina, Dettagli, Modifica e Indice.</span><span class="sxs-lookup"><span data-stu-id="a6831-160">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="a6831-161">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="a6831-161">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="updated"></a><span data-ttu-id="a6831-162">File aggiornati</span><span class="sxs-lookup"><span data-stu-id="a6831-162">Updated</span></span>

* <span data-ttu-id="a6831-163">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="a6831-163">*Startup.cs*</span></span>

<span data-ttu-id="a6831-164">I file creati e aggiornati sono illustrati nella sezione successiva.</span><span class="sxs-lookup"><span data-stu-id="a6831-164">The created and updated files are explained in the next section.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="a6831-165">Visual Studio Code/Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="a6831-165">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="a6831-166">Il processo di scaffolding crea i file seguenti:</span><span class="sxs-lookup"><span data-stu-id="a6831-166">The scaffold process creates the following files:</span></span>

* <span data-ttu-id="a6831-167">*Pages/Movies*: Crea, Elimina, Dettagli, Modifica e Indice.</span><span class="sxs-lookup"><span data-stu-id="a6831-167">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>

<span data-ttu-id="a6831-168">I file creati sono illustrati nella sezione successiva.</span><span class="sxs-lookup"><span data-stu-id="a6831-168">The created files are explained in the next section.</span></span>

---

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="a6831-169">Migrazione iniziale</span><span class="sxs-lookup"><span data-stu-id="a6831-169">Initial migration</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a6831-170">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a6831-170">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a6831-171">In questa sezione viene usata la Console di Gestione pacchetti (PMC) per:</span><span class="sxs-lookup"><span data-stu-id="a6831-171">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="a6831-172">Aggiungere una migrazione iniziale.</span><span class="sxs-lookup"><span data-stu-id="a6831-172">Add an initial migration.</span></span>
* <span data-ttu-id="a6831-173">Aggiornare il database con la migrazione iniziale.</span><span class="sxs-lookup"><span data-stu-id="a6831-173">Update the database with the initial migration.</span></span>

<span data-ttu-id="a6831-174">Dal menu **Strumenti** selezionare **Gestione pacchetti NuGet** >  **Console di Gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="a6831-174">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Menu della Console di Gestione pacchetti](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="a6831-176">Nella Console di Gestione pacchetti immettere i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="a6831-176">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration InitialCreate
Update-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a6831-177">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a6831-177">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="a6831-178">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="a6831-178">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---

<span data-ttu-id="a6831-179">I comandi precedenti generano l'avviso seguente: "No type was specified for the decimal column 'Price' on entity type 'Movie'. (Nessun tipo specificato per la colonna decimale 'Price' nel tipo di entità 'Movie').</span><span class="sxs-lookup"><span data-stu-id="a6831-179">The preceding commands generate the following warning: "No type was specified for the decimal column 'Price' on entity type 'Movie'.</span></span> <span data-ttu-id="a6831-180">This will cause values to be silently truncated if they do not fit in the default precision and scale. (I valori saranno quindi automaticamente troncati se non rispettano la precisione e la scala predefinite).</span><span class="sxs-lookup"><span data-stu-id="a6831-180">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span></span> <span data-ttu-id="a6831-181">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'. (Specificare in modo esplicito il tipo di colonna di SQL Server che può supportare tutti i valori usando 'HasColumnType()')"</span><span class="sxs-lookup"><span data-stu-id="a6831-181">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'."</span></span>

<span data-ttu-id="a6831-182">È possibile ignorare tale avviso. Verrà risolto in un'esercitazione successiva.</span><span class="sxs-lookup"><span data-stu-id="a6831-182">You can ignore that warning, it will be fixed in a later tutorial.</span></span>

<span data-ttu-id="a6831-183">Il comando `ef migrations add InitialCreate` genera un codice per creare lo schema del database iniziale.</span><span class="sxs-lookup"><span data-stu-id="a6831-183">The `ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span> <span data-ttu-id="a6831-184">Lo schema è basato sul modello specificato in `DbContext` (nel file *RazorPagesMovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="a6831-184">The schema is based on the model specified in the `DbContext` (In the *RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="a6831-185">L'argomento `InitialCreate` viene usato per denominare le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="a6831-185">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="a6831-186">È possibile usare qualsiasi nome, ma per convenzione viene selezionato un nome che descrive la migrazione.</span><span class="sxs-lookup"><span data-stu-id="a6831-186">Any name can be used, but by convention a name is selected that describes the migration.</span></span>

<span data-ttu-id="a6831-187">Il comando `ef database update` esegue il metodo `Up` nel file *Migrations/\<time-stamp>_InitialCreate.cs*.</span><span class="sxs-lookup"><span data-stu-id="a6831-187">The `ef database update` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="a6831-188">Il metodo `Up` crea il database.</span><span class="sxs-lookup"><span data-stu-id="a6831-188">The `Up` method creates the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a6831-189">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a6831-189">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="a6831-190">Esaminare il contesto registrato con l'inserimento di dipendenze</span><span class="sxs-lookup"><span data-stu-id="a6831-190">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="a6831-191">ASP.NET Core viene compilato tramite [dependency injection](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="a6831-191">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="a6831-192">I servizi, ad esempio il contesto di database di Entity Framework Core, vengono registrati tramite dependency injection durante l'avvio dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="a6831-192">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="a6831-193">Questi servizi vengono quindi offerti ai componenti per cui sono necessari (ad esempio Razor Pages) tramite i parametri del costruttore.</span><span class="sxs-lookup"><span data-stu-id="a6831-193">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="a6831-194">Più avanti nell'esercitazione viene illustrato il codice del costruttore che ottiene un'istanza del contesto di database.</span><span class="sxs-lookup"><span data-stu-id="a6831-194">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="a6831-195">Lo strumento di scaffolding ha creato automaticamente un contesto del database e lo ha registrato per la dependency injection.</span><span class="sxs-lookup"><span data-stu-id="a6831-195">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="a6831-196">Esaminare il metodo `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="a6831-196">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="a6831-197">La riga evidenziata è stata aggiunta dallo scaffolder:</span><span class="sxs-lookup"><span data-stu-id="a6831-197">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_ConfigureServices&highlight=5-6)]

<span data-ttu-id="a6831-198">`RazorPagesMovieContext` coordina la funzionalità di EF Core (Create, Read, Update, Delete e così via) per il modello `Movie`.</span><span class="sxs-lookup"><span data-stu-id="a6831-198">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="a6831-199">Il contesto dei dati (`RazorPagesMovieContext`) è derivato da [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="a6831-199">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="a6831-200">Il contesto dei dati specifica le entità incluse nel modello di dati.</span><span class="sxs-lookup"><span data-stu-id="a6831-200">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="a6831-201">il codice precedente crea una proprietà [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) per il set di entità.</span><span class="sxs-lookup"><span data-stu-id="a6831-201">The preceding code creates a [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="a6831-202">Nella terminologia di Entity Framework, un set di entità corrisponde in genere alla tabella di un database.</span><span class="sxs-lookup"><span data-stu-id="a6831-202">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="a6831-203">Un'entità corrisponde a una riga nella tabella.</span><span class="sxs-lookup"><span data-stu-id="a6831-203">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="a6831-204">Il nome della stringa di connessione viene passato al contesto chiamando un metodo in un oggetto [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions).</span><span class="sxs-lookup"><span data-stu-id="a6831-204">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="a6831-205">Per lo sviluppo locale, il [sistema di configurazione di ASP.NET Core](xref:fundamentals/configuration/index) legge la stringa di connessione dal file *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="a6831-205">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a6831-206">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a6831-206">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="a6831-207">Esaminare il metodo `Up`.</span><span class="sxs-lookup"><span data-stu-id="a6831-207">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="a6831-208">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="a6831-208">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="a6831-209">Esaminare il metodo `Up`.</span><span class="sxs-lookup"><span data-stu-id="a6831-209">Examine the `Up` method.</span></span>

---

<span data-ttu-id="a6831-210">Il comando `Add-Migration` genera un codice per creare lo schema del database iniziale.</span><span class="sxs-lookup"><span data-stu-id="a6831-210">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="a6831-211">Lo schema è basato sul modello specificato in `RazorPagesMovieContext` (nel file *Data/RazorPagesMovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="a6831-211">The schema is based on the model specified in the `RazorPagesMovieContext` (In the *Data/RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="a6831-212">L'argomento `Initial` viene usato per denominare le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="a6831-212">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="a6831-213">È possibile usare qualsiasi nome, ma per convenzione viene usato un nome che descrive la migrazione.</span><span class="sxs-lookup"><span data-stu-id="a6831-213">Any name can be used, but by convention a name that describes the migration is used.</span></span> <span data-ttu-id="a6831-214">Per altre informazioni, vedere <xref:data/ef-mvc/migrations>.</span><span class="sxs-lookup"><span data-stu-id="a6831-214">For more information, see <xref:data/ef-mvc/migrations>.</span></span>

<span data-ttu-id="a6831-215">Il comando `Update-Database` esegue il metodo `Up` nel file *Migrations/{time-stamp}_InitialCreate.cs*, che crea il database.</span><span class="sxs-lookup"><span data-stu-id="a6831-215">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="a6831-216">Eseguire il test dell'app</span><span class="sxs-lookup"><span data-stu-id="a6831-216">Test the app</span></span>

* <span data-ttu-id="a6831-217">Eseguire l'app e accodare `/Movies` all'URL nel browser (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="a6831-217">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="a6831-218">Se viene visualizzato l'errore:</span><span class="sxs-lookup"><span data-stu-id="a6831-218">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="a6831-219">Non è stato eseguita la [migrazione](#pmc).</span><span class="sxs-lookup"><span data-stu-id="a6831-219">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="a6831-220">Eseguire il test del collegamento **Crea**.</span><span class="sxs-lookup"><span data-stu-id="a6831-220">Test the **Create** link.</span></span>

  ![Pagina Crea](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="a6831-222">Potrebbe non essere possibile immettere virgole decimali nel campo `Price`.</span><span class="sxs-lookup"><span data-stu-id="a6831-222">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="a6831-223">Per supportare la [convalida jQuery](https://jqueryvalidation.org/) per impostazioni locali diverse dall'inglese che usano la virgola (",") come separatore decimale e per formati di data diversi da quello dell'inglese (Stati Uniti), è necessario localizzare l'app.</span><span class="sxs-lookup"><span data-stu-id="a6831-223">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="a6831-224">Per istruzioni sulla localizzazione, vedere [questo problema su GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="a6831-224">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="a6831-225">Eseguire il test dei collegamenti **Modifica**, **Dettagli** e **Elimina**.</span><span class="sxs-lookup"><span data-stu-id="a6831-225">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="a6831-226">L'esercitazione successiva illustra i file creati tramite scaffolding.</span><span class="sxs-lookup"><span data-stu-id="a6831-226">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a6831-227">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="a6831-227">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a6831-228">[Precedente: Introduzione](xref:tutorials/razor-pages/razor-pages-start)
> [Avanti: Pagine Razor create tramite scaffolding](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="a6831-228">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end

<!--  ::: moniker previous version   -->
::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="a6831-229">In questa sezione si aggiungono alcune classi per la gestione di filmati in un database.</span><span class="sxs-lookup"><span data-stu-id="a6831-229">In this section, classes are added for managing movies in a database.</span></span> <span data-ttu-id="a6831-230">Queste classi vengono usate con [Entity Framework Core](/ef/core) (EF Core) per lavorare in un database.</span><span class="sxs-lookup"><span data-stu-id="a6831-230">These classes are used with [Entity Framework Core](/ef/core) (EF Core) to work with a database.</span></span> <span data-ttu-id="a6831-231">EF Core è un framework object-relational mapping (ORM) che semplifica il codice di accesso ai dati.</span><span class="sxs-lookup"><span data-stu-id="a6831-231">EF Core is an object-relational mapping (ORM) framework that simplifies data access code.</span></span>

<span data-ttu-id="a6831-232">Le classi di modello sono dette classi POCO (da "plain-old CLR objects") perché non hanno alcuna dipendenza in EF Core.</span><span class="sxs-lookup"><span data-stu-id="a6831-232">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="a6831-233">Definiscono le proprietà dei dati archiviati nel database.</span><span class="sxs-lookup"><span data-stu-id="a6831-233">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="a6831-234">Aggiungere un modello di dati</span><span class="sxs-lookup"><span data-stu-id="a6831-234">Add a data model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a6831-235">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a6831-235">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a6831-236">Fare clic con il pulsante destro del mouse sul progetto **RazorPagesMovie** > **Aggiungi** > **Nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="a6831-236">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="a6831-237">Assegnare il nome *Models* alla cartella.</span><span class="sxs-lookup"><span data-stu-id="a6831-237">Name the folder *Models*.</span></span>

<span data-ttu-id="a6831-238">Fare clic con il pulsante destro del mouse sulla cartella *Models*.</span><span class="sxs-lookup"><span data-stu-id="a6831-238">Right click the *Models* folder.</span></span> <span data-ttu-id="a6831-239">Selezionare **Aggiungi** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="a6831-239">Select **Add** > **Class**.</span></span> <span data-ttu-id="a6831-240">Denominare la classe **Movie**.</span><span class="sxs-lookup"><span data-stu-id="a6831-240">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a6831-241">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a6831-241">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="a6831-242">Aggiungere una cartella denominata *Models*.</span><span class="sxs-lookup"><span data-stu-id="a6831-242">Add a folder named *Models*.</span></span>
* <span data-ttu-id="a6831-243">Aggiungere una classe alla cartella *Models* denominata *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="a6831-243">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="a6831-244">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="a6831-244">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="a6831-245">In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto **RazorPagesMovie**, quindi selezionare **Aggiungi** > **Nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="a6831-245">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="a6831-246">Assegnare il nome *Models* alla cartella.</span><span class="sxs-lookup"><span data-stu-id="a6831-246">Name the folder *Models*.</span></span>
* <span data-ttu-id="a6831-247">Fare clic con il pulsante destro del mouse sulla cartella *Models* e scegliere **Aggiungi** > **Nuovo file**.</span><span class="sxs-lookup"><span data-stu-id="a6831-247">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="a6831-248">Nella finestra di dialogo **Nuovo file**:</span><span class="sxs-lookup"><span data-stu-id="a6831-248">In the **New File** dialog:</span></span>

  * <span data-ttu-id="a6831-249">Selezionare **Generale** nel riquadro a sinistra.</span><span class="sxs-lookup"><span data-stu-id="a6831-249">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="a6831-250">Selezionare **Classe vuota** nel riquadro centrale.</span><span class="sxs-lookup"><span data-stu-id="a6831-250">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="a6831-251">Denominare la classe **Movie** e selezionare **Nuovo**.</span><span class="sxs-lookup"><span data-stu-id="a6831-251">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

---

<span data-ttu-id="a6831-252">Compilare il progetto per verificare che non siano presenti errori di compilazione.</span><span class="sxs-lookup"><span data-stu-id="a6831-252">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="a6831-253">Eseguire lo scaffolding del modello *Movie*</span><span class="sxs-lookup"><span data-stu-id="a6831-253">Scaffold the movie model</span></span>

<span data-ttu-id="a6831-254">In questa sezione viene eseguito lo scaffolding del modello *Movie*.</span><span class="sxs-lookup"><span data-stu-id="a6831-254">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="a6831-255">Lo strumento di scaffolding crea quindi le pagine per le operazioni CRUD (creazione, lettura, aggiornamento ed eliminazione) per il modello *Movie*.</span><span class="sxs-lookup"><span data-stu-id="a6831-255">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a6831-256">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a6831-256">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a6831-257">Creare una cartella *Pages/Movies*:</span><span class="sxs-lookup"><span data-stu-id="a6831-257">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="a6831-258">Fare clic con il pulsante destro del mouse sulla cartella *Pages* > **Aggiungi** > **Nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="a6831-258">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="a6831-259">Assegnare il nome *Movies* alla cartella</span><span class="sxs-lookup"><span data-stu-id="a6831-259">Name the folder *Movies*</span></span>

<span data-ttu-id="a6831-260">Fare clic con il pulsante destro del mouse sulla cartella *Pages/Movies* > **Aggiungi** > **Nuovo elemento di scaffolding**.</span><span class="sxs-lookup"><span data-stu-id="a6831-260">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Immagine relativa alle istruzioni precedenti.](model/_static/sca.png)

<span data-ttu-id="a6831-262">Nella finestra di dialogo **Aggiungi scaffolding** selezionare **Razor che usano Entity Framework (CRUD)** > **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="a6831-262">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Immagine relativa alle istruzioni precedenti.](model/_static/add_scaffold.png)

<span data-ttu-id="a6831-264">Completare la finestra di dialogo **Pagine Razor che usano Entity Framework (CRUD)** :</span><span class="sxs-lookup"><span data-stu-id="a6831-264">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>
<!-- In the next section, change 
(plus) sign and accept the generated name 
to use Data, it should not use models. That will make the namespace the same for the VS version and the CLI version
-->

* <span data-ttu-id="a6831-265">Nel menu a discesa **Classe modello** selezionare **Movie (RazorPagesMovie.Models)** .</span><span class="sxs-lookup"><span data-stu-id="a6831-265">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="a6831-266">Nella riga **Classe contesto di dati** selezionare il segno più **+** e accettare il nome generato **RazorPagesMovie.Models.RazorPagesMovieContext**.</span><span class="sxs-lookup"><span data-stu-id="a6831-266">In the **Data context class** row, select the **+** (plus) sign and accept the generated name **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="a6831-267">Selezionare **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="a6831-267">Select **Add**.</span></span>

![Immagine relativa alle istruzioni precedenti.](model/_static/arp.png)

<span data-ttu-id="a6831-269">Il file *appsettings.json* è stato aggiornato con la stringa di connessione usata per connettersi a un database locale.</span><span class="sxs-lookup"><span data-stu-id="a6831-269">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a6831-270">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a6831-270">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="a6831-271">Aprire una finestra di comando nella directory del progetto (la directory che contiene i file *Program.cs*, *Startup.cs* e *csproj*).</span><span class="sxs-lookup"><span data-stu-id="a6831-271">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="a6831-272">Installare lo strumento di scaffolding:</span><span class="sxs-lookup"><span data-stu-id="a6831-272">Install the scaffolding tool:</span></span>

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="a6831-273">**Per Windows**: Eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="a6831-273">**For Windows**: Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="a6831-274">**Per macOS e Linux**: Eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="a6831-274">**For macOS and Linux**: Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="a6831-275">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="a6831-275">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="a6831-276">Aprire una finestra di comando nella directory del progetto (la directory che contiene i file *Program.cs*, *Startup.cs* e *csproj*).</span><span class="sxs-lookup"><span data-stu-id="a6831-276">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="a6831-277">Installare lo strumento di scaffolding:</span><span class="sxs-lookup"><span data-stu-id="a6831-277">Install the scaffolding tool:</span></span>

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="a6831-278">Eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="a6831-278">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

---

<span data-ttu-id="a6831-279">Il processo di scaffolding crea e aggiorna i file seguenti:</span><span class="sxs-lookup"><span data-stu-id="a6831-279">The scaffold process creates and updates the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="a6831-280">File creati</span><span class="sxs-lookup"><span data-stu-id="a6831-280">Files created</span></span>

* <span data-ttu-id="a6831-281">*Pages/Movies*: Crea, Elimina, Dettagli, Modifica e Indice.</span><span class="sxs-lookup"><span data-stu-id="a6831-281">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="a6831-282">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="a6831-282">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="file-updated"></a><span data-ttu-id="a6831-283">File aggiornato</span><span class="sxs-lookup"><span data-stu-id="a6831-283">File updated</span></span>

* <span data-ttu-id="a6831-284">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="a6831-284">*Startup.cs*</span></span>

<span data-ttu-id="a6831-285">I file creati e aggiornati sono illustrati nella sezione successiva.</span><span class="sxs-lookup"><span data-stu-id="a6831-285">The created and updated files are explained in the next section.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="a6831-286">Migrazione iniziale</span><span class="sxs-lookup"><span data-stu-id="a6831-286">Initial migration</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a6831-287">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a6831-287">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="a6831-288">In questa sezione viene usata la Console di Gestione pacchetti (PMC) per:</span><span class="sxs-lookup"><span data-stu-id="a6831-288">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="a6831-289">Aggiungere una migrazione iniziale.</span><span class="sxs-lookup"><span data-stu-id="a6831-289">Add an initial migration.</span></span>
* <span data-ttu-id="a6831-290">Aggiornare il database con la migrazione iniziale.</span><span class="sxs-lookup"><span data-stu-id="a6831-290">Update the database with the initial migration.</span></span>

<span data-ttu-id="a6831-291">Dal menu **Strumenti** selezionare **Gestione pacchetti NuGet** >  **Console di Gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="a6831-291">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Menu della Console di Gestione pacchetti](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="a6831-293">Nella Console di Gestione pacchetti immettere i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="a6831-293">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration Initial
Update-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a6831-294">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a6831-294">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="a6831-295">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="a6831-295">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---

<span data-ttu-id="a6831-296">I comandi precedenti generano l'avviso seguente: "No type was specified for the decimal column 'Price' on entity type 'Movie'. (Nessun tipo specificato per la colonna decimale 'Price' nel tipo di entità 'Movie').</span><span class="sxs-lookup"><span data-stu-id="a6831-296">The preceding commands generate the following warning: "No type was specified for the decimal column 'Price' on entity type 'Movie'.</span></span> <span data-ttu-id="a6831-297">This will cause values to be silently truncated if they do not fit in the default precision and scale. (I valori saranno quindi automaticamente troncati se non rispettano la precisione e la scala predefinite).</span><span class="sxs-lookup"><span data-stu-id="a6831-297">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span></span> <span data-ttu-id="a6831-298">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'. (Specificare in modo esplicito il tipo di colonna di SQL Server che può supportare tutti i valori usando 'HasColumnType()')"</span><span class="sxs-lookup"><span data-stu-id="a6831-298">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'."</span></span>

<span data-ttu-id="a6831-299">È possibile ignorare tale avviso. Verrà risolto in un'esercitazione successiva.</span><span class="sxs-lookup"><span data-stu-id="a6831-299">You can ignore that warning, it will be fixed in a later tutorial.</span></span>

<span data-ttu-id="a6831-300">Il comando `ef migrations add InitialCreate` genera un codice per creare lo schema del database iniziale.</span><span class="sxs-lookup"><span data-stu-id="a6831-300">The `ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span> <span data-ttu-id="a6831-301">Lo schema è basato sul modello specificato in `DbContext` (nel file *RazorPagesMovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="a6831-301">The schema is based on the model specified in the `DbContext` (In the *RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="a6831-302">L'argomento `InitialCreate` viene usato per denominare le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="a6831-302">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="a6831-303">È possibile usare qualsiasi nome, ma per convenzione viene selezionato un nome che descrive la migrazione.</span><span class="sxs-lookup"><span data-stu-id="a6831-303">Any name can be used, but by convention a name is selected that describes the migration.</span></span>

<span data-ttu-id="a6831-304">Il comando `ef database update` esegue il metodo `Up` nel file *Migrations/\<time-stamp>_InitialCreate.cs*.</span><span class="sxs-lookup"><span data-stu-id="a6831-304">The `ef database update` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="a6831-305">Il metodo `Up` crea il database.</span><span class="sxs-lookup"><span data-stu-id="a6831-305">The `Up` method creates the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a6831-306">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a6831-306">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="a6831-307">Esaminare il contesto registrato con l'inserimento di dipendenze</span><span class="sxs-lookup"><span data-stu-id="a6831-307">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="a6831-308">ASP.NET Core viene compilato tramite [dependency injection](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="a6831-308">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="a6831-309">I servizi, ad esempio il contesto di database di Entity Framework Core, vengono registrati tramite dependency injection durante l'avvio dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="a6831-309">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="a6831-310">Questi servizi vengono quindi offerti ai componenti per cui sono necessari (ad esempio Razor Pages) tramite i parametri del costruttore.</span><span class="sxs-lookup"><span data-stu-id="a6831-310">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="a6831-311">Più avanti nell'esercitazione viene illustrato il codice del costruttore che ottiene un'istanza del contesto di database.</span><span class="sxs-lookup"><span data-stu-id="a6831-311">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="a6831-312">Lo strumento di scaffolding ha creato automaticamente un contesto del database e lo ha registrato per la dependency injection.</span><span class="sxs-lookup"><span data-stu-id="a6831-312">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="a6831-313">Esaminare il metodo `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="a6831-313">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="a6831-314">La riga evidenziata è stata aggiunta dallo scaffolder:</span><span class="sxs-lookup"><span data-stu-id="a6831-314">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

<span data-ttu-id="a6831-315">`RazorPagesMovieContext` coordina la funzionalità di EF Core (Create, Read, Update, Delete e così via) per il modello `Movie`.</span><span class="sxs-lookup"><span data-stu-id="a6831-315">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="a6831-316">Il contesto dei dati (`RazorPagesMovieContext`) è derivato da [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="a6831-316">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="a6831-317">Il contesto dei dati specifica le entità incluse nel modello di dati.</span><span class="sxs-lookup"><span data-stu-id="a6831-317">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="a6831-318">il codice precedente crea una proprietà [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) per il set di entità.</span><span class="sxs-lookup"><span data-stu-id="a6831-318">The preceding code creates a [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="a6831-319">Nella terminologia di Entity Framework, un set di entità corrisponde in genere alla tabella di un database.</span><span class="sxs-lookup"><span data-stu-id="a6831-319">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="a6831-320">Un'entità corrisponde a una riga nella tabella.</span><span class="sxs-lookup"><span data-stu-id="a6831-320">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="a6831-321">Il nome della stringa di connessione viene passato al contesto chiamando un metodo in un oggetto [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions).</span><span class="sxs-lookup"><span data-stu-id="a6831-321">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="a6831-322">Per lo sviluppo locale, il [sistema di configurazione di ASP.NET Core](xref:fundamentals/configuration/index) legge la stringa di connessione dal file *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="a6831-322">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="a6831-323">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="a6831-323">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="a6831-324">Esaminare il metodo `Up`.</span><span class="sxs-lookup"><span data-stu-id="a6831-324">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="a6831-325">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="a6831-325">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="a6831-326">Esaminare il metodo `Up`.</span><span class="sxs-lookup"><span data-stu-id="a6831-326">Examine the `Up` method.</span></span>

---

<span data-ttu-id="a6831-327">Il comando `Add-Migration` genera un codice per creare lo schema del database iniziale.</span><span class="sxs-lookup"><span data-stu-id="a6831-327">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="a6831-328">Lo schema è basato sul modello specificato in `RazorPagesMovieContext` (nel file *Data/RazorPagesMovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="a6831-328">The schema is based on the model specified in the `RazorPagesMovieContext` (In the *Data/RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="a6831-329">L'argomento `Initial` viene usato per denominare le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="a6831-329">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="a6831-330">È possibile usare qualsiasi nome, ma per convenzione viene usato un nome che descrive la migrazione.</span><span class="sxs-lookup"><span data-stu-id="a6831-330">Any name can be used, but by convention a name that describes the migration is used.</span></span> <span data-ttu-id="a6831-331">Per altre informazioni, vedere <xref:data/ef-mvc/migrations>.</span><span class="sxs-lookup"><span data-stu-id="a6831-331">For more information, see <xref:data/ef-mvc/migrations>.</span></span>

<span data-ttu-id="a6831-332">Il comando `Update-Database` esegue il metodo `Up` nel file *Migrations/{time-stamp}_InitialCreate.cs*, che crea il database.</span><span class="sxs-lookup"><span data-stu-id="a6831-332">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="a6831-333">Eseguire il test dell'app</span><span class="sxs-lookup"><span data-stu-id="a6831-333">Test the app</span></span>

* <span data-ttu-id="a6831-334">Eseguire l'app e accodare `/Movies` all'URL nel browser (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="a6831-334">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="a6831-335">Se viene visualizzato l'errore:</span><span class="sxs-lookup"><span data-stu-id="a6831-335">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="a6831-336">Non è stato eseguita la [migrazione](#pmc).</span><span class="sxs-lookup"><span data-stu-id="a6831-336">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="a6831-337">Eseguire il test del collegamento **Crea**.</span><span class="sxs-lookup"><span data-stu-id="a6831-337">Test the **Create** link.</span></span>

  ![Pagina Crea](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="a6831-339">Potrebbe non essere possibile immettere virgole decimali nel campo `Price`.</span><span class="sxs-lookup"><span data-stu-id="a6831-339">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="a6831-340">Per supportare la [convalida jQuery](https://jqueryvalidation.org/) per impostazioni locali diverse dall'inglese che usano la virgola (",") come separatore decimale e per formati di data diversi da quello dell'inglese (Stati Uniti), è necessario localizzare l'app.</span><span class="sxs-lookup"><span data-stu-id="a6831-340">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="a6831-341">Per istruzioni sulla localizzazione, vedere [questo problema su GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="a6831-341">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="a6831-342">Eseguire il test dei collegamenti **Modifica**, **Dettagli** e **Elimina**.</span><span class="sxs-lookup"><span data-stu-id="a6831-342">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="a6831-343">L'esercitazione successiva illustra i file creati tramite scaffolding.</span><span class="sxs-lookup"><span data-stu-id="a6831-343">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a6831-344">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="a6831-344">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a6831-345">[Precedente: Introduzione](xref:tutorials/razor-pages/razor-pages-start)
> [Avanti: Pagine Razor create tramite scaffolding](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="a6831-345">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end
