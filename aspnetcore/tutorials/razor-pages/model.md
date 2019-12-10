---
title: Aggiungere un modello a un'app Razor Pages in ASP.NET Core
author: rick-anderson
description: Scoprire come aggiungere classi per la gestione dei film in un database tramite Entity Framework Core (EF Core).
ms.author: riande
ms.date: 12/05/2019
uid: tutorials/razor-pages/model
ms.openlocfilehash: 95b6d3e016edcd2e13207c8e658cf0d2fb21f945
ms.sourcegitcommit: 4e3edff24ba6e43a103fee1b126c9826241bb37b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/10/2019
ms.locfileid: "74959080"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="008d1-103">Aggiungere un modello a un'app Razor Pages in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="008d1-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

<span data-ttu-id="008d1-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="008d1-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="008d1-105">In questa sezione vengono aggiunte le classi per la gestione dei film in un [database SQLite](https://www.sqlite.org/index.html)multipiattaforma.</span><span class="sxs-lookup"><span data-stu-id="008d1-105">In this section, classes are added for managing movies in a cross-platform [SQLite database](https://www.sqlite.org/index.html).</span></span> <span data-ttu-id="008d1-106">Le app create da un modello di ASP.NET Core usano un database SQLite.</span><span class="sxs-lookup"><span data-stu-id="008d1-106">Apps created from an ASP.NET Core template use a SQLite database.</span></span> <span data-ttu-id="008d1-107">Le classi modello dell'app vengono usate con [Entity Framework Core (EF Core)](/ef/core) ([SQLite EF Core provider di database](/ef/core/providers/sqlite)) per lavorare con il database.</span><span class="sxs-lookup"><span data-stu-id="008d1-107">The app's model classes are used with [Entity Framework Core (EF Core)](/ef/core) ([SQLite EF Core Database Provider](/ef/core/providers/sqlite)) to work with the database.</span></span> <span data-ttu-id="008d1-108">EF Core è un framework ORM (Object-Relational Mapping) che semplifica l'accesso ai dati.</span><span class="sxs-lookup"><span data-stu-id="008d1-108">EF Core is an object-relational mapping (ORM) framework that simplifies data access.</span></span>

<span data-ttu-id="008d1-109">Le classi di modello sono dette classi POCO (da "plain-old CLR objects") perché non hanno alcuna dipendenza in EF Core.</span><span class="sxs-lookup"><span data-stu-id="008d1-109">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="008d1-110">Definiscono le proprietà dei dati archiviati nel database.</span><span class="sxs-lookup"><span data-stu-id="008d1-110">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="008d1-111">Aggiungere un modello di dati</span><span class="sxs-lookup"><span data-stu-id="008d1-111">Add a data model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="008d1-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="008d1-112">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="008d1-113">Fare clic con il pulsante destro del mouse sul progetto **RazorPagesMovie** > **Aggiungi** > **Nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="008d1-113">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="008d1-114">Assegnare il nome *Models* alla cartella.</span><span class="sxs-lookup"><span data-stu-id="008d1-114">Name the folder *Models*.</span></span>

<span data-ttu-id="008d1-115">Fare clic con il pulsante destro del mouse sulla cartella *Models*.</span><span class="sxs-lookup"><span data-stu-id="008d1-115">Right click the *Models* folder.</span></span> <span data-ttu-id="008d1-116">Selezionare **Aggiungi** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="008d1-116">Select **Add** > **Class**.</span></span> <span data-ttu-id="008d1-117">Denominare la classe **Movie**.</span><span class="sxs-lookup"><span data-stu-id="008d1-117">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="008d1-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="008d1-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="008d1-119">Aggiungere una cartella denominata *Modelli*.</span><span class="sxs-lookup"><span data-stu-id="008d1-119">Add a folder named *Models*.</span></span>
* <span data-ttu-id="008d1-120">Aggiungere una classe alla cartella *Models* denominata *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="008d1-120">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="008d1-121">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="008d1-121">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="008d1-122">In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto **RazorPagesMovie**, quindi selezionare **Aggiungi** > **Nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="008d1-122">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="008d1-123">Assegnare il nome *Models* alla cartella.</span><span class="sxs-lookup"><span data-stu-id="008d1-123">Name the folder *Models*.</span></span>
* <span data-ttu-id="008d1-124">Fare clic con il pulsante destro del mouse sulla cartella *Models* e scegliere **Aggiungi** > **Nuovo file**.</span><span class="sxs-lookup"><span data-stu-id="008d1-124">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="008d1-125">Nella finestra di dialogo **Nuovo file**:</span><span class="sxs-lookup"><span data-stu-id="008d1-125">In the **New File** dialog:</span></span>

  * <span data-ttu-id="008d1-126">Selezionare **Generale** nel riquadro a sinistra.</span><span class="sxs-lookup"><span data-stu-id="008d1-126">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="008d1-127">Selezionare **Classe vuota** nel riquadro centrale.</span><span class="sxs-lookup"><span data-stu-id="008d1-127">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="008d1-128">Denominare la classe **Movie** e selezionare **Nuovo**.</span><span class="sxs-lookup"><span data-stu-id="008d1-128">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

---

<span data-ttu-id="008d1-129">Compilare il progetto per verificare che non siano presenti errori di compilazione.</span><span class="sxs-lookup"><span data-stu-id="008d1-129">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="008d1-130">Eseguire lo scaffolding del modello *Movie*</span><span class="sxs-lookup"><span data-stu-id="008d1-130">Scaffold the movie model</span></span>

<span data-ttu-id="008d1-131">In questa sezione viene eseguito lo scaffolding del modello *Movie*.</span><span class="sxs-lookup"><span data-stu-id="008d1-131">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="008d1-132">Lo strumento di scaffolding crea quindi le pagine per le operazioni CRUD (creazione, lettura, aggiornamento ed eliminazione) per il modello *Movie*.</span><span class="sxs-lookup"><span data-stu-id="008d1-132">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="008d1-133">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="008d1-133">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="008d1-134">Creare una cartella *Pages/Movies*:</span><span class="sxs-lookup"><span data-stu-id="008d1-134">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="008d1-135">Fare clic con il pulsante destro del mouse sulla cartella *Pages* > **Aggiungi** > **Nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="008d1-135">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="008d1-136">Assegnare il nome *Movies* alla cartella</span><span class="sxs-lookup"><span data-stu-id="008d1-136">Name the folder *Movies*</span></span>

<span data-ttu-id="008d1-137">Fare clic con il pulsante destro del mouse sulla cartella *Pages/Movies* > **Aggiungi** > **Nuovo elemento di scaffolding**.</span><span class="sxs-lookup"><span data-stu-id="008d1-137">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Immagine relativa alle istruzioni precedenti.](model/_static/sca.png)

