---
title: Aggiunta di un nuovo campo a una pagina Razor
author: rick-anderson
description: Illustra come aggiungere un nuovo campo a una pagina Razor con Entity Framework Core
keywords: ASP.NET Core, Entity Framework Core, migrazioni
ms.author: riande
manager: wpickett
ms.date: 8/7/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/new-field
ms.openlocfilehash: bda00f290043251ad308192c5b1a873ae7cd0d85
ms.sourcegitcommit: e832a9b9f41a8b26a8c88edfd8fc35b8bfd97d5d
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/18/2017
---
# <a name="adding-a-new-field-to-a-razor-page"></a><span data-ttu-id="4ad80-104">Aggiunta di un nuovo campo a una pagina Razor</span><span class="sxs-lookup"><span data-stu-id="4ad80-104">Adding a new field to a Razor Page</span></span>

<span data-ttu-id="4ad80-105">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="4ad80-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="4ad80-106">In questa sezione si userà Migrazioni Code First di [Entity Framework](http://docs.efproject.net/en/latest/platforms/aspnetcore/new-db.html) per aggiungere un nuovo campo al modello e migrare questa modifica nel database.</span><span class="sxs-lookup"><span data-stu-id="4ad80-106">In this section you'll use [Entity Framework](http://docs.efproject.net/en/latest/platforms/aspnetcore/new-db.html) Code First Migrations to add a new field to the model and migrate that change to the database.</span></span>

<span data-ttu-id="4ad80-107">Quando si usa Code First di Entity Framework per creare automaticamente un database, viene aggiunta una tabella al database per rilevare se lo schema del database è sincronizzato con le classi del modello da cui è stato generato.</span><span class="sxs-lookup"><span data-stu-id="4ad80-107">When you use EF Code First to automatically create a database, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="4ad80-108">Se questi elementi non sono sincronizzati, Entity Framework genera un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="4ad80-108">If they aren't in sync, EF throws an exception.</span></span> <span data-ttu-id="4ad80-109">In questo modo è più semplice individuare problemi di incoerenza nel database o nel codice.</span><span class="sxs-lookup"><span data-stu-id="4ad80-109">This makes it easier to find inconsistent database/code issues.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="4ad80-110">Aggiunta di una proprietà Rating al modello Movie</span><span class="sxs-lookup"><span data-stu-id="4ad80-110">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="4ad80-111">Aprire il file *Models/Movie.cs* e aggiungere una proprietà `Rating`:</span><span class="sxs-lookup"><span data-stu-id="4ad80-111">Open the *Models/Movie.cs* file and add a `Rating` property:</span></span>

<span data-ttu-id="4ad80-112">[!code-csharp[Principale](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]</span><span class="sxs-lookup"><span data-stu-id="4ad80-112">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]</span></span>

<span data-ttu-id="4ad80-113">Compilare l'app (CTRL+MAIUSC+B).</span><span class="sxs-lookup"><span data-stu-id="4ad80-113">Build the app (Ctrl+Shift+B).</span></span>

<span data-ttu-id="4ad80-114">Modificare *Pages/Movies/Index.cshtml* e aggiungere un campo `Rating`:</span><span class="sxs-lookup"><span data-stu-id="4ad80-114">Edit *Pages/Movies/Index.cshtml*, and add a `Rating` field:</span></span>

<span data-ttu-id="4ad80-115">[!code-cshtml[Principale](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=40-42,61-63)]</span><span class="sxs-lookup"><span data-stu-id="4ad80-115">[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=40-42,61-63)]</span></span>

<span data-ttu-id="4ad80-116">Aggiungere il campo `Rating` alle pagine Delete (Elimina) e Details (Dettagli).</span><span class="sxs-lookup"><span data-stu-id="4ad80-116">Add the `Rating` field to the Delete and Details pages.</span></span>

<span data-ttu-id="4ad80-117">Aggiornare *Create.cshtml* con un campo `Rating`.</span><span class="sxs-lookup"><span data-stu-id="4ad80-117">Update *Create.cshtml* with a `Rating` field.</span></span> <span data-ttu-id="4ad80-118">È possibile copiare e incollare l'elemento `<div>` precedente e aggiornare i campi usando intelliSense.</span><span class="sxs-lookup"><span data-stu-id="4ad80-118">You can copy/paste the previous `<div>` element and let intelliSense help you update the fields.</span></span> <span data-ttu-id="4ad80-119">IntelliSense funziona con gli [helper tag](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="4ad80-119">IntelliSense works with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

![Lo sviluppatore ha digitato la lettera R per il valore dell'attributo asp-for nel secondo elemento di etichetta della vista.](new-field/_static/cr.png)

<span data-ttu-id="4ad80-123">Il codice seguente mostra il file *Create.cshtml* con un campo `Rating`:</span><span class="sxs-lookup"><span data-stu-id="4ad80-123">The following code shows *Create.cshtml* with a `Rating` field:</span></span>

<span data-ttu-id="4ad80-124">[!code-cshtml[Principale](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Create.cshtml?highlight=31-35)]</span><span class="sxs-lookup"><span data-stu-id="4ad80-124">[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Create.cshtml?highlight=31-35)]</span></span>

