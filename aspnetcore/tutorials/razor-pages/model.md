---
title: Aggiungere un modello a un'app Razor Pages in ASP.NET Core
author: rick-anderson
description: Scoprire come aggiungere classi per la gestione dei film in un database tramite Entity Framework Core (EF Core).
ms.author: riande
ms.date: 11/05/2019
uid: tutorials/razor-pages/model
ms.openlocfilehash: f2c9c2fc8112ef8a1a5afdbe448de6319c43521d
ms.sourcegitcommit: 6628cd23793b66e4ce88788db641a5bbf470c3c1
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/07/2019
ms.locfileid: "73761230"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="bf3fe-103">Aggiungere un modello a un'app Razor Pages in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bf3fe-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

<span data-ttu-id="bf3fe-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="bf3fe-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="bf3fe-105">In questa sezione vengono aggiunte le classi per la gestione dei film in un [database SQLite](https://www.sqlite.org/index.html)multipiattaforma.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-105">In this section, classes are added for managing movies in a cross-platform [SQLite database](https://www.sqlite.org/index.html).</span></span> <span data-ttu-id="bf3fe-106">Le app create da un modello di ASP.NET Core usano un database SQLite.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-106">Apps created from an ASP.NET Core template use a SQLite database.</span></span> <span data-ttu-id="bf3fe-107">Le classi modello dell'app vengono usate con [Entity Framework Core (EF Core)](/ef/core) ([SQLite EF Core provider di database](/ef/core/providers/sqlite)) per lavorare con il database.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-107">The app's model classes are used with [Entity Framework Core (EF Core)](/ef/core) ([SQLite EF Core Database Provider](/ef/core/providers/sqlite)) to work with the database.</span></span> <span data-ttu-id="bf3fe-108">EF Core è un framework ORM (Object-Relational Mapping) che semplifica l'accesso ai dati.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-108">EF Core is an object-relational mapping (ORM) framework that simplifies data access.</span></span>

<span data-ttu-id="bf3fe-109">Le classi di modello sono dette classi POCO (da "plain-old CLR objects") perché non hanno alcuna dipendenza in EF Core.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-109">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="bf3fe-110">Definiscono le proprietà dei dati archiviati nel database.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-110">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="bf3fe-111">Aggiungere un modello di dati</span><span class="sxs-lookup"><span data-stu-id="bf3fe-111">Add a data model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="bf3fe-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bf3fe-112">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="bf3fe-113">Fare clic con il pulsante destro del mouse sul progetto **RazorPagesMovie** > **Aggiungi** > **Nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-113">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="bf3fe-114">Assegnare il nome *Modelli* alla cartella.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-114">Name the folder *Models*.</span></span>

<span data-ttu-id="bf3fe-115">Fare clic con il pulsante destro del mouse sulla cartella *Models*.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-115">Right click the *Models* folder.</span></span> <span data-ttu-id="bf3fe-116">Selezionare **Aggiungi** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-116">Select **Add** > **Class**.</span></span> <span data-ttu-id="bf3fe-117">Denominare la classe **Movie**.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-117">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="bf3fe-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="bf3fe-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="bf3fe-119">Aggiungere una cartella denominata *Models*.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-119">Add a folder named *Models*.</span></span>
* <span data-ttu-id="bf3fe-120">Aggiungere una classe alla cartella *Modello* denominata *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-120">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="bf3fe-121">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="bf3fe-121">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="bf3fe-122">In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto **RazorPagesMovie**, quindi selezionare **Aggiungi** > **Nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-122">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="bf3fe-123">Assegnare il nome *Modelli* alla cartella.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-123">Name the folder *Models*.</span></span>
* <span data-ttu-id="bf3fe-124">Fare clic con il pulsante destro del mouse sulla cartella *Models* e scegliere **Aggiungi** > **Nuovo file**.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-124">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="bf3fe-125">Nella finestra di dialogo **Nuovo file**:</span><span class="sxs-lookup"><span data-stu-id="bf3fe-125">In the **New File** dialog:</span></span>

  * <span data-ttu-id="bf3fe-126">Selezionare **Generale** nel riquadro a sinistra.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-126">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="bf3fe-127">Selezionare **Classe vuota** nel riquadro centrale.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-127">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="bf3fe-128">Denominare la classe **Filmato** e selezionare **Nuovo**.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-128">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

---

<span data-ttu-id="bf3fe-129">Compilare il progetto per verificare che non siano presenti errori di compilazione.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-129">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="bf3fe-130">Eseguire lo scaffolding del modello di filmato</span><span class="sxs-lookup"><span data-stu-id="bf3fe-130">Scaffold the movie model</span></span>

<span data-ttu-id="bf3fe-131">In questa sezione viene eseguito lo scaffolding del modello *Movie*.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-131">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="bf3fe-132">Lo strumento di scaffolding crea quindi le pagine per le operazioni CRUD (creazione, lettura, aggiornamento ed eliminazione) per il modello di filmato.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-132">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="bf3fe-133">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bf3fe-133">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="bf3fe-134">Creare una cartella *Pages/Movies*:</span><span class="sxs-lookup"><span data-stu-id="bf3fe-134">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="bf3fe-135">Fare clic con il pulsante destro del mouse sulla cartella *Pages* > **Aggiungi** > **Nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-135">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="bf3fe-136">Assegnare il nome *Movies* alla cartella</span><span class="sxs-lookup"><span data-stu-id="bf3fe-136">Name the folder *Movies*</span></span>

<span data-ttu-id="bf3fe-137">Fare clic con il pulsante destro del mouse sulla cartella *Pages/Movies* > **Aggiungi** > **Nuovo elemento di scaffolding**.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-137">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Immagine relativa alle istruzioni precedenti.](model/_static/sca.png)

<span data-ttu-id="bf3fe-139">Nella finestra di dialogo **Aggiungi scaffolding** selezionare **Razor che usano Entity Framework (CRUD)** > **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-139">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Immagine relativa alle istruzioni precedenti.](model/_static/add_scaffold.png)

<span data-ttu-id="bf3fe-141">Completare la finestra di dialogo **Pagine Razor che usano Entity Framework (CRUD)** :</span><span class="sxs-lookup"><span data-stu-id="bf3fe-141">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="bf3fe-142">Nel menu a discesa **Classe modello** selezionare **Movie (RazorPagesMovie.Models)** .</span><span class="sxs-lookup"><span data-stu-id="bf3fe-142">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="bf3fe-143">Nella riga **Classe contesto di dati** selezionare il segno più **+** e modificare il nome generato da RazorPagesMovie.**Models**.RazorPagesMovieContext a RazorPagesMovie.**Data**.RazorPagesMovieContext.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-143">In the **Data context class** row, select the **+** (plus) sign and change the generated name from RazorPagesMovie.**Models**.RazorPagesMovieContext to RazorPagesMovie.**Data**.RazorPagesMovieContext.</span></span> <span data-ttu-id="bf3fe-144">[Questa modifica](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) non è obbligatoria.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-144">[This change](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) is not required.</span></span> <span data-ttu-id="bf3fe-145">Crea la classe del contesto di database con lo spazio dei nomi corretto.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-145">It creates the database context class with the correct namespace.</span></span>
* <span data-ttu-id="bf3fe-146">Selezionare **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-146">Select **Add**.</span></span>

![Immagine relativa alle istruzioni precedenti.](model/_static/3/arp.png)

<span data-ttu-id="bf3fe-148">Il file *appsettings.json* è stato aggiornato con la stringa di connessione usata per connettersi a un database locale.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-148">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="bf3fe-149">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="bf3fe-149">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="bf3fe-150">Aprire una finestra di comando nella directory del progetto (la directory che contiene i file *Program.cs*, *Startup.cs* e *csproj*).</span><span class="sxs-lookup"><span data-stu-id="bf3fe-150">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="bf3fe-151">Installare lo strumento di scaffolding:</span><span class="sxs-lookup"><span data-stu-id="bf3fe-151">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="bf3fe-152">**Per Windows**: eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="bf3fe-152">**For Windows**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="bf3fe-153">**Per MacOS e Linux**: eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="bf3fe-153">**For macOS and Linux**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

[!INCLUDE [use SQL Server in production](~/includes/RP/sqlitedev.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="bf3fe-154">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="bf3fe-154">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="bf3fe-155">Aprire una finestra di comando nella directory del progetto (la directory che contiene i file *Program.cs*, *Startup.cs* e *csproj*).</span><span class="sxs-lookup"><span data-stu-id="bf3fe-155">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="bf3fe-156">Installare lo strumento di scaffolding:</span><span class="sxs-lookup"><span data-stu-id="bf3fe-156">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="bf3fe-157">Eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="bf3fe-157">Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

[!INCLUDE [use SQL Server in production](~/includes/RP/sqlitedev.md)]

---

### <a name="files-created"></a><span data-ttu-id="bf3fe-158">File creati</span><span class="sxs-lookup"><span data-stu-id="bf3fe-158">Files created</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="bf3fe-159">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bf3fe-159">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="bf3fe-160">Il processo di scaffolding crea e aggiorna i file seguenti:</span><span class="sxs-lookup"><span data-stu-id="bf3fe-160">The scaffold process creates and updates the following files:</span></span>

* <span data-ttu-id="bf3fe-161">*Pages/Movies*: pagine Create (Crea), Delete (Elimina), Details (Dettagli), Edit (Modifica) e Index (Indice).</span><span class="sxs-lookup"><span data-stu-id="bf3fe-161">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="bf3fe-162">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="bf3fe-162">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="updated"></a><span data-ttu-id="bf3fe-163">File aggiornati</span><span class="sxs-lookup"><span data-stu-id="bf3fe-163">Updated</span></span>

* <span data-ttu-id="bf3fe-164">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="bf3fe-164">*Startup.cs*</span></span>

<span data-ttu-id="bf3fe-165">I file creati e aggiornati sono illustrati nella sezione successiva.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-165">The created and updated files are explained in the next section.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="bf3fe-166">Visual Studio Code/Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="bf3fe-166">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="bf3fe-167">Il processo di scaffolding crea i file seguenti:</span><span class="sxs-lookup"><span data-stu-id="bf3fe-167">The scaffold process creates the following files:</span></span>

* <span data-ttu-id="bf3fe-168">*Pages/Movies*: pagine Create (Crea), Delete (Elimina), Details (Dettagli), Edit (Modifica) e Index (Indice).</span><span class="sxs-lookup"><span data-stu-id="bf3fe-168">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>

<span data-ttu-id="bf3fe-169">I file creati sono illustrati nella sezione successiva.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-169">The created files are explained in the next section.</span></span>

---

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="bf3fe-170">Migrazione iniziale</span><span class="sxs-lookup"><span data-stu-id="bf3fe-170">Initial migration</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="bf3fe-171">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bf3fe-171">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="bf3fe-172">In questa sezione viene usata la Console di Gestione pacchetti (PMC) per:</span><span class="sxs-lookup"><span data-stu-id="bf3fe-172">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="bf3fe-173">Aggiungere una migrazione iniziale.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-173">Add an initial migration.</span></span>
* <span data-ttu-id="bf3fe-174">Aggiornare il database con la migrazione iniziale.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-174">Update the database with the initial migration.</span></span>

<span data-ttu-id="bf3fe-175">Dal menu **Strumenti** selezionare **Gestione pacchetti NuGet** >  **Console di Gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-175">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Menu della Console di Gestione pacchetti](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="bf3fe-177">In PMC, immettere i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="bf3fe-177">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration InitialCreate
Update-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="bf3fe-178">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="bf3fe-178">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="bf3fe-179">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="bf3fe-179">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---

<span data-ttu-id="bf3fe-180">I comandi precedenti generano l'avviso seguente: "nessun tipo specificato per la colonna decimale ' Price ' nel tipo di entità' Movie '.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-180">The preceding commands generate the following warning: "No type was specified for the decimal column 'Price' on entity type 'Movie'.</span></span> <span data-ttu-id="bf3fe-181">This will cause values to be silently truncated if they do not fit in the default precision and scale. (I valori saranno quindi automaticamente troncati se non rispettano la precisione e la scala predefinite).</span><span class="sxs-lookup"><span data-stu-id="bf3fe-181">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span></span> <span data-ttu-id="bf3fe-182">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'. (Specificare in modo esplicito il tipo di colonna di SQL Server che può supportare tutti i valori usando 'HasColumnType()')"</span><span class="sxs-lookup"><span data-stu-id="bf3fe-182">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'."</span></span>

<span data-ttu-id="bf3fe-183">È possibile ignorare tale avviso. Verrà risolto in un'esercitazione successiva.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-183">You can ignore that warning, it will be fixed in a later tutorial.</span></span>

<span data-ttu-id="bf3fe-184">Il comando Migrations genera il codice per creare lo schema del database iniziale.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-184">The migrations command generates code to create the initial database schema.</span></span> <span data-ttu-id="bf3fe-185">Lo schema è basato sul modello specificato in `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-185">The schema is based on the model specified in `DbContext`.</span></span> <span data-ttu-id="bf3fe-186">L'argomento `InitialCreate` viene usato per denominare le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-186">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="bf3fe-187">È possibile usare qualsiasi nome, ma per convenzione viene selezionato un nome che descrive la migrazione.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-187">Any name can be used, but by convention a name is selected that describes the migration.</span></span>

<span data-ttu-id="bf3fe-188">Il comando `update` esegue il `Up` metodo nelle migrazioni che non sono state applicate.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-188">The `update` command runs the `Up` method in migrations that have not been applied.</span></span> <span data-ttu-id="bf3fe-189">In questo caso, `update` esegue il `Up` metodo in *migrazioni/\<timestamp > _InitialCreate file con estensione cs* , che crea il database.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-189">In this case, `update` runs the `Up` method in  *Migrations/\<time-stamp>_InitialCreate.cs* file, which creates the database.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="bf3fe-190">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bf3fe-190">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="bf3fe-191">Esaminare il contesto registrato con l'inserimento di dipendenze</span><span class="sxs-lookup"><span data-stu-id="bf3fe-191">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="bf3fe-192">ASP.NET Core viene compilato con l'[inserimento di dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="bf3fe-192">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="bf3fe-193">I servizi, ad esempio il contesto di database di Entity Framework Core, vengono registrati con l'inserimento delle dipendenze durante l'avvio dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-193">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="bf3fe-194">Questi servizi vengono quindi offerti ai componenti per cui sono necessari (ad esempio Razor Pages) tramite i parametri del costruttore.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-194">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="bf3fe-195">Più avanti nell'esercitazione viene illustrato il codice del costruttore che ottiene un'istanza del contesto di database.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-195">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="bf3fe-196">Lo strumento di scaffolding ha creato automaticamente un contesto del database e lo ha registrato per la dependency injection.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-196">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="bf3fe-197">Esaminare il metodo `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-197">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="bf3fe-198">La riga evidenziata è stata aggiunta dallo scaffolder:</span><span class="sxs-lookup"><span data-stu-id="bf3fe-198">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_ConfigureServices&highlight=5-6)]

<span data-ttu-id="bf3fe-199">`RazorPagesMovieContext` coordina la funzionalità di EF Core (Create, Read, Update, Delete e così via) per il modello `Movie`.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-199">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="bf3fe-200">Il contesto dei dati (`RazorPagesMovieContext`) è derivato da [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="bf3fe-200">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="bf3fe-201">Il contesto dei dati specifica le entità incluse nel modello di dati.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-201">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="bf3fe-202">il codice precedente crea una proprietà [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) per il set di entità.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-202">The preceding code creates a [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="bf3fe-203">Nella terminologia di Entity Framework, un set di entità corrisponde in genere alla tabella di un database.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-203">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="bf3fe-204">Un'entità corrisponde a una riga nella tabella.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-204">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="bf3fe-205">Il nome della stringa di connessione viene passato al contesto chiamando un metodo in un oggetto [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions).</span><span class="sxs-lookup"><span data-stu-id="bf3fe-205">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="bf3fe-206">Per lo sviluppo locale, il [sistema di configurazione di ASP.NET Core](xref:fundamentals/configuration/index) legge la stringa di connessione dal file *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-206">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="bf3fe-207">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="bf3fe-207">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="bf3fe-208">Esaminare il metodo `Up`.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-208">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="bf3fe-209">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="bf3fe-209">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="bf3fe-210">Esaminare il metodo `Up`.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-210">Examine the `Up` method.</span></span>

---

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="bf3fe-211">Eseguire il test dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="bf3fe-211">Test the app</span></span>

* <span data-ttu-id="bf3fe-212">Eseguire l'app e accodare `/Movies` all'URL nel browser (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="bf3fe-212">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="bf3fe-213">Se viene visualizzato l'errore:</span><span class="sxs-lookup"><span data-stu-id="bf3fe-213">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="bf3fe-214">Non è stato eseguita la [migrazione](#pmc).</span><span class="sxs-lookup"><span data-stu-id="bf3fe-214">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="bf3fe-215">Eseguire il test del collegamento **Crea**.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-215">Test the **Create** link.</span></span>

  ![Pagina Create](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="bf3fe-217">Potrebbe non essere possibile immettere virgole decimali nel campo `Price`.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-217">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="bf3fe-218">Per supportare la [convalida jQuery](https://jqueryvalidation.org/) per impostazioni locali diverse dall'inglese che usano la virgola (",") come separatore decimale e per formati di data diversi da quello dell'inglese (Stati Uniti), è necessario localizzare l'app.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-218">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="bf3fe-219">Per istruzioni sulla globalizzazione, vedere [questo problema su GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="bf3fe-219">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="bf3fe-220">Eseguire il test dei collegamenti **Modifica**, **Dettagli** e **Elimina**.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-220">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="bf3fe-221">L'esercitazione successiva illustra i file creati tramite scaffolding.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-221">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="bf3fe-222">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="bf3fe-222">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="bf3fe-223">[Precedente: Introduzione](xref:tutorials/razor-pages/razor-pages-start)
> [Successivo: Pagine Razor create tramite scaffolding](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="bf3fe-223">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end

<!--  ::: moniker previous version   -->
::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="bf3fe-224">In questa sezione vengono aggiunte le classi per la gestione dei film in un [database SQLite](https://www.sqlite.org/index.html)multipiattaforma.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-224">In this section, classes are added for managing movies in a cross-platform [SQLite database](https://www.sqlite.org/index.html).</span></span> <span data-ttu-id="bf3fe-225">Le app create da un modello di ASP.NET Core usano un database SQLite.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-225">Apps created from an ASP.NET Core template use a SQLite database.</span></span> <span data-ttu-id="bf3fe-226">Le classi modello dell'app vengono usate con [Entity Framework Core (EF Core)](/ef/core) ([SQLite EF Core provider di database](/ef/core/providers/sqlite)) per lavorare con il database.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-226">The app's model classes are used with [Entity Framework Core (EF Core)](/ef/core) ([SQLite EF Core Database Provider](/ef/core/providers/sqlite)) to work with the database.</span></span> <span data-ttu-id="bf3fe-227">EF Core è un framework ORM (Object-Relational Mapping) che semplifica l'accesso ai dati.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-227">EF Core is an object-relational mapping (ORM) framework that simplifies data access.</span></span>

<span data-ttu-id="bf3fe-228">Le classi di modello sono dette classi POCO (da "plain-old CLR objects") perché non hanno alcuna dipendenza in EF Core.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-228">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="bf3fe-229">Definiscono le proprietà dei dati archiviati nel database.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-229">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="bf3fe-230">Aggiungere un modello di dati</span><span class="sxs-lookup"><span data-stu-id="bf3fe-230">Add a data model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="bf3fe-231">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bf3fe-231">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="bf3fe-232">Fare clic con il pulsante destro del mouse sul progetto **RazorPagesMovie** > **Aggiungi** > **Nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-232">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="bf3fe-233">Assegnare il nome *Modelli* alla cartella.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-233">Name the folder *Models*.</span></span>

<span data-ttu-id="bf3fe-234">Fare clic con il pulsante destro del mouse sulla cartella *Models*.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-234">Right click the *Models* folder.</span></span> <span data-ttu-id="bf3fe-235">Selezionare **Aggiungi** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-235">Select **Add** > **Class**.</span></span> <span data-ttu-id="bf3fe-236">Denominare la classe **Movie**.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-236">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="bf3fe-237">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="bf3fe-237">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="bf3fe-238">Aggiungere una cartella denominata *Models*.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-238">Add a folder named *Models*.</span></span>
* <span data-ttu-id="bf3fe-239">Aggiungere una classe alla cartella *Modello* denominata *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-239">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="bf3fe-240">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="bf3fe-240">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="bf3fe-241">In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto **RazorPagesMovie**, quindi selezionare **Aggiungi** > **Nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-241">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="bf3fe-242">Assegnare il nome *Modelli* alla cartella.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-242">Name the folder *Models*.</span></span>
* <span data-ttu-id="bf3fe-243">Fare clic con il pulsante destro del mouse sulla cartella *Models* e scegliere **Aggiungi** > **Nuovo file**.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-243">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="bf3fe-244">Nella finestra di dialogo **Nuovo file**:</span><span class="sxs-lookup"><span data-stu-id="bf3fe-244">In the **New File** dialog:</span></span>

  * <span data-ttu-id="bf3fe-245">Selezionare **Generale** nel riquadro a sinistra.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-245">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="bf3fe-246">Selezionare **Classe vuota** nel riquadro centrale.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-246">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="bf3fe-247">Denominare la classe **Filmato** e selezionare **Nuovo**.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-247">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

---

<span data-ttu-id="bf3fe-248">Compilare il progetto per verificare che non siano presenti errori di compilazione.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-248">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="bf3fe-249">Eseguire lo scaffolding del modello di filmato</span><span class="sxs-lookup"><span data-stu-id="bf3fe-249">Scaffold the movie model</span></span>

<span data-ttu-id="bf3fe-250">In questa sezione viene eseguito lo scaffolding del modello *Movie*.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-250">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="bf3fe-251">Lo strumento di scaffolding crea quindi le pagine per le operazioni CRUD (creazione, lettura, aggiornamento ed eliminazione) per il modello di filmato.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-251">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="bf3fe-252">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bf3fe-252">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="bf3fe-253">Creare una cartella *Pages/Movies*:</span><span class="sxs-lookup"><span data-stu-id="bf3fe-253">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="bf3fe-254">Fare clic con il pulsante destro del mouse sulla cartella *Pages* > **Aggiungi** > **Nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-254">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="bf3fe-255">Assegnare il nome *Movies* alla cartella</span><span class="sxs-lookup"><span data-stu-id="bf3fe-255">Name the folder *Movies*</span></span>

<span data-ttu-id="bf3fe-256">Fare clic con il pulsante destro del mouse sulla cartella *Pages/Movies* > **Aggiungi** > **Nuovo elemento di scaffolding**.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-256">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Immagine relativa alle istruzioni precedenti.](model/_static/sca.png)

<span data-ttu-id="bf3fe-258">Nella finestra di dialogo **Aggiungi scaffolding** selezionare **Razor che usano Entity Framework (CRUD)** > **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-258">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Immagine relativa alle istruzioni precedenti.](model/_static/add_scaffold.png)

<span data-ttu-id="bf3fe-260">Completare la finestra di dialogo **Pagine Razor che usano Entity Framework (CRUD)** :</span><span class="sxs-lookup"><span data-stu-id="bf3fe-260">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>
<!-- In the next section, change 
(plus) sign and accept the generated name 
to use Data, it should not use models. That will make the namespace the same for the VS version and the CLI version
-->

* <span data-ttu-id="bf3fe-261">Nel menu a discesa **Classe modello** selezionare **Movie (RazorPagesMovie.Models)** .</span><span class="sxs-lookup"><span data-stu-id="bf3fe-261">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="bf3fe-262">Nella riga **Classe contesto di dati** selezionare il segno più **+** e accettare il nome generato **RazorPagesMovie.Models.RazorPagesMovieContext**.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-262">In the **Data context class** row, select the **+** (plus) sign and accept the generated name **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="bf3fe-263">Selezionare **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-263">Select **Add**.</span></span>

![Immagine relativa alle istruzioni precedenti.](model/_static/arp.png)

<span data-ttu-id="bf3fe-265">Il file *appsettings.json* è stato aggiornato con la stringa di connessione usata per connettersi a un database locale.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-265">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="bf3fe-266">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="bf3fe-266">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="bf3fe-267">Aprire una finestra di comando nella directory del progetto (la directory che contiene i file *Program.cs*, *Startup.cs* e *csproj*).</span><span class="sxs-lookup"><span data-stu-id="bf3fe-267">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="bf3fe-268">Installare lo strumento di scaffolding:</span><span class="sxs-lookup"><span data-stu-id="bf3fe-268">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="bf3fe-269">**Per Windows**: eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="bf3fe-269">**For Windows**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="bf3fe-270">**Per MacOS e Linux**: eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="bf3fe-270">**For macOS and Linux**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="bf3fe-271">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="bf3fe-271">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="bf3fe-272">Aprire una finestra di comando nella directory del progetto (la directory che contiene i file *Program.cs*, *Startup.cs* e *csproj*).</span><span class="sxs-lookup"><span data-stu-id="bf3fe-272">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="bf3fe-273">Installare lo strumento di scaffolding:</span><span class="sxs-lookup"><span data-stu-id="bf3fe-273">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="bf3fe-274">Eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="bf3fe-274">Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

---

<span data-ttu-id="bf3fe-275">Il processo di scaffolding crea e aggiorna i file seguenti:</span><span class="sxs-lookup"><span data-stu-id="bf3fe-275">The scaffold process creates and updates the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="bf3fe-276">File creati</span><span class="sxs-lookup"><span data-stu-id="bf3fe-276">Files created</span></span>

* <span data-ttu-id="bf3fe-277">*Pages/Movies*: pagine Create (Crea), Delete (Elimina), Details (Dettagli), Edit (Modifica) e Index (Indice).</span><span class="sxs-lookup"><span data-stu-id="bf3fe-277">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="bf3fe-278">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="bf3fe-278">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="file-updated"></a><span data-ttu-id="bf3fe-279">File aggiornato</span><span class="sxs-lookup"><span data-stu-id="bf3fe-279">File updated</span></span>

* <span data-ttu-id="bf3fe-280">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="bf3fe-280">*Startup.cs*</span></span>

<span data-ttu-id="bf3fe-281">I file creati e aggiornati sono illustrati nella sezione successiva.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-281">The created and updated files are explained in the next section.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="bf3fe-282">Migrazione iniziale</span><span class="sxs-lookup"><span data-stu-id="bf3fe-282">Initial migration</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="bf3fe-283">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bf3fe-283">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="bf3fe-284">In questa sezione viene usata la Console di Gestione pacchetti (PMC) per:</span><span class="sxs-lookup"><span data-stu-id="bf3fe-284">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="bf3fe-285">Aggiungere una migrazione iniziale.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-285">Add an initial migration.</span></span>
* <span data-ttu-id="bf3fe-286">Aggiornare il database con la migrazione iniziale.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-286">Update the database with the initial migration.</span></span>

<span data-ttu-id="bf3fe-287">Dal menu **Strumenti** selezionare **Gestione pacchetti NuGet** >  **Console di Gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-287">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Menu della Console di Gestione pacchetti](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="bf3fe-289">In PMC, immettere i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="bf3fe-289">In the PMC, enter the following commands:</span></span>

```Powershell
Add-Migration Initial
Update-Database
```

<span data-ttu-id="bf3fe-290">Il comando `Add-Migration` genera un codice per creare lo schema del database iniziale.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-290">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="bf3fe-291">Lo schema è basato sul modello specificato in `DbContext` (nel file *RazorPagesMovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="bf3fe-291">The schema is based on the model specified in the `DbContext` (In the *RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="bf3fe-292">L'argomento `InitialCreate` viene usato per assegnare un nome alla migrazione.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-292">The `InitialCreate` argument is used to name the migration.</span></span> <span data-ttu-id="bf3fe-293">È possibile usare qualsiasi nome, ma per convenzione viene usato un nome che descrive la migrazione.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-293">Any name can be used, but by convention a name that describes the migration is used.</span></span> <span data-ttu-id="bf3fe-294">Per altre informazioni, vedere <xref:data/ef-mvc/migrations>.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-294">For more information, see <xref:data/ef-mvc/migrations>.</span></span>

<span data-ttu-id="bf3fe-295">Il comando `Update-Database` esegue il metodo `Up` nel file *Migrations/\<time-stamp>_InitialCreate.cs*.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-295">The `Update-Database` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="bf3fe-296">Il metodo `Up` crea il database.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-296">The `Up` method creates the database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="bf3fe-297">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="bf3fe-297">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="bf3fe-298">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="bf3fe-298">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---
> [!NOTE]
> <span data-ttu-id="bf3fe-299">I comandi precedenti generano l'avviso seguente: "*nessun tipo specificato per la colonna decimale ' Price ' nel tipo di entità' Movie '. In questo modo i valori verranno troncati automaticamente se non rientrano nella precisione e nella scala predefinite. Specificare in modo esplicito il tipo di colonna di SQL Server in grado di contenere tutti i valori utilizzando ' HasColumnType ()'.* È possibile ignorare l'avviso, che verrà risolto in un'esercitazione successiva.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-299">The preceding commands generate the following warning: "*No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'.*" You can ignore that warning, it will be fixed in a later tutorial.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="bf3fe-300">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bf3fe-300">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="bf3fe-301">Esaminare il contesto registrato con l'inserimento di dipendenze</span><span class="sxs-lookup"><span data-stu-id="bf3fe-301">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="bf3fe-302">ASP.NET Core viene compilato con l'[inserimento di dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="bf3fe-302">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="bf3fe-303">I servizi, ad esempio il contesto di database di Entity Framework Core, vengono registrati con l'inserimento delle dipendenze durante l'avvio dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-303">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="bf3fe-304">Questi servizi vengono quindi offerti ai componenti per cui sono necessari (ad esempio Razor Pages) tramite i parametri del costruttore.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-304">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="bf3fe-305">Più avanti nell'esercitazione viene illustrato il codice del costruttore che ottiene un'istanza del contesto di database.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-305">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="bf3fe-306">Lo strumento di scaffolding ha creato automaticamente un contesto del database e lo ha registrato per la dependency injection.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-306">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="bf3fe-307">Esaminare il metodo `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-307">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="bf3fe-308">La riga evidenziata è stata aggiunta dallo scaffolder:</span><span class="sxs-lookup"><span data-stu-id="bf3fe-308">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

<span data-ttu-id="bf3fe-309">`RazorPagesMovieContext` coordina la funzionalità di EF Core (Create, Read, Update, Delete e così via) per il modello `Movie`.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-309">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="bf3fe-310">Il contesto dei dati (`RazorPagesMovieContext`) è derivato da [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="bf3fe-310">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="bf3fe-311">Il contesto dei dati specifica le entità incluse nel modello di dati.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-311">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="bf3fe-312">il codice precedente crea una proprietà [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) per il set di entità.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-312">The preceding code creates a [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="bf3fe-313">Nella terminologia di Entity Framework, un set di entità corrisponde in genere alla tabella di un database.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-313">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="bf3fe-314">Un'entità corrisponde a una riga nella tabella.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-314">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="bf3fe-315">Il nome della stringa di connessione viene passato al contesto chiamando un metodo in un oggetto [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions).</span><span class="sxs-lookup"><span data-stu-id="bf3fe-315">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="bf3fe-316">Per lo sviluppo locale, il [sistema di configurazione di ASP.NET Core](xref:fundamentals/configuration/index) legge la stringa di connessione dal file *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-316">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="bf3fe-317">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="bf3fe-317">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="bf3fe-318">Esaminare il metodo `Up`.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-318">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="bf3fe-319">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="bf3fe-319">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="bf3fe-320">Esaminare il metodo `Up`.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-320">Examine the `Up` method.</span></span>

---

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="bf3fe-321">Eseguire il test dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="bf3fe-321">Test the app</span></span>

* <span data-ttu-id="bf3fe-322">Eseguire l'app e accodare `/Movies` all'URL nel browser (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="bf3fe-322">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="bf3fe-323">Se viene visualizzato l'errore:</span><span class="sxs-lookup"><span data-stu-id="bf3fe-323">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="bf3fe-324">Non è stato eseguita la [migrazione](#pmc).</span><span class="sxs-lookup"><span data-stu-id="bf3fe-324">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="bf3fe-325">Eseguire il test del collegamento **Crea**.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-325">Test the **Create** link.</span></span>

  ![Pagina Create](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="bf3fe-327">Potrebbe non essere possibile immettere virgole decimali nel campo `Price`.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-327">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="bf3fe-328">Per supportare la [convalida jQuery](https://jqueryvalidation.org/) per impostazioni locali diverse dall'inglese che usano la virgola (",") come separatore decimale e per formati di data diversi da quello dell'inglese (Stati Uniti), è necessario localizzare l'app.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-328">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="bf3fe-329">Per istruzioni sulla globalizzazione, vedere [questo problema su GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="bf3fe-329">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="bf3fe-330">Eseguire il test dei collegamenti **Modifica**, **Dettagli** e **Elimina**.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-330">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="bf3fe-331">L'esercitazione successiva illustra i file creati tramite scaffolding.</span><span class="sxs-lookup"><span data-stu-id="bf3fe-331">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="bf3fe-332">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="bf3fe-332">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="bf3fe-333">[Precedente: Introduzione](xref:tutorials/razor-pages/razor-pages-start)
> [Successivo: Pagine Razor create tramite scaffolding](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="bf3fe-333">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end