<span data-ttu-id="008d1-139">Nella finestra di dialogo **Aggiungi scaffolding** selezionare **Razor che usano Entity Framework (CRUD)** > **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="008d1-139">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Immagine relativa alle istruzioni precedenti.](model/_static/add_scaffold.png)

<span data-ttu-id="008d1-141">Completare la finestra di dialogo **Pagine Razor che usano Entity Framework (CRUD)** :</span><span class="sxs-lookup"><span data-stu-id="008d1-141">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="008d1-142">Nel menu a discesa **Classe modello** selezionare **Movie (RazorPagesMovie.Models)** .</span><span class="sxs-lookup"><span data-stu-id="008d1-142">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="008d1-143">Nella riga **Classe contesto di dati** selezionare il segno più **+** e modificare il nome generato da RazorPagesMovie.**Models**.RazorPagesMovieContext a RazorPagesMovie.**Data**.RazorPagesMovieContext.</span><span class="sxs-lookup"><span data-stu-id="008d1-143">In the **Data context class** row, select the **+** (plus) sign and change the generated name from RazorPagesMovie.**Models**.RazorPagesMovieContext to RazorPagesMovie.**Data**.RazorPagesMovieContext.</span></span> <span data-ttu-id="008d1-144">[Questa modifica](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) non è obbligatoria.</span><span class="sxs-lookup"><span data-stu-id="008d1-144">[This change](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) is not required.</span></span> <span data-ttu-id="008d1-145">Crea la classe del contesto di database con lo spazio dei nomi corretto.</span><span class="sxs-lookup"><span data-stu-id="008d1-145">It creates the database context class with the correct namespace.</span></span>
* <span data-ttu-id="008d1-146">Selezionare **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="008d1-146">Select **Add**.</span></span>

![Immagine relativa alle istruzioni precedenti.](model/_static/3/arp.png)

<span data-ttu-id="008d1-148">Il file *appsettings.json* è stato aggiornato con la stringa di connessione usata per connettersi a un database locale.</span><span class="sxs-lookup"><span data-stu-id="008d1-148">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="008d1-149">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="008d1-149">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="008d1-150">Aprire una finestra di comando nella directory del progetto (la directory che contiene i file *Program.cs*, *Startup.cs* e *csproj*).</span><span class="sxs-lookup"><span data-stu-id="008d1-150">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="008d1-151">Installare lo strumento di scaffolding:</span><span class="sxs-lookup"><span data-stu-id="008d1-151">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="008d1-152">**Per Windows**: eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="008d1-152">**For Windows**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="008d1-153">**Per MacOS e Linux**: eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="008d1-153">**For macOS and Linux**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

