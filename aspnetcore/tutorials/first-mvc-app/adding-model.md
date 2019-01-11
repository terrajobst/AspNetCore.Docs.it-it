---
title: Aggiungere un modello a un'app ASP.NET Core MVC
author: rick-anderson
description: Aggiungere un modello a una app semplice di ASP.NET Core.
ms.author: riande
ms.date: 12/8/2017
uid: tutorials/first-mvc-app/adding-model
ms.openlocfilehash: 630b4b0549a8549d9570d701fb1691310ec442c3
ms.sourcegitcommit: 4e87712029de2aceb1cf2c52e9e3dda8195a5b8e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/14/2018
ms.locfileid: "53381855"
---
# <a name="add-a-model-to-an-aspnet-core-mvc-app"></a><span data-ttu-id="c971e-103">Aggiungere un modello a un'app ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="c971e-103">Add a model to an ASP.NET Core MVC app</span></span>

<span data-ttu-id="c971e-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT) e [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="c971e-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="c971e-105">In questa sezione si aggiungono alcune classi per la gestione di filmati in un database.</span><span class="sxs-lookup"><span data-stu-id="c971e-105">In this section, you add classes for managing movies in a database.</span></span> <span data-ttu-id="c971e-106">Queste classi saranno la parte "**M**odel" dell'app **M**VC.</span><span class="sxs-lookup"><span data-stu-id="c971e-106">These classes will be the "**M**odel" part of the **M**VC app.</span></span>

<span data-ttu-id="c971e-107">Si usano queste classi con [Entity Framework Core](/ef/core) (EF Core) per usare un database.</span><span class="sxs-lookup"><span data-stu-id="c971e-107">You use these classes with [Entity Framework Core](/ef/core) (EF Core) to work with a database.</span></span> <span data-ttu-id="c971e-108">EF Core è un framework object-relational mapping (ORM) che semplifica il codice di accesso ai dati che è necessario scrivere.</span><span class="sxs-lookup"><span data-stu-id="c971e-108">EF Core is an object-relational mapping (ORM) framework that simplifies the data access code that you have to write.</span></span>

<span data-ttu-id="c971e-109">Le classi di modello create sono dette classi POCO (da **P**lain **O**ld **C**LR **O**bjects) perché non hanno alcuna dipendenza in EF Core.</span><span class="sxs-lookup"><span data-stu-id="c971e-109">The model classes you create are known as POCO classes (from **P**lain **O**ld **C**LR **O**bjects) because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="c971e-110">Definiscono semplicemente le proprietà dei dati che verranno archiviati nel database.</span><span class="sxs-lookup"><span data-stu-id="c971e-110">They just define the properties of the data that will be stored in the database.</span></span>

<span data-ttu-id="c971e-111">In questa esercitazione si scrivono innanzitutto le classi di modello e EF Core crea il database.</span><span class="sxs-lookup"><span data-stu-id="c971e-111">In this tutorial, you write the model classes first, and EF Core creates the database.</span></span> <span data-ttu-id="c971e-112">Un approccio alternativo non trattato in questo articolo consiste nel generare classi del modello da un database esistente.</span><span class="sxs-lookup"><span data-stu-id="c971e-112">An alternate approach not covered here is to generate model classes from an existing database.</span></span> <span data-ttu-id="c971e-113">Per informazioni su questo approccio, vedere [ASP.NET Core - Existing Database](/ef/core/get-started/aspnetcore/existing-db) (Introduzione a EF Core su ASP.NET Core con un database esistente).</span><span class="sxs-lookup"><span data-stu-id="c971e-113">For information about that approach, see [ASP.NET Core - Existing Database](/ef/core/get-started/aspnetcore/existing-db).</span></span>

## <a name="add-a-data-model-class"></a><span data-ttu-id="c971e-114">Aggiungere una classe del modello di dati</span><span class="sxs-lookup"><span data-stu-id="c971e-114">Add a data model class</span></span>

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c971e-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c971e-115">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="c971e-116">Fare clic con il pulsante destro del mouse sulla cartella *Models* > **Aggiungi** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="c971e-116">Right-click the *Models* folder > **Add** > **Class**.</span></span> <span data-ttu-id="c971e-117">Denominare la classe **Movie**.</span><span class="sxs-lookup"><span data-stu-id="c971e-117">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/mvc-intro/model1b.md)]

