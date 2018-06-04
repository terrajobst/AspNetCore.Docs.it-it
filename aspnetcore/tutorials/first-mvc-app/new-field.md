---
title: Aggiungere un nuovo campo a un'app ASP.NET Core
author: rick-anderson
description: Informazioni sull'uso di Migrazioni Code First di Entity Framework per aggiungere un nuovo campo a un modello ed eseguire la migrazione di questa modifica in un database.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app/new-field
ms.openlocfilehash: a8299871671979264383fe7997e56c6708b2e741
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34729756"
---
# <a name="add-a-new-field-to-an-aspnet-core-app"></a><span data-ttu-id="88f8e-103">Aggiungere un nuovo campo a un'app ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="88f8e-103">Add a new field to an ASP.NET Core app</span></span>

<span data-ttu-id="88f8e-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="88f8e-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="88f8e-105">In questa sezione si userà Migrazioni Code First di [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) per aggiungere un nuovo campo al modello e migrare questa modifica nel database.</span><span class="sxs-lookup"><span data-stu-id="88f8e-105">In this section you'll use [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) Code First Migrations to add a new field to the model and migrate that change to the database.</span></span>

<span data-ttu-id="88f8e-106">Quando si usa Code First di Entity Framework per creare automaticamente un database, viene aggiunta una tabella al database per rilevare se lo schema del database è sincronizzato con le classi del modello da cui è stato generato.</span><span class="sxs-lookup"><span data-stu-id="88f8e-106">When you use EF Code First to automatically create a database, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="88f8e-107">Se questi elementi non sono sincronizzati, Entity Framework genera un'eccezione.</span><span class="sxs-lookup"><span data-stu-id="88f8e-107">If they aren't in sync, EF throws an exception.</span></span> <span data-ttu-id="88f8e-108">In questo modo è più semplice individuare problemi di incoerenza nel database o nel codice.</span><span class="sxs-lookup"><span data-stu-id="88f8e-108">This makes it easier to find inconsistent database/code issues.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="88f8e-109">Aggiunta di una proprietà Rating al modello Movie</span><span class="sxs-lookup"><span data-stu-id="88f8e-109">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="88f8e-110">Aprire il file *Models/Movie.cs* e aggiungere una proprietà `Rating`:</span><span class="sxs-lookup"><span data-stu-id="88f8e-110">Open the *Models/Movie.cs* file and add a `Rating` property:</span></span>

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="88f8e-111">[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Models/MovieDateRating.cs?highlight=13&name=snippet)]</span><span class="sxs-lookup"><span data-stu-id="88f8e-111">[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Models/MovieDateRating.cs?highlight=13&name=snippet)]</span></span>
::: moniker-end
::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="88f8e-112">[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]</span><span class="sxs-lookup"><span data-stu-id="88f8e-112">[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]</span></span>
::: moniker-end

<span data-ttu-id="88f8e-113">Compilare l'app (CTRL+MAIUSC+B).</span><span class="sxs-lookup"><span data-stu-id="88f8e-113">Build the app (Ctrl+Shift+B).</span></span>

<span data-ttu-id="88f8e-114">Poiché è stato aggiunto un nuovo campo alla classe `Movie`, è necessario anche aggiornare l'elenco di elementi di associazione consentiti in modo da includere questa nuova proprietà.</span><span class="sxs-lookup"><span data-stu-id="88f8e-114">Because you've added a new field to the `Movie` class, you also need to update the binding white list so this new property will be included.</span></span> <span data-ttu-id="88f8e-115">In *MoviesController.cs* aggiornare l'attributo `[Bind]` per i metodi di azione `Create` e `Edit` in modo da includere la proprietà `Rating`:</span><span class="sxs-lookup"><span data-stu-id="88f8e-115">In *MoviesController.cs*, update the `[Bind]` attribute for both the `Create` and `Edit` action methods to include the `Rating` property:</span></span>

```csharp
[Bind("ID,Title,ReleaseDate,Genre,Price,Rating")]
   ```

<span data-ttu-id="88f8e-116">È necessario anche aggiornare i modelli di vista per visualizzare, creare e modificare la nuova proprietà `Rating` nella vista del browser.</span><span class="sxs-lookup"><span data-stu-id="88f8e-116">You also need to update the view templates in order to display, create and edit the new `Rating` property in the browser view.</span></span>

<span data-ttu-id="88f8e-117">Modificare il file */Views/Movies/Index.cshtml* e aggiungere un campo `Rating`:</span><span class="sxs-lookup"><span data-stu-id="88f8e-117">Edit the */Views/Movies/Index.cshtml* file and add a `Rating` field:</span></span>

