---
title: Aggiunta di un nuovo campo a una pagina Razor
author: rick-anderson
description: Illustra come aggiungere un nuovo campo a una pagina Razor con Entity Framework Core
keywords: ASP.NET Core, Entity Framework Core, migrazioni
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/new-field
ms.openlocfilehash: 128b69513976a56104524bb803f2b8cb1daf1967
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
# <a name="adding-a-new-field-to-a-razor-page"></a><span data-ttu-id="c3c48-104">Aggiunta di un nuovo campo a una pagina Razor</span><span class="sxs-lookup"><span data-stu-id="c3c48-104">Adding a new field to a Razor Page</span></span>

<span data-ttu-id="c3c48-105">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c3c48-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="c3c48-106">In questa sezione si userà Migrazioni Code First di [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) per aggiungere un nuovo campo al modello e migrare questa modifica nel database.</span><span class="sxs-lookup"><span data-stu-id="c3c48-106">In this section you'll use [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) Code First Migrations to add a new field to the model and migrate that change to the database.</span></span>

<span data-ttu-id="c3c48-107">Quando si usa Code First di Entity Framework per creare automaticamente un database, viene aggiunta una tabella al database per rilevare se lo schema del database è sincronizzato con le classi del modello da cui è stato generato.</span><span class="sxs-lookup"><span data-stu-id="c3c48-107">When you use EF Code First to automatically create a database, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="c3c48-108">Se questi elementi non sono sincronizzati, Entity Framework genera un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="c3c48-108">If they aren't in sync, EF throws an exception.</span></span> <span data-ttu-id="c3c48-109">In questo modo è più semplice individuare problemi di incoerenza nel database o nel codice.</span><span class="sxs-lookup"><span data-stu-id="c3c48-109">This makes it easier to find inconsistent database/code issues.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="c3c48-110">Aggiunta di una proprietà Rating al modello Movie</span><span class="sxs-lookup"><span data-stu-id="c3c48-110">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="c3c48-111">Aprire il file *Models/Movie.cs* e aggiungere una proprietà `Rating`:</span><span class="sxs-lookup"><span data-stu-id="c3c48-111">Open the *Models/Movie.cs* file and add a `Rating` property:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]

<span data-ttu-id="c3c48-112">Compilare l'app (CTRL+MAIUSC+B).</span><span class="sxs-lookup"><span data-stu-id="c3c48-112">Build the app (Ctrl+Shift+B).</span></span>

<span data-ttu-id="c3c48-113">Modificare *Pages/Movies/Index.cshtml* e aggiungere un campo `Rating`:</span><span class="sxs-lookup"><span data-stu-id="c3c48-113">Edit *Pages/Movies/Index.cshtml*, and add a `Rating` field:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=40-42,61-63)]

<span data-ttu-id="c3c48-114">Aggiungere il campo `Rating` alle pagine Delete (Elimina) e Details (Dettagli).</span><span class="sxs-lookup"><span data-stu-id="c3c48-114">Add the `Rating` field to the Delete and Details pages.</span></span>

<span data-ttu-id="c3c48-115">Aggiornare *Create.cshtml* con un campo `Rating`.</span><span class="sxs-lookup"><span data-stu-id="c3c48-115">Update *Create.cshtml* with a `Rating` field.</span></span> <span data-ttu-id="c3c48-116">È possibile copiare e incollare l'elemento `<div>` precedente e aggiornare i campi usando intelliSense.</span><span class="sxs-lookup"><span data-stu-id="c3c48-116">You can copy/paste the previous `<div>` element and let intelliSense help you update the fields.</span></span> <span data-ttu-id="c3c48-117">IntelliSense funziona con gli [helper tag](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="c3c48-117">IntelliSense works with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

![Lo sviluppatore ha digitato la lettera R per il valore dell'attributo asp-for nel secondo elemento di etichetta della vista.](new-field/_static/cr.png)

<span data-ttu-id="c3c48-121">Il codice seguente mostra il file *Create.cshtml* con un campo `Rating`:</span><span class="sxs-lookup"><span data-stu-id="c3c48-121">The following code shows *Create.cshtml* with a `Rating` field:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Create.cshtml?highlight=36-40)]

<span data-ttu-id="c3c48-122">Aggiungere il campo `Rating` alla pagina Edit (Modifica).</span><span class="sxs-lookup"><span data-stu-id="c3c48-122">Add the `Rating` field to the Edit Page.</span></span>

