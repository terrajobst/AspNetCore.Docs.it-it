---
title: Aggiungere un modello a un'app Razor Pages in ASP.NET Core
author: rick-anderson
description: Scoprire come aggiungere classi per la gestione dei film in un database tramite Entity Framework Core (EF Core).
ms.author: riande
ms.date: 12/05/2019
uid: tutorials/razor-pages/model
ms.openlocfilehash: f6dbac81b4efceb30c379ab06dd715005d879228
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78658935"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="456ed-103">Aggiungere un modello a un'app Razor Pages in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="456ed-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

<span data-ttu-id="456ed-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="456ed-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<!-- In the next update on the CLI version, let the scaffolder do the same work the VS driven scaffolder does. That is, create the DB context, etc -->

<span data-ttu-id="456ed-105">In questa sezione vengono aggiunte le classi per la gestione dei film in un [database SQLite](https://www.sqlite.org/index.html)multipiattaforma.</span><span class="sxs-lookup"><span data-stu-id="456ed-105">In this section, classes are added for managing movies in a cross-platform [SQLite database](https://www.sqlite.org/index.html).</span></span> <span data-ttu-id="456ed-106">Le app create da un modello di ASP.NET Core usano un database SQLite.</span><span class="sxs-lookup"><span data-stu-id="456ed-106">Apps created from an ASP.NET Core template use a SQLite database.</span></span> <span data-ttu-id="456ed-107">Le classi modello dell'app vengono usate con [Entity Framework Core (EF Core)](/ef/core) ([SQLite EF Core provider di database](/ef/core/providers/sqlite)) per lavorare con il database.</span><span class="sxs-lookup"><span data-stu-id="456ed-107">The app's model classes are used with [Entity Framework Core (EF Core)](/ef/core) ([SQLite EF Core Database Provider](/ef/core/providers/sqlite)) to work with the database.</span></span> <span data-ttu-id="456ed-108">EF Core è un framework ORM (Object-Relational Mapping) che semplifica l'accesso ai dati.</span><span class="sxs-lookup"><span data-stu-id="456ed-108">EF Core is an object-relational mapping (ORM) framework that simplifies data access.</span></span>

<span data-ttu-id="456ed-109">Le classi di modello sono dette classi POCO (da "plain-old CLR objects") perché non hanno alcuna dipendenza in EF Core.</span><span class="sxs-lookup"><span data-stu-id="456ed-109">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="456ed-110">Definiscono le proprietà dei dati archiviati nel database.</span><span class="sxs-lookup"><span data-stu-id="456ed-110">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="456ed-111">Aggiungere un modello di dati</span><span class="sxs-lookup"><span data-stu-id="456ed-111">Add a data model</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="456ed-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="456ed-112">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="456ed-113">Fare clic con il pulsante destro del mouse sul progetto **RazorPagesMovie** > **Aggiungi** > **Nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="456ed-113">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="456ed-114">Assegnare il nome *Models* alla cartella.</span><span class="sxs-lookup"><span data-stu-id="456ed-114">Name the folder *Models*.</span></span>

<span data-ttu-id="456ed-115">Fare clic con il pulsante destro del mouse sulla cartella *Models*.</span><span class="sxs-lookup"><span data-stu-id="456ed-115">Right click the *Models* folder.</span></span> <span data-ttu-id="456ed-116">Selezionare **Aggiungi** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="456ed-116">Select **Add** > **Class**.</span></span> <span data-ttu-id="456ed-117">Denominare la classe **Movie**.</span><span class="sxs-lookup"><span data-stu-id="456ed-117">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="456ed-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="456ed-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="456ed-119">Aggiungere una cartella denominata *Models*.</span><span class="sxs-lookup"><span data-stu-id="456ed-119">Add a folder named *Models*.</span></span>
* <span data-ttu-id="456ed-120">Aggiungere una classe alla cartella *Models* denominata *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="456ed-120">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="456ed-121">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="456ed-121">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="456ed-122">In riquadro della soluzione fare clic con il pulsante destro del mouse sul progetto **RazorPagesMovie** , quindi scegliere **Aggiungi** > **nuova cartella...** . Assegnare un nome ai *modelli*di cartella.</span><span class="sxs-lookup"><span data-stu-id="456ed-122">In Solution Pad, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder...**. Name the folder *Models*.</span></span>
* <span data-ttu-id="456ed-123">Fare clic con il pulsante destro del mouse sulla cartella *Models* , quindi scegliere **Aggiungi** > **nuovo file...** .</span><span class="sxs-lookup"><span data-stu-id="456ed-123">Right-click the *Models* folder, and then select **Add** > **New File...**.</span></span>
* <span data-ttu-id="456ed-124">Nella finestra di dialogo **Nuovo file**:</span><span class="sxs-lookup"><span data-stu-id="456ed-124">In the **New File** dialog:</span></span>

  * <span data-ttu-id="456ed-125">Selezionare **Generale** nel riquadro a sinistra.</span><span class="sxs-lookup"><span data-stu-id="456ed-125">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="456ed-126">Selezionare **Classe vuota** nel riquadro centrale.</span><span class="sxs-lookup"><span data-stu-id="456ed-126">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="456ed-127">Denominare la classe **Movie** e selezionare **Nuovo**.</span><span class="sxs-lookup"><span data-stu-id="456ed-127">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

---

<span data-ttu-id="456ed-128">Compilare il progetto per verificare che non siano presenti errori di compilazione.</span><span class="sxs-lookup"><span data-stu-id="456ed-128">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="456ed-129">Eseguire lo scaffolding del modello *Movie*</span><span class="sxs-lookup"><span data-stu-id="456ed-129">Scaffold the movie model</span></span>

<span data-ttu-id="456ed-130">In questa sezione viene eseguito lo scaffolding del modello *Movie*.</span><span class="sxs-lookup"><span data-stu-id="456ed-130">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="456ed-131">Lo strumento di scaffolding crea quindi le pagine per le operazioni CRUD (creazione, lettura, aggiornamento ed eliminazione) per il modello di filmato.</span><span class="sxs-lookup"><span data-stu-id="456ed-131">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="456ed-132">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="456ed-132">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="456ed-133">Creare una cartella *Pages/Movies*:</span><span class="sxs-lookup"><span data-stu-id="456ed-133">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="456ed-134">Fare clic con il pulsante destro del mouse sulla cartella *pagine* > **Aggiungi** > **nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="456ed-134">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="456ed-135">Assegnare il nome *Movies* alla cartella</span><span class="sxs-lookup"><span data-stu-id="456ed-135">Name the folder *Movies*</span></span>

<span data-ttu-id="456ed-136">Fare clic con il pulsante destro del mouse sulla cartella *pages/Movies* > **Aggiungi** > **nuovo elemento con impalcatura**.</span><span class="sxs-lookup"><span data-stu-id="456ed-136">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Immagine relativa alle istruzioni precedenti.](model/_static/sca.png)

