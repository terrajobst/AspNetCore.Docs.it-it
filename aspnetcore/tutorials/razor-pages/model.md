---
title: Aggiungere un modello a un'app Razor Pages in ASP.NET Core
author: rick-anderson
description: Scoprire come aggiungere classi per la gestione dei film in un database tramite Entity Framework Core (EF Core).
ms.author: riande
monikerRange: '>= aspnetcore-2.2'
ms.date: 12/3/2018
uid: tutorials/razor-pages/model
ms.openlocfilehash: 91fee1db820493be671fecaee3cfb4c1b7df8bd3
ms.sourcegitcommit: 49faca2644590fc081d86db46ea5e29edfc28b7b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/09/2018
ms.locfileid: "53121363"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="eafb9-103">Aggiungere un modello a un'app Razor Pages in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="eafb9-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

<span data-ttu-id="eafb9-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="eafb9-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

<span data-ttu-id="eafb9-105">In questa sezione si aggiungono alcune classi per la gestione di filmati in un database.</span><span class="sxs-lookup"><span data-stu-id="eafb9-105">In this section, classes are added for managing movies in a database.</span></span> <span data-ttu-id="eafb9-106">Queste classi vengono usate con [Entity Framework Core](/ef/core) (EF Core) per lavorare in un database.</span><span class="sxs-lookup"><span data-stu-id="eafb9-106">These classes are used with [Entity Framework Core](/ef/core) (EF Core) to work with a database.</span></span> <span data-ttu-id="eafb9-107">EF Core è un framework object-relational mapping (ORM) che semplifica il codice di accesso ai dati.</span><span class="sxs-lookup"><span data-stu-id="eafb9-107">EF Core is an object-relational mapping (ORM) framework that simplifies data access code.</span></span>

<span data-ttu-id="eafb9-108">Le classi di modello sono dette classi POCO (da "plain-old CLR objects") perché non hanno alcuna dipendenza in EF Core.</span><span class="sxs-lookup"><span data-stu-id="eafb9-108">The model classes are known as POCO classes (from "plain-old CLR objects") because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="eafb9-109">Definiscono le proprietà dei dati archiviati nel database.</span><span class="sxs-lookup"><span data-stu-id="eafb9-109">They define the properties of the data that are stored in the database.</span></span>

<span data-ttu-id="eafb9-110">[Visualizzare o scaricare](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages-start/sample/) l'esempio.</span><span class="sxs-lookup"><span data-stu-id="eafb9-110">[View or download](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages-start/sample/) sample.</span></span>

## <a name="add-a-data-model"></a><span data-ttu-id="eafb9-111">Aggiungere un modello di dati</span><span class="sxs-lookup"><span data-stu-id="eafb9-111">Add a data model</span></span>

<!-- VS -------------------------->

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="eafb9-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="eafb9-112">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="eafb9-113">Fare clic con il pulsante destro del mouse sul progetto **RazorPagesMovie** > **Aggiungi** > **Nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="eafb9-113">Right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="eafb9-114">Assegnare il nome *Modelli* alla cartella.</span><span class="sxs-lookup"><span data-stu-id="eafb9-114">Name the folder *Models*.</span></span>

<span data-ttu-id="eafb9-115">Fare clic con il pulsante destro del mouse sulla cartella *Models*.</span><span class="sxs-lookup"><span data-stu-id="eafb9-115">Right click the *Models* folder.</span></span> <span data-ttu-id="eafb9-116">Selezionare **Aggiungi** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="eafb9-116">Select **Add** > **Class**.</span></span> <span data-ttu-id="eafb9-117">Denominare la classe **Movie**.</span><span class="sxs-lookup"><span data-stu-id="eafb9-117">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

<!-- Code -------------------------->

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="eafb9-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="eafb9-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="eafb9-119">Aggiungere una cartella denominata *Modelli*.</span><span class="sxs-lookup"><span data-stu-id="eafb9-119">Add a folder named *Models*.</span></span>
* <span data-ttu-id="eafb9-120">Aggiungere una classe alla cartella *Modello* denominata *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="eafb9-120">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

