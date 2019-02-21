---
title: Aggiungere un nuovo campo a una pagina Razor in ASP.NET Core
author: rick-anderson
description: Illustra come aggiungere un nuovo campo a una pagina Razor con Entity Framework Core
ms.author: riande
ms.custom: mvc
ms.date: 12/5/2018
uid: tutorials/razor-pages/new-field
ms.openlocfilehash: 98de39c63c992dce7d60563df316d848339b811a
ms.sourcegitcommit: d75d8eb26c2cce19876c8d5b65ac8a4b21f625ef
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/19/2019
ms.locfileid: "56410375"
---
# <a name="add-a-new-field-to-a-razor-page-in-aspnet-core"></a><span data-ttu-id="cfae8-103">Aggiungere un nuovo campo a una pagina Razor in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="cfae8-103">Add a new field to a Razor Page in ASP.NET Core</span></span>

<span data-ttu-id="cfae8-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="cfae8-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

<span data-ttu-id="cfae8-105">In questa sezione viene usato Migrazioni Code First di [Entity Framework](/ef/core/get-started/aspnetcore/new-db) per:</span><span class="sxs-lookup"><span data-stu-id="cfae8-105">In this section [Entity Framework](/ef/core/get-started/aspnetcore/new-db) Code First Migrations is used to:</span></span>

* <span data-ttu-id="cfae8-106">Aggiungere un nuovo campo al modello.</span><span class="sxs-lookup"><span data-stu-id="cfae8-106">Add a new field to the model.</span></span>
* <span data-ttu-id="cfae8-107">Eseguire la migrazione nel database della modifica al nuovo schema del campo.</span><span class="sxs-lookup"><span data-stu-id="cfae8-107">Migrate the new field schema change to the database.</span></span>

<span data-ttu-id="cfae8-108">Quando si usa Code First di Entity Framework per creare automaticamente un database, Code First:</span><span class="sxs-lookup"><span data-stu-id="cfae8-108">When using EF Code First to automatically create a database, Code First:</span></span>

* <span data-ttu-id="cfae8-109">Aggiunge una tabella al database per rilevare se lo schema del database è sincronizzato con le classi di modelli da cui è stato generato.</span><span class="sxs-lookup"><span data-stu-id="cfae8-109">Adds a table to the database to track whether the schema of the database is in sync with the model classes it was generated from.</span></span>
* <span data-ttu-id="cfae8-110">Se le classi di modelli non sono sincronizzate con il database, Entity Framework genera un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="cfae8-110">If the model classes aren't in sync with the DB, EF throws an exception.</span></span>

<span data-ttu-id="cfae8-111">La verifica automatica del modello o schema sincronizzato rende più semplice individuare i problemi di codice o database incoerente.</span><span class="sxs-lookup"><span data-stu-id="cfae8-111">Automatic verification of schema/model in sync makes it easier to find inconsistent database/code issues.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="cfae8-112">Aggiunta di una proprietà Rating al modello Movie</span><span class="sxs-lookup"><span data-stu-id="cfae8-112">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="cfae8-113">Aprire il file *Models/Movie.cs* e aggiungere una proprietà `Rating`:</span><span class="sxs-lookup"><span data-stu-id="cfae8-113">Open the *Models/Movie.cs* file and add a `Rating` property:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Models/MovieDateRating.cs?highlight=13&name=snippet)]

<span data-ttu-id="cfae8-114">Compilare l'app.</span><span class="sxs-lookup"><span data-stu-id="cfae8-114">Build the app.</span></span>

<span data-ttu-id="cfae8-115">Modificare *Pages/Movies/Index.cshtml* e aggiungere un campo `Rating`:</span><span class="sxs-lookup"><span data-stu-id="cfae8-115">Edit *Pages/Movies/Index.cshtml*, and add a `Rating` field:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/IndexRating.cshtml.?highlight=40-42,61-63)]

<span data-ttu-id="cfae8-116">Aggiornare le pagine seguenti:</span><span class="sxs-lookup"><span data-stu-id="cfae8-116">Update the following pages:</span></span>