<span data-ttu-id="456ed-138">Nella finestra di dialogo **Aggiungi impalcatura** selezionare **Razor Pages utilizzando Entity Framework (CRUD)** > **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="456ed-138">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Immagine relativa alle istruzioni precedenti.](model/_static/add_scaffold.png)

<span data-ttu-id="456ed-140">Completare la finestra di dialogo **Pagine Razor che usano Entity Framework (CRUD)** :</span><span class="sxs-lookup"><span data-stu-id="456ed-140">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="456ed-141">Nel menu a discesa **Classe modello** selezionare **Movie (RazorPagesMovie.Models)** .</span><span class="sxs-lookup"><span data-stu-id="456ed-141">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="456ed-142">Nella riga **Classe contesto di dati** selezionare il segno più **+** e modificare il nome generato da RazorPagesMovie.**Models**.RazorPagesMovieContext a RazorPagesMovie.**Data**.RazorPagesMovieContext.</span><span class="sxs-lookup"><span data-stu-id="456ed-142">In the **Data context class** row, select the **+** (plus) sign and change the generated name from RazorPagesMovie.**Models**.RazorPagesMovieContext to RazorPagesMovie.**Data**.RazorPagesMovieContext.</span></span> <span data-ttu-id="456ed-143">[Questa modifica](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) non è obbligatoria.</span><span class="sxs-lookup"><span data-stu-id="456ed-143">[This change](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) is not required.</span></span> <span data-ttu-id="456ed-144">Crea la classe del contesto di database con lo spazio dei nomi corretto.</span><span class="sxs-lookup"><span data-stu-id="456ed-144">It creates the database context class with the correct namespace.</span></span>
* <span data-ttu-id="456ed-145">Selezionare **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="456ed-145">Select **Add**.</span></span>

![Immagine relativa alle istruzioni precedenti.](model/_static/3/arp.png)

<span data-ttu-id="456ed-147">Il file *appsettings.json* è stato aggiornato con la stringa di connessione usata per connettersi a un database locale.</span><span class="sxs-lookup"><span data-stu-id="456ed-147">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="456ed-148">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="456ed-148">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="456ed-149">Aprire una finestra di comando nella directory del progetto (la directory che contiene i file *Program.cs*, *Startup.cs* e *csproj*).</span><span class="sxs-lookup"><span data-stu-id="456ed-149">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="456ed-150">Installare lo strumento di scaffolding:</span><span class="sxs-lookup"><span data-stu-id="456ed-150">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="456ed-151">**Per Windows**: eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="456ed-151">**For Windows**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="456ed-152">**Per MacOS e Linux**: eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="456ed-152">**For macOS and Linux**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  dotnet tool install --global dotnet-aspnet-codegenerator
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

