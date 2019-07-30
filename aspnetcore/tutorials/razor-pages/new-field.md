---
title: Aggiungere un nuovo campo a una pagina Razor in ASP.NET Core
author: rick-anderson
description: Illustra come aggiungere un nuovo campo a una pagina Razor con Entity Framework Core
ms.author: riande
ms.custom: mvc
ms.date: 7/23/2019
uid: tutorials/razor-pages/new-field
ms.openlocfilehash: a1c0622d97e0d2b0a5601e27688f4be7cbe068dc
ms.sourcegitcommit: 16502797ea749e2690feaa5e652a65b89c007c89
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/25/2019
ms.locfileid: "68483303"
---
# <a name="add-a-new-field-to-a-razor-page-in-aspnet-core"></a><span data-ttu-id="41b39-103">Aggiungere un nuovo campo a una pagina Razor in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="41b39-103">Add a new field to a Razor Page in ASP.NET Core</span></span>

<span data-ttu-id="41b39-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="41b39-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

[!INCLUDE[](~/includes/rp/download.md)]

<span data-ttu-id="41b39-105">In questa sezione vengono usate le Migrazioni Code First di [Entity Framework](/ef/core/get-started/aspnetcore/new-db) per:</span><span class="sxs-lookup"><span data-stu-id="41b39-105">In this section [Entity Framework](/ef/core/get-started/aspnetcore/new-db) Code First Migrations is used to:</span></span>

* <span data-ttu-id="41b39-106">Aggiungere un nuovo campo al modello.</span><span class="sxs-lookup"><span data-stu-id="41b39-106">Add a new field to the model.</span></span>
* <span data-ttu-id="41b39-107">Eseguire la migrazione nel database della modifica al nuovo schema del campo.</span><span class="sxs-lookup"><span data-stu-id="41b39-107">Migrate the new field schema change to the database.</span></span>

<span data-ttu-id="41b39-108">Quando si usa Code First di Entity Framework per creare automaticamente un database, Code First:</span><span class="sxs-lookup"><span data-stu-id="41b39-108">When using EF Code First to automatically create a database, Code First:</span></span>

* <span data-ttu-id="41b39-109">Aggiunge una tabella al database per rilevare se lo schema del database è sincronizzato con le classi di modelli da cui è stato generato.</span><span class="sxs-lookup"><span data-stu-id="41b39-109">Adds a table to the database to track whether the schema of the database is in sync with the model classes it was generated from.</span></span>
* <span data-ttu-id="41b39-110">Se le classi di modelli non sono sincronizzate con il database, Entity Framework genera un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="41b39-110">If the model classes aren't in sync with the DB, EF throws an exception.</span></span>

<span data-ttu-id="41b39-111">La verifica automatica del modello o schema sincronizzato rende più semplice individuare i problemi di codice o database incoerente.</span><span class="sxs-lookup"><span data-stu-id="41b39-111">Automatic verification of schema/model in sync makes it easier to find inconsistent database/code issues.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="41b39-112">Aggiunta di una proprietà Rating al modello Movie</span><span class="sxs-lookup"><span data-stu-id="41b39-112">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="41b39-113">Aprire il file *Models/Movie.cs* e aggiungere una proprietà `Rating`:</span><span class="sxs-lookup"><span data-stu-id="41b39-113">Open the *Models/Movie.cs* file and add a `Rating` property:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Models/MovieDateRating.cs?highlight=13&name=snippet)]

<span data-ttu-id="41b39-114">Compilare l'app.</span><span class="sxs-lookup"><span data-stu-id="41b39-114">Build the app.</span></span>

<span data-ttu-id="41b39-115">Modificare *Pages/Movies/Index.cshtml* e aggiungere un campo `Rating`:</span><span class="sxs-lookup"><span data-stu-id="41b39-115">Edit *Pages/Movies/Index.cshtml*, and add a `Rating` field:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie30/SnapShots/IndexRating.cshtml?highlight=40-42,61-63)]

