---
title: Aggiungere un modello a un'app ASP.NET Core MVC
author: rick-anderson
description: Aggiungere un modello a una semplice app ASP.NET Core.
ms.author: riande
ms.date: 8/15/2019
uid: tutorials/first-mvc-app/adding-model
ms.openlocfilehash: 40f26c8c2bf8d8aaec1da4ca2ff96cb45830914e
ms.sourcegitcommit: 57b85708f4cded99b8f008a69830cb104cd8e879
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/13/2020
ms.locfileid: "75914170"
---
# <a name="add-a-model-to-an-aspnet-core-mvc-app"></a><span data-ttu-id="e1527-103">Aggiungere un modello a un'app ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="e1527-103">Add a model to an ASP.NET Core MVC app</span></span>

<span data-ttu-id="e1527-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT) e [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="e1527-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="e1527-105">In questa sezione si aggiungono alcune classi per la gestione di filmati in un database.</span><span class="sxs-lookup"><span data-stu-id="e1527-105">In this section, you add classes for managing movies in a database.</span></span> <span data-ttu-id="e1527-106">Queste classi saranno la parte "**M**odel" dell'app **M**VC.</span><span class="sxs-lookup"><span data-stu-id="e1527-106">These classes will be the "**M**odel" part of the **M**VC app.</span></span>

<span data-ttu-id="e1527-107">Si usano queste classi con [Entity Framework Core](/ef/core) (EF Core) per usare un database.</span><span class="sxs-lookup"><span data-stu-id="e1527-107">You use these classes with [Entity Framework Core](/ef/core) (EF Core) to work with a database.</span></span> <span data-ttu-id="e1527-108">EF Core è un framework object-relational mapping (ORM) che semplifica il codice di accesso ai dati che è necessario scrivere.</span><span class="sxs-lookup"><span data-stu-id="e1527-108">EF Core is an object-relational mapping (ORM) framework that simplifies the data access code that you have to write.</span></span>

<span data-ttu-id="e1527-109">Le classi di modello create sono dette classi POCO (da **P**lain **O**ld **C**LR **O**bjects) perché non hanno alcuna dipendenza in EF Core.</span><span class="sxs-lookup"><span data-stu-id="e1527-109">The model classes you create are known as POCO classes (from **P**lain **O**ld **C**LR **O**bjects) because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="e1527-110">Definiscono semplicemente le proprietà dei dati che verranno archiviati nel database.</span><span class="sxs-lookup"><span data-stu-id="e1527-110">They just define the properties of the data that will be stored in the database.</span></span>

<span data-ttu-id="e1527-111">In questa esercitazione si scrivono innanzitutto le classi di modello e EF Core crea il database.</span><span class="sxs-lookup"><span data-stu-id="e1527-111">In this tutorial, you write the model classes first, and EF Core creates the database.</span></span> <span data-ttu-id="e1527-112">Un approccio alternativo non trattato in questo articolo consiste nel generare classi del modello da un database esistente.</span><span class="sxs-lookup"><span data-stu-id="e1527-112">An alternate approach not covered here is to generate model classes from an existing database.</span></span> <span data-ttu-id="e1527-113">Per informazioni su questo approccio, vedere [ASP.NET Core - Existing Database](/ef/core/get-started/aspnetcore/existing-db) (Introduzione a EF Core su ASP.NET Core con un database esistente).</span><span class="sxs-lookup"><span data-stu-id="e1527-113">For information about that approach, see [ASP.NET Core - Existing Database](/ef/core/get-started/aspnetcore/existing-db).</span></span>

::: moniker range=">= aspnetcore-3.0"

## <a name="add-a-data-model-class"></a><span data-ttu-id="e1527-114">Aggiungere una classe del modello di dati</span><span class="sxs-lookup"><span data-stu-id="e1527-114">Add a data model class</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e1527-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e1527-115">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="e1527-116">Fare clic con il pulsante destro del mouse sulla cartella *Models* > **Aggiungi** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="e1527-116">Right-click the *Models* folder > **Add** > **Class**.</span></span> <span data-ttu-id="e1527-117">Denominare il file *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="e1527-117">Name the file *Movie.cs*.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e1527-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e1527-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="e1527-119">Aggiungere un file denominato *Movie.cs* alla cartella *Models*.</span><span class="sxs-lookup"><span data-stu-id="e1527-119">Add a file named *Movie.cs* to the *Models* folder.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e1527-120">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="e1527-120">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="e1527-121">Fare clic con il pulsante destro del mouse sulla cartella *models* > **Aggiungi** > **nuova classe** > **classe vuota**.</span><span class="sxs-lookup"><span data-stu-id="e1527-121">Right-click the *Models* folder > **Add** > **New Class** > **Empty Class**.</span></span> <span data-ttu-id="e1527-122">Denominare il file *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="e1527-122">Name the file *Movie.cs*.</span></span>

---

<span data-ttu-id="e1527-123">Aggiornare il file *Movie.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="e1527-123">Update the *Movie.cs* file with the following code:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Models/Movie.cs)]

<span data-ttu-id="e1527-124">La classe `Movie` contiene un campo `Id`, richiesto dal database per la chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="e1527-124">The `Movie` class contains an `Id` field, which is required by the database for the primary key.</span></span>

<span data-ttu-id="e1527-125">L'attributo [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) in `ReleaseDate` specifica il tipo di dati (`Date`).</span><span class="sxs-lookup"><span data-stu-id="e1527-125">The [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) attribute on `ReleaseDate` specifies the type of the data (`Date`).</span></span> <span data-ttu-id="e1527-126">Con questo attributo:</span><span class="sxs-lookup"><span data-stu-id="e1527-126">With this attribute:</span></span>

  * <span data-ttu-id="e1527-127">l'utente non deve immettere le informazioni temporali nel campo della data.</span><span class="sxs-lookup"><span data-stu-id="e1527-127">The user is not required to enter time information in the date field.</span></span>
  * <span data-ttu-id="e1527-128">Viene visualizzata solo la data, non le informazioni temporali.</span><span class="sxs-lookup"><span data-stu-id="e1527-128">Only the date is displayed, not time information.</span></span>

<span data-ttu-id="e1527-129">L'attributo [DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) viene analizzato in un'esercitazione successiva.</span><span class="sxs-lookup"><span data-stu-id="e1527-129">[DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) are covered in a later tutorial.</span></span>

## <a name="add-nuget-packages"></a><span data-ttu-id="e1527-130">Aggiungere pacchetti NuGet</span><span class="sxs-lookup"><span data-stu-id="e1527-130">Add NuGet packages</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e1527-131">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e1527-131">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="e1527-132">Dal menu **strumenti** selezionare **gestione pacchetti NuGet** > console di **Gestione pacchetti** (PMC).</span><span class="sxs-lookup"><span data-stu-id="e1527-132">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console** (PMC).</span></span>

