---
title: Aggiunta di un nuovo campo a una pagina Razor
author: rick-anderson
description: Illustra come aggiungere un nuovo campo a una pagina Razor con Entity Framework Core
manager: wpickett
ms.author: riande
ms.date: 08/07/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/new-field
ms.openlocfilehash: 36412e9d1f3143f0d1999d0e754e6627f0984ad5
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2018
---
# <a name="adding-a-new-field-to-a-razor-page"></a><span data-ttu-id="c04ab-103">Aggiunta di un nuovo campo a una pagina Razor</span><span class="sxs-lookup"><span data-stu-id="c04ab-103">Adding a new field to a Razor Page</span></span>

<span data-ttu-id="c04ab-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c04ab-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="c04ab-105">In questa sezione si userà Migrazioni Code First di [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) per aggiungere un nuovo campo al modello e migrare questa modifica nel database.</span><span class="sxs-lookup"><span data-stu-id="c04ab-105">In this section you'll use [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) Code First Migrations to add a new field to the model and migrate that change to the database.</span></span>

<span data-ttu-id="c04ab-106">Quando si usa Code First di Entity Framework per creare automaticamente un database, viene aggiunta una tabella al database per rilevare se lo schema del database è sincronizzato con le classi del modello da cui è stato generato.</span><span class="sxs-lookup"><span data-stu-id="c04ab-106">When you use EF Code First to automatically create a database, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="c04ab-107">Se questi elementi non sono sincronizzati, Entity Framework genera un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="c04ab-107">If they aren't in sync, EF throws an exception.</span></span> <span data-ttu-id="c04ab-108">In questo modo è più semplice individuare problemi di incoerenza nel database o nel codice.</span><span class="sxs-lookup"><span data-stu-id="c04ab-108">This makes it easier to find inconsistent database/code issues.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="c04ab-109">Aggiunta di una proprietà Rating al modello Movie</span><span class="sxs-lookup"><span data-stu-id="c04ab-109">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="c04ab-110">Aprire il file *Models/Movie.cs* e aggiungere una proprietà `Rating`:</span><span class="sxs-lookup"><span data-stu-id="c04ab-110">Open the *Models/Movie.cs* file and add a `Rating` property:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]

<span data-ttu-id="c04ab-111">Compilare l'app (CTRL+MAIUSC+B).</span><span class="sxs-lookup"><span data-stu-id="c04ab-111">Build the app (Ctrl+Shift+B).</span></span>

<span data-ttu-id="c04ab-112">Modificare *Pages/Movies/Index.cshtml* e aggiungere un campo `Rating`:</span><span class="sxs-lookup"><span data-stu-id="c04ab-112">Edit *Pages/Movies/Index.cshtml*, and add a `Rating` field:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=40-42,61-63)]

<span data-ttu-id="c04ab-113">Aggiungere il campo `Rating` alle pagine Delete (Elimina) e Details (Dettagli).</span><span class="sxs-lookup"><span data-stu-id="c04ab-113">Add the `Rating` field to the Delete and Details pages.</span></span>

<span data-ttu-id="c04ab-114">Aggiornare *Create.cshtml* con un campo `Rating`.</span><span class="sxs-lookup"><span data-stu-id="c04ab-114">Update *Create.cshtml* with a `Rating` field.</span></span> <span data-ttu-id="c04ab-115">È possibile copiare e incollare l'elemento `<div>` precedente e aggiornare i campi usando intelliSense.</span><span class="sxs-lookup"><span data-stu-id="c04ab-115">You can copy/paste the previous `<div>` element and let intelliSense help you update the fields.</span></span> <span data-ttu-id="c04ab-116">IntelliSense funziona con gli [helper tag](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="c04ab-116">IntelliSense works with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

![Lo sviluppatore ha digitato la lettera R per il valore dell'attributo asp-for nel secondo elemento di etichetta della vista.](new-field/_static/cr.png)

<span data-ttu-id="c04ab-120">Il codice seguente mostra il file *Create.cshtml* con un campo `Rating`:</span><span class="sxs-lookup"><span data-stu-id="c04ab-120">The following code shows *Create.cshtml* with a `Rating` field:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Create.cshtml?highlight=36-40)]

<span data-ttu-id="c04ab-121">Aggiungere il campo `Rating` alla pagina Edit (Modifica).</span><span class="sxs-lookup"><span data-stu-id="c04ab-121">Add the `Rating` field to the Edit Page.</span></span>