<span data-ttu-id="41b39-116">Aggiornare le pagine seguenti:</span><span class="sxs-lookup"><span data-stu-id="41b39-116">Update the following pages:</span></span>

* <span data-ttu-id="41b39-117">Aggiungere il campo `Rating` alle pagine Delete (Elimina) e Details (Dettagli).</span><span class="sxs-lookup"><span data-stu-id="41b39-117">Add the `Rating` field to the Delete and Details pages.</span></span>
* <span data-ttu-id="41b39-118">Aggiornare [Create.cshtml](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Pages/Movies/Create.cshtml) con un campo `Rating`.</span><span class="sxs-lookup"><span data-stu-id="41b39-118">Update [Create.cshtml](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Pages/Movies/Create.cshtml) with a `Rating` field.</span></span>
* <span data-ttu-id="41b39-119">Aggiungere il campo `Rating` alla pagina Edit (Modifica).</span><span class="sxs-lookup"><span data-stu-id="41b39-119">Add the `Rating` field to the Edit Page.</span></span>

<span data-ttu-id="41b39-120">L'app non funzionerà finché non si aggiorna il database in modo da includere il nuovo campo.</span><span class="sxs-lookup"><span data-stu-id="41b39-120">The app won't work until the DB is updated to include the new field.</span></span> <span data-ttu-id="41b39-121">Se si esegue l'app ora, verrà visualizzato un errore `SqlException`:</span><span class="sxs-lookup"><span data-stu-id="41b39-121">If run now, the app throws a `SqlException`:</span></span>

`SqlException: Invalid column name 'Rating'.`

<span data-ttu-id="41b39-122">Questo errore viene visualizzato perché la classe del modello Movie aggiornata è diversa rispetto allo schema della tabella Movie nel database.</span><span class="sxs-lookup"><span data-stu-id="41b39-122">This error is caused by the updated Movie model class being different than the schema of the Movie table of the database.</span></span> <span data-ttu-id="41b39-123">Nella tabella del database non è presente una colonna `Rating`.</span><span class="sxs-lookup"><span data-stu-id="41b39-123">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="41b39-124">Per correggere questo errore, esistono alcuni approcci:</span><span class="sxs-lookup"><span data-stu-id="41b39-124">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="41b39-125">Fare in modo che Entity Framework elimini e crei di nuovo automaticamente il database usando il nuovo schema di classi del modello.</span><span class="sxs-lookup"><span data-stu-id="41b39-125">Have the Entity Framework automatically drop and re-create the database using the new model class schema.</span></span> <span data-ttu-id="41b39-126">Questo approccio è utile nelle prime fasi del ciclo di sviluppo e consente di migliorare rapidamente sia lo schema del modello che il database contemporaneamente.</span><span class="sxs-lookup"><span data-stu-id="41b39-126">This approach is convenient early in the development cycle; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="41b39-127">Lo svantaggio è che si perdono i dati esistenti nel database.</span><span class="sxs-lookup"><span data-stu-id="41b39-127">The downside is that you lose existing data in the database.</span></span> <span data-ttu-id="41b39-128">Non usare questo approccio in un database di produzione.</span><span class="sxs-lookup"><span data-stu-id="41b39-128">Don't use this approach on a production database!</span></span> <span data-ttu-id="41b39-129">L'eliminazione del database per la modifica dello schema e l'uso di un inizializzatore per inizializzare automaticamente un database con i dati di test è spesso un modo produttivo per sviluppare un'app.</span><span class="sxs-lookup"><span data-stu-id="41b39-129">Dropping the DB on schema changes and using an initializer to automatically seed the database with test data is often a productive way to develop an app.</span></span>