![Menu della Console di Gestione pacchetti](~/tutorials/first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="e1527-134">Nella console di Gestione pacchetti eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="e1527-134">In the PMC, run the following command:</span></span>

```powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="e1527-135">Il comando precedente aggiunge il provider EF Core SQL Server.</span><span class="sxs-lookup"><span data-stu-id="e1527-135">The preceding command adds the EF Core SQL Server provider.</span></span> <span data-ttu-id="e1527-136">Il pacchetto del provider installa il pacchetto di EF Core come dipendenza.</span><span class="sxs-lookup"><span data-stu-id="e1527-136">The provider package installs the EF Core package as a dependency.</span></span> <span data-ttu-id="e1527-137">I pacchetti aggiuntivi vengono installati automaticamente nel passaggio di scaffolding più avanti nell'esercitazione.</span><span class="sxs-lookup"><span data-stu-id="e1527-137">Additional packages are installed automatically in the scaffolding step later in the tutorial.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e1527-138">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e1527-138">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/add-EF-NuGet-SQLite-CLI.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e1527-139">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="e1527-139">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="e1527-140">Scegliere **Gestisci pacchetti NuGet**dal menu **progetto** .</span><span class="sxs-lookup"><span data-stu-id="e1527-140">From the **Project** menu, select **Manage Nuget Packages**.</span></span>

<span data-ttu-id="e1527-141">Nel campo di **ricerca** in alto a destra immettere `Microsoft.EntityFrameworkCore.SQLite` e premere il tasto **invio** per eseguire la ricerca.</span><span class="sxs-lookup"><span data-stu-id="e1527-141">In the **Search** field in the upper right, enter `Microsoft.EntityFrameworkCore.SQLite` and press the **Return** key to search.</span></span> <span data-ttu-id="e1527-142">Selezionare il pacchetto NuGet corrispondente e premere il pulsante **Aggiungi pacchetto** .</span><span class="sxs-lookup"><span data-stu-id="e1527-142">Select the matching NuGet package and press the **Add Package** button.</span></span>

![Aggiungi Entity Framework Core pacchetto NuGet](~/tutorials/first-mvc-app-mac/adding-model/_static/add-nuget-packages.png)

<span data-ttu-id="e1527-144">Verrà visualizzata la finestra di dialogo **Seleziona progetti** con il progetto `MvcMovie` selezionato.</span><span class="sxs-lookup"><span data-stu-id="e1527-144">The **Select Projects** dialog will be displayed, with the `MvcMovie` project selected.</span></span> <span data-ttu-id="e1527-145">Premere il pulsante **OK** .</span><span class="sxs-lookup"><span data-stu-id="e1527-145">Press the **Ok** button.</span></span>

<span data-ttu-id="e1527-146">Verrà visualizzata una finestra di dialogo **accettazione licenza** .</span><span class="sxs-lookup"><span data-stu-id="e1527-146">A **License Acceptance** dialog will be displayed.</span></span> <span data-ttu-id="e1527-147">Esaminare le licenze nel modo desiderato, quindi fare clic sul pulsante **Accetto** .</span><span class="sxs-lookup"><span data-stu-id="e1527-147">Review the licenses as desired, then click the **Accept** button.</span></span>

<span data-ttu-id="e1527-148">Ripetere i passaggi precedenti per installare i pacchetti NuGet seguenti:</span><span class="sxs-lookup"><span data-stu-id="e1527-148">Repeat the above steps to install the following NuGet packages:</span></span>
 * `Microsoft.VisualStudio.Web.CodeGeneration.Design`
 * `Microsoft.EntityFrameworkCore.SqlServer`
 * `Microsoft.EntityFrameworkCore.Design`

---

<a name="dc"></a>

## <a name="create-a-database-context-class"></a><span data-ttu-id="e1527-149">Creare una classe di contesto di database</span><span class="sxs-lookup"><span data-stu-id="e1527-149">Create a database context class</span></span>

<span data-ttu-id="e1527-150">Una classe di contesto di database è necessaria per coordinare le funzionalità di EF Core (creazione, lettura, aggiornamento, eliminazione) per il modello `Movie`.</span><span class="sxs-lookup"><span data-stu-id="e1527-150">A database context class is needed to coordinate EF Core functionality (Create, Read, Update, Delete) for the `Movie` model.</span></span> <span data-ttu-id="e1527-151">Il contesto di database viene derivato da [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) e specifica le entità da includere nel modello di dati.</span><span class="sxs-lookup"><span data-stu-id="e1527-151">The database context is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) and specifies the entities to include in the data model.</span></span>

<span data-ttu-id="e1527-152">Creare una cartella *Data*.</span><span class="sxs-lookup"><span data-stu-id="e1527-152">Create a *Data* folder.</span></span>

<span data-ttu-id="e1527-153">Aggiungere un file *Data/MvcMovieContext.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="e1527-153">Add a *Data/MvcMovieContext.cs* file with the following code:</span></span> 

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/zDocOnly/MvcMovieContext.cs?name=snippet)]

<span data-ttu-id="e1527-154">Il codice precedente crea una proprietà [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) per il set di entità.</span><span class="sxs-lookup"><span data-stu-id="e1527-154">The preceding code creates a [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="e1527-155">Nella terminologia di Entity Framework, un set di entità corrisponde in genere alla tabella di un database.</span><span class="sxs-lookup"><span data-stu-id="e1527-155">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="e1527-156">Un'entità corrisponde a una riga nella tabella.</span><span class="sxs-lookup"><span data-stu-id="e1527-156">An entity corresponds to a row in the table.</span></span>

<a name="reg"></a>

## <a name="register-the-database-context"></a><span data-ttu-id="e1527-157">Registrare il contesto del database</span><span class="sxs-lookup"><span data-stu-id="e1527-157">Register the database context</span></span>

<span data-ttu-id="e1527-158">ASP.NET Core viene compilato con l'[inserimento di dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="e1527-158">ASP.NET Core is built with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="e1527-159">I servizi, ad esempio il contesto di database di EF Core, devono essere registrati per l'inserimento delle dipendenze durante l'avvio dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="e1527-159">Services (such as the EF Core DB context) must be registered with DI during application startup.</span></span> <span data-ttu-id="e1527-160">Questi servizi vengono quindi offerti ai componenti per cui sono necessari (ad esempio Razor Pages) tramite i parametri del costruttore.</span><span class="sxs-lookup"><span data-stu-id="e1527-160">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="e1527-161">Più avanti nell'esercitazione viene illustrato il codice del costruttore che ottiene un'istanza del contesto di database.</span><span class="sxs-lookup"><span data-stu-id="e1527-161">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span> <span data-ttu-id="e1527-162">In questa sezione viene registrato il contesto di database per il contenitore di inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="e1527-162">In this section, you register the database context with the DI container.</span></span>

<span data-ttu-id="e1527-163">Aggiungere le istruzioni `using` seguenti all'inizio di *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="e1527-163">Add the following `using` statements at the top of *Startup.cs*:</span></span>

```csharp
using MvcMovie.Data;
using Microsoft.EntityFrameworkCore;
```

<span data-ttu-id="e1527-164">Aggiungere il codice evidenziato seguente in `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="e1527-164">Add the following highlighted code in `Startup.ConfigureServices`:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e1527-165">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e1527-165">Visual Studio</span></span>](#tab/visual-studio)

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Startup.cs?name=snippet_ConfigureServices&highlight=6-7)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="e1527-166">Visual Studio Code/Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="e1527-166">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Startup.cs?name=snippet_UseSqlite&highlight=6-7)]

---

<span data-ttu-id="e1527-167">Il nome della stringa di connessione viene passato al contesto chiamando un metodo in un oggetto [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions).</span><span class="sxs-lookup"><span data-stu-id="e1527-167">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="e1527-168">Per lo sviluppo locale, il [sistema di configurazione di ASP.NET Core](xref:fundamentals/configuration/index) legge la stringa di connessione dal file *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="e1527-168">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

<a name="cs"></a>

## <a name="add-a-database-connection-string"></a><span data-ttu-id="e1527-169">Aggiungere una stringa di connessione del database</span><span class="sxs-lookup"><span data-stu-id="e1527-169">Add a database connection string</span></span>

