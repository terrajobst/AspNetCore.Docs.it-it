---
title: Aggiungere un nuovo campo a un'app ASP.NET Core MVC
author: rick-anderson
description: Informazioni sull'uso di Migrazioni Code First di Entity Framework per aggiungere un nuovo campo a un modello ed eseguire la migrazione di questa modifica in un database.
ms.author: riande
ms.custom: mvc
ms.date: 12/13/2018
uid: tutorials/first-mvc-app/new-field
ms.openlocfilehash: 7993b36bf9115225e082d2929bb253aba5b18310
ms.sourcegitcommit: cec77d5ad8a0cedb1ecbec32834111492afd0cd2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/10/2019
ms.locfileid: "54207369"
---
# <a name="add-a-new-field-to-an-aspnet-core-mvc-app"></a><span data-ttu-id="5e7a3-103">Aggiungere un nuovo campo a un'app ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="5e7a3-103">Add a new field to an ASP.NET Core MVC app</span></span>

<span data-ttu-id="5e7a3-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="5e7a3-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="5e7a3-105">In questa sezione viene usato Migrazioni Code First di [Entity Framework](/ef/core/get-started/aspnetcore/new-db) per:</span><span class="sxs-lookup"><span data-stu-id="5e7a3-105">In this section [Entity Framework](/ef/core/get-started/aspnetcore/new-db) Code First Migrations is used to:</span></span>

* <span data-ttu-id="5e7a3-106">Aggiungere un nuovo campo al modello.</span><span class="sxs-lookup"><span data-stu-id="5e7a3-106">Add a new field to the model.</span></span>
* <span data-ttu-id="5e7a3-107">Eseguire la migrazione del nuovo campo al database.</span><span class="sxs-lookup"><span data-stu-id="5e7a3-107">Migrate the new field to the database.</span></span>

<span data-ttu-id="5e7a3-108">Quando si usa EF Code First per creare automaticamente un database, Code First:</span><span class="sxs-lookup"><span data-stu-id="5e7a3-108">When EF Code First is used to automatically create a database, Code First:</span></span>

* <span data-ttu-id="5e7a3-109">Aggiunge una tabella al database per rilevare lo schema del database.</span><span class="sxs-lookup"><span data-stu-id="5e7a3-109">Adds a table to the database to  track the schema of the database.</span></span>
* <span data-ttu-id="5e7a3-110">Verifica che il database sia sincronizzato con le classi del modello dalle quali è stato generato.</span><span class="sxs-lookup"><span data-stu-id="5e7a3-110">Verify the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="5e7a3-111">Se questi elementi non sono sincronizzati, Entity Framework genera un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="5e7a3-111">If they aren't in sync, EF throws an exception.</span></span> <span data-ttu-id="5e7a3-112">In questo modo è più semplice individuare problemi di incoerenza nel database o nel codice.</span><span class="sxs-lookup"><span data-stu-id="5e7a3-112">This makes it easier to find inconsistent database/code issues.</span></span>

## <a name="add-a-rating-property-to-the-movie-model"></a><span data-ttu-id="5e7a3-113">Aggiungere una proprietà Rating al modello Movie</span><span class="sxs-lookup"><span data-stu-id="5e7a3-113">Add a Rating Property to the Movie Model</span></span>

<span data-ttu-id="5e7a3-114">Aggiungere una proprietà `Rating` a *Models/Movie.cs*:</span><span class="sxs-lookup"><span data-stu-id="5e7a3-114">Add a `Rating` property to *Models/Movie.cs*:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Models/MovieDateRating.cs?highlight=13&name=snippet)]

<span data-ttu-id="5e7a3-115">Compilare l'app (CTRL+MAIUSC+B).</span><span class="sxs-lookup"><span data-stu-id="5e7a3-115">Build the app (Ctrl+Shift+B).</span></span>

<span data-ttu-id="5e7a3-116">Poiché è stato aggiunto un nuovo campo alla classe `Movie`, è necessario aggiornare l'elenco di elementi di associazione consentiti per includere questa nuova proprietà.</span><span class="sxs-lookup"><span data-stu-id="5e7a3-116">Because you've added a new field to the `Movie` class, you need to update the binding white list so this new property will be included.</span></span> <span data-ttu-id="5e7a3-117">In *MoviesController.cs* aggiornare l'attributo `[Bind]` per i metodi di azione `Create` e `Edit` in modo da includere la proprietà `Rating`:</span><span class="sxs-lookup"><span data-stu-id="5e7a3-117">In *MoviesController.cs*, update the `[Bind]` attribute for both the `Create` and `Edit` action methods to include the `Rating` property:</span></span>

```csharp
[Bind("ID,Title,ReleaseDate,Genre,Price,Rating")]
   ```