2. <span data-ttu-id="41b39-130">Modificare esplicitamente lo schema del database esistente in modo che corrisponda alle classi del modello.</span><span class="sxs-lookup"><span data-stu-id="41b39-130">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="41b39-131">Il vantaggio di questo approccio è che i dati vengono mantenuti.</span><span class="sxs-lookup"><span data-stu-id="41b39-131">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="41b39-132">È possibile apportare questa modifica manualmente o creando uno script di modifica del database.</span><span class="sxs-lookup"><span data-stu-id="41b39-132">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="41b39-133">Usare Migrazioni Code First per aggiornare lo schema del database.</span><span class="sxs-lookup"><span data-stu-id="41b39-133">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="41b39-134">Per questa esercitazione usare Migrazioni Code First.</span><span class="sxs-lookup"><span data-stu-id="41b39-134">For this tutorial, use Code First Migrations.</span></span>

<span data-ttu-id="41b39-135">Aggiornare la classe `SeedData` in modo che fornisca un valore per la nuova colonna.</span><span class="sxs-lookup"><span data-stu-id="41b39-135">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="41b39-136">Di seguito viene illustrata una modifica di esempio, ma si apporterà questa modifica per ogni blocco `new Movie`.</span><span class="sxs-lookup"><span data-stu-id="41b39-136">A sample change is shown below, but you'll want to make this change for each `new Movie` block.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Models/SeedDataRating.cs?name=snippet1&highlight=8)]

<span data-ttu-id="41b39-137">Vedere il [file SeedData.cs completato](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Models/SeedDataRating.cs).</span><span class="sxs-lookup"><span data-stu-id="41b39-137">See the [completed SeedData.cs file](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Models/SeedDataRating.cs).</span></span>

