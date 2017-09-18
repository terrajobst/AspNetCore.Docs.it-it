# <a name="working-with-sqlite-in-an-aspnet-core-mvc-project"></a><span data-ttu-id="d62a1-101">Utilizzo di SQLite in un progetto ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="d62a1-101">Working with SQLite in an ASP.NET Core MVC project</span></span>

<span data-ttu-id="d62a1-102">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="d62a1-102">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="d62a1-103">L'oggetto `MvcMovieContext` gestisce l'attività di connessione al database e di mapping degli oggetti `Movie` ai record di database.</span><span class="sxs-lookup"><span data-stu-id="d62a1-103">The `MvcMovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="d62a1-104">Il contesto del database viene registrato nel contenitore di [inserimento dipendenze](xref:fundamentals/dependency-injection) nel metodo `ConfigureServices` nel file *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="d62a1-104">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

<span data-ttu-id="d62a1-105">[!code-csharp[Principale](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet2&highlight=6-8)]</span><span class="sxs-lookup"><span data-stu-id="d62a1-105">[!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet2&highlight=6-8)]</span></span>

## <a name="sqlite"></a><span data-ttu-id="d62a1-106">SQLite</span><span class="sxs-lookup"><span data-stu-id="d62a1-106">SQLite</span></span>

<span data-ttu-id="d62a1-107">Nel sito Web di [SQLite](https://www.sqlite.org/) si dichiara:</span><span class="sxs-lookup"><span data-stu-id="d62a1-107">The [SQLite](https://www.sqlite.org/) website states:</span></span>

> <span data-ttu-id="d62a1-108">SQLite è un motore di database SQL autonomo, a elevata affidabilità, incorporato, completo e di dominio pubblico.</span><span class="sxs-lookup"><span data-stu-id="d62a1-108">SQLite is a self-contained, high-reliability, embedded, full-featured, public-domain, SQL database engine.</span></span> <span data-ttu-id="d62a1-109">SQLite è il motore di database più usato in tutto il mondo.</span><span class="sxs-lookup"><span data-stu-id="d62a1-109">SQLite is the most used database engine in the world.</span></span>

<span data-ttu-id="d62a1-110">Per gestire e visualizzare un database SQLite, sono disponibili numerosi strumenti di terze parti che è possibile scaricare.</span><span class="sxs-lookup"><span data-stu-id="d62a1-110">There are many third party tools you can download to manage and view a SQLite database.</span></span> <span data-ttu-id="d62a1-111">L'immagine seguente è stata tratta da [DB Browser per SQLite](http://sqlitebrowser.org/).</span><span class="sxs-lookup"><span data-stu-id="d62a1-111">The image below is from [DB Browser for SQLite](http://sqlitebrowser.org/).</span></span> <span data-ttu-id="d62a1-112">Se si preferisce uno strumento SQLite in particolare, aggiungere un commento sui motivi della preferenza.</span><span class="sxs-lookup"><span data-stu-id="d62a1-112">If you have a favorite SQLite tool, leave a comment on what you like about it.</span></span>

![DB Browser for SQLite con il database dei film](../../tutorials/first-mvc-app-xplat/working-with-sql/_static/dbb.png)

## <a name="seed-the-database"></a><span data-ttu-id="d62a1-114">Specificare il valore di inizializzazione del database</span><span class="sxs-lookup"><span data-stu-id="d62a1-114">Seed the database</span></span>

<span data-ttu-id="d62a1-115">Creare una nuova classe denominata `SeedData` nella cartella *Models*.</span><span class="sxs-lookup"><span data-stu-id="d62a1-115">Create a new class named `SeedData` in the *Models* folder.</span></span> <span data-ttu-id="d62a1-116">Sostituire il codice generato con il seguente:</span><span class="sxs-lookup"><span data-stu-id="d62a1-116">Replace the generated code with the following:</span></span>

<span data-ttu-id="d62a1-117">[!code-csharp[Principale](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/SeedData.cs?name=snippet_1)]</span><span class="sxs-lookup"><span data-stu-id="d62a1-117">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/SeedData.cs?name=snippet_1)]</span></span>

<span data-ttu-id="d62a1-118">Se sono presenti film nel database, l'inizializzatore del valore di inizializzazione viene restituito.</span><span class="sxs-lookup"><span data-stu-id="d62a1-118">If there are any movies in the DB, the seed initializer returns.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>
### <a name="add-the-seed-initializer"></a><span data-ttu-id="d62a1-119">Aggiungere l'inizializzatore del valore di inizializzazione</span><span class="sxs-lookup"><span data-stu-id="d62a1-119">Add the seed initializer</span></span>

<span data-ttu-id="d62a1-120">Aggiungere l'inizializzatore del valore di inizializzazione al metodo `Main` nel file *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="d62a1-120">Add the seed initializer to the `Main` method in the *Program.cs* file:</span></span>

<span data-ttu-id="d62a1-121">[!code-csharp[Principale](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Program.cs?highlight=6,16-32)]</span><span class="sxs-lookup"><span data-stu-id="d62a1-121">[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Program.cs?highlight=6,16-32)]</span></span>

### <a name="test-the-app"></a><span data-ttu-id="d62a1-122">Eseguire il test dell'app</span><span class="sxs-lookup"><span data-stu-id="d62a1-122">Test the app</span></span>

<span data-ttu-id="d62a1-123">Eliminare tutti i record del database; verrà quindi eseguito il metodo di inizializzazione.</span><span class="sxs-lookup"><span data-stu-id="d62a1-123">Delete all the records in the DB (So the seed method will run).</span></span> <span data-ttu-id="d62a1-124">Arrestare e avviare l'app per inizializzare il database.</span><span class="sxs-lookup"><span data-stu-id="d62a1-124">Stop and start the app to seed the database.</span></span>
   
<span data-ttu-id="d62a1-125">L'app mostra i dati inizializzati.</span><span class="sxs-lookup"><span data-stu-id="d62a1-125">The app shows the seeded data.</span></span>

![Browser aperto sull'app per i film MVC con i dati dei film](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)
