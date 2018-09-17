---
title: Aggiungere un nuovo campo a una pagina Razor in ASP.NET Core
author: rick-anderson
description: Illustra come aggiungere un nuovo campo a una pagina Razor con Entity Framework Core
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/new-field
ms.openlocfilehash: 6c23e5ab21dbb94c69ba50200a1d76647e22410a
ms.sourcegitcommit: 4afaa55918262c8dcbd3efa9584959a731b47681
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/14/2018
ms.locfileid: "45613453"
---
# <a name="add-a-new-field-to-a-razor-page-in-aspnet-core"></a><span data-ttu-id="fbf6a-103">Aggiungere un nuovo campo a una pagina Razor in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fbf6a-103">Add a new field to a Razor Page in ASP.NET Core</span></span>

<span data-ttu-id="fbf6a-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="fbf6a-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="fbf6a-105">In questa sezione viene usato Migrazioni Code First di [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) per aggiungere un nuovo campo al modello e migrare questa modifica nel database.</span><span class="sxs-lookup"><span data-stu-id="fbf6a-105">In this section you use [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) Code First Migrations to add a new field to the model and migrate that change to the database.</span></span>

<span data-ttu-id="fbf6a-106">Quando si usa Code First di Entity Framework per creare automaticamente un database, Code First:</span><span class="sxs-lookup"><span data-stu-id="fbf6a-106">When using EF Code First to automatically create a database, Code First:</span></span>

* <span data-ttu-id="fbf6a-107">Aggiunge una tabella al database per rilevare se lo schema del database è sincronizzato con le classi di modelli da cui è stato generato.</span><span class="sxs-lookup"><span data-stu-id="fbf6a-107">Adds a table to the database to track whether the schema of the database is in sync with the model classes it was generated from.</span></span>
* <span data-ttu-id="fbf6a-108">Se le classi di modelli non sono sincronizzate con il database, Entity Framework genera un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="fbf6a-108">If the model classes aren't in sync with the DB, EF throws an exception.</span></span> 

<span data-ttu-id="fbf6a-109">La verifica automatica del modello o schema sincronizzato rende più semplice individuare i problemi di codice o database incoerente.</span><span class="sxs-lookup"><span data-stu-id="fbf6a-109">Automatic verification of schema/model in sync makes it easier to find inconsistent database/code issues.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="fbf6a-110">Aggiunta di una proprietà Rating al modello Movie</span><span class="sxs-lookup"><span data-stu-id="fbf6a-110">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="fbf6a-111">Aprire il file *Models/Movie.cs* e aggiungere una proprietà `Rating`:</span><span class="sxs-lookup"><span data-stu-id="fbf6a-111">Open the *Models/Movie.cs* file and add a `Rating` property:</span></span>

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Models/MovieDateRating.cs?highlight=13&name=snippet)]

::: moniker-end

<span data-ttu-id="fbf6a-112">Compilare l'app (CTRL+MAIUSC+B).</span><span class="sxs-lookup"><span data-stu-id="fbf6a-112">Build the app (Ctrl+Shift+B).</span></span>

<span data-ttu-id="fbf6a-113">Modificare *Pages/Movies/Index.cshtml* e aggiungere un campo `Rating`:</span><span class="sxs-lookup"><span data-stu-id="fbf6a-113">Edit *Pages/Movies/Index.cshtml*, and add a `Rating` field:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=40-42,61-63)]

<span data-ttu-id="fbf6a-114">Aggiungere il campo `Rating` alle pagine Delete (Elimina) e Details (Dettagli).</span><span class="sxs-lookup"><span data-stu-id="fbf6a-114">Add the `Rating` field to the Delete and Details pages.</span></span>

<span data-ttu-id="fbf6a-115">Aggiornare *Create.cshtml* con un campo `Rating`.</span><span class="sxs-lookup"><span data-stu-id="fbf6a-115">Update *Create.cshtml* with a `Rating` field.</span></span> <span data-ttu-id="fbf6a-116">È possibile copiare e incollare l'elemento `<div>` precedente e aggiornare i campi usando intelliSense.</span><span class="sxs-lookup"><span data-stu-id="fbf6a-116">You can copy/paste the previous `<div>` element and let intelliSense help you update the fields.</span></span> <span data-ttu-id="fbf6a-117">IntelliSense funziona con gli [helper tag](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="fbf6a-117">IntelliSense works with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

![Lo sviluppatore ha digitato la lettera R per il valore dell'attributo asp-for nel secondo elemento di etichetta della vista.](new-field/_static/cr.png)

<span data-ttu-id="fbf6a-121">Il codice seguente mostra il file *Create.cshtml* con un campo `Rating`:</span><span class="sxs-lookup"><span data-stu-id="fbf6a-121">The following code shows *Create.cshtml* with a `Rating` field:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Create.cshtml?highlight=36-40)]