<span data-ttu-id="e1527-170">Aggiungere una stringa di connessione al file *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="e1527-170">Add a connection string to the *appsettings.json* file:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e1527-171">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e1527-171">Visual Studio</span></span>](#tab/visual-studio)

[!code-json[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/appsettings.json?highlight=10-12)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="e1527-172">Visual Studio Code/Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="e1527-172">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!code-json[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/appsettings_SQLite.json?highlight=10-12)]

---

<span data-ttu-id="e1527-173">Compilare il progetto per controllare se sono presenti errori del compilatore.</span><span class="sxs-lookup"><span data-stu-id="e1527-173">Build the project as a check for compiler errors.</span></span>

## <a name="scaffold-movie-pages"></a><span data-ttu-id="e1527-174">Eseguire lo scaffolding delle pagine Movie</span><span class="sxs-lookup"><span data-stu-id="e1527-174">Scaffold movie pages</span></span>

<span data-ttu-id="e1527-175">Usare lo strumento di scaffolding per generare le pagine per le operazioni CRUD (creazione, lettura, aggiornamento ed eliminazione) per il modello Movie.</span><span class="sxs-lookup"><span data-stu-id="e1527-175">Use the scaffolding tool to produce Create, Read, Update, and Delete (CRUD) pages for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e1527-176">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e1527-176">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="e1527-177">In **Esplora soluzioni** fare clic con il pulsante destro del mouse sulla cartella *Controller* **> Aggiungi > Nuovo elemento di scaffolding**.</span><span class="sxs-lookup"><span data-stu-id="e1527-177">In **Solution Explorer**, right-click the *Controllers* folder **> Add > New Scaffolded Item**.</span></span>

![vista del passaggio precedente](adding-model/_static/add_controller21.png)

<span data-ttu-id="e1527-179">Nella finestra di dialogo **Aggiungi scaffolding** selezionare **Controller MVC con visualizzazioni, che usa Entity Framework**.</span><span class="sxs-lookup"><span data-stu-id="e1527-179">In the **Add Scaffold** dialog, select **MVC Controller with views, using Entity Framework > Add**.</span></span>

![Finestra di dialogo Aggiungi scaffolding](adding-model/_static/add_scaffold21.png)

<span data-ttu-id="e1527-181">Completare la finestra di dialogo **Aggiungi controller**:</span><span class="sxs-lookup"><span data-stu-id="e1527-181">Complete the **Add Controller** dialog:</span></span>

* <span data-ttu-id="e1527-182">**Classe modello:** *Movie (MvcMovie. Models)*</span><span class="sxs-lookup"><span data-stu-id="e1527-182">**Model class:** *Movie (MvcMovie.Models)*</span></span>
* <span data-ttu-id="e1527-183">**Classe del contesto dati:** *MvcMovieContext (MvcMovie. Data)*</span><span class="sxs-lookup"><span data-stu-id="e1527-183">**Data context class:** *MvcMovieContext (MvcMovie.Data)*</span></span>

![Aggiungere il contesto dati](adding-model/_static/dc3.png)

* <span data-ttu-id="e1527-185">**Viste:** mantenere il valore predefinito di ogni opzione selezionata</span><span class="sxs-lookup"><span data-stu-id="e1527-185">**Views:** Keep the default of each option checked</span></span>
* <span data-ttu-id="e1527-186">**Nome del controller:** mantenere il valore predefinito *MoviesController*</span><span class="sxs-lookup"><span data-stu-id="e1527-186">**Controller name:** Keep the default *MoviesController*</span></span>
* <span data-ttu-id="e1527-187">Selezionare **Aggiungi**</span><span class="sxs-lookup"><span data-stu-id="e1527-187">Select **Add**</span></span>

<span data-ttu-id="e1527-188">Visual Studio crea:</span><span class="sxs-lookup"><span data-stu-id="e1527-188">Visual Studio creates:</span></span>

* <span data-ttu-id="e1527-189">Un controller di film (*Controllers/MoviesController.cs*)</span><span class="sxs-lookup"><span data-stu-id="e1527-189">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="e1527-190">File di vista Razor per le pagine Create (Crea), Delete (Elimina), Details (Dettagli), Edit (Modifica) e Index (Indice) (*Views/Movies/\*.cshtml*)</span><span class="sxs-lookup"><span data-stu-id="e1527-190">Razor view files for Create, Delete, Details, Edit, and Index pages (*Views/Movies/\*.cshtml*)</span></span>

<span data-ttu-id="e1527-191">La creazione automatica di questi file è nota come *scaffolding*.</span><span class="sxs-lookup"><span data-stu-id="e1527-191">The automatic creation of these files is known as *scaffolding*.</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e1527-192">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e1527-192">Visual Studio Code</span></span>](#tab/visual-studio-code) 

* <span data-ttu-id="e1527-193">Aprire una finestra di comando nella directory del progetto (la directory che contiene i file *Program.cs*, *Startup.cs* e *csproj*).</span><span class="sxs-lookup"><span data-stu-id="e1527-193">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>

* <span data-ttu-id="e1527-194">In Linux esportare il percorso dello strumento di scaffolding:</span><span class="sxs-lookup"><span data-stu-id="e1527-194">On Linux, export the scaffold tool path:</span></span>

  ```console
    export PATH=$HOME/.dotnet/tools:$PATH
  ```

* <span data-ttu-id="e1527-195">Eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="e1527-195">Run the following command:</span></span>

  ```dotnetcli
   dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

  [!INCLUDE [explains scaffold generated params](~/includes/mvc-intro/model4.md)]

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e1527-196">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="e1527-196">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="e1527-197">Aprire una finestra di comando nella directory del progetto (la directory che contiene i file *Program.cs*, *Startup.cs* e *csproj*).</span><span class="sxs-lookup"><span data-stu-id="e1527-197">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>

* <span data-ttu-id="e1527-198">Eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="e1527-198">Run the following command:</span></span>

  ```dotnetcli
   dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

  [!INCLUDE [explains scaffold generated params](~/includes/mvc-intro/model4.md)]

---

<!-- End of tabs                  -->

<span data-ttu-id="e1527-199">Non è possibile usare ancora le pagine sottoposte a scaffolding perché il database non esiste.</span><span class="sxs-lookup"><span data-stu-id="e1527-199">You can't use the scaffolded pages yet because the database doesn't exist.</span></span> <span data-ttu-id="e1527-200">Se si esegue l'app e si fa clic sul collegamento dell' **app Movie** , viene visualizzato un messaggio di errore *non è possibile aprire il database* o una *tabella di questo tipo:* il messaggio di errore Movie.</span><span class="sxs-lookup"><span data-stu-id="e1527-200">If you run the app and click on the **Movie App** link, you get a *Cannot open database* or *no such table: Movie* error message.</span></span>

<a name="migration"></a>

## <a name="initial-migration"></a><span data-ttu-id="e1527-201">Migrazione iniziale</span><span class="sxs-lookup"><span data-stu-id="e1527-201">Initial migration</span></span>

<span data-ttu-id="e1527-202">Usare la funzionalità [Migrazioni](xref:data/ef-mvc/migrations) di EF Core per creare il database.</span><span class="sxs-lookup"><span data-stu-id="e1527-202">Use the EF Core [Migrations](xref:data/ef-mvc/migrations) feature to create the database.</span></span> <span data-ttu-id="e1527-203">Migrazioni è un set di strumenti che consentono di creare e aggiornare un database in modo che corrisponda al modello di dati.</span><span class="sxs-lookup"><span data-stu-id="e1527-203">Migrations is a set of tools that let you create and update a database to match your data model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e1527-204">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e1527-204">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="e1527-205">Dal menu **strumenti** selezionare **gestione pacchetti NuGet** > console di **Gestione pacchetti** (PMC).</span><span class="sxs-lookup"><span data-stu-id="e1527-205">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console** (PMC).</span></span>

<span data-ttu-id="e1527-206">Nella Console di Gestione pacchetti immettere i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="e1527-206">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration InitialCreate
Update-Database
```

* <span data-ttu-id="e1527-207">`Add-Migration InitialCreate`: genera un file di migrazione *Migrations/{timestamp} _InitialCreate. cs* .</span><span class="sxs-lookup"><span data-stu-id="e1527-207">`Add-Migration InitialCreate`: Generates a *Migrations/{timestamp}_InitialCreate.cs* migration file.</span></span> <span data-ttu-id="e1527-208">L'argomento `InitialCreate` è il nome della migrazione.</span><span class="sxs-lookup"><span data-stu-id="e1527-208">The `InitialCreate` argument is the migration name.</span></span> <span data-ttu-id="e1527-209">È possibile usare qualsiasi nome, ma per convenzione viene selezionato un nome che descrive la migrazione.</span><span class="sxs-lookup"><span data-stu-id="e1527-209">Any name can be used, but by convention, a name is selected that describes the migration.</span></span> <span data-ttu-id="e1527-210">Trattandosi della prima migrazione, la classe generata contiene il codice per creare lo schema del database.</span><span class="sxs-lookup"><span data-stu-id="e1527-210">Because this is the first migration, the generated class contains code to create the database schema.</span></span> <span data-ttu-id="e1527-211">Lo schema del database si basa sul modello specificato nella classe `MvcMovieContext`.</span><span class="sxs-lookup"><span data-stu-id="e1527-211">The database schema is based on the model specified in the `MvcMovieContext` class.</span></span>

* <span data-ttu-id="e1527-212">`Update-Database`: aggiorna il database alla migrazione più recente, creata dal comando precedente.</span><span class="sxs-lookup"><span data-stu-id="e1527-212">`Update-Database`: Updates the database to the latest migration, which the previous command created.</span></span> <span data-ttu-id="e1527-213">Il comando esegue il metodo `Up` nel file *Migrations/{time-stamp}_InitialCreate.cs*, che crea il database.</span><span class="sxs-lookup"><span data-stu-id="e1527-213">This command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

  <span data-ttu-id="e1527-214">Il comando database update genera l'avviso seguente:</span><span class="sxs-lookup"><span data-stu-id="e1527-214">The database update command generates the following warning:</span></span> 

  > <span data-ttu-id="e1527-215">No type was specified for the decimal column 'Price' on entity type 'Movie'. (Nessun tipo specificato per la colonna decimale 'Price' nel tipo di entità 'Movie').</span><span class="sxs-lookup"><span data-stu-id="e1527-215">No type was specified for the decimal column 'Price' on entity type 'Movie'.</span></span> <span data-ttu-id="e1527-216">This will cause values to be silently truncated if they do not fit in the default precision and scale. (I valori saranno quindi automaticamente troncati se non rispettano la precisione e la scala predefinite).</span><span class="sxs-lookup"><span data-stu-id="e1527-216">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span></span> <span data-ttu-id="e1527-217">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'. (Specificare in modo esplicito il tipo di colonna di SQL Server che può supportare tutti i valori usando 'HasColumnType()').</span><span class="sxs-lookup"><span data-stu-id="e1527-217">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'.</span></span>

  <span data-ttu-id="e1527-218">È possibile ignorare tale avviso. Verrà risolto in un'esercitazione successiva.</span><span class="sxs-lookup"><span data-stu-id="e1527-218">You can ignore that warning, it will be fixed in a later tutorial.</span></span>

[!INCLUDE [more information on the PMC tools for EF Core](~/includes/ef-pmc.md)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="e1527-219">Visual Studio Code/Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="e1527-219">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="e1527-220">Eseguire i seguenti comandi dell'interfaccia della riga di comando di .NET Core:</span><span class="sxs-lookup"><span data-stu-id="e1527-220">Run the following .NET Core CLI commands:</span></span>

```dotnetcli
dotnet ef migrations add InitialCreate
dotnet ef database update
```

* <span data-ttu-id="e1527-221">`ef migrations add InitialCreate`: genera un file di migrazione *Migrations/{timestamp} _InitialCreate. cs* .</span><span class="sxs-lookup"><span data-stu-id="e1527-221">`ef migrations add InitialCreate`: Generates an *Migrations/{timestamp}_InitialCreate.cs* migration file.</span></span> <span data-ttu-id="e1527-222">L'argomento `InitialCreate` è il nome della migrazione.</span><span class="sxs-lookup"><span data-stu-id="e1527-222">The `InitialCreate` argument is the migration name.</span></span> <span data-ttu-id="e1527-223">È possibile usare qualsiasi nome, ma per convenzione viene selezionato un nome che descrive la migrazione.</span><span class="sxs-lookup"><span data-stu-id="e1527-223">Any name can be used, but by convention, a name is selected that describes the migration.</span></span> <span data-ttu-id="e1527-224">Trattandosi della prima migrazione, la classe generata contiene il codice per creare lo schema del database.</span><span class="sxs-lookup"><span data-stu-id="e1527-224">Because this is the first migration, the generated class contains code to create the database schema.</span></span> <span data-ttu-id="e1527-225">Lo schema del database è basato sul modello specificato nella classe `MvcMovieContext` (nel file *Data/MvcMovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="e1527-225">The database schema is based on the model specified in the `MvcMovieContext` class (in the *Data/MvcMovieContext.cs* file).</span></span>

* <span data-ttu-id="e1527-226">`ef database update`: aggiorna il database alla migrazione più recente, creata dal comando precedente.</span><span class="sxs-lookup"><span data-stu-id="e1527-226">`ef database update`: Updates the database to the latest migration, which the previous command created.</span></span> <span data-ttu-id="e1527-227">Il comando esegue il metodo `Up` nel file *Migrations/{time-stamp}_InitialCreate.cs*, che crea il database.</span><span class="sxs-lookup"><span data-stu-id="e1527-227">This command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

[!INCLUDE [ more information on the CLI tools for EF Core](~/includes/ef-cli.md)]

---

### <a name="the-initialcreate-class"></a><span data-ttu-id="e1527-228">Classe InitialCreate</span><span class="sxs-lookup"><span data-stu-id="e1527-228">The InitialCreate class</span></span>

<span data-ttu-id="e1527-229">Esaminare il file di migrazione *Migrations/{timestamp}_InitialCreate.cs*:</span><span class="sxs-lookup"><span data-stu-id="e1527-229">Examine the *Migrations/{timestamp}_InitialCreate.cs* migration file:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Migrations/20190805165915_InitialCreate.cs?name=snippet)]

 <span data-ttu-id="e1527-230">Il metodo `Up` crea la tabella Movie e configura `Id` come chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="e1527-230">The `Up` method creates the Movie table and configures `Id` as the primary key.</span></span> <span data-ttu-id="e1527-231">Il metodo `Down` annulla le modifiche dello schema apportate dalla migrazione `Up`.</span><span class="sxs-lookup"><span data-stu-id="e1527-231">The `Down` method reverts the schema changes made by the `Up` migration.</span></span>

<a name="test"></a>

## <a name="test-the-app"></a><span data-ttu-id="e1527-232">Eseguire il test dell'app</span><span class="sxs-lookup"><span data-stu-id="e1527-232">Test the app</span></span>

* <span data-ttu-id="e1527-233">Eseguire l'app e fare clic sul collegamento **Movie App**.</span><span class="sxs-lookup"><span data-stu-id="e1527-233">Run the app and click the **Movie App** link.</span></span>

  <span data-ttu-id="e1527-234">Se si ottiene un'eccezione simile a una delle seguenti:</span><span class="sxs-lookup"><span data-stu-id="e1527-234">If you get an exception similar to one of the following:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e1527-235">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e1527-235">Visual Studio</span></span>](#tab/visual-studio)

  ```console
  SqlException: Cannot open database "MvcMovieContext-1" requested by the login. The login failed.
  ```

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="e1527-236">Visual Studio Code/Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="e1527-236">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

  ```console
  SqliteException: SQLite Error 1: 'no such table: Movie'.
  ```

---
  <span data-ttu-id="e1527-237">Probabilmente non è stato eseguito il [passaggio delle migrazioni](#migration).</span><span class="sxs-lookup"><span data-stu-id="e1527-237">You probably missed the [migrations step](#migration).</span></span>

* <span data-ttu-id="e1527-238">Testare la pagina **Create**.</span><span class="sxs-lookup"><span data-stu-id="e1527-238">Test the **Create** page.</span></span> <span data-ttu-id="e1527-239">Immettere e inviare i dati.</span><span class="sxs-lookup"><span data-stu-id="e1527-239">Enter and submit data.</span></span>

  > [!NOTE]
  > <span data-ttu-id="e1527-240">Potrebbe non essere possibile immettere virgole decimali nel campo `Price`.</span><span class="sxs-lookup"><span data-stu-id="e1527-240">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="e1527-241">Per supportare la [convalida jQuery](https://jqueryvalidation.org/) per impostazioni locali diverse dall'inglese che usano la virgola (",") come separatore decimale e per formati di data diversi da quello dell'inglese (Stati Uniti), è necessario localizzare l'app.</span><span class="sxs-lookup"><span data-stu-id="e1527-241">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="e1527-242">Per istruzioni sulla globalizzazione, vedere [questo problema su GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="e1527-242">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="e1527-243">Testare le pagine **Edit**, **Details** e **Delete**.</span><span class="sxs-lookup"><span data-stu-id="e1527-243">Test the **Edit**, **Details**, and **Delete** pages.</span></span>

## <a name="dependency-injection-in-the-controller"></a><span data-ttu-id="e1527-244">Inserimento delle dipendenze nel controller</span><span class="sxs-lookup"><span data-stu-id="e1527-244">Dependency injection in the controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e1527-245">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e1527-245">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="e1527-246">Aprire il file *Controllers/MoviesController.cs* ed esaminare il costruttore:</span><span class="sxs-lookup"><span data-stu-id="e1527-246">Open the *Controllers/MoviesController.cs* file and examine the constructor:</span></span>

<!-- l.. Make copy of Movies controller (or use the old one as I did in the 3.0 upgrade) because we comment out the initial index method and update it later  -->

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_1)]

<span data-ttu-id="e1527-247">Il costruttore usa l'[inserimento dipendenze](xref:fundamentals/dependency-injection) per inserire il contesto del database (`MvcMovieContext`) nel controller.</span><span class="sxs-lookup"><span data-stu-id="e1527-247">The constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`MvcMovieContext`) into the controller.</span></span> <span data-ttu-id="e1527-248">Il contesto di database viene usato in ognuno dei metodi [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) nel controller.</span><span class="sxs-lookup"><span data-stu-id="e1527-248">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="e1527-249">Visual Studio Code/Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="e1527-249">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_1)]

<span data-ttu-id="e1527-250">Il costruttore usa l'[inserimento dipendenze](xref:fundamentals/dependency-injection) per inserire il contesto del database (`MvcMovieContext`) nel controller.</span><span class="sxs-lookup"><span data-stu-id="e1527-250">The constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`MvcMovieContext`) into the controller.</span></span> <span data-ttu-id="e1527-251">Il contesto di database viene usato in ognuno dei metodi [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) nel controller.</span><span class="sxs-lookup"><span data-stu-id="e1527-251">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

[!INCLUDE [use SQL Server in production](~/includes/RP/sqlitedev.md)]

---
<!-- end of tabs --->

<a name="strongly-typed-models-keyword-label"></a>
<a name="strongly-typed-models-and-the--keyword"></a>

## <a name="strongly-typed-models-and-the-model-keyword"></a><span data-ttu-id="e1527-252">Modelli fortemente tipizzati e parola chiave @model</span><span class="sxs-lookup"><span data-stu-id="e1527-252">Strongly typed models and the @model keyword</span></span>

<span data-ttu-id="e1527-253">In un passaggio precedente di questa esercitazione è stato esaminato come un controller può passare oggetti o dati a una vista usando il dizionario `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="e1527-253">Earlier in this tutorial, you saw how a controller can pass data or objects to a view using the `ViewData` dictionary.</span></span> <span data-ttu-id="e1527-254">Il dizionario `ViewData` è un oggetto dinamico che fornisce un modo pratico ad associazione tardiva per passare informazioni a una vista.</span><span class="sxs-lookup"><span data-stu-id="e1527-254">The `ViewData` dictionary is a dynamic object that provides a convenient late-bound way to pass information to a view.</span></span>

<span data-ttu-id="e1527-255">MVC consente anche di passare oggetti modello fortemente tipizzati a una vista.</span><span class="sxs-lookup"><span data-stu-id="e1527-255">MVC also provides the ability to pass strongly typed model objects to a view.</span></span> <span data-ttu-id="e1527-256">Questo approccio fortemente tipizzato consente il controllo del codice in fase di compilazione.</span><span class="sxs-lookup"><span data-stu-id="e1527-256">This strongly typed approach enables compile time code checking.</span></span> <span data-ttu-id="e1527-257">Con il meccanismo di scaffolding è stato usato questo approccio, ovvero il passaggio di un modello fortemente tipizzato, con le viste e la classe `MoviesController`.</span><span class="sxs-lookup"><span data-stu-id="e1527-257">The scaffolding mechanism used this approach (that is, passing a strongly typed model) with the `MoviesController` class and views.</span></span>

<span data-ttu-id="e1527-258">Aprire il metodo `Details` nel file *Controllers/MoviesController.cs*:</span><span class="sxs-lookup"><span data-stu-id="e1527-258">Examine the generated `Details` method in the *Controllers/MoviesController.cs* file:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_details)]

