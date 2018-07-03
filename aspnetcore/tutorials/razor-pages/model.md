---
title: Aggiungere un modello a un'app Razor Pages in ASP.NET Core
author: rick-anderson
description: Scoprire come aggiungere classi per la gestione dei film in un database tramite Entity Framework Core (EF Core).
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/model
ms.openlocfilehash: 508cca07fa96c20e228d2c55c9fb101f7fc3cb02
ms.sourcegitcommit: 79b756ea03eae77a716f500ef88253ee9b1464d2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/22/2018
ms.locfileid: "36327552"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="f00b9-103">Aggiungere un modello a un'app Razor Pages in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f00b9-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

[!INCLUDE [model1](~/includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="f00b9-104">Aggiungere un modello di dati</span><span class="sxs-lookup"><span data-stu-id="f00b9-104">Add a data model</span></span>

<span data-ttu-id="f00b9-105">In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto **RazorPagesMovie** > **Aggiungi** > **Nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="f00b9-105">In Solution Explorer, right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="f00b9-106">Assegnare il nome *Modelli* alla cartella.</span><span class="sxs-lookup"><span data-stu-id="f00b9-106">Name the folder *Models*.</span></span>

<span data-ttu-id="f00b9-107">Fare clic con il pulsante destro del mouse sulla cartella *Models*.</span><span class="sxs-lookup"><span data-stu-id="f00b9-107">Right click the *Models* folder.</span></span> <span data-ttu-id="f00b9-108">Selezionare **Aggiungi** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="f00b9-108">Select **Add** > **Class**.</span></span> <span data-ttu-id="f00b9-109">Assegnare il nome **Movie** alla classe e aggiungere le proprietà seguenti:</span><span class="sxs-lookup"><span data-stu-id="f00b9-109">Name the class **Movie** and add the following properties:</span></span>

<span data-ttu-id="f00b9-110">Sostituire il contenuto della classe `Movie` con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="f00b9-110">Replace the contents of the `Movie` class with the following code:</span></span>

<span data-ttu-id="f00b9-111">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie21/Models/Movie1.cs?name=snippet)]</span><span class="sxs-lookup"><span data-stu-id="f00b9-111">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie21/Models/Movie1.cs?name=snippet)]</span></span>

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="f00b9-112">Eseguire lo scaffolding del modello di filmato</span><span class="sxs-lookup"><span data-stu-id="f00b9-112">Scaffold the movie model</span></span>

<span data-ttu-id="f00b9-113">In questa sezione viene eseguito lo scaffolding del modello di filmato.</span><span class="sxs-lookup"><span data-stu-id="f00b9-113">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="f00b9-114">Lo strumento di scaffolding crea quindi le pagine per le operazioni CRUD (creazione, lettura, aggiornamento ed eliminazione) per il modello di filmato.</span><span class="sxs-lookup"><span data-stu-id="f00b9-114">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

<span data-ttu-id="f00b9-115">Creare una cartella *Pages/Movies*:</span><span class="sxs-lookup"><span data-stu-id="f00b9-115">Create a *Pages/Movies* folder:</span></span>

* <span data-ttu-id="f00b9-116">In **Esplora soluzioni** fare clic con il pulsante destro del mouse sulla cartella *Pages* > **Aggiungi** > **Nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="f00b9-116">In **Solution Explorer**, right click on the *Pages* folder > **Add** > **New Folder**.</span></span>
* <span data-ttu-id="f00b9-117">Assegnare il nome *Movies* alla cartella</span><span class="sxs-lookup"><span data-stu-id="f00b9-117">Name the folder *Movies*</span></span>

<span data-ttu-id="f00b9-118">In **Esplora soluzioni** fare clic con il pulsante destro del mouse sulla cartella *Pages/Movies* > **Aggiungi** > **Nuovo elemento di scaffolding**.</span><span class="sxs-lookup"><span data-stu-id="f00b9-118">In **Solution Explorer**, right click on the *Pages/Movies* folder > **Add** > **New Scaffolded Item**.</span></span>

![Immagine relativa alle istruzioni precedenti.](model/_static/sca.png)

<span data-ttu-id="f00b9-120">Nella finestra di dialogo **Aggiungi scaffolding** selezionare **Pagine Razor che usano Entity Framework (CRUD)** > **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="f00b9-120">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **ADD**.</span></span>

![Immagine relativa alle istruzioni precedenti.](model/_static/add_scaffold.png)