<span data-ttu-id="41b39-138">Compilare la soluzione.</span><span class="sxs-lookup"><span data-stu-id="41b39-138">Build the solution.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="41b39-139">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="41b39-139">Visual Studio</span></span>](#tab/visual-studio)

<a name="pmc"></a>

### <a name="add-a-migration-for-the-rating-field"></a><span data-ttu-id="41b39-140">Aggiungere una migrazione per il campo Rating</span><span class="sxs-lookup"><span data-stu-id="41b39-140">Add a migration for the rating field</span></span>

<span data-ttu-id="41b39-141">Dal menu **Strumenti** selezionare **Gestione pacchetti NuGet > Console di Gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="41b39-141">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>
<span data-ttu-id="41b39-142">Nella Console di Gestione pacchetti immettere i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="41b39-142">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration Rating
Update-Database
```

<span data-ttu-id="41b39-143">Il comando `Add-Migration` indica al framework di:</span><span class="sxs-lookup"><span data-stu-id="41b39-143">The `Add-Migration` command tells the framework to:</span></span>

* <span data-ttu-id="41b39-144">Confrontare il modello `Movie` con lo schema di database `Movie`.</span><span class="sxs-lookup"><span data-stu-id="41b39-144">Compare the `Movie` model with the `Movie` DB schema.</span></span>
* <span data-ttu-id="41b39-145">Creare codice per migrare lo schema del database nel nuovo modello.</span><span class="sxs-lookup"><span data-stu-id="41b39-145">Create code to migrate the DB schema to the new model.</span></span>

<span data-ttu-id="41b39-146">Il nome "Rating" è arbitrario e viene usato per denominare il file di migrazione.</span><span class="sxs-lookup"><span data-stu-id="41b39-146">The name "Rating" is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="41b39-147">È consigliabile usare un nome significativo per il file di migrazione.</span><span class="sxs-lookup"><span data-stu-id="41b39-147">It's helpful to use a meaningful name for the migration file.</span></span>

<span data-ttu-id="41b39-148">Il comando `Update-Database` indica al framework di applicare le modifiche dello schema al database.</span><span class="sxs-lookup"><span data-stu-id="41b39-148">The `Update-Database` command tells the framework to apply the schema changes to the database.</span></span>

<a name="ssox"></a>

<span data-ttu-id="41b39-149">Se si eliminano tutti i record nel database, il database viene inizializzato e viene incluso il campo `Rating`.</span><span class="sxs-lookup"><span data-stu-id="41b39-149">If you delete all the records in the DB, the initializer will seed the DB and include the `Rating` field.</span></span> <span data-ttu-id="41b39-150">È possibile eseguire questa operazione con i collegamenti di eliminazione nel browser o da [Sql Server Object Explorer](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span><span class="sxs-lookup"><span data-stu-id="41b39-150">You can do this with the delete links in the browser or from [Sql Server Object Explorer](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span></span>

<span data-ttu-id="41b39-151">Un'altra opzione è quella di eliminare il database e usare le migrazioni per ricreare il database.</span><span class="sxs-lookup"><span data-stu-id="41b39-151">Another option is to delete the database and use migrations to re-create the database.</span></span> <span data-ttu-id="41b39-152">Per eliminare il database da SSOX:</span><span class="sxs-lookup"><span data-stu-id="41b39-152">To delete the database in SSOX:</span></span>

* <span data-ttu-id="41b39-153">Selezionare il database in SSOX.</span><span class="sxs-lookup"><span data-stu-id="41b39-153">Select the database in SSOX.</span></span>
* <span data-ttu-id="41b39-154">Fare clic con il pulsante destro del mouse sul database e selezionare *Elimina*.</span><span class="sxs-lookup"><span data-stu-id="41b39-154">Right click on the database, and select *Delete*.</span></span>
* <span data-ttu-id="41b39-155">Selezionare **Chiudi connessioni esistenti**.</span><span class="sxs-lookup"><span data-stu-id="41b39-155">Check **Close existing connections**.</span></span>
* <span data-ttu-id="41b39-156">Scegliere **OK**.</span><span class="sxs-lookup"><span data-stu-id="41b39-156">Select **OK**.</span></span>
* <span data-ttu-id="41b39-157">Nella [Console di Gestione pacchetti](xref:tutorials/razor-pages/new-field#pmc) aggiornare il database:</span><span class="sxs-lookup"><span data-stu-id="41b39-157">In the [PMC](xref:tutorials/razor-pages/new-field#pmc), update the database:</span></span>

  ```powershell
  Update-Database
  ```

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="41b39-158">Visual Studio Code/Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="41b39-158">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

### <a name="drop-and-re-create-the-database"></a><span data-ttu-id="41b39-159">Eliminare e ricreare il database</span><span class="sxs-lookup"><span data-stu-id="41b39-159">Drop and re-create the database</span></span>

[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

<span data-ttu-id="41b39-160">Eliminare la cartella della migrazione.</span><span class="sxs-lookup"><span data-stu-id="41b39-160">Delete the migration folder.</span></span>  <span data-ttu-id="41b39-161">Usare i comandi seguenti per ricreare il database.</span><span class="sxs-lookup"><span data-stu-id="41b39-161">Use the following commands to recreate the database.</span></span>

```console
dotnet ef database drop
dotnet ef migrations add InitialCreate
dotnet ef database update
```

---

<span data-ttu-id="41b39-162">Eseguire l'app e verificare che sia possibile creare/modificare/visualizzare i film con un campo `Rating`.</span><span class="sxs-lookup"><span data-stu-id="41b39-162">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="41b39-163">Se il database non è inizializzato, impostare un punto di interruzione nel metodo `SeedData.Initialize`.</span><span class="sxs-lookup"><span data-stu-id="41b39-163">If the database isn't seeded, set a break point in the `SeedData.Initialize` method.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="41b39-164">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="41b39-164">Additional resources</span></span>

* [<span data-ttu-id="41b39-165">Versione YouTube dell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="41b39-165">YouTube version of this tutorial</span></span>](https://youtu.be/3i7uMxiGGR8)

> [!div class="step-by-step"]
> <span data-ttu-id="41b39-166">[Precedente: Aggiunta della funzionalità di ricerca](xref:tutorials/razor-pages/search)
> [Successivo: Aggiunta della convalida](xref:tutorials/razor-pages/validation)</span><span class="sxs-lookup"><span data-stu-id="41b39-166">[Previous: Adding Search](xref:tutorials/razor-pages/search)
[Next: Adding Validation](xref:tutorials/razor-pages/validation)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE[](~/includes/rp/download.md)]

<span data-ttu-id="41b39-167">In questa sezione vengono usate le Migrazioni Code First di [Entity Framework](/ef/core/get-started/aspnetcore/new-db) per:</span><span class="sxs-lookup"><span data-stu-id="41b39-167">In this section [Entity Framework](/ef/core/get-started/aspnetcore/new-db) Code First Migrations is used to:</span></span>

* <span data-ttu-id="41b39-168">Aggiungere un nuovo campo al modello.</span><span class="sxs-lookup"><span data-stu-id="41b39-168">Add a new field to the model.</span></span>
* <span data-ttu-id="41b39-169">Eseguire la migrazione nel database della modifica al nuovo schema del campo.</span><span class="sxs-lookup"><span data-stu-id="41b39-169">Migrate the new field schema change to the database.</span></span>

<span data-ttu-id="41b39-170">Quando si usa Code First di Entity Framework per creare automaticamente un database, Code First:</span><span class="sxs-lookup"><span data-stu-id="41b39-170">When using EF Code First to automatically create a database, Code First:</span></span>

* <span data-ttu-id="41b39-171">Aggiunge una tabella al database per rilevare se lo schema del database è sincronizzato con le classi di modelli da cui è stato generato.</span><span class="sxs-lookup"><span data-stu-id="41b39-171">Adds a table to the database to track whether the schema of the database is in sync with the model classes it was generated from.</span></span>
* <span data-ttu-id="41b39-172">Se le classi di modelli non sono sincronizzate con il database, Entity Framework genera un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="41b39-172">If the model classes aren't in sync with the DB, EF throws an exception.</span></span>

<span data-ttu-id="41b39-173">La verifica automatica del modello o schema sincronizzato rende più semplice individuare i problemi di codice o database incoerente.</span><span class="sxs-lookup"><span data-stu-id="41b39-173">Automatic verification of schema/model in sync makes it easier to find inconsistent database/code issues.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="41b39-174">Aggiunta di una proprietà Rating al modello Movie</span><span class="sxs-lookup"><span data-stu-id="41b39-174">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="41b39-175">Aprire il file *Models/Movie.cs* e aggiungere una proprietà `Rating`:</span><span class="sxs-lookup"><span data-stu-id="41b39-175">Open the *Models/Movie.cs* file and add a `Rating` property:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Models/MovieDateRating.cs?highlight=13&name=snippet)]

<span data-ttu-id="41b39-176">Compilare l'app.</span><span class="sxs-lookup"><span data-stu-id="41b39-176">Build the app.</span></span>

<span data-ttu-id="41b39-177">Modificare *Pages/Movies/Index.cshtml* e aggiungere un campo `Rating`:</span><span class="sxs-lookup"><span data-stu-id="41b39-177">Edit *Pages/Movies/Index.cshtml*, and add a `Rating` field:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/IndexRating.cshtml?highlight=40-42,61-63)]

<span data-ttu-id="41b39-178">Aggiornare le pagine seguenti:</span><span class="sxs-lookup"><span data-stu-id="41b39-178">Update the following pages:</span></span>

* <span data-ttu-id="41b39-179">Aggiungere il campo `Rating` alle pagine Delete (Elimina) e Details (Dettagli).</span><span class="sxs-lookup"><span data-stu-id="41b39-179">Add the `Rating` field to the Delete and Details pages.</span></span>
* <span data-ttu-id="41b39-180">Aggiornare [Create.cshtml](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Create.cshtml) con un campo `Rating`.</span><span class="sxs-lookup"><span data-stu-id="41b39-180">Update [Create.cshtml](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Create.cshtml) with a `Rating` field.</span></span>
* <span data-ttu-id="41b39-181">Aggiungere il campo `Rating` alla pagina Edit (Modifica).</span><span class="sxs-lookup"><span data-stu-id="41b39-181">Add the `Rating` field to the Edit Page.</span></span>

<span data-ttu-id="41b39-182">L'app non funzionerà finché non si aggiorna il database in modo da includere il nuovo campo.</span><span class="sxs-lookup"><span data-stu-id="41b39-182">The app won't work until the DB is updated to include the new field.</span></span> <span data-ttu-id="41b39-183">Se si esegue l'app ora, verrà visualizzato un errore `SqlException`:</span><span class="sxs-lookup"><span data-stu-id="41b39-183">If run now, the app throws a `SqlException`:</span></span>

`SqlException: Invalid column name 'Rating'.`

<span data-ttu-id="41b39-184">Questo errore viene visualizzato perché la classe del modello Movie aggiornata è diversa rispetto allo schema della tabella Movie nel database.</span><span class="sxs-lookup"><span data-stu-id="41b39-184">This error is caused by the updated Movie model class being different than the schema of the Movie table of the database.</span></span> <span data-ttu-id="41b39-185">Nella tabella del database non è presente una colonna `Rating`.</span><span class="sxs-lookup"><span data-stu-id="41b39-185">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="41b39-186">Per correggere questo errore, esistono alcuni approcci:</span><span class="sxs-lookup"><span data-stu-id="41b39-186">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="41b39-187">Fare in modo che Entity Framework elimini e crei di nuovo automaticamente il database usando il nuovo schema di classi del modello.</span><span class="sxs-lookup"><span data-stu-id="41b39-187">Have the Entity Framework automatically drop and re-create the database using the new model class schema.</span></span> <span data-ttu-id="41b39-188">Questo approccio è utile nelle prime fasi del ciclo di sviluppo e consente di migliorare rapidamente sia lo schema del modello che il database contemporaneamente.</span><span class="sxs-lookup"><span data-stu-id="41b39-188">This approach is convenient early in the development cycle; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="41b39-189">Lo svantaggio è che si perdono i dati esistenti nel database.</span><span class="sxs-lookup"><span data-stu-id="41b39-189">The downside is that you lose existing data in the database.</span></span> <span data-ttu-id="41b39-190">Non usare questo approccio in un database di produzione.</span><span class="sxs-lookup"><span data-stu-id="41b39-190">Don't use this approach on a production database!</span></span> <span data-ttu-id="41b39-191">L'eliminazione del database per la modifica dello schema e l'uso di un inizializzatore per inizializzare automaticamente un database con i dati di test è spesso un modo produttivo per sviluppare un'app.</span><span class="sxs-lookup"><span data-stu-id="41b39-191">Dropping the DB on schema changes and using an initializer to automatically seed the database with test data is often a productive way to develop an app.</span></span>

2. <span data-ttu-id="41b39-192">Modificare esplicitamente lo schema del database esistente in modo che corrisponda alle classi del modello.</span><span class="sxs-lookup"><span data-stu-id="41b39-192">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="41b39-193">Il vantaggio di questo approccio è che i dati vengono mantenuti.</span><span class="sxs-lookup"><span data-stu-id="41b39-193">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="41b39-194">È possibile apportare questa modifica manualmente o creando uno script di modifica del database.</span><span class="sxs-lookup"><span data-stu-id="41b39-194">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="41b39-195">Usare Migrazioni Code First per aggiornare lo schema del database.</span><span class="sxs-lookup"><span data-stu-id="41b39-195">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="41b39-196">Per questa esercitazione usare Migrazioni Code First.</span><span class="sxs-lookup"><span data-stu-id="41b39-196">For this tutorial, use Code First Migrations.</span></span>

<span data-ttu-id="41b39-197">Aggiornare la classe `SeedData` in modo che fornisca un valore per la nuova colonna.</span><span class="sxs-lookup"><span data-stu-id="41b39-197">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="41b39-198">Di seguito viene illustrata una modifica di esempio, ma si apporterà questa modifica per ogni blocco `new Movie`.</span><span class="sxs-lookup"><span data-stu-id="41b39-198">A sample change is shown below, but you'll want to make this change for each `new Movie` block.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Models/SeedDataRating.cs?name=snippet1&highlight=8)]

<span data-ttu-id="41b39-199">Vedere il [file SeedData.cs completato](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Models/SeedDataRating.cs).</span><span class="sxs-lookup"><span data-stu-id="41b39-199">See the [completed SeedData.cs file](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Models/SeedDataRating.cs).</span></span>

<span data-ttu-id="41b39-200">Compilare la soluzione.</span><span class="sxs-lookup"><span data-stu-id="41b39-200">Build the solution.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="41b39-201">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="41b39-201">Visual Studio</span></span>](#tab/visual-studio)

<a name="pmc"></a>

### <a name="add-a-migration-for-the-rating-field"></a><span data-ttu-id="41b39-202">Aggiungere una migrazione per il campo Rating</span><span class="sxs-lookup"><span data-stu-id="41b39-202">Add a migration for the rating field</span></span>

<span data-ttu-id="41b39-203">Dal menu **Strumenti** selezionare **Gestione pacchetti NuGet > Console di Gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="41b39-203">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>
<span data-ttu-id="41b39-204">Nella Console di Gestione pacchetti immettere i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="41b39-204">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration Rating
Update-Database
```

<span data-ttu-id="41b39-205">Il comando `Add-Migration` indica al framework di:</span><span class="sxs-lookup"><span data-stu-id="41b39-205">The `Add-Migration` command tells the framework to:</span></span>

* <span data-ttu-id="41b39-206">Confrontare il modello `Movie` con lo schema di database `Movie`.</span><span class="sxs-lookup"><span data-stu-id="41b39-206">Compare the `Movie` model with the `Movie` DB schema.</span></span>
* <span data-ttu-id="41b39-207">Creare codice per migrare lo schema del database nel nuovo modello.</span><span class="sxs-lookup"><span data-stu-id="41b39-207">Create code to migrate the DB schema to the new model.</span></span>

<span data-ttu-id="41b39-208">Il nome "Rating" è arbitrario e viene usato per denominare il file di migrazione.</span><span class="sxs-lookup"><span data-stu-id="41b39-208">The name "Rating" is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="41b39-209">È consigliabile usare un nome significativo per il file di migrazione.</span><span class="sxs-lookup"><span data-stu-id="41b39-209">It's helpful to use a meaningful name for the migration file.</span></span>

<span data-ttu-id="41b39-210">Il comando `Update-Database` indica al framework di applicare le modifiche dello schema al database.</span><span class="sxs-lookup"><span data-stu-id="41b39-210">The `Update-Database` command tells the framework to apply the schema changes to the database.</span></span>

<a name="ssox"></a>

<span data-ttu-id="41b39-211">Se si eliminano tutti i record nel database, il database viene inizializzato e viene incluso il campo `Rating`.</span><span class="sxs-lookup"><span data-stu-id="41b39-211">If you delete all the records in the DB, the initializer will seed the DB and include the `Rating` field.</span></span> <span data-ttu-id="41b39-212">È possibile eseguire questa operazione con i collegamenti di eliminazione nel browser o da [Sql Server Object Explorer](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span><span class="sxs-lookup"><span data-stu-id="41b39-212">You can do this with the delete links in the browser or from [Sql Server Object Explorer](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span></span>

<span data-ttu-id="41b39-213">Un'altra opzione è quella di eliminare il database e usare le migrazioni per ricreare il database.</span><span class="sxs-lookup"><span data-stu-id="41b39-213">Another option is to delete the database and use migrations to re-create the database.</span></span> <span data-ttu-id="41b39-214">Per eliminare il database da SSOX:</span><span class="sxs-lookup"><span data-stu-id="41b39-214">To delete the database in SSOX:</span></span>

* <span data-ttu-id="41b39-215">Selezionare il database in SSOX.</span><span class="sxs-lookup"><span data-stu-id="41b39-215">Select the database in SSOX.</span></span>
* <span data-ttu-id="41b39-216">Fare clic con il pulsante destro del mouse sul database e selezionare *Elimina*.</span><span class="sxs-lookup"><span data-stu-id="41b39-216">Right click on the database, and select *Delete*.</span></span>
* <span data-ttu-id="41b39-217">Selezionare **Chiudi connessioni esistenti**.</span><span class="sxs-lookup"><span data-stu-id="41b39-217">Check **Close existing connections**.</span></span>
* <span data-ttu-id="41b39-218">Scegliere **OK**.</span><span class="sxs-lookup"><span data-stu-id="41b39-218">Select **OK**.</span></span>
* <span data-ttu-id="41b39-219">Nella [Console di Gestione pacchetti](xref:tutorials/razor-pages/new-field#pmc) aggiornare il database:</span><span class="sxs-lookup"><span data-stu-id="41b39-219">In the [PMC](xref:tutorials/razor-pages/new-field#pmc), update the database:</span></span>

  ```powershell
  Update-Database
  ```

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="41b39-220">Visual Studio Code/Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="41b39-220">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

### <a name="drop-and-re-create-the-database"></a><span data-ttu-id="41b39-221">Eliminare e ricreare il database</span><span class="sxs-lookup"><span data-stu-id="41b39-221">Drop and re-create the database</span></span>

[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

<span data-ttu-id="41b39-222">Eliminare il database e usare le migrazioni per ricreare il database.</span><span class="sxs-lookup"><span data-stu-id="41b39-222">Delete the database and use migrations to re-create the database.</span></span> <span data-ttu-id="41b39-223">Per eliminare il database, eliminare il file di database (*MvcMovie.db*).</span><span class="sxs-lookup"><span data-stu-id="41b39-223">To delete the database, delete the database file (*MvcMovie.db*).</span></span> <span data-ttu-id="41b39-224">Quindi eseguire il comando `ef database update`:</span><span class="sxs-lookup"><span data-stu-id="41b39-224">Then run the `ef database update` command:</span></span>

```console
dotnet ef database update
```

---

<span data-ttu-id="41b39-225">Eseguire l'app e verificare che sia possibile creare/modificare/visualizzare i film con un campo `Rating`.</span><span class="sxs-lookup"><span data-stu-id="41b39-225">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="41b39-226">Se il database non è inizializzato, impostare un punto di interruzione nel metodo `SeedData.Initialize`.</span><span class="sxs-lookup"><span data-stu-id="41b39-226">If the database isn't seeded, set a break point in the `SeedData.Initialize` method.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="41b39-227">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="41b39-227">Additional resources</span></span>

* [<span data-ttu-id="41b39-228">Versione YouTube dell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="41b39-228">YouTube version of this tutorial</span></span>](https://youtu.be/3i7uMxiGGR8)

> [!div class="step-by-step"]
> <span data-ttu-id="41b39-229">[Precedente: Aggiunta della funzionalità di ricerca](xref:tutorials/razor-pages/search)
> [Successivo: Aggiunta della convalida](xref:tutorials/razor-pages/validation)</span><span class="sxs-lookup"><span data-stu-id="41b39-229">[Previous: Adding Search](xref:tutorials/razor-pages/search)
[Next: Adding Validation](xref:tutorials/razor-pages/validation)</span></span>

::: moniker-end
