---
title: Aggiungere un modello a un'app Razor Pages in ASP.NET Core
author: rick-anderson
description: Scoprire come aggiungere classi per la gestione dei film in un database tramite Entity Framework Core (EF Core).
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/model
ms.openlocfilehash: c4b23f75da298e4ee804f649219c2ce466b6d6ea
ms.sourcegitcommit: 710fc5fcac258cc8415976dc66bdb355b3e061d5
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/26/2018
ms.locfileid: "52299443"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="9c57d-103">Aggiungere un modello a un'app Razor Pages in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9c57d-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

[!INCLUDE [model1](~/includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="9c57d-104">Aggiungere un modello di dati</span><span class="sxs-lookup"><span data-stu-id="9c57d-104">Add a data model</span></span>

<span data-ttu-id="9c57d-105">In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto **RazorPagesMovie** > **Aggiungi** > **Nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="9c57d-105">In Solution Explorer, right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="9c57d-106">Assegnare il nome *Modelli* alla cartella.</span><span class="sxs-lookup"><span data-stu-id="9c57d-106">Name the folder *Models*.</span></span>

<span data-ttu-id="9c57d-107">Fare clic con il pulsante destro del mouse sulla cartella *Models*.</span><span class="sxs-lookup"><span data-stu-id="9c57d-107">Right click the *Models* folder.</span></span> <span data-ttu-id="9c57d-108">Selezionare **Aggiungi** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="9c57d-108">Select **Add** > **Class**.</span></span> <span data-ttu-id="9c57d-109">Assegnare un nome alla classe **Movie** e sostituire il contenuto della classe `Movie` con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="9c57d-109">Name the class **Movie** and replace the contents of the `Movie` class with the following code:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie21/Models/Movie1.cs?name=snippet)]

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="9c57d-110">Eseguire lo scaffolding del modello di filmato</span><span class="sxs-lookup"><span data-stu-id="9c57d-110">Scaffold the movie model</span></span>

<span data-ttu-id="9c57d-111">In questa sezione viene eseguito lo scaffolding del modello di filmato.</span><span class="sxs-lookup"><span data-stu-id="9c57d-111">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="9c57d-112">Lo strumento di scaffolding crea quindi le pagine per le operazioni CRUD (creazione, lettura, aggiornamento ed eliminazione) per il modello di filmato.</span><span class="sxs-lookup"><span data-stu-id="9c57d-112">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

<span data-ttu-id="9c57d-113">Creare una cartella *Pages/Movies*:</span><span class="sxs-lookup"><span data-stu-id="9c57d-113">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="9c57d-114">In **Esplora soluzioni** fare clic con il pulsante destro del mouse sulla cartella *Pages* > **Aggiungi** > **Nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="9c57d-114">In **Solution Explorer**, right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="9c57d-115">Assegnare il nome *Movies* alla cartella</span><span class="sxs-lookup"><span data-stu-id="9c57d-115">Name the folder *Movies*</span></span>

<span data-ttu-id="9c57d-116">In **Esplora soluzioni** fare clic con il pulsante destro del mouse sulla cartella *Pages/Movies* > **Aggiungi** > **Nuovo elemento di scaffolding**.</span><span class="sxs-lookup"><span data-stu-id="9c57d-116">In **Solution Explorer**, right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Immagine relativa alle istruzioni precedenti.](model/_static/sca.png)

<span data-ttu-id="9c57d-118">Nella finestra di dialogo **Aggiungi scaffolding** selezionare **Razor che usano Entity Framework (CRUD)** > **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="9c57d-118">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **Add**.</span></span>

![Immagine relativa alle istruzioni precedenti.](model/_static/add_scaffold.png)

<span data-ttu-id="9c57d-120">Completare la finestra di dialogo **Pagine Razor che usano Entity Framework (CRUD)**:</span><span class="sxs-lookup"><span data-stu-id="9c57d-120">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="9c57d-121">Nel menu a discesa **Classe modello** selezionare **Movie (RazorPagesMovie.Models)**.</span><span class="sxs-lookup"><span data-stu-id="9c57d-121">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="9c57d-122">Nella riga **Classe contesto di dati** selezionare il segno più **+** e accettare il nome generato **RazorPagesMovie.Models.RazorPagesMovieContext**.</span><span class="sxs-lookup"><span data-stu-id="9c57d-122">In the **Data context class** row, select the **+** (plus) sign and accept the generated name **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="9c57d-123">Selezionare **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="9c57d-123">Select **Add**.</span></span>

