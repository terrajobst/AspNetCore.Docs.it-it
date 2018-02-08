# <a name="working-with-sqlite-in-and-razor-pages"></a><span data-ttu-id="83bd3-101">Uso di SQLite nelle pagine Razor</span><span class="sxs-lookup"><span data-stu-id="83bd3-101">Working with SQLite in and Razor Pages</span></span>

<span data-ttu-id="83bd3-102">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="83bd3-102">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="83bd3-103">L'oggetto `MovieContext` gestisce l'attività di connessione al database e di mapping degli oggetti `Movie` ai record di database.</span><span class="sxs-lookup"><span data-stu-id="83bd3-103">The `MovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="83bd3-104">Il contesto del database viene registrato nel contenitore di [inserimento dipendenze](xref:fundamentals/dependency-injection) nel metodo `ConfigureServices` nel file *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="83bd3-104">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

[!code-csharp[Main](code/Startup.cs?name=snippet2&highlight=6-8)]

## <a name="sqlite"></a><span data-ttu-id="83bd3-105">SQLite</span><span class="sxs-lookup"><span data-stu-id="83bd3-105">SQLite</span></span>

<span data-ttu-id="83bd3-106">Nel sito Web di [SQLite](https://www.sqlite.org/) si dichiara:</span><span class="sxs-lookup"><span data-stu-id="83bd3-106">The [SQLite](https://www.sqlite.org/) website states:</span></span>

> <span data-ttu-id="83bd3-107">SQLite è un motore di database SQL autonomo, a elevata affidabilità, incorporato, completo e di dominio pubblico.</span><span class="sxs-lookup"><span data-stu-id="83bd3-107">SQLite is a self-contained, high-reliability, embedded, full-featured, public-domain, SQL database engine.</span></span> <span data-ttu-id="83bd3-108">SQLite è il motore di database più usato in tutto il mondo.</span><span class="sxs-lookup"><span data-stu-id="83bd3-108">SQLite is the most used database engine in the world.</span></span>

<span data-ttu-id="83bd3-109">Per gestire e visualizzare un database SQLite, sono disponibili numerosi strumenti di terze parti che è possibile scaricare.</span><span class="sxs-lookup"><span data-stu-id="83bd3-109">There are many third party tools you can download to manage and view a SQLite database.</span></span> <span data-ttu-id="83bd3-110">L'immagine seguente è stata tratta da [DB Browser per SQLite](http://sqlitebrowser.org/).</span><span class="sxs-lookup"><span data-stu-id="83bd3-110">The image below is from [DB Browser for SQLite](http://sqlitebrowser.org/).</span></span> <span data-ttu-id="83bd3-111">Se si preferisce uno strumento SQLite in particolare, aggiungere un commento sui motivi della preferenza.</span><span class="sxs-lookup"><span data-stu-id="83bd3-111">If you have a favorite SQLite tool, leave a comment on what you like about it.</span></span>

![DB Browser for SQLite con il database dei film](../../tutorials/first-mvc-app-xplat/working-with-sql/_static/dbb.png)

## <a name="seed-the-database"></a><span data-ttu-id="83bd3-113">Specificare il valore di inizializzazione del database</span><span class="sxs-lookup"><span data-stu-id="83bd3-113">Seed the database</span></span>

<span data-ttu-id="83bd3-114">Creare una nuova classe denominata `SeedData` nella cartella *Models*.</span><span class="sxs-lookup"><span data-stu-id="83bd3-114">Create a new class named `SeedData` in the *Models* folder.</span></span> <span data-ttu-id="83bd3-115">Sostituire il codice generato con il seguente:</span><span class="sxs-lookup"><span data-stu-id="83bd3-115">Replace the generated code with the following:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/SeedData.cs?name=snippet_1)]

<span data-ttu-id="83bd3-116">Se sono presenti film nel database, l'inizializzatore del valore di inizializzazione viene restituito.</span><span class="sxs-lookup"><span data-stu-id="83bd3-116">If there are any movies in the DB, the seed initializer returns.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>
### <a name="add-the-seed-initializer"></a><span data-ttu-id="83bd3-117">Aggiungere l'inizializzatore del valore di inizializzazione</span><span class="sxs-lookup"><span data-stu-id="83bd3-117">Add the seed initializer</span></span>

<span data-ttu-id="83bd3-118">Aggiungere l'inizializzatore del valore di inizializzazione al metodo `Main` nel file *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="83bd3-118">Add the seed initializer to the `Main` method in the *Program.cs* file:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Program.cs?highlight=6,16-32)]

### <a name="test-the-app"></a><span data-ttu-id="83bd3-119">Eseguire il test dell'app</span><span class="sxs-lookup"><span data-stu-id="83bd3-119">Test the app</span></span>

<span data-ttu-id="83bd3-120">Eliminare tutti i record del database; verrà quindi eseguito il metodo di inizializzazione.</span><span class="sxs-lookup"><span data-stu-id="83bd3-120">Delete all the records in the DB (So the seed method will run).</span></span> <span data-ttu-id="83bd3-121">Arrestare e avviare l'app per inizializzare il database.</span><span class="sxs-lookup"><span data-stu-id="83bd3-121">Stop and start the app to seed the database.</span></span>
   
<span data-ttu-id="83bd3-122">L'app mostra i dati inizializzati.</span><span class="sxs-lookup"><span data-stu-id="83bd3-122">The app shows the seeded data.</span></span>