<span data-ttu-id="c3c48-123">L'app non funzionerà finché non si aggiorna il database in modo da includere il nuovo campo.</span><span class="sxs-lookup"><span data-stu-id="c3c48-123">The app won't work until the DB is updated to include the new field.</span></span> <span data-ttu-id="c3c48-124">Se si esegue l'app ora, verrà visualizzato un errore `SqlException`:</span><span class="sxs-lookup"><span data-stu-id="c3c48-124">If run now, the app throws a `SqlException`:</span></span>

```
SqlException: Invalid column name 'Rating'.
```

<span data-ttu-id="c3c48-125">Questo errore viene visualizzato perché la classe del modello Movie aggiornata è diversa rispetto allo schema della tabella Movie nel database.</span><span class="sxs-lookup"><span data-stu-id="c3c48-125">This error is caused by the updated Movie model class being different than the schema of the Movie table of the database.</span></span> <span data-ttu-id="c3c48-126">Nella tabella del database non è presente una colonna `Rating`.</span><span class="sxs-lookup"><span data-stu-id="c3c48-126">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="c3c48-127">Per correggere questo errore, esistono alcuni approcci:</span><span class="sxs-lookup"><span data-stu-id="c3c48-127">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="c3c48-128">Fare in modo che Entity Framework elimini e crei di nuovo automaticamente il database usando il nuovo schema di classi del modello.</span><span class="sxs-lookup"><span data-stu-id="c3c48-128">Have the Entity Framework automatically drop and re-create the database using  the new model class schema.</span></span> <span data-ttu-id="c3c48-129">Questo approccio è utile nelle prime fasi del ciclo di sviluppo e consente di migliorare rapidamente lo schema del modello e il database insieme.</span><span class="sxs-lookup"><span data-stu-id="c3c48-129">This approach is convenient early in the development cycle; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="c3c48-130">Lo svantaggio è che si perdono i dati esistenti nel database.</span><span class="sxs-lookup"><span data-stu-id="c3c48-130">The downside is that you lose existing data in the database.</span></span> <span data-ttu-id="c3c48-131">Non usare questo approccio in un database di produzione.</span><span class="sxs-lookup"><span data-stu-id="c3c48-131">You don't want to use this approach on a production database!</span></span> <span data-ttu-id="c3c48-132">L'eliminazione del database per la modifica dello schema e l'uso di un inizializzatore per inizializzare automaticamente un database con i dati di test è spesso un modo produttivo per sviluppare un'app.</span><span class="sxs-lookup"><span data-stu-id="c3c48-132">Dropping the DB on schema changes and using an initializer to automatically seed the database with test data is often a productive way to develop an app.</span></span>

2. <span data-ttu-id="c3c48-133">Modificare esplicitamente lo schema del database esistente in modo che corrisponda alle classi del modello.</span><span class="sxs-lookup"><span data-stu-id="c3c48-133">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="c3c48-134">Il vantaggio di questo approccio è che i dati vengono mantenuti.</span><span class="sxs-lookup"><span data-stu-id="c3c48-134">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="c3c48-135">È possibile apportare questa modifica manualmente o creando uno script di modifica del database.</span><span class="sxs-lookup"><span data-stu-id="c3c48-135">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="c3c48-136">Usare Migrazioni Code First per aggiornare lo schema del database.</span><span class="sxs-lookup"><span data-stu-id="c3c48-136">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="c3c48-137">Per questa esercitazione usare Migrazioni Code First.</span><span class="sxs-lookup"><span data-stu-id="c3c48-137">For this tutorial, use Code First Migrations.</span></span>

<span data-ttu-id="c3c48-138">Aggiornare la classe `SeedData` in modo che fornisca un valore per la nuova colonna.</span><span class="sxs-lookup"><span data-stu-id="c3c48-138">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="c3c48-139">Di seguito viene illustrata una modifica di esempio, ma si apporterà questa modifica per ogni blocco `new Movie`.</span><span class="sxs-lookup"><span data-stu-id="c3c48-139">A sample change is shown below, but you'll want to make this change for each `new Movie` block.</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs?name=snippet1&highlight=8)]

<span data-ttu-id="c3c48-140">Vedere il [file SeedData.cs completato](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs).</span><span class="sxs-lookup"><span data-stu-id="c3c48-140">See the [completed SeedData.cs file](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs).</span></span>

<span data-ttu-id="c3c48-141">Compilare la soluzione.</span><span class="sxs-lookup"><span data-stu-id="c3c48-141">Build the solution.</span></span>