<span data-ttu-id="fbf6a-122">Aggiungere il campo `Rating` alla pagina Edit (Modifica).</span><span class="sxs-lookup"><span data-stu-id="fbf6a-122">Add the `Rating` field to the Edit Page.</span></span>

<span data-ttu-id="fbf6a-123">L'app non funzionerà finché non si aggiorna il database in modo da includere il nuovo campo.</span><span class="sxs-lookup"><span data-stu-id="fbf6a-123">The app won't work until the DB is updated to include the new field.</span></span> <span data-ttu-id="fbf6a-124">Se si esegue l'app ora, verrà visualizzato un errore `SqlException`:</span><span class="sxs-lookup"><span data-stu-id="fbf6a-124">If run now, the app throws a `SqlException`:</span></span>

```
SqlException: Invalid column name 'Rating'.
```

<span data-ttu-id="fbf6a-125">Questo errore viene visualizzato perché la classe del modello Movie aggiornata è diversa rispetto allo schema della tabella Movie nel database.</span><span class="sxs-lookup"><span data-stu-id="fbf6a-125">This error is caused by the updated Movie model class being different than the schema of the Movie table of the database.</span></span> <span data-ttu-id="fbf6a-126">Nella tabella del database non è presente una colonna `Rating`.</span><span class="sxs-lookup"><span data-stu-id="fbf6a-126">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="fbf6a-127">Per correggere questo errore, esistono alcuni approcci:</span><span class="sxs-lookup"><span data-stu-id="fbf6a-127">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="fbf6a-128">Fare in modo che Entity Framework elimini e crei di nuovo automaticamente il database usando il nuovo schema di classi del modello.</span><span class="sxs-lookup"><span data-stu-id="fbf6a-128">Have the Entity Framework automatically drop and re-create the database using  the new model class schema.</span></span> <span data-ttu-id="fbf6a-129">Questo approccio è utile nelle prime fasi del ciclo di sviluppo e consente di migliorare rapidamente lo schema del modello e il database insieme.</span><span class="sxs-lookup"><span data-stu-id="fbf6a-129">This approach is convenient early in the development cycle; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="fbf6a-130">Lo svantaggio è che si perdono i dati esistenti nel database.</span><span class="sxs-lookup"><span data-stu-id="fbf6a-130">The downside is that you lose existing data in the database.</span></span> <span data-ttu-id="fbf6a-131">Non usare questo approccio in un database di produzione.</span><span class="sxs-lookup"><span data-stu-id="fbf6a-131">You don't want to use this approach on a production database!</span></span> <span data-ttu-id="fbf6a-132">L'eliminazione del database per la modifica dello schema e l'uso di un inizializzatore per inizializzare automaticamente un database con i dati di test è spesso un modo produttivo per sviluppare un'app.</span><span class="sxs-lookup"><span data-stu-id="fbf6a-132">Dropping the DB on schema changes and using an initializer to automatically seed the database with test data is often a productive way to develop an app.</span></span>

2. <span data-ttu-id="fbf6a-133">Modificare esplicitamente lo schema del database esistente in modo che corrisponda alle classi del modello.</span><span class="sxs-lookup"><span data-stu-id="fbf6a-133">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="fbf6a-134">Il vantaggio di questo approccio è che i dati vengono mantenuti.</span><span class="sxs-lookup"><span data-stu-id="fbf6a-134">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="fbf6a-135">È possibile apportare questa modifica manualmente o creando uno script di modifica del database.</span><span class="sxs-lookup"><span data-stu-id="fbf6a-135">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="fbf6a-136">Usare Migrazioni Code First per aggiornare lo schema del database.</span><span class="sxs-lookup"><span data-stu-id="fbf6a-136">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="fbf6a-137">Per questa esercitazione usare Migrazioni Code First.</span><span class="sxs-lookup"><span data-stu-id="fbf6a-137">For this tutorial, use Code First Migrations.</span></span>

<span data-ttu-id="fbf6a-138">Aggiornare la classe `SeedData` in modo che fornisca un valore per la nuova colonna.</span><span class="sxs-lookup"><span data-stu-id="fbf6a-138">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="fbf6a-139">Di seguito viene illustrata una modifica di esempio, ma si apporterà questa modifica per ogni blocco `new Movie`.</span><span class="sxs-lookup"><span data-stu-id="fbf6a-139">A sample change is shown below, but you'll want to make this change for each `new Movie` block.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs?name=snippet1&highlight=8)]

::: moniker range="= aspnetcore-2.0"
<span data-ttu-id="fbf6a-140">Vedere il [file SeedData.cs completato](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs).</span><span class="sxs-lookup"><span data-stu-id="fbf6a-140">See the [completed SeedData.cs file](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs).</span></span>
::: moniker-end

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="fbf6a-141">Vedere il [file SeedData.cs completato](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Models/SeedDataRating.cs).</span><span class="sxs-lookup"><span data-stu-id="fbf6a-141">See the [completed SeedData.cs file](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie21/Models/SeedDataRating.cs).</span></span>
::: moniker-end