<span data-ttu-id="4ad80-125">Aggiungere il campo `Rating` alla pagina Edit (Modifica).</span><span class="sxs-lookup"><span data-stu-id="4ad80-125">Add the `Rating` field to the Edit Page.</span></span>

<span data-ttu-id="4ad80-126">L'app non funzionerà finché non si aggiorna il database in modo da includere il nuovo campo.</span><span class="sxs-lookup"><span data-stu-id="4ad80-126">The app won't work until the DB is updated to include the new field.</span></span> <span data-ttu-id="4ad80-127">Se si esegue l'app ora, verrà visualizzato un errore `SqlException`:</span><span class="sxs-lookup"><span data-stu-id="4ad80-127">If run now, the app throws a `SqlException`:</span></span>

`SqlException: Invalid column name 'Rating'.`

<span data-ttu-id="4ad80-128">Questo errore viene visualizzato perché la classe del modello Movie aggiornata è diversa rispetto allo schema della tabella Movie nel database.</span><span class="sxs-lookup"><span data-stu-id="4ad80-128">This error is caused by the updated Movie model class being different than the schema of the Movie table of the database.</span></span> <span data-ttu-id="4ad80-129">Nella tabella del database non è presente una colonna `Rating`.</span><span class="sxs-lookup"><span data-stu-id="4ad80-129">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="4ad80-130">Per correggere questo errore, esistono alcuni approcci:</span><span class="sxs-lookup"><span data-stu-id="4ad80-130">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="4ad80-131">Fare in modo che Entity Framework elimini e crei di nuovo automaticamente il database usando il nuovo schema di classi del modello.</span><span class="sxs-lookup"><span data-stu-id="4ad80-131">Have the Entity Framework automatically drop and re-create the database using  the new model class schema.</span></span> <span data-ttu-id="4ad80-132">Questo approccio è utile nelle prime fasi del ciclo di sviluppo e consente di migliorare rapidamente lo schema del modello e il database insieme.</span><span class="sxs-lookup"><span data-stu-id="4ad80-132">This approach is convenient early in the development cycle; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="4ad80-133">Lo svantaggio è che si perdono i dati esistenti nel database.</span><span class="sxs-lookup"><span data-stu-id="4ad80-133">The downside is that you lose existing data in the database.</span></span> <span data-ttu-id="4ad80-134">Non usare questo approccio in un database di produzione.</span><span class="sxs-lookup"><span data-stu-id="4ad80-134">You don't want to use this approach on a production database!</span></span> <span data-ttu-id="4ad80-135">L'eliminazione del database per la modifica dello schema e l'uso di un inizializzatore per inizializzare automaticamente un database con i dati di test è spesso un modo produttivo per sviluppare un'app.</span><span class="sxs-lookup"><span data-stu-id="4ad80-135">Dropping the DB on schema changes and using an initializer to automatically seed the database with test data is often a productive way to develop an app.</span></span>

2. <span data-ttu-id="4ad80-136">Modificare esplicitamente lo schema del database esistente in modo che corrisponda alle classi del modello.</span><span class="sxs-lookup"><span data-stu-id="4ad80-136">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="4ad80-137">Il vantaggio di questo approccio è che i dati vengono mantenuti.</span><span class="sxs-lookup"><span data-stu-id="4ad80-137">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="4ad80-138">È possibile apportare questa modifica manualmente o creando uno script di modifica del database.</span><span class="sxs-lookup"><span data-stu-id="4ad80-138">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="4ad80-139">Usare Migrazioni Code First per aggiornare lo schema del database.</span><span class="sxs-lookup"><span data-stu-id="4ad80-139">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="4ad80-140">Per questa esercitazione usare Migrazioni Code First.</span><span class="sxs-lookup"><span data-stu-id="4ad80-140">For this tutorial, use Code First Migrations.</span></span>

<span data-ttu-id="4ad80-141">Aggiornare la classe `SeedData` in modo che fornisca un valore per la nuova colonna.</span><span class="sxs-lookup"><span data-stu-id="4ad80-141">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="4ad80-142">Di seguito viene illustrata una modifica di esempio, ma si apporterà questa modifica per ogni blocco `new Movie`.</span><span class="sxs-lookup"><span data-stu-id="4ad80-142">A sample change is shown below, but you'll want to make this change for each `new Movie` block.</span></span>

<span data-ttu-id="4ad80-143">[!code-csharp[Principale](razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs?name=snippet1&highlight=6)]</span><span class="sxs-lookup"><span data-stu-id="4ad80-143">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs?name=snippet1&highlight=6)]</span></span>