[!INCLUDE [use SQL Server in production](~/includes/RP/sqlitedev.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="008d1-154">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="008d1-154">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="008d1-155">Aprire una finestra di comando nella directory del progetto (la directory che contiene i file *Program.cs*, *Startup.cs* e *csproj*).</span><span class="sxs-lookup"><span data-stu-id="008d1-155">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="008d1-156">Installare lo strumento di scaffolding:</span><span class="sxs-lookup"><span data-stu-id="008d1-156">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="008d1-157">Eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="008d1-157">Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

[!INCLUDE [use SQL Server in production](~/includes/RP/sqlitedev.md)]

---

### <a name="files-created"></a><span data-ttu-id="008d1-158">File creati</span><span class="sxs-lookup"><span data-stu-id="008d1-158">Files created</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="008d1-159">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="008d1-159">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="008d1-160">Il processo di scaffolding crea e aggiorna i file seguenti:</span><span class="sxs-lookup"><span data-stu-id="008d1-160">The scaffold process creates and updates the following files:</span></span>

* <span data-ttu-id="008d1-161">*Pages/Movies*: pagine Create (Crea), Delete (Elimina), Details (Dettagli), Edit (Modifica) e Index (Indice).</span><span class="sxs-lookup"><span data-stu-id="008d1-161">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="008d1-162">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="008d1-162">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="updated"></a><span data-ttu-id="008d1-163">File aggiornati</span><span class="sxs-lookup"><span data-stu-id="008d1-163">Updated</span></span>

* <span data-ttu-id="008d1-164">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="008d1-164">*Startup.cs*</span></span>

<span data-ttu-id="008d1-165">I file creati e aggiornati sono illustrati nella sezione successiva.</span><span class="sxs-lookup"><span data-stu-id="008d1-165">The created and updated files are explained in the next section.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="008d1-166">Visual Studio Code/Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="008d1-166">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="008d1-167">Il processo di scaffolding crea i file seguenti:</span><span class="sxs-lookup"><span data-stu-id="008d1-167">The scaffold process creates the following files:</span></span>

* <span data-ttu-id="008d1-168">*Pages/Movies*: pagine Create (Crea), Delete (Elimina), Details (Dettagli), Edit (Modifica) e Index (Indice).</span><span class="sxs-lookup"><span data-stu-id="008d1-168">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>

<span data-ttu-id="008d1-169">I file creati sono illustrati nella sezione successiva.</span><span class="sxs-lookup"><span data-stu-id="008d1-169">The created files are explained in the next section.</span></span>

---

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="008d1-170">Migrazione iniziale</span><span class="sxs-lookup"><span data-stu-id="008d1-170">Initial migration</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="008d1-171">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="008d1-171">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="008d1-172">In questa sezione viene usata la Console di Gestione pacchetti (PMC) per:</span><span class="sxs-lookup"><span data-stu-id="008d1-172">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="008d1-173">Aggiungere una migrazione iniziale.</span><span class="sxs-lookup"><span data-stu-id="008d1-173">Add an initial migration.</span></span>
* <span data-ttu-id="008d1-174">Aggiornare il database con la migrazione iniziale.</span><span class="sxs-lookup"><span data-stu-id="008d1-174">Update the database with the initial migration.</span></span>

<span data-ttu-id="008d1-175">Dal menu **Strumenti** selezionare **Gestione pacchetti NuGet** >  **Console di Gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="008d1-175">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Menu della Console di Gestione pacchetti](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="008d1-177">In PMC, immettere i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="008d1-177">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration InitialCreate
Update-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="008d1-178">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="008d1-178">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="008d1-179">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="008d1-179">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---

<span data-ttu-id="008d1-180">I comandi precedenti generano l'avviso seguente: "nessun tipo specificato per la colonna decimale ' Price ' nel tipo di entità' Movie '.</span><span class="sxs-lookup"><span data-stu-id="008d1-180">The preceding commands generate the following warning: "No type was specified for the decimal column 'Price' on entity type 'Movie'.</span></span> <span data-ttu-id="008d1-181">This will cause values to be silently truncated if they do not fit in the default precision and scale. (I valori saranno quindi automaticamente troncati se non rispettano la precisione e la scala predefinite).</span><span class="sxs-lookup"><span data-stu-id="008d1-181">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span></span> <span data-ttu-id="008d1-182">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'. (Specificare in modo esplicito il tipo di colonna di SQL Server che può supportare tutti i valori usando 'HasColumnType()')"</span><span class="sxs-lookup"><span data-stu-id="008d1-182">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'."</span></span>

<span data-ttu-id="008d1-183">È possibile ignorare tale avviso. Verrà risolto in un'esercitazione successiva.</span><span class="sxs-lookup"><span data-stu-id="008d1-183">You can ignore that warning, it will be fixed in a later tutorial.</span></span>

<span data-ttu-id="008d1-184">Il comando Migrations genera il codice per creare lo schema del database iniziale.</span><span class="sxs-lookup"><span data-stu-id="008d1-184">The migrations command generates code to create the initial database schema.</span></span> <span data-ttu-id="008d1-185">Lo schema è basato sul modello specificato in `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="008d1-185">The schema is based on the model specified in `DbContext`.</span></span> <span data-ttu-id="008d1-186">L'argomento `InitialCreate` viene usato per denominare le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="008d1-186">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="008d1-187">È possibile usare qualsiasi nome, ma per convenzione viene selezionato un nome che descrive la migrazione.</span><span class="sxs-lookup"><span data-stu-id="008d1-187">Any name can be used, but by convention a name is selected that describes the migration.</span></span>

<span data-ttu-id="008d1-188">Il comando `update` esegue il `Up` metodo nelle migrazioni che non sono state applicate.</span><span class="sxs-lookup"><span data-stu-id="008d1-188">The `update` command runs the `Up` method in migrations that have not been applied.</span></span> <span data-ttu-id="008d1-189">In questo caso, `update` esegue il `Up` metodo in *migrazioni/\<timestamp > _InitialCreate file con estensione cs* , che crea il database.</span><span class="sxs-lookup"><span data-stu-id="008d1-189">In this case, `update` runs the `Up` method in  *Migrations/\<time-stamp>_InitialCreate.cs* file, which creates the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="008d1-190">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="008d1-190">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="008d1-191">Esaminare il contesto registrato con l'inserimento di dipendenze</span><span class="sxs-lookup"><span data-stu-id="008d1-191">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="008d1-192">ASP.NET Core viene compilato tramite [dependency injection](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="008d1-192">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="008d1-193">I servizi, ad esempio il contesto di database di Entity Framework Core, vengono registrati tramite dependency injection durante l'avvio dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="008d1-193">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="008d1-194">Questi servizi vengono quindi offerti ai componenti per cui sono necessari (ad esempio Razor Pages) tramite i parametri del costruttore.</span><span class="sxs-lookup"><span data-stu-id="008d1-194">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="008d1-195">Più avanti nell'esercitazione viene illustrato il codice del costruttore che ottiene un'istanza del contesto di database.</span><span class="sxs-lookup"><span data-stu-id="008d1-195">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="008d1-196">Lo strumento di scaffolding ha creato automaticamente un contesto del database e lo ha registrato per la dependency injection.</span><span class="sxs-lookup"><span data-stu-id="008d1-196">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="008d1-197">Esaminare il metodo `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="008d1-197">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="008d1-198">La riga evidenziata è stata aggiunta dallo scaffolder:</span><span class="sxs-lookup"><span data-stu-id="008d1-198">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_ConfigureServices&highlight=5-6)]