<span data-ttu-id="e1527-259">In genere il parametro `id` viene passato come dati di route.</span><span class="sxs-lookup"><span data-stu-id="e1527-259">The `id` parameter is generally passed as route data.</span></span> <span data-ttu-id="e1527-260">Ad esempio `https://localhost:5001/movies/details/1` imposta:</span><span class="sxs-lookup"><span data-stu-id="e1527-260">For example `https://localhost:5001/movies/details/1` sets:</span></span>

* <span data-ttu-id="e1527-261">Il controller sul controller `movies` (primo segmento di URL).</span><span class="sxs-lookup"><span data-stu-id="e1527-261">The controller to the `movies` controller (the first URL segment).</span></span>
* <span data-ttu-id="e1527-262">L'azione su `details` (secondo segmento di URL).</span><span class="sxs-lookup"><span data-stu-id="e1527-262">The action to `details` (the second URL segment).</span></span>
* <span data-ttu-id="e1527-263">L'ID su 1 (ultimo segmento di URL).</span><span class="sxs-lookup"><span data-stu-id="e1527-263">The id to 1 (the last URL segment).</span></span>

<span data-ttu-id="e1527-264">È possibile anche passare `id` con una stringa di query nel modo seguente:</span><span class="sxs-lookup"><span data-stu-id="e1527-264">You can also pass in the `id` with a query string as follows:</span></span>