<span data-ttu-id="fbf6a-142">Compilare la soluzione.</span><span class="sxs-lookup"><span data-stu-id="fbf6a-142">Build the solution.</span></span>

<a name="pmc"></a> <span data-ttu-id="fbf6a-143">Dal menu **Strumenti** selezionare **Gestione pacchetti NuGet > Console di Gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="fbf6a-143">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>
<span data-ttu-id="fbf6a-144">Nella Console di Gestione pacchetti immettere i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="fbf6a-144">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration Rating
Update-Database
```

<span data-ttu-id="fbf6a-145">Il comando `Add-Migration` indica al framework di:</span><span class="sxs-lookup"><span data-stu-id="fbf6a-145">The `Add-Migration` command tells the framework to:</span></span>

* <span data-ttu-id="fbf6a-146">Confrontare il modello `Movie` con lo schema di database `Movie`.</span><span class="sxs-lookup"><span data-stu-id="fbf6a-146">Compare the `Movie` model with the `Movie` DB schema.</span></span>
* <span data-ttu-id="fbf6a-147">Creare codice per migrare lo schema del database nel nuovo modello.</span><span class="sxs-lookup"><span data-stu-id="fbf6a-147">Create code to migrate the DB schema to the new model.</span></span>

<span data-ttu-id="fbf6a-148">Il nome "Rating" è arbitrario e viene usato per denominare il file di migrazione.</span><span class="sxs-lookup"><span data-stu-id="fbf6a-148">The name "Rating" is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="fbf6a-149">È consigliabile usare un nome significativo per il file di migrazione.</span><span class="sxs-lookup"><span data-stu-id="fbf6a-149">It's helpful to use a meaningful name for the migration file.</span></span>

<a name="ssox"></a> <span data-ttu-id="fbf6a-150">Se si eliminano tutti i record nel database, il database verrà inizializzato e verrà incluso il campo `Rating`.</span><span class="sxs-lookup"><span data-stu-id="fbf6a-150">If you delete all the records in the DB, the initializer will seed the DB and include the `Rating` field.</span></span> <span data-ttu-id="fbf6a-151">È possibile eseguire questa operazione con i collegamenti di eliminazione nel browser o da [Sql Server Object Explorer](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span><span class="sxs-lookup"><span data-stu-id="fbf6a-151">You can do this with the delete links in the browser or from [Sql Server Object Explorer](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span></span> <span data-ttu-id="fbf6a-152">Per eliminare il database da SSOX:</span><span class="sxs-lookup"><span data-stu-id="fbf6a-152">To delete the database from SSOX:</span></span>

* <span data-ttu-id="fbf6a-153">Selezionare il database in SSOX.</span><span class="sxs-lookup"><span data-stu-id="fbf6a-153">Select the database in SSOX.</span></span>
* <span data-ttu-id="fbf6a-154">Fare clic con il pulsante destro del mouse sul database e selezionare *Elimina*.</span><span class="sxs-lookup"><span data-stu-id="fbf6a-154">Right click on the database, and select *Delete*.</span></span>
* <span data-ttu-id="fbf6a-155">Selezionare **Chiudi connessioni esistenti**.</span><span class="sxs-lookup"><span data-stu-id="fbf6a-155">Check **Close existing connections**.</span></span>
* <span data-ttu-id="fbf6a-156">Scegliere **OK**.</span><span class="sxs-lookup"><span data-stu-id="fbf6a-156">Select **OK**.</span></span>
* <span data-ttu-id="fbf6a-157">Nella [Console di Gestione pacchetti](xref:tutorials/razor-pages/new-field#pmc) aggiornare il database:</span><span class="sxs-lookup"><span data-stu-id="fbf6a-157">In the [PMC](xref:tutorials/razor-pages/new-field#pmc), update the database:</span></span>

  ```powershell
  Update-Database
  ```

<span data-ttu-id="fbf6a-158">Eseguire l'app e verificare che sia possibile creare/modificare/visualizzare i film con un campo `Rating`.</span><span class="sxs-lookup"><span data-stu-id="fbf6a-158">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="fbf6a-159">Se il database non viene inizializzato, arrestare IIS Express e quindi eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="fbf6a-159">If the database isn't seeded, stop IIS Express, and then run the app.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="fbf6a-160">[Precedente: Aggiunta della funzionalità di ricerca](xref:tutorials/razor-pages/search)
> [Successivo: Aggiunta della funzionalità di convalida](xref:tutorials/razor-pages/validation)</span><span class="sxs-lookup"><span data-stu-id="fbf6a-160">[Previous: Adding Search](xref:tutorials/razor-pages/search)
[Next: Adding Validation](xref:tutorials/razor-pages/validation)</span></span>