<span data-ttu-id="5e7a3-118">Aggiornare i modelli di vista per visualizzare, creare e modificare la nuova proprietà `Rating` nella vista del browser.</span><span class="sxs-lookup"><span data-stu-id="5e7a3-118">Update the view templates in order to display, create, and edit the new `Rating` property in the browser view.</span></span>

<span data-ttu-id="5e7a3-119">Modificare il file */Views/Movies/Index.cshtml* e aggiungere un campo `Rating`:</span><span class="sxs-lookup"><span data-stu-id="5e7a3-119">Edit the */Views/Movies/Index.cshtml* file and add a `Rating` field:</span></span>

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexGenreRating.cshtml?highlight=16,38&range=24-64)]

<span data-ttu-id="5e7a3-120">Aggiornare */Views/Movies/Create.cshtml* con un campo `Rating`.</span><span class="sxs-lookup"><span data-stu-id="5e7a3-120">Update the */Views/Movies/Create.cshtml* with a `Rating` field.</span></span>

<!-- VS -------------------------->
# <a name="visual-studio--visual-studio-for-mactabvisual-studiovisual-studio-mac"></a>[<span data-ttu-id="5e7a3-121">Visual Studio/Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="5e7a3-121">Visual Studio / Visual Studio for Mac</span></span>](#tab/visual-studio+visual-studio-mac)