<!-- Code -------------------------->
# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="c971e-118">Visual Studio Code/Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="c971e-118">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="c971e-119">Aggiungere una classe alla cartella *Modello* denominata *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="c971e-119">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/mvc-intro/model1b.md)]
[!INCLUDE [model 2](~/includes/mvc-intro/model2.md)]

---  
<!-- End of VS tabs -->

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="c971e-120">Eseguire lo scaffolding del modello di filmato</span><span class="sxs-lookup"><span data-stu-id="c971e-120">Scaffold the movie model</span></span>

<span data-ttu-id="c971e-121">In questa sezione viene eseguito lo scaffolding del modello di filmato.</span><span class="sxs-lookup"><span data-stu-id="c971e-121">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="c971e-122">Lo strumento di scaffolding crea quindi le pagine per le operazioni CRUD (creazione, lettura, aggiornamento ed eliminazione) per il modello di filmato.</span><span class="sxs-lookup"><span data-stu-id="c971e-122">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c971e-123">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c971e-123">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="c971e-124">In **Esplora soluzioni** fare clic con il pulsante destro del mouse sulla cartella *Controller* **> Aggiungi > Nuovo elemento di scaffolding**.</span><span class="sxs-lookup"><span data-stu-id="c971e-124">In **Solution Explorer**, right-click the *Controllers* folder **> Add > New Scaffolded Item**.</span></span>

![vista del passaggio precedente](adding-model/_static/add_controller21.png)

<span data-ttu-id="c971e-126">Nella finestra di dialogo **Aggiungi scaffolding** selezionare **Controller MVC con visualizzazioni, che usa Entity Framework**.</span><span class="sxs-lookup"><span data-stu-id="c971e-126">In the **Add Scaffold** dialog, select **MVC Controller with views, using Entity Framework > Add**.</span></span>

![Finestra di dialogo Aggiungi scaffolding](adding-model/_static/add_scaffold21.png)

<span data-ttu-id="c971e-128">Completare la finestra di dialogo **Aggiungi controller**:</span><span class="sxs-lookup"><span data-stu-id="c971e-128">Complete the **Add Controller** dialog:</span></span>

* <span data-ttu-id="c971e-129">**Classe modello:** *Movie (MvcMovie.Models)*</span><span class="sxs-lookup"><span data-stu-id="c971e-129">**Model class:** *Movie (MvcMovie.Models)*</span></span>
* <span data-ttu-id="c971e-130">**Classe del contesto dei dati:** selezionare l'icona **+** e aggiungere il valore predefinito **MvcMovie.Models.MvcMovieContext**</span><span class="sxs-lookup"><span data-stu-id="c971e-130">**Data context class:** Select the **+** icon and add the default **MvcMovie.Models.MvcMovieContext**</span></span>

![Aggiungere il contesto dati](adding-model/_static/dc.png)

* <span data-ttu-id="c971e-132">**Viste:** mantenere il valore predefinito di ogni opzione selezionata</span><span class="sxs-lookup"><span data-stu-id="c971e-132">**Views:** Keep the default of each option checked</span></span>
* <span data-ttu-id="c971e-133">**Nome controller:** mantenere il valore predefinito *MoviesController*</span><span class="sxs-lookup"><span data-stu-id="c971e-133">**Controller name:** Keep the default *MoviesController*</span></span>
* <span data-ttu-id="c971e-134">Selezionare **Aggiungi**</span><span class="sxs-lookup"><span data-stu-id="c971e-134">Select **Add**</span></span>

![Finestra di dialogo Aggiungi controller](adding-model/_static/add_controller2.png)

<span data-ttu-id="c971e-136">Visual Studio crea:</span><span class="sxs-lookup"><span data-stu-id="c971e-136">Visual Studio creates:</span></span>