`https://localhost:5001/movies/details?id=1`

<span data-ttu-id="e1527-265">Il parametro `id` viene definito come [tipo nullable](/dotnet/csharp/programming-guide/nullable-types/index) (`int?`) nel caso in cui non venga fornito un valore ID.</span><span class="sxs-lookup"><span data-stu-id="e1527-265">The `id` parameter is defined as a [nullable type](/dotnet/csharp/programming-guide/nullable-types/index) (`int?`) in case an ID value isn't provided.</span></span>

<span data-ttu-id="e1527-266">Un'[espressione lambda](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) viene passata a `FirstOrDefaultAsync` per selezionare le entità film che corrispondono al valore della stringa di query o dei dati di route.</span><span class="sxs-lookup"><span data-stu-id="e1527-266">A [lambda expression](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) is passed in to `FirstOrDefaultAsync` to select movie entities that match the route data or query string value.</span></span>

```csharp
var movie = await _context.Movie
    .FirstOrDefaultAsync(m => m.Id == id);
```

<span data-ttu-id="e1527-267">Se viene trovato un film, viene passata un'istanza del modello `Movie` alla vista `Details`:</span><span class="sxs-lookup"><span data-stu-id="e1527-267">If a movie is found, an instance of the `Movie` model is passed to the `Details` view:</span></span>

```csharp
return View(movie);
   ```

<span data-ttu-id="e1527-268">Esaminare il contenuto del file *Views/Movies/Details.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="e1527-268">Examine the contents of the *Views/Movies/Details.cshtml* file:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/DetailsOriginal.cshtml)]

<span data-ttu-id="e1527-269">L'istruzione `@model` all'inizio del file di vista specifica il tipo di oggetto previsto dalla vista.</span><span class="sxs-lookup"><span data-stu-id="e1527-269">The `@model` statement at the top of the view file specifies the type of object that the view expects.</span></span> <span data-ttu-id="e1527-270">Quando è stato creato il controller dei film, è stata inclusa l'istruzione `@model` seguente:</span><span class="sxs-lookup"><span data-stu-id="e1527-270">When the movie controller was created, the following `@model` statement was included:</span></span>

```HTML
@model MvcMovie.Models.Movie
   ```

<span data-ttu-id="e1527-271">Questa direttiva `@model` consente di accedere al film che il controller ha passato alla vista.</span><span class="sxs-lookup"><span data-stu-id="e1527-271">This `@model` directive allows access to the movie that the controller passed to the view.</span></span> <span data-ttu-id="e1527-272">L'oggetto `Model` è fortemente tipizzato.</span><span class="sxs-lookup"><span data-stu-id="e1527-272">The `Model` object is strongly typed.</span></span> <span data-ttu-id="e1527-273">Ad esempio, nella vista *Details.cshtml* il codice passa ogni campo di film agli helper HTML `DisplayNameFor` e `DisplayFor` con l'oggetto `Model` fortemente tipizzato.</span><span class="sxs-lookup"><span data-stu-id="e1527-273">For example, in the *Details.cshtml* view, the code passes each movie field to the `DisplayNameFor` and `DisplayFor` HTML Helpers with the strongly typed `Model` object.</span></span> <span data-ttu-id="e1527-274">Le viste e i metodi `Create` e `Edit` passano anche un oggetto modello `Movie`.</span><span class="sxs-lookup"><span data-stu-id="e1527-274">The `Create` and `Edit` methods and views also pass a `Movie` model object.</span></span>

<span data-ttu-id="e1527-275">Esaminare la vista *Index.cshtml* e il metodo `Index` nel controller Movies.</span><span class="sxs-lookup"><span data-stu-id="e1527-275">Examine the *Index.cshtml* view and the `Index` method in the Movies controller.</span></span> <span data-ttu-id="e1527-276">Si noti che il codice crea un oggetto `List` quando chiama il metodo `View`.</span><span class="sxs-lookup"><span data-stu-id="e1527-276">Notice how the code creates a `List` object when it calls the `View` method.</span></span> <span data-ttu-id="e1527-277">Il codice passa questo elenco `Movies` dal metodo di azione `Index` alla vista:</span><span class="sxs-lookup"><span data-stu-id="e1527-277">The code passes this `Movies` list from the `Index` action method to the view:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_index)]

<span data-ttu-id="e1527-278">Al momento della creazione del controller di film, lo scaffolding ha incluso l'istruzione `@model` all'inizio del file *Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="e1527-278">When the movies controller was created, scaffolding included the following `@model` statement at the top of the *Index.cshtml* file:</span></span>

<!-- Copy Index.cshtml to IndexOriginal.cshtml -->

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexOriginal.cshtml?range=1)]

<span data-ttu-id="e1527-279">La direttiva `@model` consente di accedere all'elenco di film che il controller ha passato alla vista usando un oggetto `Model` fortemente tipizzato.</span><span class="sxs-lookup"><span data-stu-id="e1527-279">The `@model` directive allows you to access the list of movies that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="e1527-280">Ad esempio, nella vista *Index.cshtml* il codice scorre i film con un'istruzione `foreach` sull'oggetto fortemente tipizzato `Model`:</span><span class="sxs-lookup"><span data-stu-id="e1527-280">For example, in the *Index.cshtml* view, the code loops through the movies with a `foreach` statement over the strongly typed `Model` object:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexOriginal.cshtml?highlight=1,31,34,37,40,43,46-48)]

<span data-ttu-id="e1527-281">Poiché l'oggetto `Model` è fortemente tipizzato (come un oggetto `IEnumerable<Movie>`), ogni elemento nel ciclo viene tipizzato come `Movie`.</span><span class="sxs-lookup"><span data-stu-id="e1527-281">Because the `Model` object is strongly typed (as an `IEnumerable<Movie>` object), each item in the loop is typed as `Movie`.</span></span> <span data-ttu-id="e1527-282">Tra gli altri vantaggi, si ottiene un controllo del codice in fase di compilazione.</span><span class="sxs-lookup"><span data-stu-id="e1527-282">Among other benefits, this means that you get compile time checking of the code.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e1527-283">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="e1527-283">Additional resources</span></span>

* [<span data-ttu-id="e1527-284">Helper tag</span><span class="sxs-lookup"><span data-stu-id="e1527-284">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="e1527-285">Globalizzazione e localizzazione</span><span class="sxs-lookup"><span data-stu-id="e1527-285">Globalization and localization</span></span>](xref:fundamentals/localization)