[!code-HTML[](start-mvc/sample/MvcMovie/Views/Movies/IndexGenreRating.cshtml?highlight=17,39&range=24-64)]

<span data-ttu-id="88f8e-118">Aggiornare */Views/Movies/Create.cshtml* con un campo `Rating`.</span><span class="sxs-lookup"><span data-stu-id="88f8e-118">Update the */Views/Movies/Create.cshtml* with a `Rating` field.</span></span> <span data-ttu-id="88f8e-119">È possibile copiare e incollare il "gruppo di moduli" precedente e aggiornare i campi usando intelliSense.</span><span class="sxs-lookup"><span data-stu-id="88f8e-119">You can copy/paste the previous "form group" and let intelliSense help you update the fields.</span></span> <span data-ttu-id="88f8e-120">IntelliSense funziona con gli [helper tag](xref:mvc/views/tag-helpers/intro).</span><span class="sxs-lookup"><span data-stu-id="88f8e-120">IntelliSense works with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="88f8e-121">Nota: nella versione RTM di Visual Studio 2017 è necessario installare [Servizi di linguaggio Razor](https://marketplace.visualstudio.com/items?itemName=ms-madsk.RazorLanguageServices) per intelliSense di Razor.</span><span class="sxs-lookup"><span data-stu-id="88f8e-121">Note: In the RTM verison of Visual Studio 2017 you need to install the [Razor Language Services](https://marketplace.visualstudio.com/items?itemName=ms-madsk.RazorLanguageServices) for Razor intelliSense.</span></span> <span data-ttu-id="88f8e-122">Il problema verrà risolto nella versione successiva.</span><span class="sxs-lookup"><span data-stu-id="88f8e-122">This will be fixed in the next release.</span></span>

![Lo sviluppatore ha digitato la lettera R per il valore dell'attributo asp-for nel secondo elemento di etichetta della vista.](new-field/_static/cr.png)

<span data-ttu-id="88f8e-126">L'app non funzionerà finché non si aggiorna il database in modo da includere il nuovo campo.</span><span class="sxs-lookup"><span data-stu-id="88f8e-126">The app won't work until we update the DB to include the new field.</span></span> <span data-ttu-id="88f8e-127">Se si esegue l'app ora, verrà visualizzato il seguente errore `SqlException`:</span><span class="sxs-lookup"><span data-stu-id="88f8e-127">If you run it now, you'll get the following `SqlException`:</span></span>

`SqlException: Invalid column name 'Rating'.`

<span data-ttu-id="88f8e-128">Questo errore viene visualizzato perché la classe del modello Movie aggiornata è diversa rispetto allo schema della tabella Movie nel database esistente.</span><span class="sxs-lookup"><span data-stu-id="88f8e-128">You're seeing this error because the updated Movie model class is different than the schema of the Movie table of the existing database.</span></span> <span data-ttu-id="88f8e-129">Nella tabella del database non è presente una colonna Rating.</span><span class="sxs-lookup"><span data-stu-id="88f8e-129">(There's no Rating column in the database table.)</span></span>

<span data-ttu-id="88f8e-130">Per correggere questo errore, esistono alcuni approcci:</span><span class="sxs-lookup"><span data-stu-id="88f8e-130">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="88f8e-131">Fare in modo che Entity Framework elimini e crei di nuovo automaticamente il database in base al nuovo schema di classi del modello.</span><span class="sxs-lookup"><span data-stu-id="88f8e-131">Have the Entity Framework automatically drop and re-create the database based on the new model class schema.</span></span> <span data-ttu-id="88f8e-132">Questo approccio è molto utile nelle prime fasi del ciclo di sviluppo in una fase attiva di sviluppo di un database di test e consente di migliorare rapidamente lo schema del modello e il database insieme.</span><span class="sxs-lookup"><span data-stu-id="88f8e-132">This approach is very convenient early in the development cycle when you are doing active development on a test database; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="88f8e-133">Lo svantaggio è che si perdono i dati esistenti nel database e non è quindi possibile usare questo approccio in un database di produzione.</span><span class="sxs-lookup"><span data-stu-id="88f8e-133">The downside, though, is that you lose existing data in the database — so you don't want to use this approach on a production database!</span></span> <span data-ttu-id="88f8e-134">Un modo efficace per sviluppare un'applicazione consiste nell'inizializzare automaticamente un database con dati di test usando un inizializzatore.</span><span class="sxs-lookup"><span data-stu-id="88f8e-134">Using an initializer to automatically seed a database with test data is often a productive way to develop an application.</span></span>

2. <span data-ttu-id="88f8e-135">Modificare esplicitamente lo schema del database esistente in modo che corrisponda alle classi del modello.</span><span class="sxs-lookup"><span data-stu-id="88f8e-135">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="88f8e-136">Il vantaggio di questo approccio è che i dati vengono mantenuti.</span><span class="sxs-lookup"><span data-stu-id="88f8e-136">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="88f8e-137">È possibile apportare questa modifica manualmente o creando uno script di modifica del database.</span><span class="sxs-lookup"><span data-stu-id="88f8e-137">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="88f8e-138">Usare Migrazioni Code First per aggiornare lo schema del database.</span><span class="sxs-lookup"><span data-stu-id="88f8e-138">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="88f8e-139">Per questa esercitazione si userà Migrazioni Code First.</span><span class="sxs-lookup"><span data-stu-id="88f8e-139">For this tutorial, we'll use Code First Migrations.</span></span>

<span data-ttu-id="88f8e-140">Aggiornare la classe `SeedData` in modo che fornisca un valore per la nuova colonna.</span><span class="sxs-lookup"><span data-stu-id="88f8e-140">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="88f8e-141">Di seguito viene illustrata una modifica di esempio, ma si apporterà questa modifica per ogni `new Movie`.</span><span class="sxs-lookup"><span data-stu-id="88f8e-141">A sample change is shown below, but you'll want to make this change for each `new Movie`.</span></span>

[!code-csharp[](start-mvc/sample/MvcMovie/Models/SeedDataRating.cs?name=snippet1&highlight=6)]

<span data-ttu-id="88f8e-142">Compilare la soluzione.</span><span class="sxs-lookup"><span data-stu-id="88f8e-142">Build the solution.</span></span>

<span data-ttu-id="88f8e-143">Dal menu **Strumenti** selezionare **Gestione pacchetti NuGet > Console di Gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="88f8e-143">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>

  ![Menu della Console di Gestione pacchetti](adding-model/_static/pmc.png)

<span data-ttu-id="88f8e-145">Nella Console di Gestione pacchetti immettere i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="88f8e-145">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration Rating
Update-Database
```

<span data-ttu-id="88f8e-146">Il comando `Add-Migration` indica al framework di migrazione di esaminare il modello `Movie` corrente con lo schema di database `Movie` corrente e creare il codice necessario per migrare il database nel nuovo modello.</span><span class="sxs-lookup"><span data-stu-id="88f8e-146">The `Add-Migration` command tells the migration framework to examine the current `Movie` model with the current `Movie` DB schema and create the necessary code to migrate the DB to the new model.</span></span> <span data-ttu-id="88f8e-147">Il nome "Rating" è arbitrario e viene usato per denominare il file di migrazione.</span><span class="sxs-lookup"><span data-stu-id="88f8e-147">The name "Rating" is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="88f8e-148">È consigliabile usare un nome significativo per il file di migrazione.</span><span class="sxs-lookup"><span data-stu-id="88f8e-148">It's helpful to use a meaningful name for the migration file.</span></span>

<span data-ttu-id="88f8e-149">Se si eliminano tutti i record nel database, il database verrà inizializzato e verrà incluso il campo `Rating`.</span><span class="sxs-lookup"><span data-stu-id="88f8e-149">If you delete all the records in the DB, the initialize will seed the DB and include the `Rating` field.</span></span> <span data-ttu-id="88f8e-150">È possibile eseguire questa operazione con i collegamenti di eliminazione nel browser o da SSOX.</span><span class="sxs-lookup"><span data-stu-id="88f8e-150">You can do this with the delete links in the browser or from SSOX.</span></span>

<span data-ttu-id="88f8e-151">Eseguire l'app e verificare che sia possibile creare/modificare/visualizzare i film con un campo `Rating`.</span><span class="sxs-lookup"><span data-stu-id="88f8e-151">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="88f8e-152">Aggiungere anche il campo `Rating` ai modelli di vista `Edit`, `Details` e `Delete`.</span><span class="sxs-lookup"><span data-stu-id="88f8e-152">You should also add the `Rating` field to the `Edit`, `Details`, and `Delete` view templates.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="88f8e-153">[Precedente](search.md)
> [Successivo](validation.md)</span><span class="sxs-lookup"><span data-stu-id="88f8e-153">[Previous](search.md)
[Next](validation.md)</span></span>  