* <span data-ttu-id="c971e-137">Una [classe del contesto di database](xref:data/ef-mvc/intro#create-the-database-context) Entity Framework Core (*Data/MvcMovieContext.cs*)</span><span class="sxs-lookup"><span data-stu-id="c971e-137">An Entity Framework Core [database context class](xref:data/ef-mvc/intro#create-the-database-context) (*Data/MvcMovieContext.cs*)</span></span>
* <span data-ttu-id="c971e-138">Un controller di film (*Controllers/MoviesController.cs*)</span><span class="sxs-lookup"><span data-stu-id="c971e-138">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="c971e-139">File di vista Razor per le pagine Create (Crea), Delete (Elimina), Details (Dettagli), Edit (Modifica) e Index (Indice) (<em>Views/Movies/&ast;.cshtml</em>)</span><span class="sxs-lookup"><span data-stu-id="c971e-139">Razor view files for Create, Delete, Details, Edit, and Index pages (<em>Views/Movies/&ast;.cshtml</em>)</span></span>

<span data-ttu-id="c971e-140">La creazione automatica del contesto di database e di viste e metodi di azione [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, delete) è nota come *scaffolding*.</span><span class="sxs-lookup"><span data-stu-id="c971e-140">The automatic creation of the database context and [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views is known as *scaffolding*.</span></span>

<!-- Code -------------------------->
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="c971e-141">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c971e-141">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="c971e-142">Aprire una finestra di comando nella directory del progetto (la directory che contiene i file *Program.cs*, *Startup.cs* e *csproj*).</span><span class="sxs-lookup"><span data-stu-id="c971e-142">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="c971e-143">Installare lo strumento di scaffolding:</span><span class="sxs-lookup"><span data-stu-id="c971e-143">Install the scaffolding tool:</span></span>

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="c971e-144">Eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="c971e-144">Run the following command:</span></span>

  ```console
     dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold generated params](~/includes/mvc-intro/model4.md)]

<!-- Mac -------------------------->

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="c971e-145">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="c971e-145">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="c971e-146">Aprire una finestra di comando nella directory del progetto (la directory che contiene i file *Program.cs*, *Startup.cs* e *csproj*).</span><span class="sxs-lookup"><span data-stu-id="c971e-146">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="c971e-147">Installare lo strumento di scaffolding:</span><span class="sxs-lookup"><span data-stu-id="c971e-147">Install the scaffolding tool:</span></span>

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="c971e-148">Eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="c971e-148">Run the following command:</span></span>

  ```console
     dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

---

<!-- End of VS tabs                  -->

<span data-ttu-id="c971e-149">Se si esegue l'app e si fa clic sul collegamento **Mvc Movie**, viene visualizzato un errore simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="c971e-149">If you run the app and click on the **Mvc Movie** link, you get an error similar to the following:</span></span>

``` error
An unhandled exception occurred while processing the request.

SqlException: Cannot open database "MvcMovieContext-<GUID removed>" requested by the login. The login failed.
Login failed for user 'Rick'.

System.Data.SqlClient.SqlInternalConnectionTds..ctor(DbConnectionPoolIdentity identity, SqlConnectionString
```

<span data-ttu-id="c971e-150">È necessario creare il database, quindi si usa la funzionalità di [migrazioni](xref:data/ef-mvc/migrations) di Entity Framework Core a tale scopo.</span><span class="sxs-lookup"><span data-stu-id="c971e-150">You need to create the database, and you use the EF Core [Migrations](xref:data/ef-mvc/migrations) feature to do that.</span></span> <span data-ttu-id="c971e-151">Le migrazioni consentono di creare un database che corrisponde al modello di dati e aggiornare lo schema del database quando cambia il modello di dati.</span><span class="sxs-lookup"><span data-stu-id="c971e-151">Migrations lets you create a database that matches your data model and update the database schema when your data model changes.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="c971e-152">Migrazione iniziale</span><span class="sxs-lookup"><span data-stu-id="c971e-152">Initial migration</span></span>

<!-- VS -------------------------->

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c971e-153">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c971e-153">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="c971e-154">In questa sezione viene usata la Console di Gestione pacchetti (PMC) per:</span><span class="sxs-lookup"><span data-stu-id="c971e-154">In this section, the Package Manager Console (PMC) is used to:</span></span>

* <span data-ttu-id="c971e-155">Aggiungere una migrazione iniziale.</span><span class="sxs-lookup"><span data-stu-id="c971e-155">Add an initial migration.</span></span>
* <span data-ttu-id="c971e-156">Aggiornare il database con la migrazione iniziale.</span><span class="sxs-lookup"><span data-stu-id="c971e-156">Update the database with the initial migration.</span></span>

<span data-ttu-id="c971e-157">Dal menu **Strumenti** selezionare **Gestione pacchetti NuGet** > **Console di Gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="c971e-157">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Menu della Console di Gestione pacchetti](~/tutorials/first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="c971e-159">Nella Console di Gestione pacchetti immettere i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="c971e-159">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration Initial
Update-Database
```

<span data-ttu-id="c971e-160">Il comando `Add-Migration` genera un codice per creare lo schema del database iniziale.</span><span class="sxs-lookup"><span data-stu-id="c971e-160">The `Add-Migration` command generates code to create the initial database schema.</span></span>
<!-- Code -------------------------->

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="c971e-161">Visual Studio Code/Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="c971e-161">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]
<span data-ttu-id="c971e-162">Il comando `ef migrations add InitialCreate` genera un codice per creare lo schema del database iniziale.</span><span class="sxs-lookup"><span data-stu-id="c971e-162">The `ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span>

---  
<!-- End of VS tabs -->

<span data-ttu-id="c971e-163">Lo schema si basa sul modello specificato in `DbContext`, nel file *Models/MvcMovieContext.cs*.</span><span class="sxs-lookup"><span data-stu-id="c971e-163">The schema is based on the model specified in the `DbContext` (In the *Models/MvcMovieContext.cs* file).</span></span> <span data-ttu-id="c971e-164">L'argomento `InitialCreate` viene usato per denominare le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="c971e-164">The `InitialCreate` argument is used to name the migrations.</span></span> <span data-ttu-id="c971e-165">È possibile usare qualsiasi nome, ma per convenzione viene selezionato un nome che descrive la migrazione.</span><span class="sxs-lookup"><span data-stu-id="c971e-165">Any name can be used, but by convention a name is selected that describes the migration.</span></span>

<span data-ttu-id="c971e-166">Il comando `ef database update` esegue il metodo `Up` nel file *Migrations/\<time-stamp>_InitialCreate.cs*.</span><span class="sxs-lookup"><span data-stu-id="c971e-166">The `ef database update` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="c971e-167">Il metodo `Up` crea il database.</span><span class="sxs-lookup"><span data-stu-id="c971e-167">The `Up` method creates the database.</span></span>

<!-- VS -------------------------->

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c971e-168">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c971e-168">Visual Studio</span></span>](#tab/visual-studio)

## <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="c971e-169">Esaminare il contesto registrato con l'inserimento di dipendenze</span><span class="sxs-lookup"><span data-stu-id="c971e-169">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="c971e-170">ASP.NET Core viene compilato con l'[inserimento di dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="c971e-170">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="c971e-171">I servizi, ad esempio il contesto di database di Entity Framework Core, vengono registrati con l'inserimento delle dipendenze durante l'avvio dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="c971e-171">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="c971e-172">Questi servizi vengono quindi offerti ai componenti per cui sono necessari (ad esempio Razor Pages) tramite i parametri del costruttore.</span><span class="sxs-lookup"><span data-stu-id="c971e-172">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="c971e-173">Più avanti nell'esercitazione viene illustrato il codice del costruttore che ottiene un'istanza del contesto di database.</span><span class="sxs-lookup"><span data-stu-id="c971e-173">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="c971e-174">Lo strumento di scaffolding ha creato automaticamente un contesto del database e lo ha registrato con il contenitore di inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="c971e-174">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="c971e-175">Esaminare il metodo `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="c971e-175">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="c971e-176">La riga evidenziata è stata aggiunta dallo scaffolder:</span><span class="sxs-lookup"><span data-stu-id="c971e-176">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

<span data-ttu-id="c971e-177">`MvcMovieContext` coordina la funzionalità di EF Core (Create, Read, Update, Delete e così via) per il modello `Movie`.</span><span class="sxs-lookup"><span data-stu-id="c971e-177">The `MvcMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="c971e-178">Il contesto dei dati (`MvcMovieContext`) è derivato da [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="c971e-178">The data context (`MvcMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="c971e-179">Il contesto dei dati specifica le entità incluse nel modello di dati:</span><span class="sxs-lookup"><span data-stu-id="c971e-179">The data context specifies which entities are included in the data model:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Data/MvcMovieContext.cs)]