> [!div class="step-by-step"]
> <span data-ttu-id="e1527-286">[Precedente - Aggiunta di una vista](adding-view.md)
> [Successivo - Utilizzo del linguaggio SQL](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="e1527-286">[Previous Adding a View](adding-view.md)
[Next Working with SQL](working-with-sql.md)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

## <a name="add-a-data-model-class"></a><span data-ttu-id="e1527-287">Aggiungere una classe del modello di dati</span><span class="sxs-lookup"><span data-stu-id="e1527-287">Add a data model class</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e1527-288">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e1527-288">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="e1527-289">Fare clic con il pulsante destro del mouse sulla cartella *Models* > **Aggiungi** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="e1527-289">Right-click the *Models* folder > **Add** > **Class**.</span></span> <span data-ttu-id="e1527-290">Denominare la classe **Movie**.</span><span class="sxs-lookup"><span data-stu-id="e1527-290">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/mvc-intro/model1b.md)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="e1527-291">Visual Studio Code/Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="e1527-291">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="e1527-292">Aggiungere una classe alla cartella *Modello* denominata *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="e1527-292">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/mvc-intro/model1b.md)]
[!INCLUDE [model 2](~/includes/mvc-intro/model2.md)]

---

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="e1527-293">Eseguire lo scaffolding del modello di filmato</span><span class="sxs-lookup"><span data-stu-id="e1527-293">Scaffold the movie model</span></span>

<span data-ttu-id="e1527-294">In questa sezione viene eseguito lo scaffolding del modello *Movie*.</span><span class="sxs-lookup"><span data-stu-id="e1527-294">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="e1527-295">Lo strumento di scaffolding crea quindi le pagine per le operazioni CRUD (creazione, lettura, aggiornamento ed eliminazione) per il modello *Movie*.</span><span class="sxs-lookup"><span data-stu-id="e1527-295">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e1527-296">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e1527-296">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="e1527-297">In **Esplora soluzioni** fare clic con il pulsante destro del mouse sulla cartella *Controller* **> Aggiungi > Nuovo elemento di scaffolding**.</span><span class="sxs-lookup"><span data-stu-id="e1527-297">In **Solution Explorer**, right-click the *Controllers* folder **> Add > New Scaffolded Item**.</span></span>

![vista del passaggio precedente](adding-model/_static/add_controller21.png)

<span data-ttu-id="e1527-299">Nella finestra di dialogo **Aggiungi scaffolding** selezionare **Controller MVC con visualizzazioni, che usa Entity Framework**.</span><span class="sxs-lookup"><span data-stu-id="e1527-299">In the **Add Scaffold** dialog, select **MVC Controller with views, using Entity Framework > Add**.</span></span>

![Finestra di dialogo Aggiungi scaffolding](adding-model/_static/add_scaffold21.png)

<span data-ttu-id="e1527-301">Completare la finestra di dialogo **Aggiungi controller**:</span><span class="sxs-lookup"><span data-stu-id="e1527-301">Complete the **Add Controller** dialog:</span></span>

* <span data-ttu-id="e1527-302">**Classe modello:** *Movie (MvcMovie. Models)*</span><span class="sxs-lookup"><span data-stu-id="e1527-302">**Model class:** *Movie (MvcMovie.Models)*</span></span>
* <span data-ttu-id="e1527-303">**Classe del contesto dati:** selezionare l'icona **+** e aggiungere il valore predefinito **MvcMovie.Models.MvcMovieContext**</span><span class="sxs-lookup"><span data-stu-id="e1527-303">**Data context class:** Select the **+** icon and add the default **MvcMovie.Models.MvcMovieContext**</span></span>

![Aggiungere il contesto dati](adding-model/_static/dc.png)

* <span data-ttu-id="e1527-305">**Viste:** mantenere il valore predefinito di ogni opzione selezionata</span><span class="sxs-lookup"><span data-stu-id="e1527-305">**Views:** Keep the default of each option checked</span></span>
* <span data-ttu-id="e1527-306">**Nome del controller:** mantenere il valore predefinito *MoviesController*</span><span class="sxs-lookup"><span data-stu-id="e1527-306">**Controller name:** Keep the default *MoviesController*</span></span>
* <span data-ttu-id="e1527-307">Selezionare **Aggiungi**</span><span class="sxs-lookup"><span data-stu-id="e1527-307">Select **Add**</span></span>

![Finestra di dialogo Aggiungi controller](adding-model/_static/add_controller2.png)

<span data-ttu-id="e1527-309">Visual Studio crea:</span><span class="sxs-lookup"><span data-stu-id="e1527-309">Visual Studio creates:</span></span>

* <span data-ttu-id="e1527-310">Una [classe del contesto di database](xref:data/ef-mvc/intro#create-the-database-context) Entity Framework Core (*Data/MvcMovieContext.cs*)</span><span class="sxs-lookup"><span data-stu-id="e1527-310">An Entity Framework Core [database context class](xref:data/ef-mvc/intro#create-the-database-context) (*Data/MvcMovieContext.cs*)</span></span>
* <span data-ttu-id="e1527-311">Un controller di film (*Controllers/MoviesController.cs*)</span><span class="sxs-lookup"><span data-stu-id="e1527-311">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="e1527-312">File di vista Razor per le pagine Create (Crea), Delete (Elimina), Details (Dettagli), Edit (Modifica) e Index (Indice) (*Views/Movies/\*.cshtml*)</span><span class="sxs-lookup"><span data-stu-id="e1527-312">Razor view files for Create, Delete, Details, Edit, and Index pages (*Views/Movies/\*.cshtml*)</span></span>

<span data-ttu-id="e1527-313">La creazione automatica del contesto di database e di viste e metodi di azione [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, delete) è nota come *scaffolding*.</span><span class="sxs-lookup"><span data-stu-id="e1527-313">The automatic creation of the database context and [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views is known as *scaffolding*.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e1527-314">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e1527-314">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="e1527-315">Aprire una finestra di comando nella directory del progetto (la directory che contiene i file *Program.cs*, *Startup.cs* e *csproj*).</span><span class="sxs-lookup"><span data-stu-id="e1527-315">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="e1527-316">Installare lo strumento di scaffolding:</span><span class="sxs-lookup"><span data-stu-id="e1527-316">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="e1527-317">In Linux esportare il percorso dello strumento di scaffolding:</span><span class="sxs-lookup"><span data-stu-id="e1527-317">On Linux, export the scaffold tool path:</span></span>

  ```console
    export PATH=$HOME/.dotnet/tools:$PATH
  ```

* <span data-ttu-id="e1527-318">Eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="e1527-318">Run the following command:</span></span>

  ```dotnetcli
   dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold generated params](~/includes/mvc-intro/model4.md)]

<!-- Mac -------------------------->

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e1527-319">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="e1527-319">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="e1527-320">Aprire una finestra di comando nella directory del progetto (la directory che contiene i file *Program.cs*, *Startup.cs* e *csproj*).</span><span class="sxs-lookup"><span data-stu-id="e1527-320">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="e1527-321">Installare lo strumento di scaffolding:</span><span class="sxs-lookup"><span data-stu-id="e1527-321">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="e1527-322">Eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="e1527-322">Run the following command:</span></span>

  ```dotnetcli
   dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold generated params](~/includes/mvc-intro/model4.md)]

---

<!-- End of VS tabs                  -->

<span data-ttu-id="e1527-323">Se si esegue l'app e si fa clic sul collegamento **Mvc Movie**, viene visualizzato un errore simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="e1527-323">If you run the app and click on the **Mvc Movie** link, you get an error similar to the following:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e1527-324">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e1527-324">Visual Studio</span></span>](#tab/visual-studio)

``` error
An unhandled exception occurred while processing the request.

SqlException: Cannot open database "MvcMovieContext-<GUID removed>" requested by the login. The login failed.
Login failed for user 'Rick'.

System.Data.SqlClient.SqlInternalConnectionTds..ctor(DbConnectionPoolIdentity identity, SqlConnectionString
```

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="e1527-325">Visual Studio Code/Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="e1527-325">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

``` error
An unhandled exception occurred while processing the request.
SqliteException: SQLite Error 1: 'no such table: Movie'.
Microsoft.Data.Sqlite.SqliteException.ThrowExceptionForRC(int rc, sqlite3 db)

```

---

<span data-ttu-id="e1527-326">È necessario creare il database, quindi si usa la funzionalità di [migrazioni](xref:data/ef-mvc/migrations) di Entity Framework Core a tale scopo.</span><span class="sxs-lookup"><span data-stu-id="e1527-326">You need to create the database, and you use the EF Core [Migrations](xref:data/ef-mvc/migrations) feature to do that.</span></span> <span data-ttu-id="e1527-327">Le migrazioni consentono di creare un database che corrisponde al modello di dati e aggiornare lo schema del database quando cambia il modello di dati.</span><span class="sxs-lookup"><span data-stu-id="e1527-327">Migrations lets you create a database that matches your data model and update the database schema when your data model changes.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="e1527-328">Migrazione iniziale</span><span class="sxs-lookup"><span data-stu-id="e1527-328">Initial migration</span></span>

<span data-ttu-id="e1527-329">In questa sezione vengono completate le attività seguenti:</span><span class="sxs-lookup"><span data-stu-id="e1527-329">In this section, the following tasks are completed:</span></span>

* <span data-ttu-id="e1527-330">Aggiungere una migrazione iniziale.</span><span class="sxs-lookup"><span data-stu-id="e1527-330">Add an initial migration.</span></span>
* <span data-ttu-id="e1527-331">Aggiornare il database con la migrazione iniziale.</span><span class="sxs-lookup"><span data-stu-id="e1527-331">Update the database with the initial migration.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e1527-332">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e1527-332">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="e1527-333">Dal menu **strumenti** selezionare **gestione pacchetti NuGet** > console di **Gestione pacchetti** (PMC).</span><span class="sxs-lookup"><span data-stu-id="e1527-333">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console** (PMC).</span></span>

   ![Menu della Console di Gestione pacchetti](~/tutorials/first-mvc-app/adding-model/_static/pmc.png)

1. <span data-ttu-id="e1527-335">Nella Console di Gestione pacchetti immettere i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="e1527-335">In the PMC, enter the following commands:</span></span>

   ```PMC
   Add-Migration Initial
   Update-Database
   ```

   <span data-ttu-id="e1527-336">Il comando `Add-Migration` genera un codice per creare lo schema del database iniziale.</span><span class="sxs-lookup"><span data-stu-id="e1527-336">The `Add-Migration` command generates code to create the initial database schema.</span></span>

   <span data-ttu-id="e1527-337">Lo schema del database si basa sul modello specificato nella classe `MvcMovieContext`.</span><span class="sxs-lookup"><span data-stu-id="e1527-337">The database schema is based on the model specified in the `MvcMovieContext` class.</span></span> <span data-ttu-id="e1527-338">L'argomento `Initial` è il nome della migrazione.</span><span class="sxs-lookup"><span data-stu-id="e1527-338">The `Initial` argument is the migration name.</span></span> <span data-ttu-id="e1527-339">È possibile usare qualsiasi nome, ma per convenzione viene usato un nome che descrive la migrazione.</span><span class="sxs-lookup"><span data-stu-id="e1527-339">Any name can be used, but by convention, a name that describes the migration is used.</span></span> <span data-ttu-id="e1527-340">Per ulteriori informazioni, vedere <xref:data/ef-mvc/migrations>.</span><span class="sxs-lookup"><span data-stu-id="e1527-340">For more information, see <xref:data/ef-mvc/migrations>.</span></span>

   <span data-ttu-id="e1527-341">Il comando `Update-Database` esegue il metodo `Up` nel file *Migrations/{time-stamp}_InitialCreate.cs*, che crea il database.</span><span class="sxs-lookup"><span data-stu-id="e1527-341">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="e1527-342">Visual Studio Code/Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="e1527-342">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

<span data-ttu-id="e1527-343">Il comando `ef migrations add InitialCreate` genera un codice per creare lo schema del database iniziale.</span><span class="sxs-lookup"><span data-stu-id="e1527-343">The `ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span>

<span data-ttu-id="e1527-344">Lo schema del database è basato sul modello specificato nella classe `MvcMovieContext` (nel file *Data/MvcMovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="e1527-344">The database schema is based on the model specified in the `MvcMovieContext` class (in the *Data/MvcMovieContext.cs* file).</span></span> <span data-ttu-id="e1527-345">L'argomento `InitialCreate` è il nome della migrazione.</span><span class="sxs-lookup"><span data-stu-id="e1527-345">The `InitialCreate` argument is the migration name.</span></span> <span data-ttu-id="e1527-346">È possibile usare qualsiasi nome, ma per convenzione viene selezionato un nome che descrive la migrazione.</span><span class="sxs-lookup"><span data-stu-id="e1527-346">Any name can be used, but by convention, a name is selected that describes the migration.</span></span>

---

## <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="e1527-347">Esaminare il contesto registrato con l'inserimento di dipendenze</span><span class="sxs-lookup"><span data-stu-id="e1527-347">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="e1527-348">ASP.NET Core viene compilato con l'[inserimento di dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="e1527-348">ASP.NET Core is built with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="e1527-349">I servizi, ad esempio il contesto di database di EF Core, vengono registrati con l'inserimento delle dipendenze durante l'avvio dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="e1527-349">Services (such as the EF Core DB context) are registered with DI during application startup.</span></span> <span data-ttu-id="e1527-350">Questi servizi vengono quindi offerti ai componenti per cui sono necessari (ad esempio Razor Pages) tramite i parametri del costruttore.</span><span class="sxs-lookup"><span data-stu-id="e1527-350">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="e1527-351">Più avanti nell'esercitazione viene illustrato il codice del costruttore che ottiene un'istanza del contesto di database.</span><span class="sxs-lookup"><span data-stu-id="e1527-351">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e1527-352">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e1527-352">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="e1527-353">Lo strumento di scaffolding ha creato automaticamente un contesto del database e lo ha registrato con il contenitore di inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="e1527-353">The scaffolding tool automatically created a DB context and registered it with the DI container.</span></span>

<span data-ttu-id="e1527-354">Esaminare il metodo `Startup.ConfigureServices` seguente.</span><span class="sxs-lookup"><span data-stu-id="e1527-354">Examine the following `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="e1527-355">La riga evidenziata è stata aggiunta dallo scaffolder:</span><span class="sxs-lookup"><span data-stu-id="e1527-355">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=14-15)]