<span data-ttu-id="008d1-199">`RazorPagesMovieContext` coordina la funzionalità di EF Core (Create, Read, Update, Delete e così via) per il modello `Movie`.</span><span class="sxs-lookup"><span data-stu-id="008d1-199">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="008d1-200">Il contesto dei dati (`RazorPagesMovieContext`) è derivato da [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="008d1-200">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="008d1-201">Il contesto dei dati specifica le entità incluse nel modello di dati.</span><span class="sxs-lookup"><span data-stu-id="008d1-201">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="008d1-202">Il codice precedente crea una proprietà [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) per il set di entità.</span><span class="sxs-lookup"><span data-stu-id="008d1-202">The preceding code creates a [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="008d1-203">Nella terminologia di Entity Framework, un set di entità corrisponde in genere alla tabella di un database.</span><span class="sxs-lookup"><span data-stu-id="008d1-203">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="008d1-204">Un'entità corrisponde a una riga nella tabella.</span><span class="sxs-lookup"><span data-stu-id="008d1-204">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="008d1-205">Il nome della stringa di connessione viene passato al contesto chiamando un metodo in un oggetto [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions).</span><span class="sxs-lookup"><span data-stu-id="008d1-205">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="008d1-206">Per lo sviluppo locale, il [sistema di configurazione di ASP.NET Core](xref:fundamentals/configuration/index) legge la stringa di connessione dal file *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="008d1-206">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="008d1-207">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="008d1-207">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="008d1-208">Esaminare il metodo `Up`.</span><span class="sxs-lookup"><span data-stu-id="008d1-208">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="008d1-209">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="008d1-209">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="008d1-210">Esaminare il metodo `Up`.</span><span class="sxs-lookup"><span data-stu-id="008d1-210">Examine the `Up` method.</span></span>

---

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="008d1-211">Eseguire il test dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="008d1-211">Test the app</span></span>

* <span data-ttu-id="008d1-212">Eseguire l'app e accodare `/Movies` all'URL nel browser (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="008d1-212">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="008d1-213">Se viene visualizzato l'errore:</span><span class="sxs-lookup"><span data-stu-id="008d1-213">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="008d1-214">Non è stato eseguita la [migrazione](#pmc).</span><span class="sxs-lookup"><span data-stu-id="008d1-214">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="008d1-215">Eseguire il test del collegamento **Crea**.</span><span class="sxs-lookup"><span data-stu-id="008d1-215">Test the **Create** link.</span></span>

  ![Pagina Create](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="008d1-217">Potrebbe non essere possibile immettere virgole decimali nel campo `Price`.</span><span class="sxs-lookup"><span data-stu-id="008d1-217">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="008d1-218">Per supportare la [convalida jQuery](https://jqueryvalidation.org/) per impostazioni locali diverse dall'inglese che usano la virgola (",") come separatore decimale e per formati di data diversi da quello dell'inglese (Stati Uniti), è necessario localizzare l'app.</span><span class="sxs-lookup"><span data-stu-id="008d1-218">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="008d1-219">Per istruzioni sulla localizzazione, vedere [questo problema su GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="008d1-219">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="008d1-220">Eseguire il test dei collegamenti **Modifica**, **Dettagli** e **Elimina**.</span><span class="sxs-lookup"><span data-stu-id="008d1-220">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="008d1-221">L'esercitazione successiva illustra i file creati dallo scaffolding.</span><span class="sxs-lookup"><span data-stu-id="008d1-221">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="008d1-222">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="008d1-222">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="008d1-223">[Precedente: Introduzione](xref:tutorials/razor-pages/razor-pages-start)
> [Successivo: Pagine Razor create tramite scaffolding](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="008d1-223">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end

<!--  ::: moniker previous version   -->
::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="008d1-224">In questa sezione vengono aggiunte le classi per la gestione dei film in un [database SQLite](https://www.sqlite.org/index.html)multipiattaforma.</span><span class="sxs-lookup"><span data-stu-id="008d1-224">In this section, classes are added for managing movies in a cross-platform [SQLite database](https://www.sqlite.org/index.html).</span></span> <span data-ttu-id="008d1-225">Le app create da un modello di ASP.NET Core usano un database SQLite.</span><span class="sxs-lookup"><span data-stu-id="008d1-225">Apps created from an ASP.NET Core template use a SQLite database.</span></span> <span data-ttu-id="008d1-226">Le classi modello dell'app vengono usate con [Entity Framework Core (EF Core)](/ef/core) ([SQLite EF Core provider di database](/ef/core/providers/sqlite)) per lavorare con il database.</span><span class="sxs-lookup"><span data-stu-id="008d1-226">The app's model classes are used with [Entity Framework Core (EF Core)](/ef/core) ([SQLite EF Core Database Provider](/ef/core/providers/sqlite)) to work with the database.</span></span> <span data-ttu-id="008d1-227">EF Core è un framework ORM (Object-Relational Mapping) che semplifica l'accesso ai dati.</span><span class="sxs-lookup"><span data-stu-id="008d1-227">EF Core is an object-relational mapping (ORM) framework that simplifies data access.</span></span>

<span data-ttu-id="008d1-228">Le classi di modello sono dette classi POCO (da "plain-old CLR objects") perché non hanno alcuna dipendenza in EF Core.</span><span class="sxs-lookup"><span data-stu-id="008d1-228">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="008d1-229">Definiscono le proprietà dei dati archiviati nel database.</span><span class="sxs-lookup"><span data-stu-id="008d1-229">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="008d1-230">Aggiungere un modello di dati</span><span class="sxs-lookup"><span data-stu-id="008d1-230">Add a data model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="008d1-231">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="008d1-231">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="008d1-232">Fare clic con il pulsante destro del mouse sul progetto **RazorPagesMovie** > **Aggiungi** > **Nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="008d1-232">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="008d1-233">Assegnare il nome *Models* alla cartella.</span><span class="sxs-lookup"><span data-stu-id="008d1-233">Name the folder *Models*.</span></span>

<span data-ttu-id="008d1-234">Fare clic con il pulsante destro del mouse sulla cartella *Models*.</span><span class="sxs-lookup"><span data-stu-id="008d1-234">Right click the *Models* folder.</span></span> <span data-ttu-id="008d1-235">Selezionare **Aggiungi** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="008d1-235">Select **Add** > **Class**.</span></span> <span data-ttu-id="008d1-236">Denominare la classe **Movie**.</span><span class="sxs-lookup"><span data-stu-id="008d1-236">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="008d1-237">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="008d1-237">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="008d1-238">Aggiungere una cartella denominata *Modelli*.</span><span class="sxs-lookup"><span data-stu-id="008d1-238">Add a folder named *Models*.</span></span>
* <span data-ttu-id="008d1-239">Aggiungere una classe alla cartella *Models* denominata *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="008d1-239">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="008d1-240">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="008d1-240">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="008d1-241">In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto **RazorPagesMovie**, quindi selezionare **Aggiungi** > **Nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="008d1-241">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="008d1-242">Assegnare il nome *Models* alla cartella.</span><span class="sxs-lookup"><span data-stu-id="008d1-242">Name the folder *Models*.</span></span>
* <span data-ttu-id="008d1-243">Fare clic con il pulsante destro del mouse sulla cartella *Models* e scegliere **Aggiungi** > **Nuovo file**.</span><span class="sxs-lookup"><span data-stu-id="008d1-243">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="008d1-244">Nella finestra di dialogo **Nuovo file**:</span><span class="sxs-lookup"><span data-stu-id="008d1-244">In the **New File** dialog:</span></span>

  * <span data-ttu-id="008d1-245">Selezionare **Generale** nel riquadro a sinistra.</span><span class="sxs-lookup"><span data-stu-id="008d1-245">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="008d1-246">Selezionare **Classe vuota** nel riquadro centrale.</span><span class="sxs-lookup"><span data-stu-id="008d1-246">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="008d1-247">Denominare la classe **Movie** e selezionare **Nuovo**.</span><span class="sxs-lookup"><span data-stu-id="008d1-247">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

---

<span data-ttu-id="008d1-248">Compilare il progetto per verificare che non siano presenti errori di compilazione.</span><span class="sxs-lookup"><span data-stu-id="008d1-248">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="008d1-249">Eseguire lo scaffolding del modello *Movie*</span><span class="sxs-lookup"><span data-stu-id="008d1-249">Scaffold the movie model</span></span>

<span data-ttu-id="008d1-250">In questa sezione viene eseguito lo scaffolding del modello *Movie*.</span><span class="sxs-lookup"><span data-stu-id="008d1-250">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="008d1-251">Lo strumento di scaffolding crea quindi le pagine per le operazioni CRUD (creazione, lettura, aggiornamento ed eliminazione) per il modello *Movie*.</span><span class="sxs-lookup"><span data-stu-id="008d1-251">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="008d1-252">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="008d1-252">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="008d1-253">Creare una cartella *Pages/Movies*:</span><span class="sxs-lookup"><span data-stu-id="008d1-253">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="008d1-254">Fare clic con il pulsante destro del mouse sulla cartella *Pages* > **Aggiungi** > **Nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="008d1-254">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="008d1-255">Assegnare il nome *Movies* alla cartella</span><span class="sxs-lookup"><span data-stu-id="008d1-255">Name the folder *Movies*</span></span>

<span data-ttu-id="008d1-256">Fare clic con il pulsante destro del mouse sulla cartella *Pages/Movies* > **Aggiungi** > **Nuovo elemento di scaffolding**.</span><span class="sxs-lookup"><span data-stu-id="008d1-256">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Immagine relativa alle istruzioni precedenti.](model/_static/sca.png)

<span data-ttu-id="008d1-258">Nella finestra di dialogo **Aggiungi scaffolding** selezionare **Razor che usano Entity Framework (CRUD)** > **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="008d1-258">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Immagine relativa alle istruzioni precedenti.](model/_static/add_scaffold.png)

<span data-ttu-id="008d1-260">Completare la finestra di dialogo **Pagine Razor che usano Entity Framework (CRUD)** :</span><span class="sxs-lookup"><span data-stu-id="008d1-260">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>
<!-- In the next section, change 
(plus) sign and accept the generated name 
to use Data, it should not use models. That will make the namespace the same for the VS version and the CLI version
-->

* <span data-ttu-id="008d1-261">Nel menu a discesa **Classe modello** selezionare **Movie (RazorPagesMovie.Models)** .</span><span class="sxs-lookup"><span data-stu-id="008d1-261">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="008d1-262">Nella riga **Classe contesto di dati** selezionare il segno più **+** e accettare il nome generato **RazorPagesMovie.Models.RazorPagesMovieContext**.</span><span class="sxs-lookup"><span data-stu-id="008d1-262">In the **Data context class** row, select the **+** (plus) sign and accept the generated name **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="008d1-263">Selezionare **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="008d1-263">Select **Add**.</span></span>

![Immagine relativa alle istruzioni precedenti.](model/_static/arp.png)

<span data-ttu-id="008d1-265">Il file *appsettings.json* è stato aggiornato con la stringa di connessione usata per connettersi a un database locale.</span><span class="sxs-lookup"><span data-stu-id="008d1-265">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="008d1-266">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="008d1-266">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="008d1-267">Aprire una finestra di comando nella directory del progetto (la directory che contiene i file *Program.cs*, *Startup.cs* e *csproj*).</span><span class="sxs-lookup"><span data-stu-id="008d1-267">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>

* <span data-ttu-id="008d1-268">**Per Windows**: eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="008d1-268">**For Windows**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="008d1-269">**Per MacOS e Linux**: eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="008d1-269">**For macOS and Linux**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="008d1-270">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="008d1-270">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="008d1-271">Aprire una finestra di comando nella directory del progetto (la directory che contiene i file *Program.cs*, *Startup.cs* e *csproj*).</span><span class="sxs-lookup"><span data-stu-id="008d1-271">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="008d1-272">Eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="008d1-272">Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

---

<span data-ttu-id="008d1-273">Il processo di scaffolding crea e aggiorna i file seguenti:</span><span class="sxs-lookup"><span data-stu-id="008d1-273">The scaffold process creates and updates the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="008d1-274">File creati</span><span class="sxs-lookup"><span data-stu-id="008d1-274">Files created</span></span>

* <span data-ttu-id="008d1-275">*Pages/Movies*: pagine Create (Crea), Delete (Elimina), Details (Dettagli), Edit (Modifica) e Index (Indice).</span><span class="sxs-lookup"><span data-stu-id="008d1-275">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="008d1-276">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="008d1-276">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="file-updated"></a><span data-ttu-id="008d1-277">File aggiornato</span><span class="sxs-lookup"><span data-stu-id="008d1-277">File updated</span></span>

* <span data-ttu-id="008d1-278">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="008d1-278">*Startup.cs*</span></span>

<span data-ttu-id="008d1-279">I file creati e aggiornati sono illustrati nella sezione successiva.</span><span class="sxs-lookup"><span data-stu-id="008d1-279">The created and updated files are explained in the next section.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="008d1-280">Migrazione iniziale</span><span class="sxs-lookup"><span data-stu-id="008d1-280">Initial migration</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="008d1-281">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="008d1-281">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="008d1-282">In questa sezione viene usata la Console di Gestione pacchetti (PMC) per:</span><span class="sxs-lookup"><span data-stu-id="008d1-282">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="008d1-283">Aggiungere una migrazione iniziale.</span><span class="sxs-lookup"><span data-stu-id="008d1-283">Add an initial migration.</span></span>
* <span data-ttu-id="008d1-284">Aggiornare il database con la migrazione iniziale.</span><span class="sxs-lookup"><span data-stu-id="008d1-284">Update the database with the initial migration.</span></span>

<span data-ttu-id="008d1-285">Dal menu **Strumenti** selezionare **Gestione pacchetti NuGet** >  **Console di Gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="008d1-285">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Menu della Console di Gestione pacchetti](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="008d1-287">In PMC, immettere i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="008d1-287">In the PMC, enter the following commands:</span></span>

```Powershell
Add-Migration Initial
Update-Database
```

<span data-ttu-id="008d1-288">Il comando `Add-Migration` genera un codice per creare lo schema del database iniziale.</span><span class="sxs-lookup"><span data-stu-id="008d1-288">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="008d1-289">Lo schema è basato sul modello specificato in `DbContext` (nel file *RazorPagesMovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="008d1-289">The schema is based on the model specified in the `DbContext` (In the *RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="008d1-290">L'argomento `InitialCreate` viene usato per assegnare un nome alla migrazione.</span><span class="sxs-lookup"><span data-stu-id="008d1-290">The `InitialCreate` argument is used to name the migration.</span></span> <span data-ttu-id="008d1-291">È possibile usare qualsiasi nome, ma per convenzione viene usato un nome che descrive la migrazione.</span><span class="sxs-lookup"><span data-stu-id="008d1-291">Any name can be used, but by convention a name that describes the migration is used.</span></span> <span data-ttu-id="008d1-292">Per ulteriori informazioni, vedere <xref:data/ef-mvc/migrations>.</span><span class="sxs-lookup"><span data-stu-id="008d1-292">For more information, see <xref:data/ef-mvc/migrations>.</span></span>

<span data-ttu-id="008d1-293">Il comando `Update-Database` esegue il metodo `Up` nel file *Migrations/\<time-stamp>_InitialCreate.cs*.</span><span class="sxs-lookup"><span data-stu-id="008d1-293">The `Update-Database` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="008d1-294">Il metodo `Up` crea il database.</span><span class="sxs-lookup"><span data-stu-id="008d1-294">The `Up` method creates the database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="008d1-295">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="008d1-295">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="008d1-296">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="008d1-296">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---
> [!NOTE]
> <span data-ttu-id="008d1-297">I comandi precedenti generano l'avviso seguente: "*nessun tipo specificato per la colonna decimale ' Price ' nel tipo di entità' Movie '. In questo modo i valori verranno troncati automaticamente se non rientrano nella precisione e nella scala predefinite. Specificare in modo esplicito il tipo di colonna di SQL Server in grado di contenere tutti i valori utilizzando ' HasColumnType ()'.* È possibile ignorare l'avviso, che verrà risolto in un'esercitazione successiva.</span><span class="sxs-lookup"><span data-stu-id="008d1-297">The preceding commands generate the following warning: "*No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'.*" You can ignore that warning, it will be fixed in a later tutorial.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="008d1-298">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="008d1-298">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="008d1-299">Esaminare il contesto registrato con l'inserimento di dipendenze</span><span class="sxs-lookup"><span data-stu-id="008d1-299">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="008d1-300">ASP.NET Core viene compilato tramite [dependency injection](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="008d1-300">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="008d1-301">I servizi, ad esempio il contesto di database di Entity Framework Core, vengono registrati tramite dependency injection durante l'avvio dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="008d1-301">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="008d1-302">Questi servizi vengono quindi offerti ai componenti per cui sono necessari (ad esempio Razor Pages) tramite i parametri del costruttore.</span><span class="sxs-lookup"><span data-stu-id="008d1-302">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="008d1-303">Più avanti nell'esercitazione viene illustrato il codice del costruttore che ottiene un'istanza del contesto di database.</span><span class="sxs-lookup"><span data-stu-id="008d1-303">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="008d1-304">Lo strumento di scaffolding ha creato automaticamente un contesto del database e lo ha registrato per la dependency injection.</span><span class="sxs-lookup"><span data-stu-id="008d1-304">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="008d1-305">Esaminare il metodo `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="008d1-305">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="008d1-306">La riga evidenziata è stata aggiunta dallo scaffolder:</span><span class="sxs-lookup"><span data-stu-id="008d1-306">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

<span data-ttu-id="008d1-307">`RazorPagesMovieContext` coordina la funzionalità di EF Core (Create, Read, Update, Delete e così via) per il modello `Movie`.</span><span class="sxs-lookup"><span data-stu-id="008d1-307">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="008d1-308">Il contesto dei dati (`RazorPagesMovieContext`) è derivato da [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="008d1-308">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="008d1-309">Il contesto dei dati specifica le entità incluse nel modello di dati.</span><span class="sxs-lookup"><span data-stu-id="008d1-309">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="008d1-310">Il codice precedente crea una proprietà [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) per il set di entità.</span><span class="sxs-lookup"><span data-stu-id="008d1-310">The preceding code creates a [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="008d1-311">Nella terminologia di Entity Framework, un set di entità corrisponde in genere alla tabella di un database.</span><span class="sxs-lookup"><span data-stu-id="008d1-311">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="008d1-312">Un'entità corrisponde a una riga nella tabella.</span><span class="sxs-lookup"><span data-stu-id="008d1-312">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="008d1-313">Il nome della stringa di connessione viene passato al contesto chiamando un metodo in un oggetto [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions).</span><span class="sxs-lookup"><span data-stu-id="008d1-313">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="008d1-314">Per lo sviluppo locale, il [sistema di configurazione di ASP.NET Core](xref:fundamentals/configuration/index) legge la stringa di connessione dal file *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="008d1-314">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="008d1-315">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="008d1-315">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="008d1-316">Esaminare il metodo `Up`.</span><span class="sxs-lookup"><span data-stu-id="008d1-316">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="008d1-317">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="008d1-317">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="008d1-318">Esaminare il metodo `Up`.</span><span class="sxs-lookup"><span data-stu-id="008d1-318">Examine the `Up` method.</span></span>

---

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="008d1-319">Eseguire il test dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="008d1-319">Test the app</span></span>

* <span data-ttu-id="008d1-320">Eseguire l'app e accodare `/Movies` all'URL nel browser (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="008d1-320">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="008d1-321">Se viene visualizzato l'errore:</span><span class="sxs-lookup"><span data-stu-id="008d1-321">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="008d1-322">Non è stato eseguita la [migrazione](#pmc).</span><span class="sxs-lookup"><span data-stu-id="008d1-322">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="008d1-323">Eseguire il test del collegamento **Crea**.</span><span class="sxs-lookup"><span data-stu-id="008d1-323">Test the **Create** link.</span></span>

  ![Pagina Create](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="008d1-325">Potrebbe non essere possibile immettere virgole decimali nel campo `Price`.</span><span class="sxs-lookup"><span data-stu-id="008d1-325">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="008d1-326">Per supportare la [convalida jQuery](https://jqueryvalidation.org/) per impostazioni locali diverse dall'inglese che usano la virgola (",") come separatore decimale e per formati di data diversi da quello dell'inglese (Stati Uniti), è necessario localizzare l'app.</span><span class="sxs-lookup"><span data-stu-id="008d1-326">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="008d1-327">Per istruzioni sulla localizzazione, vedere [questo problema su GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="008d1-327">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="008d1-328">Eseguire il test dei collegamenti **Modifica**, **Dettagli** e **Elimina**.</span><span class="sxs-lookup"><span data-stu-id="008d1-328">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="008d1-329">L'esercitazione successiva illustra i file creati dallo scaffolding.</span><span class="sxs-lookup"><span data-stu-id="008d1-329">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="008d1-330">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="008d1-330">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="008d1-331">[Precedente: Introduzione](xref:tutorials/razor-pages/razor-pages-start)
> [Successivo: Pagine Razor create tramite scaffolding](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="008d1-331">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end