<!-- Mac -------------------------->
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="eafb9-121">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="eafb9-121">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="eafb9-122">In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto **RazorPagesMovie**, quindi selezionare **Aggiungi** > **Nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="eafb9-122">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="eafb9-123">Assegnare il nome *Modelli* alla cartella.</span><span class="sxs-lookup"><span data-stu-id="eafb9-123">Name the folder *Models*.</span></span>
* <span data-ttu-id="eafb9-124">Fare clic con il pulsante destro del mouse sulla cartella *Modelli* e scegliere **Aggiungi** > **Nuovo file**.</span><span class="sxs-lookup"><span data-stu-id="eafb9-124">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="eafb9-125">Nella finestra di dialogo **Nuovo file**:</span><span class="sxs-lookup"><span data-stu-id="eafb9-125">In the **New File** dialog:</span></span>

  * <span data-ttu-id="eafb9-126">Selezionare **Generale** nel riquadro a sinistra.</span><span class="sxs-lookup"><span data-stu-id="eafb9-126">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="eafb9-127">Selezionare **Classe vuota** nel riquadro centrale.</span><span class="sxs-lookup"><span data-stu-id="eafb9-127">Select **Empty Class** in the center pain.</span></span>
  * <span data-ttu-id="eafb9-128">Denominare la classe **Filmato** e selezionare **Nuovo**.</span><span class="sxs-lookup"><span data-stu-id="eafb9-128">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

<!-- End of VS tabs -->

---

<span data-ttu-id="eafb9-129">Compilare il progetto per verificare che non siano presenti errori di compilazione.</span><span class="sxs-lookup"><span data-stu-id="eafb9-129">Build the project to verify there are no compilation errors.</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="eafb9-130">Eseguire lo scaffolding del modello di filmato</span><span class="sxs-lookup"><span data-stu-id="eafb9-130">Scaffold the movie model</span></span>

<span data-ttu-id="eafb9-131">In questa sezione viene eseguito lo scaffolding del modello di filmato.</span><span class="sxs-lookup"><span data-stu-id="eafb9-131">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="eafb9-132">Lo strumento di scaffolding crea quindi le pagine per le operazioni CRUD (creazione, lettura, aggiornamento ed eliminazione) per il modello di filmato.</span><span class="sxs-lookup"><span data-stu-id="eafb9-132">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

<!-- VS -------------------------->

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="eafb9-133">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="eafb9-133">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="eafb9-134">Creare una cartella *Pages/Movies*:</span><span class="sxs-lookup"><span data-stu-id="eafb9-134">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="eafb9-135">Fare clic con il pulsante destro del mouse sulla cartella *Pages* > **Aggiungi** > **Nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="eafb9-135">Right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="eafb9-136">Assegnare il nome *Movies* alla cartella</span><span class="sxs-lookup"><span data-stu-id="eafb9-136">Name the folder *Movies*</span></span>

<span data-ttu-id="eafb9-137">Fare clic con il pulsante destro del mouse sulla cartella *Pages* > **Aggiungi** > **Nuovo elemento di scaffolding**.</span><span class="sxs-lookup"><span data-stu-id="eafb9-137">Right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Immagine relativa alle istruzioni precedenti.](model/_static/sca.png)

<span data-ttu-id="eafb9-139">Nella finestra di dialogo **Aggiungi scaffolding** selezionare **Pagine Razor che usano Entity Framework (CRUD)** > **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="eafb9-139">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Immagine relativa alle istruzioni precedenti.](model/_static/add_scaffold.png)

<span data-ttu-id="eafb9-141">Completare la finestra di dialogo **Pagine Razor che usano Entity Framework (CRUD)**:</span><span class="sxs-lookup"><span data-stu-id="eafb9-141">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="eafb9-142">Nel menu a discesa **Classe modello** selezionare **Movie (RazorPagesMovie.Models)**.</span><span class="sxs-lookup"><span data-stu-id="eafb9-142">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="eafb9-143">Nella riga **Classe contesto di dati** selezionare il segno più **+** e accettare il nome generato **RazorPagesMovie.Models.RazorPagesMovieContext**.</span><span class="sxs-lookup"><span data-stu-id="eafb9-143">In the **Data context class** row, select the **+** (plus) sign and accept the generated name **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="eafb9-144">Selezionare **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="eafb9-144">Select **Add**.</span></span>