<span data-ttu-id="c3c48-142"><a name="pmc"></a> Dal menu **Strumenti** selezionare **Gestione pacchetti NuGet > Console di Gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="c3c48-142"><a name="pmc"></a> From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>
<span data-ttu-id="c3c48-143">Nella Console di Gestione pacchetti immettere i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="c3c48-143">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration Rating
Update-Database
```

<span data-ttu-id="c3c48-144">Il comando `Add-Migration` indica al framework di:</span><span class="sxs-lookup"><span data-stu-id="c3c48-144">The `Add-Migration` command tells the framework to:</span></span>

* <span data-ttu-id="c3c48-145">Confrontare il modello `Movie` con lo schema di database `Movie`.</span><span class="sxs-lookup"><span data-stu-id="c3c48-145">Compare the `Movie` model with the `Movie` DB schema.</span></span>
* <span data-ttu-id="c3c48-146">Creare codice per migrare lo schema del database nel nuovo modello.</span><span class="sxs-lookup"><span data-stu-id="c3c48-146">Create code to migrate the DB schema to the new model.</span></span>

<span data-ttu-id="c3c48-147">Il nome "Rating" è arbitrario e viene usato per denominare il file di migrazione.</span><span class="sxs-lookup"><span data-stu-id="c3c48-147">The name "Rating" is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="c3c48-148">È consigliabile usare un nome significativo per il file di migrazione.</span><span class="sxs-lookup"><span data-stu-id="c3c48-148">It's helpful to use a meaningful name for the migration file.</span></span>

<span data-ttu-id="c3c48-149"><a name="ssox"></a> Se si eliminano tutti i record nel database, il database verrà inizializzato e verrà incluso il campo `Rating`.</span><span class="sxs-lookup"><span data-stu-id="c3c48-149"><a name="ssox"></a> If you delete all the records in the DB, the initializer will seed the DB and include the `Rating` field.</span></span> <span data-ttu-id="c3c48-150">È possibile eseguire questa operazione con i collegamenti di eliminazione nel browser o da [Sql Server Object Explorer](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span><span class="sxs-lookup"><span data-stu-id="c3c48-150">You can do this with the delete links in the browser or from [Sql Server Object Explorer](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span></span> <span data-ttu-id="c3c48-151">Per eliminare il database da SSOX:</span><span class="sxs-lookup"><span data-stu-id="c3c48-151">To delete the database from SSOX:</span></span>

* <span data-ttu-id="c3c48-152">Selezionare il database in SSOX.</span><span class="sxs-lookup"><span data-stu-id="c3c48-152">Select the database in SSOX.</span></span>
* <span data-ttu-id="c3c48-153">Fare clic con il pulsante destro del mouse sul database e selezionare *Elimina*.</span><span class="sxs-lookup"><span data-stu-id="c3c48-153">Right click on the database, and select *Delete*.</span></span>
* <span data-ttu-id="c3c48-154">Selezionare **Chiudi connessioni esistenti**.</span><span class="sxs-lookup"><span data-stu-id="c3c48-154">Check **Close existing connections**.</span></span>
* <span data-ttu-id="c3c48-155">Scegliere **OK**.</span><span class="sxs-lookup"><span data-stu-id="c3c48-155">Select **OK**.</span></span>
* <span data-ttu-id="c3c48-156">Nella [Console di Gestione pacchetti](xref:tutorials/razor-pages/new-field#pmc) aggiornare il database:</span><span class="sxs-lookup"><span data-stu-id="c3c48-156">In the [PMC](xref:tutorials/razor-pages/new-field#pmc), update the database:</span></span>

  ```powershell
  Update-Database
  ```

<span data-ttu-id="c3c48-157">Eseguire l'app e verificare che sia possibile creare/modificare/visualizzare i film con un campo `Rating`.</span><span class="sxs-lookup"><span data-stu-id="c3c48-157">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="c3c48-158">Se il database non viene inizializzato, arrestare IIS Express e quindi eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="c3c48-158">If the database is not seeded, stop IIS Express, and then run the app.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="c3c48-159">[Precedente: Aggiunta della funzionalità di ricerca](xref:tutorials/razor-pages/search)
[Successivo: Aggiunta della funzionalità di convalida](xref:tutorials/razor-pages/validation)</span><span class="sxs-lookup"><span data-stu-id="c3c48-159">[Previous: Adding Search](xref:tutorials/razor-pages/search)
[Next: Adding Validation](xref:tutorials/razor-pages/validation)</span></span>