<span data-ttu-id="e1527-356">`MvcMovieContext` coordina la funzionalità di EF Core (Create, Read, Update, Delete e così via) per il modello `Movie`.</span><span class="sxs-lookup"><span data-stu-id="e1527-356">The `MvcMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="e1527-357">Il contesto dei dati (`MvcMovieContext`) è derivato da [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="e1527-357">The data context (`MvcMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="e1527-358">Il contesto dei dati specifica le entità incluse nel modello di dati:</span><span class="sxs-lookup"><span data-stu-id="e1527-358">The data context specifies which entities are included in the data model:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Data/MvcMovieContext.cs)]

<span data-ttu-id="e1527-359">Il codice precedente crea una proprietà [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) per il set di entità.</span><span class="sxs-lookup"><span data-stu-id="e1527-359">The preceding code creates a [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="e1527-360">Nella terminologia di Entity Framework, un set di entità corrisponde in genere alla tabella di un database.</span><span class="sxs-lookup"><span data-stu-id="e1527-360">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="e1527-361">Un'entità corrisponde a una riga nella tabella.</span><span class="sxs-lookup"><span data-stu-id="e1527-361">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="e1527-362">Il nome della stringa di connessione viene passato al contesto chiamando un metodo in un oggetto [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions).</span><span class="sxs-lookup"><span data-stu-id="e1527-362">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="e1527-363">Per lo sviluppo locale, il [sistema di configurazione di ASP.NET Core](xref:fundamentals/configuration/index) legge la stringa di connessione dal file *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="e1527-363">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="e1527-364">Visual Studio Code/Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="e1527-364">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="e1527-365">È stato creato un contesto del database, poi registrato per il contenitore di inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="e1527-365">You created a DB context and registered it with the DI container.</span></span>

---

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="e1527-366">Eseguire il test dell'app</span><span class="sxs-lookup"><span data-stu-id="e1527-366">Test the app</span></span>

* <span data-ttu-id="e1527-367">Eseguire l'app e accodare `/Movies` all'URL nel browser (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="e1527-367">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="e1527-368">Se si verifica un'eccezione di database simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="e1527-368">If you get a database exception similar to the following:</span></span>

```console
SqlException: Cannot open database "MvcMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="e1527-369">Non è stato eseguita la [migrazione](#pmc).</span><span class="sxs-lookup"><span data-stu-id="e1527-369">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="e1527-370">Eseguire il test del collegamento **Crea**.</span><span class="sxs-lookup"><span data-stu-id="e1527-370">Test the **Create** link.</span></span> <span data-ttu-id="e1527-371">Immettere e inviare i dati.</span><span class="sxs-lookup"><span data-stu-id="e1527-371">Enter and submit data.</span></span>

  > [!NOTE]
  > <span data-ttu-id="e1527-372">Potrebbe non essere possibile immettere virgole decimali nel campo `Price`.</span><span class="sxs-lookup"><span data-stu-id="e1527-372">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="e1527-373">Per supportare la [convalida jQuery](https://jqueryvalidation.org/) per impostazioni locali diverse dall'inglese che usano la virgola (",") come separatore decimale e per formati di data diversi da quello dell'inglese (Stati Uniti), è necessario localizzare l'app.</span><span class="sxs-lookup"><span data-stu-id="e1527-373">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="e1527-374">Per istruzioni sulla globalizzazione, vedere [questo problema su GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span><span class="sxs-lookup"><span data-stu-id="e1527-374">For globalization instructions, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="e1527-375">Eseguire il test dei collegamenti **Modifica**, **Dettagli** e **Elimina**.</span><span class="sxs-lookup"><span data-stu-id="e1527-375">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="e1527-376">Esaminare la classe `Startup`:</span><span class="sxs-lookup"><span data-stu-id="e1527-376">Examine the `Startup` class:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=13-99)]

<span data-ttu-id="e1527-377">Il codice evidenziato riportato in precedenza mostra il contesto del database dei film che viene aggiunto al contenitore di [inserimento dipendenze](xref:fundamentals/dependency-injection):</span><span class="sxs-lookup"><span data-stu-id="e1527-377">The preceding highlighted code shows the movie database context being added to the [Dependency Injection](xref:fundamentals/dependency-injection) container:</span></span>

* <span data-ttu-id="e1527-378">`services.AddDbContext<MvcMovieContext>(options =>` specifica il database da usare e la stringa di connessione.</span><span class="sxs-lookup"><span data-stu-id="e1527-378">`services.AddDbContext<MvcMovieContext>(options =>` specifies the database to use and the connection string.</span></span>
* <span data-ttu-id="e1527-379">`=>` è un [operatore lambda](/dotnet/articles/csharp/language-reference/operators/lambda-operator)</span><span class="sxs-lookup"><span data-stu-id="e1527-379">`=>` is a [lambda operator](/dotnet/articles/csharp/language-reference/operators/lambda-operator)</span></span>

<span data-ttu-id="e1527-380">Aprire il file *Controllers/MoviesController.cs* ed esaminare il costruttore:</span><span class="sxs-lookup"><span data-stu-id="e1527-380">Open the *Controllers/MoviesController.cs* file and examine the constructor:</span></span>

<!-- l.. Make copy of Movies controller because we comment out the initial index method and update it later  -->

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_1)]

<span data-ttu-id="e1527-381">Il costruttore usa l'[inserimento dipendenze](xref:fundamentals/dependency-injection) per inserire il contesto del database (`MvcMovieContext`) nel controller.</span><span class="sxs-lookup"><span data-stu-id="e1527-381">The constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`MvcMovieContext`) into the controller.</span></span> <span data-ttu-id="e1527-382">Il contesto di database viene usato in ognuno dei metodi [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) nel controller.</span><span class="sxs-lookup"><span data-stu-id="e1527-382">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

<a name="strongly-typed-models-keyword-label"></a>
<a name="strongly-typed-models-and-the--keyword"></a>

## <a name="strongly-typed-models-and-the-model-keyword"></a><span data-ttu-id="e1527-383">Modelli fortemente tipizzati e parola chiave @model</span><span class="sxs-lookup"><span data-stu-id="e1527-383">Strongly typed models and the @model keyword</span></span>

<span data-ttu-id="e1527-384">In un passaggio precedente di questa esercitazione è stato esaminato come un controller può passare oggetti o dati a una vista usando il dizionario `ViewData`.</span><span class="sxs-lookup"><span data-stu-id="e1527-384">Earlier in this tutorial, you saw how a controller can pass data or objects to a view using the `ViewData` dictionary.</span></span> <span data-ttu-id="e1527-385">Il dizionario `ViewData` è un oggetto dinamico che fornisce un modo pratico ad associazione tardiva per passare informazioni a una vista.</span><span class="sxs-lookup"><span data-stu-id="e1527-385">The `ViewData` dictionary is a dynamic object that provides a convenient late-bound way to pass information to a view.</span></span>

<span data-ttu-id="e1527-386">MVC consente anche di passare oggetti modello fortemente tipizzati a una vista.</span><span class="sxs-lookup"><span data-stu-id="e1527-386">MVC also provides the ability to pass strongly typed model objects to a view.</span></span> <span data-ttu-id="e1527-387">Questo approccio fortemente tipizzato consente un migliore controllo del codice in fase di compilazione.</span><span class="sxs-lookup"><span data-stu-id="e1527-387">This strongly typed approach enables better compile time checking of your code.</span></span> <span data-ttu-id="e1527-388">Con il meccanismo di scaffolding è stato usato questo approccio, ovvero il passaggio di un modello fortemente tipizzato, con le viste e la classe `MoviesController` quando sono stati creati i metodi e le viste.</span><span class="sxs-lookup"><span data-stu-id="e1527-388">The scaffolding mechanism used this approach (that is, passing a strongly typed model) with the `MoviesController` class and views when it created the methods and views.</span></span>

<span data-ttu-id="e1527-389">Aprire il metodo `Details` nel file *Controllers/MoviesController.cs*:</span><span class="sxs-lookup"><span data-stu-id="e1527-389">Examine the generated `Details` method in the *Controllers/MoviesController.cs* file:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_details)]

<span data-ttu-id="e1527-390">In genere il parametro `id` viene passato come dati di route.</span><span class="sxs-lookup"><span data-stu-id="e1527-390">The `id` parameter is generally passed as route data.</span></span> <span data-ttu-id="e1527-391">Ad esempio `https://localhost:5001/movies/details/1` imposta:</span><span class="sxs-lookup"><span data-stu-id="e1527-391">For example `https://localhost:5001/movies/details/1` sets:</span></span>

* <span data-ttu-id="e1527-392">Il controller sul controller `movies` (primo segmento di URL).</span><span class="sxs-lookup"><span data-stu-id="e1527-392">The controller to the `movies` controller (the first URL segment).</span></span>
* <span data-ttu-id="e1527-393">L'azione su `details` (secondo segmento di URL).</span><span class="sxs-lookup"><span data-stu-id="e1527-393">The action to `details` (the second URL segment).</span></span>
* <span data-ttu-id="e1527-394">L'ID su 1 (ultimo segmento di URL).</span><span class="sxs-lookup"><span data-stu-id="e1527-394">The id to 1 (the last URL segment).</span></span>

<span data-ttu-id="e1527-395">È possibile anche passare `id` con una stringa di query nel modo seguente:</span><span class="sxs-lookup"><span data-stu-id="e1527-395">You can also pass in the `id` with a query string as follows:</span></span>

`https://localhost:5001/movies/details?id=1`

<span data-ttu-id="e1527-396">Il parametro `id` viene definito come [tipo nullable](/dotnet/csharp/programming-guide/nullable-types/index) (`int?`) nel caso in cui non venga fornito un valore ID.</span><span class="sxs-lookup"><span data-stu-id="e1527-396">The `id` parameter is defined as a [nullable type](/dotnet/csharp/programming-guide/nullable-types/index) (`int?`) in case an ID value isn't provided.</span></span>

<span data-ttu-id="e1527-397">Un'[espressione lambda](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) viene passata a `FirstOrDefaultAsync` per selezionare le entità film che corrispondono al valore della stringa di query o dei dati di route.</span><span class="sxs-lookup"><span data-stu-id="e1527-397">A [lambda expression](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) is passed in to `FirstOrDefaultAsync` to select movie entities that match the route data or query string value.</span></span>

```csharp
var movie = await _context.Movie
    .FirstOrDefaultAsync(m => m.Id == id);
```

<span data-ttu-id="e1527-398">Se viene trovato un film, viene passata un'istanza del modello `Movie` alla vista `Details`:</span><span class="sxs-lookup"><span data-stu-id="e1527-398">If a movie is found, an instance of the `Movie` model is passed to the `Details` view:</span></span>

```csharp
return View(movie);
   ```

<span data-ttu-id="e1527-399">Esaminare il contenuto del file *Views/Movies/Details.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="e1527-399">Examine the contents of the *Views/Movies/Details.cshtml* file:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/DetailsOriginal.cshtml)]

<span data-ttu-id="e1527-400">Includendo un'istruzione `@model` all'inizio del file di vista, è possibile specificare il tipo di oggetto previsto dalla vista.</span><span class="sxs-lookup"><span data-stu-id="e1527-400">By including a `@model` statement at the top of the view file, you can specify the type of object that the view expects.</span></span> <span data-ttu-id="e1527-401">Al momento della creazione del controller di film, l'istruzione `@model` seguente è stata inclusa automaticamente all'inizio del file *Details.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="e1527-401">When you created the movie controller, the following `@model` statement was automatically included at the top of the *Details.cshtml* file:</span></span>

```HTML
@model MvcMovie.Models.Movie
   ```

<span data-ttu-id="e1527-402">Questa direttiva `@model` consente di accedere al film passato dal controller alla vista usando un oggetto `Model` fortemente tipizzato.</span><span class="sxs-lookup"><span data-stu-id="e1527-402">This `@model` directive allows you to access the movie that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="e1527-403">Ad esempio, nella vista *Details.cshtml* il codice passa ogni campo di film agli helper HTML `DisplayNameFor` e `DisplayFor` con l'oggetto `Model` fortemente tipizzato.</span><span class="sxs-lookup"><span data-stu-id="e1527-403">For example, in the *Details.cshtml* view, the code passes each movie field to the `DisplayNameFor` and `DisplayFor` HTML Helpers with the strongly typed `Model` object.</span></span> <span data-ttu-id="e1527-404">Le viste e i metodi `Create` e `Edit` passano anche un oggetto modello `Movie`.</span><span class="sxs-lookup"><span data-stu-id="e1527-404">The `Create` and `Edit` methods and views also pass a `Movie` model object.</span></span>

<span data-ttu-id="e1527-405">Esaminare la vista *Index.cshtml* e il metodo `Index` nel controller Movies.</span><span class="sxs-lookup"><span data-stu-id="e1527-405">Examine the *Index.cshtml* view and the `Index` method in the Movies controller.</span></span> <span data-ttu-id="e1527-406">Si noti che il codice crea un oggetto `List` quando chiama il metodo `View`.</span><span class="sxs-lookup"><span data-stu-id="e1527-406">Notice how the code creates a `List` object when it calls the `View` method.</span></span> <span data-ttu-id="e1527-407">Il codice passa questo elenco `Movies` dal metodo di azione `Index` alla vista:</span><span class="sxs-lookup"><span data-stu-id="e1527-407">The code passes this `Movies` list from the `Index` action method to the view:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_index)]