![Immagine relativa alle istruzioni precedenti.](model/_static/arp.png)

<span data-ttu-id="eafb9-146">Il file *appsettings.json* è stato aggiornato con la stringa di connessione usata per connettersi a un database locale.</span><span class="sxs-lookup"><span data-stu-id="eafb9-146">The *appsettings.json* file is updated with the connection string used to connect to a local database.</span></span>

<!-- Code -------------------------->

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="eafb9-147">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="eafb9-147">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="eafb9-148">Aprire una finestra di comando nella directory del progetto (la directory che contiene i file *Program.cs*, *Startup.cs* e *csproj*).</span><span class="sxs-lookup"><span data-stu-id="eafb9-148">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="eafb9-149">Installare lo strumento di scaffolding:</span><span class="sxs-lookup"><span data-stu-id="eafb9-149">Install the scaffolding tool:</span></span>

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="eafb9-150">**Per Windows**: eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="eafb9-150">**For Windows**: Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="eafb9-151">**Per MacOS e Linux**: eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="eafb9-151">**For macOS and Linux**: Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

<!-- Mac -------------------------->

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="eafb9-152">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="eafb9-152">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="eafb9-153">Aprire una finestra di comando nella directory del progetto (la directory che contiene i file *Program.cs*, *Startup.cs* e *csproj*).</span><span class="sxs-lookup"><span data-stu-id="eafb9-153">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="eafb9-154">Eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="eafb9-154">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

---

<span data-ttu-id="eafb9-155">Il processo di scaffolding crea e aggiorna i file seguenti:</span><span class="sxs-lookup"><span data-stu-id="eafb9-155">The scaffold process creates and updates the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="eafb9-156">File creati</span><span class="sxs-lookup"><span data-stu-id="eafb9-156">Files created</span></span>

* <span data-ttu-id="eafb9-157">*Pages/Movies*: pagine Create (Crea), Delete (Elimina), Details (Dettagli), Edit (Modifica) e Index (Indice).</span><span class="sxs-lookup"><span data-stu-id="eafb9-157">*Pages/Movies*: Create, Delete, Details, Edit, and Index.</span></span>
* <span data-ttu-id="eafb9-158">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="eafb9-158">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="file-updated"></a><span data-ttu-id="eafb9-159">File aggiornato</span><span class="sxs-lookup"><span data-stu-id="eafb9-159">File updated</span></span>

* <span data-ttu-id="eafb9-160">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="eafb9-160">*Startup.cs*</span></span>

<span data-ttu-id="eafb9-161">I file creati e aggiornati sono illustrati nella sezione successiva.</span><span class="sxs-lookup"><span data-stu-id="eafb9-161">The created and updated files are explained in the next section.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="eafb9-162">Migrazione iniziale</span><span class="sxs-lookup"><span data-stu-id="eafb9-162">Initial migration</span></span>