[!INCLUDE [use SQL Server in production](~/includes/RP/sqlitedev.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="456ed-153">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="456ed-153">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="456ed-154">Creare una cartella *Pages/Movies*:</span><span class="sxs-lookup"><span data-stu-id="456ed-154">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="456ed-155">Fare clic con il pulsante destro del mouse sulla cartella *pagine* > **Aggiungi** > **nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="456ed-155">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="456ed-156">Assegnare il nome *Movies* alla cartella</span><span class="sxs-lookup"><span data-stu-id="456ed-156">Name the folder *Movies*</span></span>

<span data-ttu-id="456ed-157">Fare clic con il pulsante destro del mouse sulla cartella *pages/Movies* > **Aggiungi** > **Nuova impalcatura...** .</span><span class="sxs-lookup"><span data-stu-id="456ed-157">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolding...**.</span></span>

![Immagine relativa alle istruzioni precedenti.](model/_static/scaMac.png)

<span data-ttu-id="456ed-159">Nella finestra di dialogo **Nuova impalcatura** selezionare **Razor Pages utilizzando Entity Framework (CRUD)** > **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="456ed-159">In the **New Scaffolding** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Next**.</span></span>

![Immagine relativa alle istruzioni precedenti.](model/_static/add_scaffoldMac.png)

<span data-ttu-id="456ed-161">Completare la finestra di dialogo **Pagine Razor che usano Entity Framework (CRUD)** :</span><span class="sxs-lookup"><span data-stu-id="456ed-161">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="456ed-162">Nell'elenco a discesa **classe modello** selezionare o digitare **Movie (RazorPagesMovie. Models)** .</span><span class="sxs-lookup"><span data-stu-id="456ed-162">In the **Model class** drop down, select, or type, **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="456ed-163">Nella riga della **classe del contesto dati** Digitare il nome della nuova classe, RazorPagesMovie. **Dati**. RazorPagesMovieContext.</span><span class="sxs-lookup"><span data-stu-id="456ed-163">In the **Data context class** row, type the name for the new class, RazorPagesMovie.**Data**.RazorPagesMovieContext.</span></span> <span data-ttu-id="456ed-164">[Questa modifica](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) non è obbligatoria.</span><span class="sxs-lookup"><span data-stu-id="456ed-164">[This change](https://developercommunity.visualstudio.com/content/problem/652166/aspnet-core-ef-scaffolder-uses-incorrect-namespace.html) is not required.</span></span> <span data-ttu-id="456ed-165">Crea la classe del contesto di database con lo spazio dei nomi corretto.</span><span class="sxs-lookup"><span data-stu-id="456ed-165">It creates the database context class with the correct namespace.</span></span>
* <span data-ttu-id="456ed-166">Selezionare **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="456ed-166">Select **Add**.</span></span>

![Immagine relativa alle istruzioni precedenti.](model/_static/arpMac.png)

<span data-ttu-id="456ed-168">Il file *appsettings.json* è stato aggiornato con la stringa di connessione usata per connettersi a un database locale.</span><span class="sxs-lookup"><span data-stu-id="456ed-168">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

### <a name="add-ef-tools"></a><span data-ttu-id="456ed-169">Aggiungi strumenti EF</span><span class="sxs-lookup"><span data-stu-id="456ed-169">Add EF tools</span></span>

<span data-ttu-id="456ed-170">Eseguire il comando interfaccia della riga di comando di .NET Core seguente:</span><span class="sxs-lookup"><span data-stu-id="456ed-170">Run the following .NET Core CLI command:</span></span>

```dotnetcli
dotnet tool install --global dotnet-ef
```

<span data-ttu-id="456ed-171">Il comando precedente aggiunge gli strumenti Entity Framework Core per l'interfaccia della riga di comando di .NET Core.</span><span class="sxs-lookup"><span data-stu-id="456ed-171">The preceding command adds the Entity Framework Core Tools for the .NET Core CLI.</span></span>

---

### <a name="files-created"></a><span data-ttu-id="456ed-172">File creati</span><span class="sxs-lookup"><span data-stu-id="456ed-172">Files created</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="456ed-173">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="456ed-173">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="456ed-174">Il processo di scaffolding crea e aggiorna i file seguenti:</span><span class="sxs-lookup"><span data-stu-id="456ed-174">The scaffold process creates and updates the following files:</span></span>

* <span data-ttu-id="456ed-175">*Pages/Movies*: pagine Create (Crea), Delete (Elimina), Details (Dettagli), Edit (Modifica) e Index (Indice).</span><span class="sxs-lookup"><span data-stu-id="456ed-175">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="456ed-176">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="456ed-176">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="updated"></a><span data-ttu-id="456ed-177">Updated</span><span class="sxs-lookup"><span data-stu-id="456ed-177">Updated</span></span>

* <span data-ttu-id="456ed-178">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="456ed-178">*Startup.cs*</span></span>

<span data-ttu-id="456ed-179">I file creati e aggiornati sono illustrati nella sezione successiva.</span><span class="sxs-lookup"><span data-stu-id="456ed-179">The created and updated files are explained in the next section.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="456ed-180">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="456ed-180">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="456ed-181">Il processo di scaffolding crea e aggiorna i file seguenti:</span><span class="sxs-lookup"><span data-stu-id="456ed-181">The scaffold process creates and updates the following files:</span></span>

* <span data-ttu-id="456ed-182">*Pages/Movies*: pagine Create (Crea), Delete (Elimina), Details (Dettagli), Edit (Modifica) e Index (Indice).</span><span class="sxs-lookup"><span data-stu-id="456ed-182">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="456ed-183">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="456ed-183">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="updated"></a><span data-ttu-id="456ed-184">Updated</span><span class="sxs-lookup"><span data-stu-id="456ed-184">Updated</span></span>

* <span data-ttu-id="456ed-185">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="456ed-185">*Startup.cs*</span></span>

<span data-ttu-id="456ed-186">I file creati e aggiornati sono illustrati nella sezione successiva.</span><span class="sxs-lookup"><span data-stu-id="456ed-186">The created and updated files are explained in the next section.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="456ed-187">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="456ed-187">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="456ed-188">Il processo di scaffolding crea i file seguenti:</span><span class="sxs-lookup"><span data-stu-id="456ed-188">The scaffold process creates the following files:</span></span>

* <span data-ttu-id="456ed-189">*Pages/Movies*: pagine Create (Crea), Delete (Elimina), Details (Dettagli), Edit (Modifica) e Index (Indice).</span><span class="sxs-lookup"><span data-stu-id="456ed-189">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>

<span data-ttu-id="456ed-190">I file creati sono illustrati nella sezione successiva.</span><span class="sxs-lookup"><span data-stu-id="456ed-190">The created files are explained in the next section.</span></span>

---

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="456ed-191">Migrazione iniziale</span><span class="sxs-lookup"><span data-stu-id="456ed-191">Initial migration</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="456ed-192">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="456ed-192">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="456ed-193">In questa sezione viene usata la Console di Gestione pacchetti (PMC) per:</span><span class="sxs-lookup"><span data-stu-id="456ed-193">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="456ed-194">Aggiungere una migrazione iniziale.</span><span class="sxs-lookup"><span data-stu-id="456ed-194">Add an initial migration.</span></span>
* <span data-ttu-id="456ed-195">Aggiornare il database con la migrazione iniziale.</span><span class="sxs-lookup"><span data-stu-id="456ed-195">Update the database with the initial migration.</span></span>

<span data-ttu-id="456ed-196">Dal menu **strumenti** selezionare **gestione pacchetti NuGet** > console di **Gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="456ed-196">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Menu della Console di Gestione pacchetti](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="456ed-198">In PMC, immettere i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="456ed-198">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration InitialCreate
Update-Database
```

# <a name="visual-studio-code"></a>[<span data-ttu-id="456ed-199">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="456ed-199">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="456ed-200">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="456ed-200">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---

<span data-ttu-id="456ed-201">I comandi precedenti generano l'avviso seguente: "nessun tipo specificato per la colonna decimale ' Price ' nel tipo di entità' Movie '.</span><span class="sxs-lookup"><span data-stu-id="456ed-201">The preceding commands generate the following warning: "No type was specified for the decimal column 'Price' on entity type 'Movie'.</span></span> <span data-ttu-id="456ed-202">This will cause values to be silently truncated if they do not fit in the default precision and scale. (I valori saranno quindi automaticamente troncati se non rispettano la precisione e la scala predefinite).</span><span class="sxs-lookup"><span data-stu-id="456ed-202">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span></span> <span data-ttu-id="456ed-203">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'. (Specificare in modo esplicito il tipo di colonna di SQL Server che può supportare tutti i valori usando 'HasColumnType()')"</span><span class="sxs-lookup"><span data-stu-id="456ed-203">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'."</span></span>

<span data-ttu-id="456ed-204">È possibile ignorare tale avviso. Verrà risolto in un'esercitazione successiva.</span><span class="sxs-lookup"><span data-stu-id="456ed-204">You can ignore that warning, it will be fixed in a later tutorial.</span></span>

<span data-ttu-id="456ed-205">Il comando Migrations genera il codice per creare lo schema del database iniziale.</span><span class="sxs-lookup"><span data-stu-id="456ed-205">The migrations command generates code to create the initial database schema.</span></span> <span data-ttu-id="456ed-206">Lo schema è basato sul modello specificato in `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="456ed-206">The schema is based on the model specified in `DbContext`.</span></span> <span data-ttu-id="456ed-207">L'argomento `InitialCreate` viene usato per denominare le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="456ed-207">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="456ed-208">È possibile usare qualsiasi nome, ma per convenzione viene selezionato un nome che descrive la migrazione.</span><span class="sxs-lookup"><span data-stu-id="456ed-208">Any name can be used, but by convention a name is selected that describes the migration.</span></span>

<span data-ttu-id="456ed-209">Il comando `update` esegue il `Up` metodo nelle migrazioni che non sono state applicate.</span><span class="sxs-lookup"><span data-stu-id="456ed-209">The `update` command runs the `Up` method in migrations that have not been applied.</span></span> <span data-ttu-id="456ed-210">In questo caso, `update` esegue il `Up` metodo in *migrazioni/\<timestamp > _InitialCreate file con estensione cs* , che crea il database.</span><span class="sxs-lookup"><span data-stu-id="456ed-210">In this case, `update` runs the `Up` method in  *Migrations/\<time-stamp>_InitialCreate.cs* file, which creates the database.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="456ed-211">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="456ed-211">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="456ed-212">Esaminare il contesto registrato con l'inserimento di dipendenze</span><span class="sxs-lookup"><span data-stu-id="456ed-212">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="456ed-213">ASP.NET Core viene compilato con l'[inserimento di dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="456ed-213">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="456ed-214">I servizi, ad esempio il contesto di database di Entity Framework Core, vengono registrati con l'inserimento delle dipendenze durante l'avvio dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="456ed-214">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="456ed-215">Questi servizi vengono quindi offerti ai componenti per cui sono necessari (ad esempio Razor Pages) tramite i parametri del costruttore.</span><span class="sxs-lookup"><span data-stu-id="456ed-215">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="456ed-216">Più avanti nell'esercitazione viene illustrato il codice del costruttore che ottiene un'istanza del contesto di database.</span><span class="sxs-lookup"><span data-stu-id="456ed-216">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="456ed-217">Lo strumento di scaffolding ha creato automaticamente un contesto del database e lo ha registrato per la dependency injection.</span><span class="sxs-lookup"><span data-stu-id="456ed-217">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="456ed-218">Esaminare il metodo `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="456ed-218">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="456ed-219">La riga evidenziata è stata aggiunta dallo scaffolder:</span><span class="sxs-lookup"><span data-stu-id="456ed-219">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_ConfigureServices&highlight=5-6)]

<span data-ttu-id="456ed-220">`RazorPagesMovieContext` coordina la funzionalità di EF Core (Create, Read, Update, Delete e così via) per il modello `Movie`.</span><span class="sxs-lookup"><span data-stu-id="456ed-220">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="456ed-221">Il contesto dei dati (`RazorPagesMovieContext`) è derivato da [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="456ed-221">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="456ed-222">Il contesto dei dati specifica le entità incluse nel modello di dati.</span><span class="sxs-lookup"><span data-stu-id="456ed-222">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="456ed-223">Il codice precedente crea una proprietà [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) per il set di entità.</span><span class="sxs-lookup"><span data-stu-id="456ed-223">The preceding code creates a [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="456ed-224">Nella terminologia di Entity Framework, un set di entità corrisponde in genere alla tabella di un database.</span><span class="sxs-lookup"><span data-stu-id="456ed-224">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="456ed-225">Un'entità corrisponde a una riga nella tabella.</span><span class="sxs-lookup"><span data-stu-id="456ed-225">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="456ed-226">Il nome della stringa di connessione viene passato al contesto chiamando un metodo in un oggetto [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions).</span><span class="sxs-lookup"><span data-stu-id="456ed-226">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="456ed-227">Per lo sviluppo locale, il [sistema di configurazione di ASP.NET Core](xref:fundamentals/configuration/index) legge la stringa di connessione dal file *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="456ed-227">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="456ed-228">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="456ed-228">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="456ed-229">Esaminare il metodo `Up`.</span><span class="sxs-lookup"><span data-stu-id="456ed-229">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="456ed-230">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="456ed-230">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="456ed-231">Esaminare il metodo `Up`.</span><span class="sxs-lookup"><span data-stu-id="456ed-231">Examine the `Up` method.</span></span>

---

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="456ed-232">Testare l'app</span><span class="sxs-lookup"><span data-stu-id="456ed-232">Test the app</span></span>

* <span data-ttu-id="456ed-233">Eseguire l'app e accodare `/Movies` all'URL nel browser (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="456ed-233">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="456ed-234">Se viene visualizzato l'errore:</span><span class="sxs-lookup"><span data-stu-id="456ed-234">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="456ed-235">Non è stato eseguita la [migrazione](#pmc).</span><span class="sxs-lookup"><span data-stu-id="456ed-235">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="456ed-236">Eseguire il test del collegamento **Crea**.</span><span class="sxs-lookup"><span data-stu-id="456ed-236">Test the **Create** link.</span></span>

  ![Pagina di creazione](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="456ed-238">Potrebbe non essere possibile immettere virgole decimali nel campo `Price`.</span><span class="sxs-lookup"><span data-stu-id="456ed-238">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="456ed-239">Per supportare la [convalida jQuery](https://jqueryvalidation.org/) per impostazioni locali diverse dall'inglese che usano la virgola (",") come separatore decimale e per formati di data diversi da quello dell'inglese (Stati Uniti), è necessario localizzare l'app.</span><span class="sxs-lookup"><span data-stu-id="456ed-239">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="456ed-240">Per istruzioni sulla localizzazione, vedere [questo problema su GitHub](https://github.com/dotnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="456ed-240">For globalization instructions, see [this GitHub issue](https://github.com/dotnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="456ed-241">Testare i collegamenti **Modifica**, **Dettagli** ed **Elimina**.</span><span class="sxs-lookup"><span data-stu-id="456ed-241">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="456ed-242">L'esercitazione successiva illustra i file creati dallo scaffolding.</span><span class="sxs-lookup"><span data-stu-id="456ed-242">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="456ed-243">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="456ed-243">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="456ed-244">[Precedente: Introduzione](xref:tutorials/razor-pages/razor-pages-start)
> [Successivo: Pagine Razor create tramite scaffolding](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="456ed-244">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end

<!--  ::: moniker previous version   -->
::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="456ed-245">In questa sezione vengono aggiunte le classi per la gestione dei film in un [database SQLite](https://www.sqlite.org/index.html)multipiattaforma.</span><span class="sxs-lookup"><span data-stu-id="456ed-245">In this section, classes are added for managing movies in a cross-platform [SQLite database](https://www.sqlite.org/index.html).</span></span> <span data-ttu-id="456ed-246">Le app create da un modello di ASP.NET Core usano un database SQLite.</span><span class="sxs-lookup"><span data-stu-id="456ed-246">Apps created from an ASP.NET Core template use a SQLite database.</span></span> <span data-ttu-id="456ed-247">Le classi modello dell'app vengono usate con [Entity Framework Core (EF Core)](/ef/core) ([SQLite EF Core provider di database](/ef/core/providers/sqlite)) per lavorare con il database.</span><span class="sxs-lookup"><span data-stu-id="456ed-247">The app's model classes are used with [Entity Framework Core (EF Core)](/ef/core) ([SQLite EF Core Database Provider](/ef/core/providers/sqlite)) to work with the database.</span></span> <span data-ttu-id="456ed-248">EF Core è un framework ORM (Object-Relational Mapping) che semplifica l'accesso ai dati.</span><span class="sxs-lookup"><span data-stu-id="456ed-248">EF Core is an object-relational mapping (ORM) framework that simplifies data access.</span></span>

<span data-ttu-id="456ed-249">Le classi di modello sono dette classi POCO (da "plain-old CLR objects") perché non hanno alcuna dipendenza in EF Core.</span><span class="sxs-lookup"><span data-stu-id="456ed-249">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="456ed-250">Definiscono le proprietà dei dati archiviati nel database.</span><span class="sxs-lookup"><span data-stu-id="456ed-250">They define the properties of the data that are stored in the database.</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="456ed-251">Aggiungere un modello di dati</span><span class="sxs-lookup"><span data-stu-id="456ed-251">Add a data model</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="456ed-252">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="456ed-252">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="456ed-253">Fare clic con il pulsante destro del mouse sul progetto **RazorPagesMovie** > **Aggiungi** > **Nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="456ed-253">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="456ed-254">Assegnare il nome *Models* alla cartella.</span><span class="sxs-lookup"><span data-stu-id="456ed-254">Name the folder *Models*.</span></span>

<span data-ttu-id="456ed-255">Fare clic con il pulsante destro del mouse sulla cartella *Models*.</span><span class="sxs-lookup"><span data-stu-id="456ed-255">Right click the *Models* folder.</span></span> <span data-ttu-id="456ed-256">Selezionare **Aggiungi** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="456ed-256">Select **Add** > **Class**.</span></span> <span data-ttu-id="456ed-257">Denominare la classe **Movie**.</span><span class="sxs-lookup"><span data-stu-id="456ed-257">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="456ed-258">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="456ed-258">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="456ed-259">Aggiungere una cartella denominata *Models*.</span><span class="sxs-lookup"><span data-stu-id="456ed-259">Add a folder named *Models*.</span></span>
* <span data-ttu-id="456ed-260">Aggiungere una classe alla cartella *Models* denominata *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="456ed-260">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="456ed-261">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="456ed-261">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="456ed-262">In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto **RazorPagesMovie**, quindi selezionare **Aggiungi** > **Nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="456ed-262">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="456ed-263">Assegnare il nome *Models* alla cartella.</span><span class="sxs-lookup"><span data-stu-id="456ed-263">Name the folder *Models*.</span></span>
* <span data-ttu-id="456ed-264">Fare clic con il pulsante destro del mouse sulla cartella *Models* , quindi scegliere **Aggiungi** > **nuovo file**.</span><span class="sxs-lookup"><span data-stu-id="456ed-264">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="456ed-265">Nella finestra di dialogo **Nuovo file**:</span><span class="sxs-lookup"><span data-stu-id="456ed-265">In the **New File** dialog:</span></span>

  * <span data-ttu-id="456ed-266">Selezionare **Generale** nel riquadro a sinistra.</span><span class="sxs-lookup"><span data-stu-id="456ed-266">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="456ed-267">Selezionare **Classe vuota** nel riquadro centrale.</span><span class="sxs-lookup"><span data-stu-id="456ed-267">Select **Empty Class** in the center pane.</span></span>
  * <span data-ttu-id="456ed-268">Denominare la classe **Movie** e selezionare **Nuovo**.</span><span class="sxs-lookup"><span data-stu-id="456ed-268">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

---

<span data-ttu-id="456ed-269">Compilare il progetto per verificare che non siano presenti errori di compilazione.</span><span class="sxs-lookup"><span data-stu-id="456ed-269">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="456ed-270">Eseguire lo scaffolding del modello *Movie*</span><span class="sxs-lookup"><span data-stu-id="456ed-270">Scaffold the movie model</span></span>

<span data-ttu-id="456ed-271">In questa sezione viene eseguito lo scaffolding del modello *Movie*.</span><span class="sxs-lookup"><span data-stu-id="456ed-271">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="456ed-272">Lo strumento di scaffolding crea quindi le pagine per le operazioni CRUD (creazione, lettura, aggiornamento ed eliminazione) per il modello di filmato.</span><span class="sxs-lookup"><span data-stu-id="456ed-272">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="456ed-273">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="456ed-273">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="456ed-274">Creare una cartella *Pages/Movies*:</span><span class="sxs-lookup"><span data-stu-id="456ed-274">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="456ed-275">Fare clic con il pulsante destro del mouse sulla cartella *pagine* > **Aggiungi** > **nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="456ed-275">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="456ed-276">Assegnare il nome *Movies* alla cartella</span><span class="sxs-lookup"><span data-stu-id="456ed-276">Name the folder *Movies*</span></span>

<span data-ttu-id="456ed-277">Fare clic con il pulsante destro del mouse sulla cartella *pages/Movies* > **Aggiungi** > **nuovo elemento con impalcatura**.</span><span class="sxs-lookup"><span data-stu-id="456ed-277">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Immagine relativa alle istruzioni precedenti.](model/_static/sca.png)

<span data-ttu-id="456ed-279">Nella finestra di dialogo **Aggiungi impalcatura** selezionare **Razor Pages utilizzando Entity Framework (CRUD)** > **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="456ed-279">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Immagine relativa alle istruzioni precedenti.](model/_static/add_scaffold.png)

<span data-ttu-id="456ed-281">Completare la finestra di dialogo **Pagine Razor che usano Entity Framework (CRUD)** :</span><span class="sxs-lookup"><span data-stu-id="456ed-281">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>
<!-- In the next section, change 
(plus) sign and accept the generated name 
to use Data, it should not use models. That will make the namespace the same for the VS version and the CLI version
-->

* <span data-ttu-id="456ed-282">Nel menu a discesa **Classe modello** selezionare **Movie (RazorPagesMovie.Models)** .</span><span class="sxs-lookup"><span data-stu-id="456ed-282">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="456ed-283">Nella riga **Classe contesto di dati** selezionare il segno più **+** e accettare il nome generato **RazorPagesMovie.Models.RazorPagesMovieContext**.</span><span class="sxs-lookup"><span data-stu-id="456ed-283">In the **Data context class** row, select the **+** (plus) sign and accept the generated name **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="456ed-284">Selezionare **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="456ed-284">Select **Add**.</span></span>

![Immagine relativa alle istruzioni precedenti.](model/_static/arp.png)

<span data-ttu-id="456ed-286">Il file *appsettings.json* è stato aggiornato con la stringa di connessione usata per connettersi a un database locale.</span><span class="sxs-lookup"><span data-stu-id="456ed-286">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="456ed-287">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="456ed-287">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="456ed-288">Aprire una finestra di comando nella directory del progetto (la directory che contiene i file *Program.cs*, *Startup.cs* e *csproj*).</span><span class="sxs-lookup"><span data-stu-id="456ed-288">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>

* <span data-ttu-id="456ed-289">**Per Windows**: eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="456ed-289">**For Windows**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="456ed-290">**Per MacOS e Linux**: eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="456ed-290">**For macOS and Linux**: Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="456ed-291">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="456ed-291">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="456ed-292">Creare una cartella *Pages/Movies*:</span><span class="sxs-lookup"><span data-stu-id="456ed-292">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="456ed-293">Fare clic con il pulsante destro del mouse sulla cartella *pagine* > **Aggiungi** > **nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="456ed-293">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="456ed-294">Assegnare il nome *Movies* alla cartella</span><span class="sxs-lookup"><span data-stu-id="456ed-294">Name the folder *Movies*</span></span>

<span data-ttu-id="456ed-295">Fare clic con il pulsante destro del mouse sulla cartella *pages/Movies* > **Aggiungi** > **nuovo elemento con impalcatura**.</span><span class="sxs-lookup"><span data-stu-id="456ed-295">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Immagine relativa alle istruzioni precedenti.](model/_static/scaMac.png)

<span data-ttu-id="456ed-297">Nella finestra di dialogo **Aggiungi nuova impalcatura** selezionare **Razor Pages con Entity Framework (CRUD)** > **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="456ed-297">In the **Add New Scaffolding** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Immagine relativa alle istruzioni precedenti.](model/_static/add_scaffoldMac.png)

<span data-ttu-id="456ed-299">Completare la finestra di dialogo **Pagine Razor che usano Entity Framework (CRUD)** :</span><span class="sxs-lookup"><span data-stu-id="456ed-299">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="456ed-300">Nell'elenco a discesa **classe modello** selezionare o digitare **Movie**.</span><span class="sxs-lookup"><span data-stu-id="456ed-300">In the **Model class** drop down, select or type **Movie**.</span></span>
* <span data-ttu-id="456ed-301">Nella riga della **classe del contesto dati** Digitare select the **RazorPagesMovieContext** . verrà creata una nuova classe del contesto DB con lo spazio dei nomi corretto.</span><span class="sxs-lookup"><span data-stu-id="456ed-301">In the **Data context class** row, type select the **RazorPagesMovieContext** this will create a new db context class with the correct namespace.</span></span> <span data-ttu-id="456ed-302">In questo caso sarà **RazorPagesMovie. Models. RazorPagesMovieContext**.</span><span class="sxs-lookup"><span data-stu-id="456ed-302">In this case it will be  **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="456ed-303">Selezionare **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="456ed-303">Select **Add**.</span></span>

![Immagine relativa alle istruzioni precedenti.](model/_static/arpMac.png)

<span data-ttu-id="456ed-305">Il file *appsettings.json* è stato aggiornato con la stringa di connessione usata per connettersi a un database locale.</span><span class="sxs-lookup"><span data-stu-id="456ed-305">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

---

<span data-ttu-id="456ed-306">Il processo di scaffolding crea e aggiorna i file seguenti:</span><span class="sxs-lookup"><span data-stu-id="456ed-306">The scaffold process creates and updates the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="456ed-307">File creati</span><span class="sxs-lookup"><span data-stu-id="456ed-307">Files created</span></span>

* <span data-ttu-id="456ed-308">*Pages/Movies*: pagine Create (Crea), Delete (Elimina), Details (Dettagli), Edit (Modifica) e Index (Indice).</span><span class="sxs-lookup"><span data-stu-id="456ed-308">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="456ed-309">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="456ed-309">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="file-updated"></a><span data-ttu-id="456ed-310">File aggiornato</span><span class="sxs-lookup"><span data-stu-id="456ed-310">File updated</span></span>

* <span data-ttu-id="456ed-311">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="456ed-311">*Startup.cs*</span></span>

<span data-ttu-id="456ed-312">I file creati e aggiornati sono illustrati nella sezione successiva.</span><span class="sxs-lookup"><span data-stu-id="456ed-312">The created and updated files are explained in the next section.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="456ed-313">Migrazione iniziale</span><span class="sxs-lookup"><span data-stu-id="456ed-313">Initial migration</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="456ed-314">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="456ed-314">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="456ed-315">In questa sezione viene usata la Console di Gestione pacchetti (PMC) per:</span><span class="sxs-lookup"><span data-stu-id="456ed-315">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="456ed-316">Aggiungere una migrazione iniziale.</span><span class="sxs-lookup"><span data-stu-id="456ed-316">Add an initial migration.</span></span>
* <span data-ttu-id="456ed-317">Aggiornare il database con la migrazione iniziale.</span><span class="sxs-lookup"><span data-stu-id="456ed-317">Update the database with the initial migration.</span></span>

<span data-ttu-id="456ed-318">Dal menu **strumenti** selezionare **gestione pacchetti NuGet** > console di **Gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="456ed-318">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Menu della Console di Gestione pacchetti](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="456ed-320">In PMC, immettere i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="456ed-320">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration Initial
Update-Database
```

<span data-ttu-id="456ed-321">Il comando `Add-Migration` genera un codice per creare lo schema del database iniziale.</span><span class="sxs-lookup"><span data-stu-id="456ed-321">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="456ed-322">Lo schema è basato sul modello specificato in `DbContext` (nel file *RazorPagesMovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="456ed-322">The schema is based on the model specified in the `DbContext` (In the *RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="456ed-323">L'argomento `InitialCreate` viene usato per assegnare un nome alla migrazione.</span><span class="sxs-lookup"><span data-stu-id="456ed-323">The `InitialCreate` argument is used to name the migration.</span></span> <span data-ttu-id="456ed-324">È possibile usare qualsiasi nome, ma per convenzione viene usato un nome che descrive la migrazione.</span><span class="sxs-lookup"><span data-stu-id="456ed-324">Any name can be used, but by convention a name that describes the migration is used.</span></span> <span data-ttu-id="456ed-325">Per altre informazioni, vedere <xref:data/ef-mvc/migrations>.</span><span class="sxs-lookup"><span data-stu-id="456ed-325">For more information, see <xref:data/ef-mvc/migrations>.</span></span>

<span data-ttu-id="456ed-326">Il comando `Update-Database` esegue il metodo `Up` nel file *Migrations/\<time-stamp>_InitialCreate.cs*.</span><span class="sxs-lookup"><span data-stu-id="456ed-326">The `Update-Database` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="456ed-327">Il metodo `Up` crea il database.</span><span class="sxs-lookup"><span data-stu-id="456ed-327">The `Up` method creates the database.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="456ed-328">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="456ed-328">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="456ed-329">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="456ed-329">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---
> [!NOTE]
> <span data-ttu-id="456ed-330">I comandi precedenti generano l'avviso seguente: "*nessun tipo specificato per la colonna decimale ' Price ' nel tipo di entità' Movie '. In questo modo i valori verranno troncati automaticamente se non rientrano nella precisione e nella scala predefinite. Specificare in modo esplicito il tipo di colonna di SQL Server in grado di contenere tutti i valori utilizzando ' HasColumnType ()'.* È possibile ignorare l'avviso, che verrà risolto in un'esercitazione successiva.</span><span class="sxs-lookup"><span data-stu-id="456ed-330">The preceding commands generate the following warning: "*No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'.*" You can ignore that warning, it will be fixed in a later tutorial.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="456ed-331">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="456ed-331">Visual Studio</span></span>](#tab/visual-studio)

### <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="456ed-332">Esaminare il contesto registrato con l'inserimento di dipendenze</span><span class="sxs-lookup"><span data-stu-id="456ed-332">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="456ed-333">ASP.NET Core viene compilato con l'[inserimento di dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="456ed-333">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="456ed-334">I servizi, ad esempio il contesto di database di Entity Framework Core, vengono registrati con l'inserimento delle dipendenze durante l'avvio dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="456ed-334">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="456ed-335">Questi servizi vengono quindi offerti ai componenti per cui sono necessari (ad esempio Razor Pages) tramite i parametri del costruttore.</span><span class="sxs-lookup"><span data-stu-id="456ed-335">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="456ed-336">Più avanti nell'esercitazione viene illustrato il codice del costruttore che ottiene un'istanza del contesto di database.</span><span class="sxs-lookup"><span data-stu-id="456ed-336">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="456ed-337">Lo strumento di scaffolding ha creato automaticamente un contesto del database e lo ha registrato per la dependency injection.</span><span class="sxs-lookup"><span data-stu-id="456ed-337">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="456ed-338">Esaminare il metodo `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="456ed-338">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="456ed-339">La riga evidenziata è stata aggiunta dallo scaffolder:</span><span class="sxs-lookup"><span data-stu-id="456ed-339">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

<span data-ttu-id="456ed-340">`RazorPagesMovieContext` coordina la funzionalità di EF Core (Create, Read, Update, Delete e così via) per il modello `Movie`.</span><span class="sxs-lookup"><span data-stu-id="456ed-340">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="456ed-341">Il contesto dei dati (`RazorPagesMovieContext`) è derivato da [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="456ed-341">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="456ed-342">Il contesto dei dati specifica le entità incluse nel modello di dati.</span><span class="sxs-lookup"><span data-stu-id="456ed-342">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="456ed-343">Il codice precedente crea una proprietà [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) per il set di entità.</span><span class="sxs-lookup"><span data-stu-id="456ed-343">The preceding code creates a [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="456ed-344">Nella terminologia di Entity Framework, un set di entità corrisponde in genere alla tabella di un database.</span><span class="sxs-lookup"><span data-stu-id="456ed-344">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="456ed-345">Un'entità corrisponde a una riga nella tabella.</span><span class="sxs-lookup"><span data-stu-id="456ed-345">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="456ed-346">Il nome della stringa di connessione viene passato al contesto chiamando un metodo in un oggetto [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions).</span><span class="sxs-lookup"><span data-stu-id="456ed-346">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="456ed-347">Per lo sviluppo locale, il [sistema di configurazione di ASP.NET Core](xref:fundamentals/configuration/index) legge la stringa di connessione dal file *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="456ed-347">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="456ed-348">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="456ed-348">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="456ed-349">Esaminare il metodo `Up`.</span><span class="sxs-lookup"><span data-stu-id="456ed-349">Examine the `Up` method.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="456ed-350">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="456ed-350">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="456ed-351">Esaminare il metodo `Up`.</span><span class="sxs-lookup"><span data-stu-id="456ed-351">Examine the `Up` method.</span></span>

---

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="456ed-352">Testare l'app</span><span class="sxs-lookup"><span data-stu-id="456ed-352">Test the app</span></span>

* <span data-ttu-id="456ed-353">Eseguire l'app e accodare `/Movies` all'URL nel browser (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="456ed-353">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="456ed-354">Se viene visualizzato l'errore:</span><span class="sxs-lookup"><span data-stu-id="456ed-354">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="456ed-355">Non è stato eseguita la [migrazione](#pmc).</span><span class="sxs-lookup"><span data-stu-id="456ed-355">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="456ed-356">Eseguire il test del collegamento **Crea**.</span><span class="sxs-lookup"><span data-stu-id="456ed-356">Test the **Create** link.</span></span>

  ![Pagina di creazione](model/_static/conan.png)

  > [!NOTE]
  > <span data-ttu-id="456ed-358">Potrebbe non essere possibile immettere virgole decimali nel campo `Price`.</span><span class="sxs-lookup"><span data-stu-id="456ed-358">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="456ed-359">Per supportare la [convalida jQuery](https://jqueryvalidation.org/) per impostazioni locali diverse dall'inglese che usano la virgola (",") come separatore decimale e per formati di data diversi da quello dell'inglese (Stati Uniti), è necessario localizzare l'app.</span><span class="sxs-lookup"><span data-stu-id="456ed-359">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="456ed-360">Per istruzioni sulla localizzazione, vedere [questo problema su GitHub](https://github.com/dotnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="456ed-360">For globalization instructions, see [this GitHub issue](https://github.com/dotnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="456ed-361">Testare i collegamenti **Modifica**, **Dettagli** ed **Elimina**.</span><span class="sxs-lookup"><span data-stu-id="456ed-361">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="456ed-362">L'esercitazione successiva illustra i file creati dallo scaffolding.</span><span class="sxs-lookup"><span data-stu-id="456ed-362">The next tutorial explains the files created by scaffolding.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="456ed-363">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="456ed-363">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="456ed-364">[Precedente: Introduzione](xref:tutorials/razor-pages/razor-pages-start)
> [Successivo: Pagine Razor create tramite scaffolding](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="456ed-364">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>

::: moniker-end
