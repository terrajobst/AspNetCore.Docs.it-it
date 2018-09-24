<!-- This include not used by windows version -->
# <a name="add-a-new-field-to-an-aspnet-core-mvc-app"></a><span data-ttu-id="44075-101">Aggiungere un nuovo campo a un'app ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="44075-101">Add a new field to an ASP.NET Core MVC app</span></span>

<span data-ttu-id="44075-102">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="44075-102">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="44075-103">In questa esercitazione si aggiungerà un nuovo campo alla tabella `Movies`.</span><span class="sxs-lookup"><span data-stu-id="44075-103">This tutorial will add a new field to the `Movies` table.</span></span> <span data-ttu-id="44075-104">Verrà eliminato il database e verrà creato un nuovo database quando si modifica lo schema (aggiunta di un nuovo campo).</span><span class="sxs-lookup"><span data-stu-id="44075-104">We'll drop the database and create a new one when we change the schema (add a new field).</span></span> <span data-ttu-id="44075-105">Questo flusso di lavoro funziona correttamente già in fase di sviluppo, se non sono disponibili dati di produzione da conservare.</span><span class="sxs-lookup"><span data-stu-id="44075-105">This workflow works well early in development when we don't have any production data to perserve.</span></span>

<span data-ttu-id="44075-106">Dopo che l'app è stata distribuita e i dati disponibili devono essere conservati, non è possibile eliminare il database quando è necessario modificare lo schema.</span><span class="sxs-lookup"><span data-stu-id="44075-106">Once your app is deployed and you have data that you need to perserve, you can't drop your DB when you need to change the schema.</span></span> <span data-ttu-id="44075-107">[Migrazioni Code First](/ef/core/get-started/aspnetcore/new-db) di Entity Framework consente di aggiornare lo schema e migrare il database senza perdere i dati.</span><span class="sxs-lookup"><span data-stu-id="44075-107">Entity Framework [Code First Migrations](/ef/core/get-started/aspnetcore/new-db) allows you to update your schema and migrate the database without losing data.</span></span> <span data-ttu-id="44075-108">Si tratta di una funzionalità comune quando si usa SQL Server; SQLlite non supporta molte operazioni dello schema di migrazione e sono possibili solo migrazioni molto semplici.</span><span class="sxs-lookup"><span data-stu-id="44075-108">Migrations is a popular feature when using SQL Server, but SQLlite doesn't support many migration schema operations, so only very simply migrations are possible.</span></span> <span data-ttu-id="44075-109">Per altre informazioni vedere [SQLite Limitations](/ef/core/providers/sqlite/limitations) (Limitazioni di SQLite).</span><span class="sxs-lookup"><span data-stu-id="44075-109">See [SQLite Limitations](/ef/core/providers/sqlite/limitations) for more information.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="44075-110">Aggiunta di una proprietà Rating al modello Movie</span><span class="sxs-lookup"><span data-stu-id="44075-110">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="44075-111">Aprire il file *Models/Movie.cs* e aggiungere una proprietà `Rating`:</span><span class="sxs-lookup"><span data-stu-id="44075-111">Open the *Models/Movie.cs* file and add a `Rating` property:</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Models/MovieDateRating.cs?highlight=12&name=snippet)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]

::: moniker-end

<span data-ttu-id="44075-112">Poiché è stato aggiunto un nuovo campo alla classe `Movie`, è necessario anche aggiornare l'elenco di elementi di associazione in modo da includere questa nuova proprietà.</span><span class="sxs-lookup"><span data-stu-id="44075-112">Because you've added a new field to the `Movie` class, you also need to update the binding whitelist so this new property will be included.</span></span> <span data-ttu-id="44075-113">In *MoviesController.cs* aggiornare l'attributo `[Bind]` per i metodi di azione `Create` e `Edit` in modo da includere la proprietà `Rating`:</span><span class="sxs-lookup"><span data-stu-id="44075-113">In *MoviesController.cs*, update the `[Bind]` attribute for both the `Create` and `Edit` action methods to include the `Rating` property:</span></span>

```csharp
[Bind("ID,Title,ReleaseDate,Genre,Price,Rating")]
   ```

<span data-ttu-id="44075-114">È necessario anche aggiornare i modelli di vista per visualizzare, creare e modificare la nuova proprietà `Rating` nella vista del browser.</span><span class="sxs-lookup"><span data-stu-id="44075-114">You also need to update the view templates in order to display, create, and edit the new `Rating` property in the browser view.</span></span>

<span data-ttu-id="44075-115">Modificare il file */Views/Movies/Index.cshtml* e aggiungere un campo `Rating`:</span><span class="sxs-lookup"><span data-stu-id="44075-115">Edit the */Views/Movies/Index.cshtml* file and add a `Rating` field:</span></span>

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexGenreRating.cshtml?highlight=17,39&range=24-64)]