<span data-ttu-id="c04ab-122">L'app non funzionerà finché non si aggiorna il database in modo da includere il nuovo campo.</span><span class="sxs-lookup"><span data-stu-id="c04ab-122">The app won't work until the DB is updated to include the new field.</span></span> <span data-ttu-id="c04ab-123">Se si esegue l'app ora, verrà visualizzato un errore `SqlException`:</span><span class="sxs-lookup"><span data-stu-id="c04ab-123">If run now, the app throws a `SqlException`:</span></span>

```
SqlException: Invalid column name 'Rating'.
```

<span data-ttu-id="c04ab-124">Questo errore viene visualizzato perché la classe del modello Movie aggiornata è diversa rispetto allo schema della tabella Movie nel database.</span><span class="sxs-lookup"><span data-stu-id="c04ab-124">This error is caused by the updated Movie model class being different than the schema of the Movie table of the database.</span></span> <span data-ttu-id="c04ab-125">Nella tabella del database non è presente una colonna `Rating`.</span><span class="sxs-lookup"><span data-stu-id="c04ab-125">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="c04ab-126">Per correggere questo errore, esistono alcuni approcci:</span><span class="sxs-lookup"><span data-stu-id="c04ab-126">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="c04ab-127">Fare in modo che Entity Framework elimini e crei di nuovo automaticamente il database usando il nuovo schema di classi del modello.</span><span class="sxs-lookup"><span data-stu-id="c04ab-127">Have the Entity Framework automatically drop and re-create the database using  the new model class schema.</span></span> <span data-ttu-id="c04ab-128">Questo approccio è utile nelle prime fasi del ciclo di sviluppo e consente di migliorare rapidamente lo schema del modello e il database insieme.</span><span class="sxs-lookup"><span data-stu-id="c04ab-128">This approach is convenient early in the development cycle; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="c04ab-129">Lo svantaggio è che si perdono i dati esistenti nel database.</span><span class="sxs-lookup"><span data-stu-id="c04ab-129">The downside is that you lose existing data in the database.</span></span> <span data-ttu-id="c04ab-130">Non usare questo approccio in un database di produzione.</span><span class="sxs-lookup"><span data-stu-id="c04ab-130">You don't want to use this approach on a production database!</span></span> <span data-ttu-id="c04ab-131">L'eliminazione del database per la modifica dello schema e l'uso di un inizializzatore per inizializzare automaticamente un database con i dati di test è spesso un modo produttivo per sviluppare un'app.</span><span class="sxs-lookup"><span data-stu-id="c04ab-131">Dropping the DB on schema changes and using an initializer to automatically seed the database with test data is often a productive way to develop an app.</span></span>

2. <span data-ttu-id="c04ab-132">Modificare esplicitamente lo schema del database esistente in modo che corrisponda alle classi del modello.</span><span class="sxs-lookup"><span data-stu-id="c04ab-132">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="c04ab-133">Il vantaggio di questo approccio è che i dati vengono mantenuti.</span><span class="sxs-lookup"><span data-stu-id="c04ab-133">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="c04ab-134">È possibile apportare questa modifica manualmente o creando uno script di modifica del database.</span><span class="sxs-lookup"><span data-stu-id="c04ab-134">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="c04ab-135">Usare Migrazioni Code First per aggiornare lo schema del database.</span><span class="sxs-lookup"><span data-stu-id="c04ab-135">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="c04ab-136">Per questa esercitazione usare Migrazioni Code First.</span><span class="sxs-lookup"><span data-stu-id="c04ab-136">For this tutorial, use Code First Migrations.</span></span>

<span data-ttu-id="c04ab-137">Aggiornare la classe `SeedData` in modo che fornisca un valore per la nuova colonna.</span><span class="sxs-lookup"><span data-stu-id="c04ab-137">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="c04ab-138">Di seguito viene illustrata una modifica di esempio, ma si apporterà questa modifica per ogni blocco `new Movie`.</span><span class="sxs-lookup"><span data-stu-id="c04ab-138">A sample change is shown below, but you'll want to make this change for each `new Movie` block.</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs?name=snippet1&highlight=8)]

<span data-ttu-id="c04ab-139">Vedere il [file SeedData.cs completato](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs).</span><span class="sxs-lookup"><span data-stu-id="c04ab-139">See the [completed SeedData.cs file](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs).</span></span>

<span data-ttu-id="c04ab-140">Compilare la soluzione.</span><span class="sxs-lookup"><span data-stu-id="c04ab-140">Build the solution.</span></span>