* <span data-ttu-id="cfae8-117">Aggiungere il campo `Rating` alle pagine Delete (Elimina) e Details (Dettagli).</span><span class="sxs-lookup"><span data-stu-id="cfae8-117">Add the `Rating` field to the Delete and Details pages.</span></span>
* <span data-ttu-id="cfae8-118">Aggiornare [Create.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Create.cshtml) con un campo `Rating`.</span><span class="sxs-lookup"><span data-stu-id="cfae8-118">Update [Create.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Create.cshtml) with a `Rating` field.</span></span>
* <span data-ttu-id="cfae8-119">Aggiungere il campo `Rating` alla pagina Edit (Modifica).</span><span class="sxs-lookup"><span data-stu-id="cfae8-119">Add the `Rating` field to the Edit Page.</span></span>

<span data-ttu-id="cfae8-120">L'app non funzionerà finché non si aggiorna il database in modo da includere il nuovo campo.</span><span class="sxs-lookup"><span data-stu-id="cfae8-120">The app won't work until the DB is updated to include the new field.</span></span> <span data-ttu-id="cfae8-121">Se si esegue l'app ora, verrà visualizzato un errore `SqlException`:</span><span class="sxs-lookup"><span data-stu-id="cfae8-121">If run now, the app throws a `SqlException`:</span></span>

`SqlException: Invalid column name 'Rating'.`

<span data-ttu-id="cfae8-122">Questo errore viene visualizzato perché la classe del modello Movie aggiornata è diversa rispetto allo schema della tabella Movie nel database.</span><span class="sxs-lookup"><span data-stu-id="cfae8-122">This error is caused by the updated Movie model class being different than the schema of the Movie table of the database.</span></span> <span data-ttu-id="cfae8-123">Nella tabella del database non è presente una colonna `Rating`.</span><span class="sxs-lookup"><span data-stu-id="cfae8-123">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="cfae8-124">Per correggere questo errore, esistono alcuni approcci:</span><span class="sxs-lookup"><span data-stu-id="cfae8-124">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="cfae8-125">Fare in modo che Entity Framework elimini e crei di nuovo automaticamente il database usando il nuovo schema di classi del modello.</span><span class="sxs-lookup"><span data-stu-id="cfae8-125">Have the Entity Framework automatically drop and re-create the database using the new model class schema.</span></span> <span data-ttu-id="cfae8-126">Questo approccio è utile nelle prime fasi del ciclo di sviluppo e consente di migliorare rapidamente lo schema del modello e il database insieme.</span><span class="sxs-lookup"><span data-stu-id="cfae8-126">This approach is convenient early in the development cycle; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="cfae8-127">Lo svantaggio è che si perdono i dati esistenti nel database.</span><span class="sxs-lookup"><span data-stu-id="cfae8-127">The downside is that you lose existing data in the database.</span></span> <span data-ttu-id="cfae8-128">Non usare questo approccio in un database di produzione.</span><span class="sxs-lookup"><span data-stu-id="cfae8-128">Don't use this approach on a production database!</span></span> <span data-ttu-id="cfae8-129">L'eliminazione del database per la modifica dello schema e l'uso di un inizializzatore per inizializzare automaticamente un database con i dati di test è spesso un modo produttivo per sviluppare un'app.</span><span class="sxs-lookup"><span data-stu-id="cfae8-129">Dropping the DB on schema changes and using an initializer to automatically seed the database with test data is often a productive way to develop an app.</span></span>

2. <span data-ttu-id="cfae8-130">Modificare esplicitamente lo schema del database esistente in modo che corrisponda alle classi del modello.</span><span class="sxs-lookup"><span data-stu-id="cfae8-130">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="cfae8-131">Il vantaggio di questo approccio è che i dati vengono mantenuti.</span><span class="sxs-lookup"><span data-stu-id="cfae8-131">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="cfae8-132">È possibile apportare questa modifica manualmente o creando uno script di modifica del database.</span><span class="sxs-lookup"><span data-stu-id="cfae8-132">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="cfae8-133">Usare Migrazioni Code First per aggiornare lo schema del database.</span><span class="sxs-lookup"><span data-stu-id="cfae8-133">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="cfae8-134">Per questa esercitazione usare Migrazioni Code First.</span><span class="sxs-lookup"><span data-stu-id="cfae8-134">For this tutorial, use Code First Migrations.</span></span>