<span data-ttu-id="44075-116">Aggiornare */Views/Movies/Create.cshtml* con un campo `Rating`.</span><span class="sxs-lookup"><span data-stu-id="44075-116">Update the */Views/Movies/Create.cshtml* with a `Rating` field.</span></span>

<span data-ttu-id="44075-117">L'app non funzionerà finché non si aggiorna il database in modo da includere il nuovo campo.</span><span class="sxs-lookup"><span data-stu-id="44075-117">The app won't work until we update the DB to include the new field.</span></span> <span data-ttu-id="44075-118">Se si esegue l'app ora, verrà visualizzato il seguente errore `SqliteException`:</span><span class="sxs-lookup"><span data-stu-id="44075-118">If you run it now, you'll get the following `SqliteException`:</span></span>

```
SqliteException: SQLite Error 1: 'no such column: m.Rating'.
```

<span data-ttu-id="44075-119">Questo errore viene visualizzato perché la classe del modello Movie aggiornata è diversa rispetto allo schema della tabella Movie nel database esistente.</span><span class="sxs-lookup"><span data-stu-id="44075-119">You're seeing this error because the updated Movie model class is different than the schema of the Movie table of the existing database.</span></span> <span data-ttu-id="44075-120">Nella tabella del database non è presente una colonna `Rating`.</span><span class="sxs-lookup"><span data-stu-id="44075-120">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="44075-121">Per correggere questo errore, esistono alcuni approcci:</span><span class="sxs-lookup"><span data-stu-id="44075-121">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="44075-122">Eliminare il database e fare in modo che Entity Framework crei di nuovo automaticamente il database in base al nuovo schema di classi del modello.</span><span class="sxs-lookup"><span data-stu-id="44075-122">Drop the database and have the Entity Framework automatically re-create the database based on the new model class schema.</span></span> <span data-ttu-id="44075-123">Con questo approccio si perdono i dati esistenti nel database e non è possibile quindi adottare questo approccio con un database di produzione.</span><span class="sxs-lookup"><span data-stu-id="44075-123">With this approach, you lose existing data in the database — so you can't do this with a production database!</span></span> <span data-ttu-id="44075-124">Un modo efficace per sviluppare un'app consiste nell'usare un inizializzatore per inizializzare automaticamente un database con i dati di test.</span><span class="sxs-lookup"><span data-stu-id="44075-124">Using an initializer to automatically seed a database with test data is often a productive way to develop an app.</span></span>

2. <span data-ttu-id="44075-125">Modificare manualmente lo schema del database esistente in modo che corrisponda alle classi del modello.</span><span class="sxs-lookup"><span data-stu-id="44075-125">Manually modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="44075-126">Il vantaggio di questo approccio è che i dati vengono mantenuti.</span><span class="sxs-lookup"><span data-stu-id="44075-126">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="44075-127">È possibile apportare questa modifica manualmente o creando uno script di modifica del database.</span><span class="sxs-lookup"><span data-stu-id="44075-127">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="44075-128">Usare Migrazioni Code First per aggiornare lo schema del database.</span><span class="sxs-lookup"><span data-stu-id="44075-128">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="44075-129">In questa esercitazione il database verrà eliminato e verrà creato di nuovo quando si modifica lo schema.</span><span class="sxs-lookup"><span data-stu-id="44075-129">For this tutorial, we'll drop and re-create the database when the schema changes.</span></span> <span data-ttu-id="44075-130">Per eliminare il database, eseguire il comando seguente da un terminale:</span><span class="sxs-lookup"><span data-stu-id="44075-130">Run the following command from a terminal to drop the db:</span></span>

`dotnet ef database drop`

<span data-ttu-id="44075-131">Aggiornare la classe `SeedData` in modo che fornisca un valore per la nuova colonna.</span><span class="sxs-lookup"><span data-stu-id="44075-131">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="44075-132">Di seguito viene illustrata una modifica di esempio, ma si apporterà questa modifica per ogni `new Movie`.</span><span class="sxs-lookup"><span data-stu-id="44075-132">A sample change is shown below, but you'll want to make this change for each `new Movie`.</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/SeedDataRating.cs?name=snippet1&highlight=6)]

<span data-ttu-id="44075-133">Aggiungere il campo `Rating` alla vista `Edit`, `Details` e `Delete`.</span><span class="sxs-lookup"><span data-stu-id="44075-133">Add the `Rating` field to the `Edit`, `Details`, and `Delete` view.</span></span>

<span data-ttu-id="44075-134">Eseguire l'app e verificare che sia possibile creare/modificare/visualizzare i film con un campo `Rating`.</span><span class="sxs-lookup"><span data-stu-id="44075-134">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="44075-135">modelli.</span><span class="sxs-lookup"><span data-stu-id="44075-135">templates.</span></span>