<span data-ttu-id="f00b9-122">Completare la finestra di dialogo **Pagine Razor che usano Entity Framework (CRUD)**:</span><span class="sxs-lookup"><span data-stu-id="f00b9-122">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="f00b9-123">Nel menu a discesa **Classe modello** selezionare **Movie (RazorPagesMovie.Models)**.</span><span class="sxs-lookup"><span data-stu-id="f00b9-123">In the **Model class** drop down, select **Movie (RazorPagesMovie.Models)**.</span></span>
* <span data-ttu-id="f00b9-124">Nella riga **Classe contesto di dati** selezionare il segno più **+** e accettare il nome generato **RazorPagesMovie.Models.RazorPagesMovieContext**.</span><span class="sxs-lookup"><span data-stu-id="f00b9-124">In the **Data context class** row, select the **+** (plus) sign and accept the generated name **RazorPagesMovie.Models.RazorPagesMovieContext**.</span></span>
* <span data-ttu-id="f00b9-125">Nell'elenco a discesa **Classe contesto di dati** selezionare **RazorPagesMovie.Models.RazorPagesMovieContext**</span><span class="sxs-lookup"><span data-stu-id="f00b9-125">In the **Data context class** drop down,  select **RazorPagesMovie.Models.RazorPagesMovieContext**</span></span>
* <span data-ttu-id="f00b9-126">Selezionare **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="f00b9-126">Select **Add**.</span></span>

![Immagine relativa alle istruzioni precedenti.](model/_static/arp.png)

<a name="pmc"></a>
## <a name="perform-initial-migration"></a><span data-ttu-id="f00b9-128">Eseguire la migrazione iniziale</span><span class="sxs-lookup"><span data-stu-id="f00b9-128">Perform initial migration</span></span>

<span data-ttu-id="f00b9-129">In questa sezione viene usata la Console di Gestione pacchetti per:</span><span class="sxs-lookup"><span data-stu-id="f00b9-129">In this section, you use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="f00b9-130">Aggiungere una migrazione iniziale.</span><span class="sxs-lookup"><span data-stu-id="f00b9-130">Add an initial migration.</span></span>
* <span data-ttu-id="f00b9-131">Aggiornare il database con la migrazione iniziale.</span><span class="sxs-lookup"><span data-stu-id="f00b9-131">Update the database with the initial migration.</span></span>