<!-- VS -------------------------->

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="eafb9-163">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="eafb9-163">Visual Studio</span></span>](#tab/visual-studio)

<!-- VS -------------------------->

<span data-ttu-id="eafb9-164">In questa sezione viene usata la Console di Gestione pacchetti (PMC) per:</span><span class="sxs-lookup"><span data-stu-id="eafb9-164">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="eafb9-165">Aggiungere una migrazione iniziale.</span><span class="sxs-lookup"><span data-stu-id="eafb9-165">Add an initial migration.</span></span>
* <span data-ttu-id="eafb9-166">Aggiornare il database con la migrazione iniziale.</span><span class="sxs-lookup"><span data-stu-id="eafb9-166">Update the database with the initial migration.</span></span>

<span data-ttu-id="eafb9-167">Dal menu **Strumenti** selezionare **Gestione pacchetti NuGet** > **Console di Gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="eafb9-167">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Menu della Console di Gestione pacchetti](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="eafb9-169">Nella Console di Gestione pacchetti immettere i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="eafb9-169">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration Initial
Update-Database
```

<!-- Code -------------------------->

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="eafb9-170">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="eafb9-170">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!-- Mac -------------------------->

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="eafb9-171">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="eafb9-171">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---  
<!-- End of VS tabs -->

<span data-ttu-id="eafb9-172">Il comando `ef migrations add InitialCreate` genera un codice per creare lo schema del database iniziale.</span><span class="sxs-lookup"><span data-stu-id="eafb9-172">The `ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span> <span data-ttu-id="eafb9-173">Lo schema è basato sul modello specificato in `DbContext` (nel file *Models/RazorPagesMovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="eafb9-173">The schema is based on the model specified in the `DbContext` (In the *Models/RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="eafb9-174">L'argomento `InitialCreate` viene usato per denominare le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="eafb9-174">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="eafb9-175">È possibile usare qualsiasi nome, ma per convenzione viene selezionato un nome che descrive la migrazione.</span><span class="sxs-lookup"><span data-stu-id="eafb9-175">Any name can be used, but by convention a name is selected that describes the migration.</span></span>

<span data-ttu-id="eafb9-176">Il comando `ef database update` esegue il metodo `Up` nel file *Migrations/\<time-stamp>_InitialCreate.cs*.</span><span class="sxs-lookup"><span data-stu-id="eafb9-176">The `ef database update` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="eafb9-177">Il metodo `Up` crea il database.</span><span class="sxs-lookup"><span data-stu-id="eafb9-177">The `Up` method creates the database.</span></span>

<!-- VS -------------------------->

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="eafb9-178">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="eafb9-178">Visual Studio</span></span>](#tab/visual-studio)

## <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="eafb9-179">Esaminare il contesto registrato con l'inserimento di dipendenze</span><span class="sxs-lookup"><span data-stu-id="eafb9-179">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="eafb9-180">ASP.NET Core viene compilato con l'[inserimento di dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="eafb9-180">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="eafb9-181">I servizi, ad esempio il contesto di database di Entity Framework Core, vengono registrati con l'inserimento delle dipendenze durante l'avvio dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="eafb9-181">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="eafb9-182">Questi servizi vengono quindi offerti ai componenti per cui sono necessari (ad esempio Razor Pages) tramite i parametri del costruttore.</span><span class="sxs-lookup"><span data-stu-id="eafb9-182">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="eafb9-183">Più avanti nell'esercitazione viene illustrato il codice del costruttore che ottiene un'istanza del contesto di database.</span><span class="sxs-lookup"><span data-stu-id="eafb9-183">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="eafb9-184">Lo strumento di scaffolding ha creato automaticamente un contesto del database e lo ha registrato con il contenitore di inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="eafb9-184">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="eafb9-185">Esaminare il metodo `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="eafb9-185">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="eafb9-186">La riga evidenziata è stata aggiunta dallo scaffolder:</span><span class="sxs-lookup"><span data-stu-id="eafb9-186">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

<span data-ttu-id="eafb9-187">`RazorPagesMovieContext` coordina la funzionalità di EF Core (Create, Read, Update, Delete e così via) per il modello `Movie`.</span><span class="sxs-lookup"><span data-stu-id="eafb9-187">The `RazorPagesMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="eafb9-188">Il contesto dei dati (`RazorPagesMovieContext`) è derivato da [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="eafb9-188">The data context (`RazorPagesMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="eafb9-189">Il contesto dei dati specifica le entità incluse nel modello di dati.</span><span class="sxs-lookup"><span data-stu-id="eafb9-189">The data context specifies which entities are included in the data model.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="eafb9-190">Il codice precedente crea una proprietà [DbSet/\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) per il set di entità.</span><span class="sxs-lookup"><span data-stu-id="eafb9-190">The preceding code creates a [DbSet/\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="eafb9-191">Nella terminologia di Entity Framework, un set di entità corrisponde in genere alla tabella di un database.</span><span class="sxs-lookup"><span data-stu-id="eafb9-191">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="eafb9-192">Un'entità corrisponde a una riga nella tabella.</span><span class="sxs-lookup"><span data-stu-id="eafb9-192">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="eafb9-193">Il nome della stringa di connessione viene passato al contesto chiamando un metodo in un oggetto [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions).</span><span class="sxs-lookup"><span data-stu-id="eafb9-193">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="eafb9-194">Per lo sviluppo locale, il [sistema di configurazione di ASP.NET Core](xref:fundamentals/configuration/index) legge la stringa di connessione dal file *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="eafb9-194">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>
<!-- Code -------------------------->

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="eafb9-195">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="eafb9-195">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!-- Mac -------------------------->

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="eafb9-196">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="eafb9-196">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<!-- End of VS tabs -->

---

<span data-ttu-id="eafb9-197">Il comando `Add-Migration` genera un codice per creare lo schema del database iniziale.</span><span class="sxs-lookup"><span data-stu-id="eafb9-197">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="eafb9-198">Lo schema è basato sul modello specificato in `RazorPagesMovieContext` (nel file *Data/RazorPagesMovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="eafb9-198">The schema is based on the model specified in the `RazorPagesMovieContext` (In the *Data/RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="eafb9-199">L'argomento `Initial` viene usato per denominare le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="eafb9-199">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="eafb9-200">È possibile usare qualsiasi nome, ma per convenzione viene usato un nome che descrive la migrazione.</span><span class="sxs-lookup"><span data-stu-id="eafb9-200">Any name can be used, but by convention a name that describes the migration is used.</span></span> <span data-ttu-id="eafb9-201">Per altre informazioni vedere [Introduzione alle migrazioni](xref:data/ef-mvc/migrations#introduction-to-migrations).</span><span class="sxs-lookup"><span data-stu-id="eafb9-201">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="eafb9-202">Il comando `Update-Database` esegue il metodo `Up` nel file *Migrations/{time-stamp}_InitialCreate.cs*, che crea il database.</span><span class="sxs-lookup"><span data-stu-id="eafb9-202">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="eafb9-203">Eseguire il test dell'app</span><span class="sxs-lookup"><span data-stu-id="eafb9-203">Test the app</span></span>

* <span data-ttu-id="eafb9-204">Eseguire l'app e accodare `/Movies` all'URL nel browser (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="eafb9-204">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="eafb9-205">Se viene visualizzato l'errore:</span><span class="sxs-lookup"><span data-stu-id="eafb9-205">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="eafb9-206">Non è stato eseguita la [migrazione](#pmc).</span><span class="sxs-lookup"><span data-stu-id="eafb9-206">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="eafb9-207">Eseguire il test del collegamento **Crea**.</span><span class="sxs-lookup"><span data-stu-id="eafb9-207">Test the **Create** link.</span></span>

  ![Pagina Crea](model/_static/conan.png)
  
  > [!NOTE]
  > <span data-ttu-id="eafb9-209">Potrebbe non essere possibile immettere virgole decimali nel campo `Price`.</span><span class="sxs-lookup"><span data-stu-id="eafb9-209">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="eafb9-210">Per supportare la [convalida jQuery](https://jqueryvalidation.org/) per impostazioni locali diverse dall'inglese che usano la virgola (",") come separatore decimale e per formati di data diversi da quello dell'inglese (Stati Uniti), è necessario globalizzare l'app.</span><span class="sxs-lookup"><span data-stu-id="eafb9-210">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="eafb9-211">Per istruzioni sulla globalizzazione, vedere [questo problema su GitHub](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="eafb9-211">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="eafb9-212">Eseguire il test dei collegamenti **Modifica**, **Dettagli** e **Elimina**.</span><span class="sxs-lookup"><span data-stu-id="eafb9-212">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="eafb9-213">L'esercitazione successiva illustra i file creati tramite scaffolding.</span><span class="sxs-lookup"><span data-stu-id="eafb9-213">The next tutorial explains the files created by scaffolding.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="eafb9-214">[Precedente: Introduzione](xref:tutorials/razor-pages/razor-pages-start)
> [Successivo: Pagine Razor create tramite scaffolding](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="eafb9-214">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>