<span data-ttu-id="e1527-408">Al momento della creazione del controller di film, lo scaffolding ha incluso automaticamente l'istruzione `@model` all'inizio del file *Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="e1527-408">When you created the movies controller, scaffolding automatically included the following `@model` statement at the top of the *Index.cshtml* file:</span></span>

<!-- Copy Index.cshtml to IndexOriginal.cshtml -->

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexOriginal.cshtml?range=1)]

<span data-ttu-id="e1527-409">La direttiva `@model` consente di accedere all'elenco di film che il controller ha passato alla vista usando un oggetto `Model` fortemente tipizzato.</span><span class="sxs-lookup"><span data-stu-id="e1527-409">The `@model` directive allows you to access the list of movies that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="e1527-410">Ad esempio, nella vista *Index.cshtml* il codice scorre i film con un'istruzione `foreach` sull'oggetto fortemente tipizzato `Model`:</span><span class="sxs-lookup"><span data-stu-id="e1527-410">For example, in the *Index.cshtml* view, the code loops through the movies with a `foreach` statement over the strongly typed `Model` object:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexOriginal.cshtml?highlight=1,31,34,37,40,43,46-48)]

<span data-ttu-id="e1527-411">Poiché l'oggetto `Model` è fortemente tipizzato (come un oggetto `IEnumerable<Movie>`), ogni elemento nel ciclo viene tipizzato come `Movie`.</span><span class="sxs-lookup"><span data-stu-id="e1527-411">Because the `Model` object is strongly typed (as an `IEnumerable<Movie>` object), each item in the loop is typed as `Movie`.</span></span> <span data-ttu-id="e1527-412">Tra gli altri vantaggi, si ottiene un controllo del codice in fase di compilazione:</span><span class="sxs-lookup"><span data-stu-id="e1527-412">Among other benefits, this means that you get compile time checking of the code:</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e1527-413">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="e1527-413">Additional resources</span></span>

* [<span data-ttu-id="e1527-414">Helper tag</span><span class="sxs-lookup"><span data-stu-id="e1527-414">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="e1527-415">Globalizzazione e localizzazione</span><span class="sxs-lookup"><span data-stu-id="e1527-415">Globalization and localization</span></span>](xref:fundamentals/localization)

> [!div class="step-by-step"]
> <span data-ttu-id="e1527-416">[Precedente aggiunta di una vista](adding-view.md)
> [successiva utilizzo di un database](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="e1527-416">[Previous Adding a View](adding-view.md)
[Next Working with a database](working-with-sql.md)</span></span>

::: moniker-end