<span data-ttu-id="f00b9-132">Dal menu **Strumenti** selezionare **Gestione pacchetti NuGet** >  **Console di Gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="f00b9-132">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Menu della Console di Gestione pacchetti](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="f00b9-134">Nella Console di Gestione pacchetti immettere i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="f00b9-134">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration Initial
Update-Database
```

<span data-ttu-id="f00b9-135">In alternativa, è possibile usare i comandi dell'interfaccia della riga di comando di .NET Core seguenti:</span><span class="sxs-lookup"><span data-stu-id="f00b9-135">Alternatively, the following .NET Core CLI commands can be used:</span></span>

```console
dotnet ef migrations add Initial
dotnet ef database update
```

<span data-ttu-id="f00b9-136">Ignorare il messaggio di avviso seguente che sarà risolto nell'esercitazione successiva:</span><span class="sxs-lookup"><span data-stu-id="f00b9-136">Ignore the following warning message, you fix that in the next tutorial:</span></span>

`Microsoft.EntityFrameworkCore.Model.Validation[30000]`

      *No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'.*

<span data-ttu-id="f00b9-137">Il comando `Add-Migration` genera un codice per creare lo schema del database iniziale.</span><span class="sxs-lookup"><span data-stu-id="f00b9-137">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="f00b9-138">Lo schema è basato sul modello specificato in `RazorPagesMovieContext` (nel file *Data/RazorPagesMovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="f00b9-138">The schema is based on the model specified in the `RazorPagesMovieContext` (In the *Data/RazorPagesMovieContext.cs* file).</span></span> <span data-ttu-id="f00b9-139">L'argomento `Initial` viene usato per denominare le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="f00b9-139">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="f00b9-140">È possibile usare qualunque nome, ma per convenzione si sceglie un nome che descrive la migrazione.</span><span class="sxs-lookup"><span data-stu-id="f00b9-140">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="f00b9-141">Per altre informazioni vedere [Introduzione alle migrazioni](xref:data/ef-mvc/migrations#introduction-to-migrations).</span><span class="sxs-lookup"><span data-stu-id="f00b9-141">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="f00b9-142">Il comando `Update-Database` esegue il metodo `Up` nel file *Migrations/{time-stamp}_InitialCreate.cs*, che crea il database.</span><span class="sxs-lookup"><span data-stu-id="f00b9-142">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

<span data-ttu-id="f00b9-143">Se viene visualizzato l'errore:</span><span class="sxs-lookup"><span data-stu-id="f00b9-143">If you get the error:</span></span>

<span data-ttu-id="f00b9-144">SqlException: Impossibile aprire il database "RazorPagesMovieContext-GUID" richiesto dall'account di accesso.</span><span class="sxs-lookup"><span data-stu-id="f00b9-144">SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login.</span></span> <span data-ttu-id="f00b9-145">Accesso non riuscito.</span><span class="sxs-lookup"><span data-stu-id="f00b9-145">The login failed.</span></span>
<span data-ttu-id="f00b9-146">Accesso non riuscito per l'utente "Nome-utente".</span><span class="sxs-lookup"><span data-stu-id="f00b9-146">Login failed for user 'User-name'.</span></span>

<span data-ttu-id="f00b9-147">Non è stato eseguita la [migrazione](#pmc).</span><span class="sxs-lookup"><span data-stu-id="f00b9-147">You missed the [migrations step](#pmc).</span></span>
::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!INCLUDE [model1](~/includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="f00b9-148">Aggiungere un modello di dati</span><span class="sxs-lookup"><span data-stu-id="f00b9-148">Add a data model</span></span>

<span data-ttu-id="f00b9-149">In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto **RazorPagesMovie** > **Aggiungi** > **Nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="f00b9-149">In Solution Explorer, right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="f00b9-150">Assegnare il nome *Modelli* alla cartella.</span><span class="sxs-lookup"><span data-stu-id="f00b9-150">Name the folder *Models*.</span></span>

<span data-ttu-id="f00b9-151">Fare clic con il pulsante destro del mouse sulla cartella *Models*.</span><span class="sxs-lookup"><span data-stu-id="f00b9-151">Right click the *Models* folder.</span></span> <span data-ttu-id="f00b9-152">Selezionare **Aggiungi** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="f00b9-152">Select **Add** > **Class**.</span></span> <span data-ttu-id="f00b9-153">Assegnare il nome **Movie** alla classe e aggiungere le proprietà seguenti:</span><span class="sxs-lookup"><span data-stu-id="f00b9-153">Name the class **Movie** and add the following properties:</span></span>

[!INCLUDE [model 2](~/includes/RP/model2.md)]

<a name="cs"></a>
### <a name="add-a-database-connection-string"></a><span data-ttu-id="f00b9-154">Aggiungere una stringa di connessione del database</span><span class="sxs-lookup"><span data-stu-id="f00b9-154">Add a database connection string</span></span>

<span data-ttu-id="f00b9-155">Aggiungere una stringa di connessione al file *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="f00b9-155">Add a connection string to the *appsettings.json* file.</span></span>

<span data-ttu-id="f00b9-156">[!code-json[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=8-10)]</span><span class="sxs-lookup"><span data-stu-id="f00b9-156">[!code-json[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=8-10)]</span></span>

<a name="reg"></a>
###  <a name="register-the-database-context"></a><span data-ttu-id="f00b9-157">Registrare il contesto del database</span><span class="sxs-lookup"><span data-stu-id="f00b9-157">Register the database context</span></span>

<span data-ttu-id="f00b9-158">Registrare il contesto del database con il contenitore [Inserimento dipendenze](xref:fundamentals/dependency-injection) nel [ metodo ConfigureServices della classe Startup](xref:fundamentals/startup#the-startup-class) (*Startup.cs*):</span><span class="sxs-lookup"><span data-stu-id="f00b9-158">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in the [ConfigureServices method of the Startup class](xref:fundamentals/startup#the-startup-class) (*Startup.cs*):</span></span>

<span data-ttu-id="f00b9-159">[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=3-5,7-9)]</span><span class="sxs-lookup"><span data-stu-id="f00b9-159">[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=3-5,7-9)]</span></span>

<span data-ttu-id="f00b9-160">Compilare il progetto per verificare che non ci siano errori.</span><span class="sxs-lookup"><span data-stu-id="f00b9-160">Build the project to verify you don't have any errors.</span></span>

<a name="pmc"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a><span data-ttu-id="f00b9-161">Aggiungere gli strumenti di scaffolding ed eseguire la migrazione iniziale</span><span class="sxs-lookup"><span data-stu-id="f00b9-161">Add scaffold tooling and perform initial migration</span></span>

<span data-ttu-id="f00b9-162">In questa sezione viene usata la Console di Gestione pacchetti per:</span><span class="sxs-lookup"><span data-stu-id="f00b9-162">In this section, you use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="f00b9-163">Aggiungere il pacchetto di generazione codice Web di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f00b9-163">Add the Visual Studio web code generation package.</span></span> <span data-ttu-id="f00b9-164">Questo pacchetto è necessario per eseguire il motore di scaffolding.</span><span class="sxs-lookup"><span data-stu-id="f00b9-164">This package is required to run the scaffolding engine.</span></span>
* <span data-ttu-id="f00b9-165">Aggiungere una migrazione iniziale.</span><span class="sxs-lookup"><span data-stu-id="f00b9-165">Add an initial migration.</span></span>
* <span data-ttu-id="f00b9-166">Aggiornare il database con la migrazione iniziale.</span><span class="sxs-lookup"><span data-stu-id="f00b9-166">Update the database with the initial migration.</span></span>

<span data-ttu-id="f00b9-167">Dal menu **Strumenti** selezionare **Gestione pacchetti NuGet** >  **Console di Gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="f00b9-167">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Menu della Console di Gestione pacchetti](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="f00b9-169">Nella Console di Gestione pacchetti immettere i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="f00b9-169">In the PMC, enter the following commands:</span></span>

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design -Version 2.0.3
Add-Migration Initial
Update-Database
```

<span data-ttu-id="f00b9-170">In alternativa, è possibile usare i comandi dell'interfaccia della riga di comando di .NET Core seguenti:</span><span class="sxs-lookup"><span data-stu-id="f00b9-170">Alternatively, the following .NET Core CLI commands can be used:</span></span>

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet ef migrations add Initial
dotnet ef database update
```

<span data-ttu-id="f00b9-171">Ignorare il messaggio seguente:</span><span class="sxs-lookup"><span data-stu-id="f00b9-171">Ignore the following message:</span></span>

    `Microsoft.EntityFrameworkCore.Model.Validation[30000]`

      *No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'*

<span data-ttu-id="f00b9-172">Sarà risolto nell'esercitazione successiva.</span><span class="sxs-lookup"><span data-stu-id="f00b9-172">You fix that in the next tutorial.</span></span>

<span data-ttu-id="f00b9-173">Il comando `Install-Package` installa gli strumenti necessari a eseguire il motore di scaffolding.</span><span class="sxs-lookup"><span data-stu-id="f00b9-173">The `Install-Package` command installs the tooling required to run the scaffolding engine.</span></span>

<span data-ttu-id="f00b9-174">Il comando `Add-Migration` genera un codice per creare lo schema del database iniziale.</span><span class="sxs-lookup"><span data-stu-id="f00b9-174">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="f00b9-175">Lo schema è basato sul modello specificato in `DbContext` (nel file *Models/MovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="f00b9-175">The schema is based on the model specified in the `DbContext` (In the *Models/MovieContext.cs* file).</span></span> <span data-ttu-id="f00b9-176">L'argomento `Initial` viene usato per denominare le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="f00b9-176">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="f00b9-177">È possibile usare qualunque nome, ma per convenzione si sceglie un nome che descrive la migrazione.</span><span class="sxs-lookup"><span data-stu-id="f00b9-177">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="f00b9-178">Per altre informazioni vedere [Introduzione alle migrazioni](xref:data/ef-mvc/migrations#introduction-to-migrations).</span><span class="sxs-lookup"><span data-stu-id="f00b9-178">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="f00b9-179">Il comando `Update-Database` esegue il metodo `Up` nel file *Migrations/{time-stamp}_InitialCreate.cs*, che crea il database.</span><span class="sxs-lookup"><span data-stu-id="f00b9-179">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

[!INCLUDE [model 4windows](~/includes/RP/model4Win.md)]

[!INCLUDE [model 4](~/includes/RP/model4tbl.md)]

::: moniker-end

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="f00b9-180">Eseguire il test dell'app</span><span class="sxs-lookup"><span data-stu-id="f00b9-180">Test the app</span></span>

* <span data-ttu-id="f00b9-181">Eseguire l'app e accodare `/Movies` all'URL nel browser (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="f00b9-181">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>
* <span data-ttu-id="f00b9-182">Eseguire il test del collegamento **Crea**.</span><span class="sxs-lookup"><span data-stu-id="f00b9-182">Test the **Create** link.</span></span>

  ![Pagina Crea](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* <span data-ttu-id="f00b9-184">Eseguire il test dei collegamenti **Modifica**, **Dettagli** e **Elimina**.</span><span class="sxs-lookup"><span data-stu-id="f00b9-184">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="f00b9-185">Se viene visualizzata un'eccezione SQL, verificare di avere eseguito le migrazioni e di avere aggiornato il database.</span><span class="sxs-lookup"><span data-stu-id="f00b9-185">If you get a SQL exception, verify you have run migrations and updated the database.</span></span>

<span data-ttu-id="f00b9-186">L'esercitazione successiva illustra i file creati tramite scaffolding.</span><span class="sxs-lookup"><span data-stu-id="f00b9-186">The next tutorial explains the files created by scaffolding.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f00b9-187">[Precedente: Introduzione](xref:tutorials/razor-pages/razor-pages-start)
> [Successivo: Pagine Razor create tramite scaffolding](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="f00b9-187">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>    