![Immagine relativa alle istruzioni precedenti.](model/_static/arp.png)

<span data-ttu-id="9c57d-125">Il processo di scaffolding crea e aggiorna i file seguenti:</span><span class="sxs-lookup"><span data-stu-id="9c57d-125">The scaffold process creates and updates the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="9c57d-126">File creati</span><span class="sxs-lookup"><span data-stu-id="9c57d-126">Files created</span></span>

* <span data-ttu-id="9c57d-127">*Pages/Movies*: pagine Create, Delete, Details, Edit, Index ( pagine di creazione, eliminazione, dettagli, modifica, indice).</span><span class="sxs-lookup"><span data-stu-id="9c57d-127">*Pages/Movies*: Create, Delete, Details, Edit, Index.</span></span> <span data-ttu-id="9c57d-128">Queste pagine vengono descritte in dettaglio nell'esercitazione successiva.</span><span class="sxs-lookup"><span data-stu-id="9c57d-128">These pages are detailed in the next tutorial.</span></span>
* <span data-ttu-id="9c57d-129">*Data/RazorPagesMovieContext.cs*</span><span class="sxs-lookup"><span data-stu-id="9c57d-129">*Data/RazorPagesMovieContext.cs*</span></span>

### <a name="file-updated"></a><span data-ttu-id="9c57d-130">File aggiornato</span><span class="sxs-lookup"><span data-stu-id="9c57d-130">File updated</span></span>

* <span data-ttu-id="9c57d-131">*Startup.cs*: le modifiche a questo file sono descritte in dettaglio nella sezione successiva.</span><span class="sxs-lookup"><span data-stu-id="9c57d-131">*Startup.cs*: Changes to this file are detailed in the next section.</span></span>
* <span data-ttu-id="9c57d-132">*appsettings.json*: è stata aggiunta la stringa di connessione usata per connettersi a un database locale.</span><span class="sxs-lookup"><span data-stu-id="9c57d-132">*appsettings.json*: The connection string used to connect to a local database is added.</span></span>

## <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="9c57d-133">Esaminare il contesto registrato con l'inserimento di dipendenze</span><span class="sxs-lookup"><span data-stu-id="9c57d-133">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="9c57d-134">ASP.NET Core viene compilato con l'[inserimento di dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="9c57d-134">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="9c57d-135">I servizi, ad esempio il contesto di database di Entity Framework Core, vengono registrati con l'inserimento delle dipendenze durante l'avvio dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="9c57d-135">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="9c57d-136">Questi servizi vengono quindi offerti ai componenti per cui sono necessari (ad esempio Razor Pages) tramite i parametri del costruttore.</span><span class="sxs-lookup"><span data-stu-id="9c57d-136">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="9c57d-137">Più avanti nell'esercitazione viene illustrato il codice del costruttore che ottiene un'istanza del contesto di database.</span><span class="sxs-lookup"><span data-stu-id="9c57d-137">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="9c57d-138">Lo strumento di scaffolding ha creato automaticamente un contesto del database e lo ha registrato con il contenitore di inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="9c57d-138">The scaffolding tool automatically created a DB context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="9c57d-139">Esaminare il metodo `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="9c57d-139">Examine the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="9c57d-140">La riga evidenziata è stata aggiunta dallo scaffolder:</span><span class="sxs-lookup"><span data-stu-id="9c57d-140">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Startup.cs?name=snippet_ConfigureServices&highlight=12-13)]