<span data-ttu-id="5e7a3-122">È possibile copiare e incollare il "gruppo di moduli" precedente e aggiornare i campi usando intelliSense.</span><span class="sxs-lookup"><span data-stu-id="5e7a3-122">You can copy/paste the previous "form group" and let intelliSense help you update the fields.</span></span> <span data-ttu-id="5e7a3-123">IntelliSense funziona con gli [helper tag](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="5e7a3-123">IntelliSense works with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

![Lo sviluppatore ha digitato la lettera R per il valore dell'attributo asp-for nel secondo elemento di etichetta della vista.](new-field/_static/cr.png)

<!-- Code -------------------------->
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="5e7a3-127">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5e7a3-127">Visual Studio Code</span></span>](#tab/visual-studio-code)
<!-- This tab intentionally left blank. -->
---  
<!-- End of VS tabs -->

<span data-ttu-id="5e7a3-128">Aggiornare la classe `SeedData` in modo che fornisca un valore per la nuova colonna.</span><span class="sxs-lookup"><span data-stu-id="5e7a3-128">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="5e7a3-129">Di seguito viene illustrata una modifica di esempio, ma si apporterà questa modifica per ogni `new Movie`.</span><span class="sxs-lookup"><span data-stu-id="5e7a3-129">A sample change is shown below, but you'll want to make this change for each `new Movie`.</span></span>

[!code-csharp[](start-mvc/sample/MvcMovie/Models/SeedDataRating.cs?name=snippet1&highlight=6)]

<span data-ttu-id="5e7a3-130">L'app non funzionerà finché non si aggiorna il database in modo da includere il nuovo campo.</span><span class="sxs-lookup"><span data-stu-id="5e7a3-130">The app won't work until the DB is updated to include the new field.</span></span> <span data-ttu-id="5e7a3-131">Se viene eseguita a questo punto, viene generato il seguente errore `SqlException`:</span><span class="sxs-lookup"><span data-stu-id="5e7a3-131">If it's run now, the following `SqlException` is thrown:</span></span>

`SqlException: Invalid column name 'Rating'.`

<span data-ttu-id="5e7a3-132">Questo errore si verifica perché la classe del modello Movie aggiornata è diversa dallo schema della tabella Movie nel database esistente.</span><span class="sxs-lookup"><span data-stu-id="5e7a3-132">This error occurs because the updated Movie model class is different than the schema of the Movie table of the existing database.</span></span> <span data-ttu-id="5e7a3-133">Nella tabella del database non è presente una colonna `Rating`.</span><span class="sxs-lookup"><span data-stu-id="5e7a3-133">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="5e7a3-134">Per correggere questo errore, esistono alcuni approcci:</span><span class="sxs-lookup"><span data-stu-id="5e7a3-134">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="5e7a3-135">Fare in modo che Entity Framework elimini e crei di nuovo automaticamente il database in base al nuovo schema di classi del modello.</span><span class="sxs-lookup"><span data-stu-id="5e7a3-135">Have the Entity Framework automatically drop and re-create the database based on the new model class schema.</span></span> <span data-ttu-id="5e7a3-136">Questo approccio è molto utile nelle prime fasi del ciclo di sviluppo in una fase attiva dello sviluppo di un database di test e consente di migliorare rapidamente lo schema del modello e il database insieme.</span><span class="sxs-lookup"><span data-stu-id="5e7a3-136">This approach is very convenient early in the development cycle when you're doing active development on a test database; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="5e7a3-137">Lo svantaggio è che si perdono i dati esistenti nel database e non è quindi possibile usare questo approccio in un database di produzione.</span><span class="sxs-lookup"><span data-stu-id="5e7a3-137">The downside, though, is that you lose existing data in the database — so you don't want to use this approach on a production database!</span></span> <span data-ttu-id="5e7a3-138">Un modo efficace per sviluppare un'applicazione consiste nell'inizializzare automaticamente un database con dati di test usando un inizializzatore.</span><span class="sxs-lookup"><span data-stu-id="5e7a3-138">Using an initializer to automatically seed a database with test data is often a productive way to develop an application.</span></span> <span data-ttu-id="5e7a3-139">Questo è un approccio ottimale per le fasi iniziali dello sviluppo e quando si usa SQLite.</span><span class="sxs-lookup"><span data-stu-id="5e7a3-139">This is a good approach for early development and when using SQLite.</span></span>

2. <span data-ttu-id="5e7a3-140">Modificare esplicitamente lo schema del database esistente in modo che corrisponda alle classi del modello.</span><span class="sxs-lookup"><span data-stu-id="5e7a3-140">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="5e7a3-141">Il vantaggio di questo approccio è che i dati vengono mantenuti.</span><span class="sxs-lookup"><span data-stu-id="5e7a3-141">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="5e7a3-142">È possibile apportare questa modifica manualmente o creando uno script di modifica del database.</span><span class="sxs-lookup"><span data-stu-id="5e7a3-142">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="5e7a3-143">Usare Migrazioni Code First per aggiornare lo schema del database.</span><span class="sxs-lookup"><span data-stu-id="5e7a3-143">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="5e7a3-144">Per questa esercitazione viene usato Migrazioni Code First.</span><span class="sxs-lookup"><span data-stu-id="5e7a3-144">For this tutorial, Code First Migrations is used.</span></span>

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5e7a3-145">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5e7a3-145">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="5e7a3-146">Dal menu **Strumenti** selezionare **Gestione pacchetti NuGet > Console di Gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="5e7a3-146">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>

  ![Menu della Console di Gestione pacchetti](adding-model/_static/pmc.png)

<span data-ttu-id="5e7a3-148">Nella Console di Gestione pacchetti immettere i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="5e7a3-148">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration Rating
Update-Database
```

<span data-ttu-id="5e7a3-149">Il comando `Add-Migration` indica al framework di migrazione di esaminare il modello `Movie` corrente con lo schema di database `Movie` corrente e creare il codice necessario per migrare il database nel nuovo modello.</span><span class="sxs-lookup"><span data-stu-id="5e7a3-149">The `Add-Migration` command tells the migration framework to examine the current `Movie` model with the current `Movie` DB schema and create the necessary code to migrate the DB to the new model.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="5e7a3-150">Visual Studio Code/Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="5e7a3-150">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

<span data-ttu-id="5e7a3-151">Eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="5e7a3-151">Run the following command:</span></span>

```cli
dotnet ef migrations add Rating
dotnet ef database update
```

---  
<!-- End of VS tabs -->

<span data-ttu-id="5e7a3-152">Il nome "Rating" è arbitrario e viene usato per denominare il file di migrazione.</span><span class="sxs-lookup"><span data-stu-id="5e7a3-152">The name "Rating" is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="5e7a3-153">È consigliabile usare un nome significativo per il file di migrazione.</span><span class="sxs-lookup"><span data-stu-id="5e7a3-153">It's helpful to use a meaningful name for the migration file.</span></span>

<span data-ttu-id="5e7a3-154">Se vengono eliminati tutti i record del database, il database viene inizializzato dal metodo di inizializzazione e viene incluso il campo `Rating`.</span><span class="sxs-lookup"><span data-stu-id="5e7a3-154">If all the records in the DB are deleted, the initialize method will seed the DB and include the `Rating` field.</span></span>

<span data-ttu-id="5e7a3-155">Eseguire l'app e verificare che sia possibile creare/modificare/visualizzare i film con un campo `Rating`.</span><span class="sxs-lookup"><span data-stu-id="5e7a3-155">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="5e7a3-156">È consigliabile aggiungere il campo `Rating` ai modelli di vista `Edit`, `Details` e `Delete`.</span><span class="sxs-lookup"><span data-stu-id="5e7a3-156">You should add the `Rating` field to the `Edit`, `Details`, and `Delete` view templates.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="5e7a3-157">[Precedente](search.md)
> [Successivo](validation.md)</span><span class="sxs-lookup"><span data-stu-id="5e7a3-157">[Previous](search.md)
[Next](validation.md)</span></span>  