<a name="pmc"></a> <span data-ttu-id="c04ab-141">Dal menu **Strumenti** selezionare **Gestione pacchetti NuGet > Console di Gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="c04ab-141">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>
<span data-ttu-id="c04ab-142">Nella Console di Gestione pacchetti immettere i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="c04ab-142">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration Rating
Update-Database
```

<span data-ttu-id="c04ab-143">Il comando `Add-Migration` indica al framework di:</span><span class="sxs-lookup"><span data-stu-id="c04ab-143">The `Add-Migration` command tells the framework to:</span></span>

* <span data-ttu-id="c04ab-144">Confrontare il modello `Movie` con lo schema di database `Movie`.</span><span class="sxs-lookup"><span data-stu-id="c04ab-144">Compare the `Movie` model with the `Movie` DB schema.</span></span>
* <span data-ttu-id="c04ab-145">Creare codice per migrare lo schema del database nel nuovo modello.</span><span class="sxs-lookup"><span data-stu-id="c04ab-145">Create code to migrate the DB schema to the new model.</span></span>

<span data-ttu-id="c04ab-146">Il nome "Rating" è arbitrario e viene usato per denominare il file di migrazione.</span><span class="sxs-lookup"><span data-stu-id="c04ab-146">The name "Rating" is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="c04ab-147">È consigliabile usare un nome significativo per il file di migrazione.</span><span class="sxs-lookup"><span data-stu-id="c04ab-147">It's helpful to use a meaningful name for the migration file.</span></span>

<a name="ssox"></a> <span data-ttu-id="c04ab-148">Se si eliminano tutti i record nel database, il database verrà inizializzato e verrà incluso il campo `Rating`.</span><span class="sxs-lookup"><span data-stu-id="c04ab-148">If you delete all the records in the DB, the initializer will seed the DB and include the `Rating` field.</span></span> <span data-ttu-id="c04ab-149">È possibile eseguire questa operazione con i collegamenti di eliminazione nel browser o da [Sql Server Object Explorer](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span><span class="sxs-lookup"><span data-stu-id="c04ab-149">You can do this with the delete links in the browser or from [Sql Server Object Explorer](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span></span> <span data-ttu-id="c04ab-150">Per eliminare il database da SSOX:</span><span class="sxs-lookup"><span data-stu-id="c04ab-150">To delete the database from SSOX:</span></span>

* <span data-ttu-id="c04ab-151">Selezionare il database in SSOX.</span><span class="sxs-lookup"><span data-stu-id="c04ab-151">Select the database in SSOX.</span></span>
* <span data-ttu-id="c04ab-152">Fare clic con il pulsante destro del mouse sul database e selezionare *Elimina*.</span><span class="sxs-lookup"><span data-stu-id="c04ab-152">Right click on the database, and select *Delete*.</span></span>
* <span data-ttu-id="c04ab-153">Selezionare **Chiudi connessioni esistenti**.</span><span class="sxs-lookup"><span data-stu-id="c04ab-153">Check **Close existing connections**.</span></span>
* <span data-ttu-id="c04ab-154">Scegliere **OK**.</span><span class="sxs-lookup"><span data-stu-id="c04ab-154">Select **OK**.</span></span>
* <span data-ttu-id="c04ab-155">Nella [Console di Gestione pacchetti](xref:tutorials/razor-pages/new-field#pmc) aggiornare il database:</span><span class="sxs-lookup"><span data-stu-id="c04ab-155">In the [PMC](xref:tutorials/razor-pages/new-field#pmc), update the database:</span></span>

  ```powershell
  Update-Database
  ```

<span data-ttu-id="c04ab-156">Eseguire l'app e verificare che sia possibile creare/modificare/visualizzare i film con un campo `Rating`.</span><span class="sxs-lookup"><span data-stu-id="c04ab-156">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="c04ab-157">Se il database non viene inizializzato, arrestare IIS Express e quindi eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="c04ab-157">If the database isn't seeded, stop IIS Express, and then run the app.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="c04ab-158">[Precedente: Aggiunta della funzionalità di ricerca](xref:tutorials/razor-pages/search)
[Successivo: Aggiunta della funzionalità di convalida](xref:tutorials/razor-pages/validation)</span><span class="sxs-lookup"><span data-stu-id="c04ab-158">[Previous: Adding Search](xref:tutorials/razor-pages/search)
[Next: Adding Validation](xref:tutorials/razor-pages/validation)</span></span>