<span data-ttu-id="9c57d-141">La classe principale che coordina le funzionalità di Entity Framework Core per un determinato modello di dati è la classe del contesto di database.</span><span class="sxs-lookup"><span data-stu-id="9c57d-141">The main class that coordinates EF Core functionality for a given data model is the DB context class.</span></span> <span data-ttu-id="9c57d-142">Il contesto dei dati è derivato da [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span><span class="sxs-lookup"><span data-stu-id="9c57d-142">The data context is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="9c57d-143">Il contesto dei dati specifica le entità incluse nel modello di dati.</span><span class="sxs-lookup"><span data-stu-id="9c57d-143">The data context specifies which entities are included in the data model.</span></span> <span data-ttu-id="9c57d-144">In questo progetto la classe è denominata `RazorPagesMovieContext`.</span><span class="sxs-lookup"><span data-stu-id="9c57d-144">In this project, the class is named `RazorPagesMovieContext`.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="9c57d-145">Il codice precedente crea una proprietà [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) per il set di entità.</span><span class="sxs-lookup"><span data-stu-id="9c57d-145">The preceding code creates a [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="9c57d-146">Nella terminologia di Entity Framework, un set di entità corrisponde in genere alla tabella di un database.</span><span class="sxs-lookup"><span data-stu-id="9c57d-146">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="9c57d-147">Un'entità corrisponde a una riga nella tabella.</span><span class="sxs-lookup"><span data-stu-id="9c57d-147">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="9c57d-148">Il nome della stringa di connessione viene passato al contesto chiamando un metodo in un oggetto [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions).</span><span class="sxs-lookup"><span data-stu-id="9c57d-148">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="9c57d-149">Per lo sviluppo locale, il [sistema di configurazione di ASP.NET Core](xref:fundamentals/configuration/index) legge la stringa di connessione dal file *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="9c57d-149">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

<a name="pmc"></a>
## <a name="perform-initial-migration"></a><span data-ttu-id="9c57d-150">Eseguire la migrazione iniziale</span><span class="sxs-lookup"><span data-stu-id="9c57d-150">Perform initial migration</span></span>

<span data-ttu-id="9c57d-151">In questa sezione viene usata la Console di Gestione pacchetti per:</span><span class="sxs-lookup"><span data-stu-id="9c57d-151">In this section, you use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="9c57d-152">Aggiungere una migrazione iniziale.</span><span class="sxs-lookup"><span data-stu-id="9c57d-152">Add an initial migration.</span></span>
* <span data-ttu-id="9c57d-153">Aggiornare il database con la migrazione iniziale.</span><span class="sxs-lookup"><span data-stu-id="9c57d-153">Update the database with the initial migration.</span></span>

<span data-ttu-id="9c57d-154">Dal menu **Strumenti** selezionare **Gestione pacchetti NuGet** >  **Console di Gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="9c57d-154">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Menu della Console di Gestione pacchetti](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="9c57d-156">Nella Console di Gestione pacchetti immettere i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="9c57d-156">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration Initial
Update-Database
```

<span data-ttu-id="9c57d-157">In alternativa, è possibile usare i comandi dell'interfaccia della riga di comando di .NET Core seguenti dalla cartella del progetto:</span><span class="sxs-lookup"><span data-stu-id="9c57d-157">Alternatively, the following .NET Core CLI commands can be used from the project folder:</span></span>

```console
dotnet ef migrations add Initial
dotnet ef database update
```

<span data-ttu-id="9c57d-158">Ignorare il messaggio di avviso seguente che verrà risolto in un'esercitazione successiva:</span><span class="sxs-lookup"><span data-stu-id="9c57d-158">Ignore the following warning message, which you fix in a later tutorial:</span></span>

```console
Microsoft.EntityFrameworkCore.Model.Validation[30000]
      No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'.
```

<span data-ttu-id="9c57d-159">Il comando `Add-Migration` genera un codice per creare lo schema del database iniziale.</span><span class="sxs-lookup"><span data-stu-id="9c57d-159">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="9c57d-160">Lo schema è basato sul modello specificato in `RazorPagesMovieContext` (nel file *Data/RazorPagesMovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="9c57d-160">The schema is based on the model specified in the `RazorPagesMovieContext` (In the *Data/RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="9c57d-161">L'argomento `Initial` viene usato per denominare le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="9c57d-161">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="9c57d-162">È possibile usare qualunque nome, ma per convenzione si sceglie un nome che descrive la migrazione.</span><span class="sxs-lookup"><span data-stu-id="9c57d-162">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="9c57d-163">Per altre informazioni vedere [Introduzione alle migrazioni](xref:data/ef-mvc/migrations#introduction-to-migrations).</span><span class="sxs-lookup"><span data-stu-id="9c57d-163">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="9c57d-164">Il comando `Update-Database` esegue il metodo `Up` nel file *Migrations/{time-stamp}_InitialCreate.cs*, che crea il database.</span><span class="sxs-lookup"><span data-stu-id="9c57d-164">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

<span data-ttu-id="9c57d-165">Se viene visualizzato l'errore:</span><span class="sxs-lookup"><span data-stu-id="9c57d-165">If you get the error:</span></span>

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="9c57d-166">Non è stato eseguita la [migrazione](#pmc).</span><span class="sxs-lookup"><span data-stu-id="9c57d-166">You missed the [migrations step](#pmc).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!INCLUDE [model1](~/includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="9c57d-167">Aggiungere un modello di dati</span><span class="sxs-lookup"><span data-stu-id="9c57d-167">Add a data model</span></span>

<span data-ttu-id="9c57d-168">In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto **RazorPagesMovie** > **Aggiungi** > **Nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="9c57d-168">In Solution Explorer, right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="9c57d-169">Assegnare il nome *Modelli* alla cartella.</span><span class="sxs-lookup"><span data-stu-id="9c57d-169">Name the folder *Models*.</span></span>

<span data-ttu-id="9c57d-170">Fare clic con il pulsante destro del mouse sulla cartella *Models*.</span><span class="sxs-lookup"><span data-stu-id="9c57d-170">Right click the *Models* folder.</span></span> <span data-ttu-id="9c57d-171">Selezionare **Aggiungi** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="9c57d-171">Select **Add** > **Class**.</span></span> <span data-ttu-id="9c57d-172">Assegnare il nome **Movie** alla classe e aggiungere le proprietà seguenti:</span><span class="sxs-lookup"><span data-stu-id="9c57d-172">Name the class **Movie** and add the following properties:</span></span>

[!INCLUDE [model 2](~/includes/RP/model2.md)]

<a name="cs"></a>
### <a name="add-a-database-connection-string"></a><span data-ttu-id="9c57d-173">Aggiungere una stringa di connessione del database</span><span class="sxs-lookup"><span data-stu-id="9c57d-173">Add a database connection string</span></span>

<span data-ttu-id="9c57d-174">Aggiungere una stringa di connessione al file *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="9c57d-174">Add a connection string to the *appsettings.json* file.</span></span>

[!code-json[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=8-10)]

<a name="reg"></a>
###  <a name="register-the-database-context"></a><span data-ttu-id="9c57d-175">Registrare il contesto del database</span><span class="sxs-lookup"><span data-stu-id="9c57d-175">Register the database context</span></span>

<span data-ttu-id="9c57d-176">Registrare il contesto del database con il contenitore [Inserimento dipendenze](xref:fundamentals/dependency-injection) nel [ metodo ConfigureServices della classe Startup](xref:fundamentals/startup#the-startup-class) (*Startup.cs*):</span><span class="sxs-lookup"><span data-stu-id="9c57d-176">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in the [ConfigureServices method of the Startup class](xref:fundamentals/startup#the-startup-class) (*Startup.cs*):</span></span>

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=3-5,7-9)]

<span data-ttu-id="9c57d-177">Compilare il progetto per verificare che non ci siano errori.</span><span class="sxs-lookup"><span data-stu-id="9c57d-177">Build the project to verify you don't have any errors.</span></span>

<a name="pmc"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a><span data-ttu-id="9c57d-178">Aggiungere gli strumenti di scaffolding ed eseguire la migrazione iniziale</span><span class="sxs-lookup"><span data-stu-id="9c57d-178">Add scaffold tooling and perform initial migration</span></span>

<span data-ttu-id="9c57d-179">In questa sezione viene usata la Console di Gestione pacchetti per:</span><span class="sxs-lookup"><span data-stu-id="9c57d-179">In this section, you use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="9c57d-180">Aggiungere il pacchetto di generazione codice Web di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9c57d-180">Add the Visual Studio web code generation package.</span></span> <span data-ttu-id="9c57d-181">Questo pacchetto è necessario per eseguire il motore di scaffolding.</span><span class="sxs-lookup"><span data-stu-id="9c57d-181">This package is required to run the scaffolding engine.</span></span>
* <span data-ttu-id="9c57d-182">Aggiungere una migrazione iniziale.</span><span class="sxs-lookup"><span data-stu-id="9c57d-182">Add an initial migration.</span></span>
* <span data-ttu-id="9c57d-183">Aggiornare il database con la migrazione iniziale.</span><span class="sxs-lookup"><span data-stu-id="9c57d-183">Update the database with the initial migration.</span></span>

<span data-ttu-id="9c57d-184">Dal menu **Strumenti** selezionare **Gestione pacchetti NuGet** >  **Console di Gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="9c57d-184">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Menu della Console di Gestione pacchetti](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="9c57d-186">Nella Console di Gestione pacchetti immettere i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="9c57d-186">In the PMC, enter the following commands:</span></span>

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design -Version 2.0.3
Add-Migration Initial
Update-Database
```

<span data-ttu-id="9c57d-187">In alternativa, è possibile usare i comandi dell'interfaccia della riga di comando di .NET Core seguenti:</span><span class="sxs-lookup"><span data-stu-id="9c57d-187">Alternatively, the following .NET Core CLI commands can be used:</span></span>

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet ef migrations add Initial
dotnet ef database update
```

<span data-ttu-id="9c57d-188">Ignorare il messaggio seguente:</span><span class="sxs-lookup"><span data-stu-id="9c57d-188">Ignore the following message:</span></span>

```console
Microsoft.EntityFrameworkCore.Model.Validation[30000]
      No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'
```

<span data-ttu-id="9c57d-189">Sarà risolto nell'esercitazione successiva.</span><span class="sxs-lookup"><span data-stu-id="9c57d-189">You fix that in the next tutorial.</span></span>

<span data-ttu-id="9c57d-190">Il comando `Install-Package` installa gli strumenti necessari a eseguire il motore di scaffolding.</span><span class="sxs-lookup"><span data-stu-id="9c57d-190">The `Install-Package` command installs the tooling required to run the scaffolding engine.</span></span>

<span data-ttu-id="9c57d-191">Il comando `Add-Migration` genera un codice per creare lo schema del database iniziale.</span><span class="sxs-lookup"><span data-stu-id="9c57d-191">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="9c57d-192">Lo schema è basato sul modello specificato in `DbContext` (nel file *Models/MovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="9c57d-192">The schema is based on the model specified in the `DbContext` (In the *Models/MovieContext.cs* file).</span></span> <span data-ttu-id="9c57d-193">L'argomento `Initial` viene usato per denominare le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="9c57d-193">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="9c57d-194">È possibile usare qualunque nome, ma per convenzione si sceglie un nome che descrive la migrazione.</span><span class="sxs-lookup"><span data-stu-id="9c57d-194">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="9c57d-195">Per altre informazioni vedere [Introduzione alle migrazioni](xref:data/ef-mvc/migrations#introduction-to-migrations).</span><span class="sxs-lookup"><span data-stu-id="9c57d-195">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="9c57d-196">Il comando `Update-Database` esegue il metodo `Up` nel file *Migrations/{time-stamp}_InitialCreate.cs*, che crea il database.</span><span class="sxs-lookup"><span data-stu-id="9c57d-196">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

[!INCLUDE [model 4windows](~/includes/RP/model4Win.md)]

[!INCLUDE [model 4](~/includes/RP/model4tbl.md)]

::: moniker-end

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="9c57d-197">Eseguire il test dell'app</span><span class="sxs-lookup"><span data-stu-id="9c57d-197">Test the app</span></span>

* <span data-ttu-id="9c57d-198">Eseguire l'app e accodare `/Movies` all'URL nel browser (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="9c57d-198">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>
* <span data-ttu-id="9c57d-199">Eseguire il test del collegamento **Crea**.</span><span class="sxs-lookup"><span data-stu-id="9c57d-199">Test the **Create** link.</span></span>

  ![Pagina Crea](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* <span data-ttu-id="9c57d-201">Eseguire il test dei collegamenti **Modifica**, **Dettagli** e **Elimina**.</span><span class="sxs-lookup"><span data-stu-id="9c57d-201">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="9c57d-202">Se viene visualizzata un'eccezione SQL, verificare di avere eseguito le migrazioni e di avere aggiornato il database.</span><span class="sxs-lookup"><span data-stu-id="9c57d-202">If you get a SQL exception, verify you have run migrations and updated the database.</span></span>

<span data-ttu-id="9c57d-203">L'esercitazione successiva illustra i file creati tramite scaffolding.</span><span class="sxs-lookup"><span data-stu-id="9c57d-203">The next tutorial explains the files created by scaffolding.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="9c57d-204">[Precedente: Introduzione](xref:tutorials/razor-pages/razor-pages-start)
> [Successivo: Pagine Razor create tramite scaffolding](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="9c57d-204">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>