<span data-ttu-id="4ad80-144">Vedere il [file SeedData.cs completato](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs).</span><span class="sxs-lookup"><span data-stu-id="4ad80-144">See the [completed SeedData.cs file](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs).</span></span>

<span data-ttu-id="4ad80-145">Compilare la soluzione.</span><span class="sxs-lookup"><span data-stu-id="4ad80-145">Build the solution.</span></span>

<a name="pmc"></a>

<span data-ttu-id="4ad80-146">Dal menu **Strumenti** selezionare **Gestione pacchetti NuGet > Console di Gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="4ad80-146">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>
<span data-ttu-id="4ad80-147">Nella Console di Gestione pacchetti immettere i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="4ad80-147">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration Rating
Update-Database
```

<span data-ttu-id="4ad80-148">Il comando `Add-Migration` indica al framework di:</span><span class="sxs-lookup"><span data-stu-id="4ad80-148">The `Add-Migration` command tells the framework to:</span></span>

* <span data-ttu-id="4ad80-149">Confrontare il modello `Movie` con lo schema di database `Movie`.</span><span class="sxs-lookup"><span data-stu-id="4ad80-149">Compare the `Movie` model with the `Movie` DB schema.</span></span>
* <span data-ttu-id="4ad80-150">Creare codice per migrare lo schema del database nel nuovo modello.</span><span class="sxs-lookup"><span data-stu-id="4ad80-150">Create code to migrate the DB schema to the new model.</span></span>

<span data-ttu-id="4ad80-151">Il nome "Rating" è arbitrario e viene usato per denominare il file di migrazione.</span><span class="sxs-lookup"><span data-stu-id="4ad80-151">The name "Rating" is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="4ad80-152">È consigliabile usare un nome significativo per il file di migrazione.</span><span class="sxs-lookup"><span data-stu-id="4ad80-152">It's helpful to use a meaningful name for the migration file.</span></span>

<span data-ttu-id="4ad80-153"><a name="ssox"></a> Se si eliminano tutti i record nel database, il database verrà inizializzato e verrà incluso il campo `Rating`.</span><span class="sxs-lookup"><span data-stu-id="4ad80-153"><a name="ssox"></a> If you delete all the records in the DB, the initializer will seed the DB and include the `Rating` field.</span></span> <span data-ttu-id="4ad80-154">È possibile eseguire questa operazione con i collegamenti di eliminazione nel browser o da [Sql Server Object Explorer](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span><span class="sxs-lookup"><span data-stu-id="4ad80-154">You can do this with the delete links in the browser or from [Sql Server Object Explorer](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span></span> <span data-ttu-id="4ad80-155">Per eliminare il database da SSOX:</span><span class="sxs-lookup"><span data-stu-id="4ad80-155">To delete the database from SSOX:</span></span>

* <span data-ttu-id="4ad80-156">Selezionare il database in SSOX.</span><span class="sxs-lookup"><span data-stu-id="4ad80-156">Select the database in SSOX.</span></span>
* <span data-ttu-id="4ad80-157">Fare clic con il pulsante destro del mouse sul database e selezionare *Elimina*.</span><span class="sxs-lookup"><span data-stu-id="4ad80-157">Right click on the database, and select *Delete*.</span></span>
* <span data-ttu-id="4ad80-158">Selezionare **Chiudi connessioni esistenti*</span><span class="sxs-lookup"><span data-stu-id="4ad80-158">Check **Close existing connections*</span></span>
* <span data-ttu-id="4ad80-159">Scegliere **OK**.</span><span class="sxs-lookup"><span data-stu-id="4ad80-159">Select **OK**</span></span>
* <span data-ttu-id="4ad80-160">Nella [Console di gestione pacchetti](xref:tutorials/razor-pages/new-field#pmc) aggiornare il database</span><span class="sxs-lookup"><span data-stu-id="4ad80-160">In the [PMC](xref:tutorials/razor-pages/new-field#pmc), update the database</span></span> 

    ```PMC
    Update-Database
    ```

<span data-ttu-id="4ad80-161">Eseguire l'app e verificare che sia possibile creare/modificare/visualizzare i film con un campo `Rating`.</span><span class="sxs-lookup"><span data-stu-id="4ad80-161">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="4ad80-162">Se il database non viene inizializzato, arrestare IIS Express e quindi eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="4ad80-162">If the database is not seeded, stop IIS Express, and then run the app.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="4ad80-163">[Precedente: Aggiunta della funzionalità di ricerca](xref:tutorials/razor-pages/search)
[Successivo: Aggiunta di un nuovo campo](xref:tutorials/razor-pages/new-field)</span><span class="sxs-lookup"><span data-stu-id="4ad80-163">[Previous: Adding Search](xref:tutorials/razor-pages/search)
[Next: Adding a new field](xref:tutorials/razor-pages/new-field)</span></span>