<span data-ttu-id="c971e-180">il codice precedente crea una proprietà [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) per il set di entità.</span><span class="sxs-lookup"><span data-stu-id="c971e-180">The preceding code creates a [`DbSet<Movie>`](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="c971e-181">Nella terminologia di Entity Framework, un set di entità corrisponde in genere alla tabella di un database.</span><span class="sxs-lookup"><span data-stu-id="c971e-181">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="c971e-182">Un'entità corrisponde a una riga nella tabella.</span><span class="sxs-lookup"><span data-stu-id="c971e-182">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="c971e-183">Il nome della stringa di connessione viene passato al contesto chiamando un metodo in un oggetto [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions).</span><span class="sxs-lookup"><span data-stu-id="c971e-183">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="c971e-184">Per lo sviluppo locale, il [sistema di configurazione di ASP.NET Core](xref:fundamentals/configuration/index) legge la stringa di connessione dal file *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="c971e-184">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>
<!-- Code -------------------------->

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="c971e-185">Visual Studio Code/Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="c971e-185">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="c971e-186">ASP.NET Core viene compilato con l'[inserimento di dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="c971e-186">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="c971e-187">I servizi, ad esempio il contesto di database di Entity Framework Core, vengono registrati con l'inserimento delle dipendenze durante l'avvio dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="c971e-187">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="c971e-188">Questi servizi vengono quindi offerti ai componenti per cui sono necessari (ad esempio Razor Pages) tramite i parametri del costruttore.</span><span class="sxs-lookup"><span data-stu-id="c971e-188">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="c971e-189">Più avanti nell'esercitazione viene illustrato il codice del costruttore che ottiene un'istanza del contesto di database.</span><span class="sxs-lookup"><span data-stu-id="c971e-189">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="c971e-190">È stato creato automaticamente un contesto del database che viene registrato con il contenitore di inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="c971e-190">You created a DB context and registered it with the dependency injection container.</span></span>

---

<span data-ttu-id="c971e-191">Lo schema è basato sul modello specificato in `MvcMovieContext` (nel file *Data/MvcMovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="c971e-191">The schema is based on the model specified in the `MvcMovieContext` (In the *Data/MvcMovieContext.cs* file).</span></span> <span data-ttu-id="c971e-192">L'argomento `Initial` viene usato per denominare le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="c971e-192">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="c971e-193">È possibile usare qualsiasi nome, ma per convenzione viene usato un nome che descrive la migrazione.</span><span class="sxs-lookup"><span data-stu-id="c971e-193">Any name can be used, but by convention a name that describes the migration is used.</span></span> <span data-ttu-id="c971e-194">Per altre informazioni vedere [Introduzione alle migrazioni](xref:data/ef-mvc/migrations#introduction-to-migrations).</span><span class="sxs-lookup"><span data-stu-id="c971e-194">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="c971e-195">Il comando `Update-Database` esegue il metodo `Up` nel file *Migrations/{time-stamp}_InitialCreate.cs*, che crea il database.</span><span class="sxs-lookup"><span data-stu-id="c971e-195">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="c971e-196">Eseguire il test dell'app</span><span class="sxs-lookup"><span data-stu-id="c971e-196">Test the app</span></span>

* <span data-ttu-id="c971e-197">Eseguire l'app e accodare `/Movies` all'URL nel browser (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="c971e-197">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="c971e-198">Se si verifica un'eccezione di database simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="c971e-198">If you get a database exception similar to the following:</span></span>

```console
SqlException: Cannot open database "MvcMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="c971e-199">Non è stato eseguita la [migrazione](#pmc).</span><span class="sxs-lookup"><span data-stu-id="c971e-199">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="c971e-200">Eseguire il test del collegamento **Crea**.</span><span class="sxs-lookup"><span data-stu-id="c971e-200">Test the **Create** link.</span></span>

  > [!NOTE]
  > <span data-ttu-id="c971e-201">Potrebbe non essere possibile immettere virgole decimali nel campo `Price`.</span><span class="sxs-lookup"><span data-stu-id="c971e-201">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="c971e-202">Per supportare la [convalida jQuery](https://jqueryvalidation.org/) per impostazioni locali diverse dall'inglese che usano la virgola (",") come separatore decimale e per formati di data diversi da quello dell'inglese (Stati Uniti), è necessario globalizzare l'app.</span><span class="sxs-lookup"><span data-stu-id="c971e-202">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="c971e-203">Per istruzioni sulla globalizzazione, vedere [questo problema su GitHub](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="c971e-203">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="c971e-204">Eseguire il test dei collegamenti **Modifica**, **Dettagli** e **Elimina**.</span><span class="sxs-lookup"><span data-stu-id="c971e-204">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="c971e-205">Esaminare la classe `Startup`:</span><span class="sxs-lookup"><span data-stu-id="c971e-205">Examine the `Startup` class:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=13-99)]

<span data-ttu-id="c971e-206">Il codice evidenziato riportato in precedenza mostra il contesto del database dei film che viene aggiunto al contenitore di [inserimento dipendenze](xref:fundamentals/dependency-injection):</span><span class="sxs-lookup"><span data-stu-id="c971e-206">The preceding highlighted code shows the movie database context being added to the [Dependency Injection](xref:fundamentals/dependency-injection) container:</span></span>

* <span data-ttu-id="c971e-207">`services.AddDbContext<MvcMovieContext>(options =>` specifica il database da usare e la stringa di connessione.</span><span class="sxs-lookup"><span data-stu-id="c971e-207">`services.AddDbContext<MvcMovieContext>(options =>` specifies the database to use and the connection string.</span></span>
* <span data-ttu-id="c971e-208">`=>` è un [operatore lambda](/dotnet/articles/csharp/language-reference/operators/lambda-operator)</span><span class="sxs-lookup"><span data-stu-id="c971e-208">`=>` is a [lambda operator](/dotnet/articles/csharp/language-reference/operators/lambda-operator)</span></span>

<span data-ttu-id="c971e-209">Aprire il file *Controllers/MoviesController.cs* ed esaminare il costruttore:</span><span class="sxs-lookup"><span data-stu-id="c971e-209">Open the *Controllers/MoviesController.cs* file and examine the constructor:</span></span>

<!-- l.. Make copy of Movies controller because we comment out the initial index method and update it later  -->

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_1)]

<span data-ttu-id="c971e-210">Il costruttore usa l'[inserimento dipendenze](xref:fundamentals/dependency-injection) per inserire il contesto del database (`MvcMovieContext `) nel controller.</span><span class="sxs-lookup"><span data-stu-id="c971e-210">The constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`MvcMovieContext `) into the controller.</span></span> <span data-ttu-id="c971e-211">Il contesto di database viene usato in ognuno dei metodi [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) nel controller.</span><span class="sxs-lookup"><span data-stu-id="c971e-211">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

<a name="strongly-typed-models-keyword-label"></a>
<a name="strongly-typed-models-and-the--keyword"></a>

## <a name="strongly-typed-models-and-the-model-keyword"></a><span data-ttu-id="c971e-212">Modelli fortemente tipizzati e parola chiave @model</span><span class="sxs-lookup"><span data-stu-id="c971e-212">Strongly typed models and the @model keyword</span></span>

<span data-ttu-id="c971e-213">In un passaggio precedente di questa esercitazione è stato esaminato come un controller può passare oggetti o dati a una vista usando il dizionario `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="c971e-213">Earlier in this tutorial, you saw how a controller can pass data or objects to a view using the `ViewData` dictionary.</span></span> <span data-ttu-id="c971e-214">Il dizionario `ViewData` è un oggetto dinamico che fornisce un modo pratico ad associazione tardiva per passare informazioni a una vista.</span><span class="sxs-lookup"><span data-stu-id="c971e-214">The `ViewData` dictionary is a dynamic object that provides a convenient late-bound way to pass information to a view.</span></span>

<span data-ttu-id="c971e-215">MVC consente anche di passare oggetti modello fortemente tipizzati a una vista.</span><span class="sxs-lookup"><span data-stu-id="c971e-215">MVC also provides the ability to pass strongly typed model objects to a view.</span></span> <span data-ttu-id="c971e-216">Questo approccio fortemente tipizzato consente un migliore controllo del codice in fase di compilazione.</span><span class="sxs-lookup"><span data-stu-id="c971e-216">This strongly typed approach enables better compile time checking of your code.</span></span> <span data-ttu-id="c971e-217">Con il meccanismo di scaffolding è stato usato questo approccio, ovvero il passaggio di un modello fortemente tipizzato, con le viste e la classe `MoviesController` quando sono stati creati i metodi e le viste.</span><span class="sxs-lookup"><span data-stu-id="c971e-217">The scaffolding mechanism used this approach (that is, passing a strongly typed model) with the `MoviesController` class and views when it created the methods and views.</span></span>

<span data-ttu-id="c971e-218">Aprire il metodo `Details` nel file *Controllers/MoviesController.cs*:</span><span class="sxs-lookup"><span data-stu-id="c971e-218">Examine the generated `Details` method in the *Controllers/MoviesController.cs* file:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_details)]

<span data-ttu-id="c971e-219">In genere il parametro `id` viene passato come dati di route.</span><span class="sxs-lookup"><span data-stu-id="c971e-219">The `id` parameter is generally passed as route data.</span></span> <span data-ttu-id="c971e-220">Ad esempio `https://localhost:5001/movies/details/1` imposta:</span><span class="sxs-lookup"><span data-stu-id="c971e-220">For example `https://localhost:5001/movies/details/1` sets:</span></span>

* <span data-ttu-id="c971e-221">Il controller sul controller `movies` (primo segmento di URL).</span><span class="sxs-lookup"><span data-stu-id="c971e-221">The controller to the `movies` controller (the first URL segment).</span></span>
* <span data-ttu-id="c971e-222">L'azione su `details` (secondo segmento di URL).</span><span class="sxs-lookup"><span data-stu-id="c971e-222">The action to `details` (the second URL segment).</span></span>
* <span data-ttu-id="c971e-223">L'ID su 1 (ultimo segmento di URL).</span><span class="sxs-lookup"><span data-stu-id="c971e-223">The id to 1 (the last URL segment).</span></span>

<span data-ttu-id="c971e-224">È possibile anche passare `id` con una stringa di query nel modo seguente:</span><span class="sxs-lookup"><span data-stu-id="c971e-224">You can also pass in the `id` with a query string as follows:</span></span>

`https://localhost:5001/movies/details?id=1`

<span data-ttu-id="c971e-225">Il parametro `id` viene definito come [tipo nullable](/dotnet/csharp/programming-guide/nullable-types/index) (`int?`) nel caso in cui non venga fornito un valore ID.</span><span class="sxs-lookup"><span data-stu-id="c971e-225">The `id` parameter is defined as a [nullable type](/dotnet/csharp/programming-guide/nullable-types/index) (`int?`) in case an ID value isn't provided.</span></span>

<span data-ttu-id="c971e-226">Un'[espressione lambda](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) viene passata a `FirstOrDefaultAsync` per selezionare le entità film che corrispondono al valore della stringa di query o dei dati di route.</span><span class="sxs-lookup"><span data-stu-id="c971e-226">A [lambda expression](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) is passed in to `FirstOrDefaultAsync` to select movie entities that match the route data or query string value.</span></span>

```csharp
var movie = await _context.Movie
    .FirstOrDefaultAsync(m => m.Id == id);
```

<span data-ttu-id="c971e-227">Se viene trovato un film, viene passata un'istanza del modello `Movie` alla vista `Details`:</span><span class="sxs-lookup"><span data-stu-id="c971e-227">If a movie is found, an instance of the `Movie` model is passed to the `Details` view:</span></span>

```csharp
return View(movie);
   ```

<span data-ttu-id="c971e-228">Esaminare il contenuto del file *Views/Movies/Details.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="c971e-228">Examine the contents of the *Views/Movies/Details.cshtml* file:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/DetailsOriginal.cshtml)]

<span data-ttu-id="c971e-229">Includendo un'istruzione `@model` all'inizio del file di vista, è possibile specificare il tipo di oggetto previsto dalla vista.</span><span class="sxs-lookup"><span data-stu-id="c971e-229">By including a `@model` statement at the top of the view file, you can specify the type of object that the view expects.</span></span> <span data-ttu-id="c971e-230">Al momento della creazione del controller di film, l'istruzione `@model` seguente è stata inclusa automaticamente all'inizio del file *Details.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="c971e-230">When you created the movie controller, the following `@model` statement was automatically included at the top of the *Details.cshtml* file:</span></span>

```HTML
@model MvcMovie.Models.Movie
   ```

<span data-ttu-id="c971e-231">Questa direttiva `@model` consente di accedere al film passato dal controller alla vista usando un oggetto `Model` fortemente tipizzato.</span><span class="sxs-lookup"><span data-stu-id="c971e-231">This `@model` directive allows you to access the movie that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="c971e-232">Ad esempio, nella vista *Details.cshtml* il codice passa ogni campo di film agli helper HTML `DisplayNameFor` e `DisplayFor` con l'oggetto `Model` fortemente tipizzato.</span><span class="sxs-lookup"><span data-stu-id="c971e-232">For example, in the *Details.cshtml* view, the code passes each movie field to the `DisplayNameFor` and `DisplayFor` HTML Helpers with the strongly typed `Model` object.</span></span> <span data-ttu-id="c971e-233">Le viste e i metodi `Create` e `Edit` passano anche un oggetto modello `Movie`.</span><span class="sxs-lookup"><span data-stu-id="c971e-233">The `Create` and `Edit` methods and views also pass a `Movie` model object.</span></span>

<span data-ttu-id="c971e-234">Esaminare la vista *Index.cshtml* e il metodo `Index` nel controller Movies.</span><span class="sxs-lookup"><span data-stu-id="c971e-234">Examine the *Index.cshtml* view and the `Index` method in the Movies controller.</span></span> <span data-ttu-id="c971e-235">Si noti che il codice crea un oggetto `List` quando chiama il metodo `View`.</span><span class="sxs-lookup"><span data-stu-id="c971e-235">Notice how the code creates a `List` object when it calls the `View` method.</span></span> <span data-ttu-id="c971e-236">Il codice passa questo elenco `Movies` dal metodo di azione `Index` alla vista:</span><span class="sxs-lookup"><span data-stu-id="c971e-236">The code passes this `Movies` list from the `Index` action method to the view:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_index)]