<span data-ttu-id="cfae8-135">Aggiornare la classe `SeedData` in modo che fornisca un valore per la nuova colonna.</span><span class="sxs-lookup"><span data-stu-id="cfae8-135">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="cfae8-136">Di seguito viene illustrata una modifica di esempio, ma si apporterà questa modifica per ogni blocco `new Movie`.</span><span class="sxs-lookup"><span data-stu-id="cfae8-136">A sample change is shown below, but you'll want to make this change for each `new Movie` block.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Models/SeedDataRating.cs?name=snippet1&highlight=8)]

<span data-ttu-id="cfae8-137">Vedere il [file SeedData.cs completato](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Models/SeedDataRating.cs).</span><span class="sxs-lookup"><span data-stu-id="cfae8-137">See the [completed SeedData.cs file](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Models/SeedDataRating.cs).</span></span>

<span data-ttu-id="cfae8-138">Compilare la soluzione.</span><span class="sxs-lookup"><span data-stu-id="cfae8-138">Build the solution.</span></span>

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="cfae8-139">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cfae8-139">Visual Studio</span></span>](#tab/visual-studio)

<a name="pmc"></a>

### <a name="add-a-migration-for-the-rating-field"></a><span data-ttu-id="cfae8-140">Aggiungere una migrazione per il campo Rating</span><span class="sxs-lookup"><span data-stu-id="cfae8-140">Add a migration for the rating field</span></span>