<span data-ttu-id="c971e-237">Al momento della creazione del controller di film, lo scaffolding ha incluso automaticamente l'istruzione `@model` all'inizio del file *Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="c971e-237">When you created the movies controller, scaffolding automatically included the following `@model` statement at the top of the *Index.cshtml* file:</span></span>

<!-- Copy Index.cshtml to IndexOriginal.cshtml -->

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexOriginal.cshtml?range=1)]

<span data-ttu-id="c971e-238">La direttiva `@model` consente di accedere all'elenco di film che il controller ha passato alla vista usando un oggetto `Model` fortemente tipizzato.</span><span class="sxs-lookup"><span data-stu-id="c971e-238">The `@model` directive allows you to access the list of movies that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="c971e-239">Ad esempio, nella vista *Index.cshtml* il codice scorre i film con un'istruzione `foreach` sull'oggetto fortemente tipizzato `Model`:</span><span class="sxs-lookup"><span data-stu-id="c971e-239">For example, in the *Index.cshtml* view, the code loops through the movies with a `foreach` statement over the strongly typed `Model` object:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexOriginal.cshtml?highlight=1,31,34,37,40,43,46-48)]

<span data-ttu-id="c971e-240">Poiché l'oggetto `Model` è fortemente tipizzato (come un oggetto `IEnumerable<Movie>`), ogni elemento nel ciclo viene tipizzato come `Movie`.</span><span class="sxs-lookup"><span data-stu-id="c971e-240">Because the `Model` object is strongly typed (as an `IEnumerable<Movie>` object), each item in the loop is typed as `Movie`.</span></span> <span data-ttu-id="c971e-241">Tra gli altri vantaggi, si ottiene un controllo del codice in fase di compilazione:</span><span class="sxs-lookup"><span data-stu-id="c971e-241">Among other benefits, this means that you get compile time checking of the code:</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c971e-242">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="c971e-242">Additional resources</span></span>

* [<span data-ttu-id="c971e-243">Helper tag</span><span class="sxs-lookup"><span data-stu-id="c971e-243">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="c971e-244">Globalizzazione e localizzazione</span><span class="sxs-lookup"><span data-stu-id="c971e-244">Globalization and localization</span></span>](xref:fundamentals/localization)

> [!div class="step-by-step"]
> <span data-ttu-id="c971e-245">[Precedente - Aggiunta di una vista](adding-view.md)
> [Successivo - Utilizzo del linguaggio SQL](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="c971e-245">[Previous Adding a View](adding-view.md)
[Next Working with SQL](working-with-sql.md)</span></span>  