<span data-ttu-id="cfae8-141">Dal menu **Strumenti** selezionare **Gestione pacchetti NuGet > Console di Gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="cfae8-141">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>
<span data-ttu-id="cfae8-142">Nella Console di Gestione pacchetti immettere i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="cfae8-142">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration Rating
Update-Database
```

<span data-ttu-id="cfae8-143">Il comando `Add-Migration` indica al framework di:</span><span class="sxs-lookup"><span data-stu-id="cfae8-143">The `Add-Migration` command tells the framework to:</span></span>

* <span data-ttu-id="cfae8-144">Confrontare il modello `Movie` con lo schema di database `Movie`.</span><span class="sxs-lookup"><span data-stu-id="cfae8-144">Compare the `Movie` model with the `Movie` DB schema.</span></span>
* <span data-ttu-id="cfae8-145">Creare codice per migrare lo schema del database nel nuovo modello.</span><span class="sxs-lookup"><span data-stu-id="cfae8-145">Create code to migrate the DB schema to the new model.</span></span>

<span data-ttu-id="cfae8-146">Il nome "Rating" è arbitrario e viene usato per denominare il file di migrazione.</span><span class="sxs-lookup"><span data-stu-id="cfae8-146">The name "Rating" is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="cfae8-147">È consigliabile usare un nome significativo per il file di migrazione.</span><span class="sxs-lookup"><span data-stu-id="cfae8-147">It's helpful to use a meaningful name for the migration file.</span></span>

<span data-ttu-id="cfae8-148">Il comando `Update-Database` indica al framework di applicare le modifiche dello schema al database.</span><span class="sxs-lookup"><span data-stu-id="cfae8-148">The `Update-Database` command tells the framework to apply the schema changes to the database.</span></span>

<a name="ssox"></a>

<span data-ttu-id="cfae8-149">Se si eliminano tutti i record nel database, il database viene inizializzato e viene incluso il campo `Rating`.</span><span class="sxs-lookup"><span data-stu-id="cfae8-149">If you delete all the records in the DB, the initializer will seed the DB and include the `Rating` field.</span></span> <span data-ttu-id="cfae8-150">È possibile eseguire questa operazione con i collegamenti di eliminazione nel browser o da [Sql Server Object Explorer](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span><span class="sxs-lookup"><span data-stu-id="cfae8-150">You can do this with the delete links in the browser or from [Sql Server Object Explorer](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span></span>

<span data-ttu-id="cfae8-151">Un'altra opzione è quella di eliminare il database e usare le migrazioni per ricreare il database.</span><span class="sxs-lookup"><span data-stu-id="cfae8-151">Another option is to delete the database and use migrations to re-create the database.</span></span> <span data-ttu-id="cfae8-152">Per eliminare il database da SSOX:</span><span class="sxs-lookup"><span data-stu-id="cfae8-152">To delete the database in SSOX:</span></span>

* <span data-ttu-id="cfae8-153">Selezionare il database in SSOX.</span><span class="sxs-lookup"><span data-stu-id="cfae8-153">Select the database in SSOX.</span></span>
* <span data-ttu-id="cfae8-154">Fare clic con il pulsante destro del mouse sul database e selezionare *Elimina*.</span><span class="sxs-lookup"><span data-stu-id="cfae8-154">Right click on the database, and select *Delete*.</span></span>
* <span data-ttu-id="cfae8-155">Selezionare **Chiudi connessioni esistenti**.</span><span class="sxs-lookup"><span data-stu-id="cfae8-155">Check **Close existing connections**.</span></span>
* <span data-ttu-id="cfae8-156">Scegliere **OK**.</span><span class="sxs-lookup"><span data-stu-id="cfae8-156">Select **OK**.</span></span>
* <span data-ttu-id="cfae8-157">Nella [Console di Gestione pacchetti](xref:tutorials/razor-pages/new-field#pmc) aggiornare il database:</span><span class="sxs-lookup"><span data-stu-id="cfae8-157">In the [PMC](xref:tutorials/razor-pages/new-field#pmc), update the database:</span></span>

  ```powershell
  Update-Database
  ```

<!-- Code -------------------------->
# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="cfae8-158">Visual Studio Code/Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="cfae8-158">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<!-- copy/paste this tab to the next. Not worth an include  -->

<span data-ttu-id="cfae8-159">Eseguire i seguenti comandi dell'interfaccia della riga di comando di .NET Core:</span><span class="sxs-lookup"><span data-stu-id="cfae8-159">Run the following .NET Core CLI commands:</span></span>

```console
dotnet ef migrations add Rating
dotnet ef database update
```

<span data-ttu-id="cfae8-160">Il comando `ef migrations add` indica al framework di:</span><span class="sxs-lookup"><span data-stu-id="cfae8-160">The `ef migrations add` command tells the framework to:</span></span>

* <span data-ttu-id="cfae8-161">Confrontare il modello `Movie` con lo schema di database `Movie`.</span><span class="sxs-lookup"><span data-stu-id="cfae8-161">Compare the `Movie` model with the `Movie` DB schema.</span></span>
* <span data-ttu-id="cfae8-162">Creare codice per migrare lo schema del database nel nuovo modello.</span><span class="sxs-lookup"><span data-stu-id="cfae8-162">Create code to migrate the DB schema to the new model.</span></span>

<span data-ttu-id="cfae8-163">Il nome "Rating" è arbitrario e viene usato per denominare il file di migrazione.</span><span class="sxs-lookup"><span data-stu-id="cfae8-163">The name "Rating" is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="cfae8-164">È consigliabile usare un nome significativo per il file di migrazione.</span><span class="sxs-lookup"><span data-stu-id="cfae8-164">It's helpful to use a meaningful name for the migration file.</span></span>

<span data-ttu-id="cfae8-165">Il comando `ef database update` indica al framework di applicare le modifiche dello schema al database.</span><span class="sxs-lookup"><span data-stu-id="cfae8-165">The `ef database update` command tells the framework to apply the schema changes to the database.</span></span>

<span data-ttu-id="cfae8-166">Se si eliminano tutti i record nel database, il database viene inizializzato e viene incluso il campo `Rating`.</span><span class="sxs-lookup"><span data-stu-id="cfae8-166">If you delete all the records in the DB, the initializer will seed the DB and include the `Rating` field.</span></span> <span data-ttu-id="cfae8-167">È possibile eseguire questa operazione con i collegamenti di eliminazione nel browser o mediante uno strumento SQLite.</span><span class="sxs-lookup"><span data-stu-id="cfae8-167">You can do this with the delete links in the browser or by using a SQLite tool.</span></span>

<span data-ttu-id="cfae8-168">Un'altra opzione è quella di eliminare il database e usare le migrazioni per ricreare il database.</span><span class="sxs-lookup"><span data-stu-id="cfae8-168">Another option is to delete the database and use migrations to re-create the database.</span></span> <span data-ttu-id="cfae8-169">Per eliminare il database, eliminare il file di database (*MvcMovie.db*).</span><span class="sxs-lookup"><span data-stu-id="cfae8-169">To delete the database, delete the database file (*MvcMovie.db*).</span></span> <span data-ttu-id="cfae8-170">Quindi eseguire il comando `ef database update`:</span><span class="sxs-lookup"><span data-stu-id="cfae8-170">Then run the `ef database update` command:</span></span> 

```console
dotnet ef database update
```

> [!NOTE]
> <span data-ttu-id="cfae8-171">Molte operazioni di modifica dello schema non sono supportate dal provider EF Core SQLite.</span><span class="sxs-lookup"><span data-stu-id="cfae8-171">Many schema change operations are not supported by the EF Core SQLite provider.</span></span> <span data-ttu-id="cfae8-172">Ad esempio l'aggiunta di una colonna è supportata, ma la rimozione di una colonna non è supportata.</span><span class="sxs-lookup"><span data-stu-id="cfae8-172">For example, adding a column is supported, but removing a column is not supported.</span></span> <span data-ttu-id="cfae8-173">Se si aggiunge una migrazione per rimuovere una colonna, il comando `ef migrations add` ha esito positivo, ma il comando `ef database update` ha esito negativo.</span><span class="sxs-lookup"><span data-stu-id="cfae8-173">If you add a migration to remove a column, the `ef migrations add` command succeeds but the `ef database update` command fails.</span></span> <span data-ttu-id="cfae8-174">È possibile risolvere alcune limitazioni creando manualmente codice di migrazione per eseguire una ricompilazione della tabella.</span><span class="sxs-lookup"><span data-stu-id="cfae8-174">You can work around some of the limitations by manually writing migrations code to perform a table rebuild.</span></span> <span data-ttu-id="cfae8-175">Una ricompilazione della tabella comporta la ridenominazione della tabella esistente, la creazione di una nuova tabella, la copia di dati nella nuova tabella e l'eliminazione della tabella precedente.</span><span class="sxs-lookup"><span data-stu-id="cfae8-175">A table rebuild involves renaming the existing table, creating a new table, copying data to the new table, and dropping the old table.</span></span> <span data-ttu-id="cfae8-176">Per altre informazioni, vedere le seguenti risorse:</span><span class="sxs-lookup"><span data-stu-id="cfae8-176">For more information, see the following resources:</span></span>
> * [<span data-ttu-id="cfae8-177">Limitazioni del provider di database SQLite per EF Core</span><span class="sxs-lookup"><span data-stu-id="cfae8-177">SQLite EF Core Database Provider Limitations</span></span>](/ef/core/providers/sqlite/limitations)
> * [<span data-ttu-id="cfae8-178">Personalizzare il codice di migrazione</span><span class="sxs-lookup"><span data-stu-id="cfae8-178">Customize migration code</span></span>](/ef/core/managing-schemas/migrations/#customize-migration-code)
> * [<span data-ttu-id="cfae8-179">Seeding dei dati</span><span class="sxs-lookup"><span data-stu-id="cfae8-179">Data seeding</span></span>](/ef/core/modeling/data-seeding)

---  
<!-- End of VS tabs -->

<span data-ttu-id="cfae8-180">Eseguire l'app e verificare che sia possibile creare/modificare/visualizzare i film con un campo `Rating`.</span><span class="sxs-lookup"><span data-stu-id="cfae8-180">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="cfae8-181">Se il database non è inizializzato, impostare un punto di interruzione nel metodo `SeedData.Initialize`.</span><span class="sxs-lookup"><span data-stu-id="cfae8-181">If the database isn't seeded, set a break point in the `SeedData.Initialize` method.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="cfae8-182">[Precedente: Aggiunta della funzionalità di ricerca](xref:tutorials/razor-pages/search)
> [Successivo: Aggiunta della convalida](xref:tutorials/razor-pages/validation)</span><span class="sxs-lookup"><span data-stu-id="cfae8-182">[Previous: Adding Search](xref:tutorials/razor-pages/search)
[Next: Adding Validation](xref:tutorials/razor-pages/validation)</span></span>
