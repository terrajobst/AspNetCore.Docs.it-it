# <a name="add-search-to-an-aspnet-core-razor-pages-app"></a><span data-ttu-id="1d0e3-101">Aggiungere la funzionalità di ricerca a un'app ASP.NET Core Razor Pages</span><span class="sxs-lookup"><span data-stu-id="1d0e3-101">Add search to an ASP.NET Core Razor Pages app</span></span>

<span data-ttu-id="1d0e3-102">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="1d0e3-102">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="1d0e3-103">In questo documento viene aggiunta una funzionalità di ricerca alla pagina di indice che consente di ricercare i film in base al *genere* o al *nome*.</span><span class="sxs-lookup"><span data-stu-id="1d0e3-103">In this document, search capability is added to the Index page that enables searching movies by *genre* or *name*.</span></span>

<span data-ttu-id="1d0e3-104">Aggiornare il metodo `OnGetAsync` della pagina di indice con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="1d0e3-104">Update the Index page's `OnGetAsync` method with the following code:</span></span>

[!code-cshtml[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/_ViewStart.cshtml)]

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_1stSearch)]

<span data-ttu-id="1d0e3-105">La prima riga del metodo `OnGetAsync` crea una query [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) per selezionare i film:</span><span class="sxs-lookup"><span data-stu-id="1d0e3-105">The first line of the `OnGetAsync` method creates a [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) query to select the movies:</span></span>

```csharp
var movies = from m in _context.Movie
             select m;
```

<span data-ttu-id="1d0e3-106">La query viene *solo* definita in questo punto, ma **non** viene eseguita nel database.</span><span class="sxs-lookup"><span data-stu-id="1d0e3-106">The query is *only* defined at this point, it has **not** been run against the database.</span></span>

<span data-ttu-id="1d0e3-107">Se il parametro `searchString` contiene una stringa, la query dei film viene modificata per filtrare gli elementi in base alla stringa di ricerca:</span><span class="sxs-lookup"><span data-stu-id="1d0e3-107">If the `searchString` parameter contains a string, the movies query is modified to filter on the search string:</span></span>

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SearchNull)]

<span data-ttu-id="1d0e3-108">Il codice `s => s.Title.Contains()` è un'[espressione lambda](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span><span class="sxs-lookup"><span data-stu-id="1d0e3-108">The `s => s.Title.Contains()` code is a [Lambda Expression](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="1d0e3-109">Le espressioni lambda vengono usate nelle query [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) basate sul metodo come argomenti dei metodi di operatore query standard, ad esempio il metodo [Where](/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) o `Contains` (usato nel codice precedente).</span><span class="sxs-lookup"><span data-stu-id="1d0e3-109">Lambdas are used in method-based [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) queries as arguments to standard query operator methods such as the [Where](/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) method or `Contains` (used in the preceding code).</span></span> <span data-ttu-id="1d0e3-110">Le query LINQ non vengono eseguite al momento della definizione o della modifica chiamando un metodo, ad esempio `Where`, `Contains` o `OrderBy`.</span><span class="sxs-lookup"><span data-stu-id="1d0e3-110">LINQ queries are not executed when they're defined or when they're modified by calling a method (such as `Where`, `Contains`  or `OrderBy`).</span></span> <span data-ttu-id="1d0e3-111">L'esecuzione della query viene invece posticipata.</span><span class="sxs-lookup"><span data-stu-id="1d0e3-111">Rather, query execution is deferred.</span></span> <span data-ttu-id="1d0e3-112">Per esecuzione posticipata si intende che la valutazione di un'espressione viene ritardata finché non viene eseguita l'iterazione del valore ottenuto o non viene chiamato il metodo `ToListAsync`.</span><span class="sxs-lookup"><span data-stu-id="1d0e3-112">That means the evaluation of an expression is delayed until its realized value is iterated over or the `ToListAsync` method is called.</span></span> <span data-ttu-id="1d0e3-113">Per altre informazioni, vedere [Esecuzione di query](/dotnet/framework/data/adonet/ef/language-reference/query-execution).</span><span class="sxs-lookup"><span data-stu-id="1d0e3-113">See [Query Execution](/dotnet/framework/data/adonet/ef/language-reference/query-execution) for more information.</span></span>

<span data-ttu-id="1d0e3-114">**Nota:** il metodo [Contains](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) viene eseguito sul database, non nel codice C#.</span><span class="sxs-lookup"><span data-stu-id="1d0e3-114">**Note:** The [Contains](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) method is run on the database, not in the C# code.</span></span> <span data-ttu-id="1d0e3-115">La distinzione tra maiuscole/minuscole nella query dipende dal database e dalle regole di confronto.</span><span class="sxs-lookup"><span data-stu-id="1d0e3-115">The case sensitivity on the query depends on the database and the collation.</span></span> <span data-ttu-id="1d0e3-116">In SQL Server `Contains` esegue il mapping a [SQL LIKE](/sql/t-sql/language-elements/like-transact-sql) che fa distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="1d0e3-116">On SQL Server, `Contains` maps to [SQL LIKE](/sql/t-sql/language-elements/like-transact-sql), which is case insensitive.</span></span> <span data-ttu-id="1d0e3-117">In SQLite, con le regole di confronto predefinite si fa distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="1d0e3-117">In SQLite, with the default collation, it's case sensitive.</span></span>

<span data-ttu-id="1d0e3-118">Passare alla pagina Movies e aggiungere una stringa di query, ad esempio `?searchString=Ghost` all'URL, ad esempio `http://localhost:5000/Movies?searchString=Ghost`.</span><span class="sxs-lookup"><span data-stu-id="1d0e3-118">Navigate to the Movies page and append a query string such as `?searchString=Ghost` to the URL (for example, `http://localhost:5000/Movies?searchString=Ghost`).</span></span> <span data-ttu-id="1d0e3-119">Vengono visualizzati i film filtrati.</span><span class="sxs-lookup"><span data-stu-id="1d0e3-119">The filtered movies are displayed.</span></span>

![Vista Index](../../tutorials/razor-pages/search/_static/ghost.png)

<span data-ttu-id="1d0e3-121">Se il modello di route seguente viene aggiunto alla pagina di indice, la stringa di ricerca può essere passata come segmento di URL, ad esempio `http://localhost:5000/Movies/ghost`.</span><span class="sxs-lookup"><span data-stu-id="1d0e3-121">If the following route template is added to the Index page, the search string can be passed as a URL segment (for example, `http://localhost:5000/Movies/ghost`).</span></span>

```cshtml
@page "{searchString?}"
```

<span data-ttu-id="1d0e3-122">Il vincolo di route precedente consente la ricerca del titolo come dati della route (un segmento URL), anziché come valore della stringa di query.</span><span class="sxs-lookup"><span data-stu-id="1d0e3-122">The preceding route constraint allows searching the title as route data (a URL segment) instead of as a query string value.</span></span>  <span data-ttu-id="1d0e3-123">Il carattere `?` in `"{searchString?}"` indica che questo parametro di route è facoltativo.</span><span class="sxs-lookup"><span data-stu-id="1d0e3-123">The `?` in `"{searchString?}"` means this is an optional route parameter.</span></span>

![Vista Index con la parola ghost aggiunta all'URL e un elenco restituito di due film: Ghostbusters e Ghostbusters 2](../../tutorials/razor-pages/search/_static/g2.png)

<span data-ttu-id="1d0e3-125">Non è tuttavia possibile supporre che gli utenti modifichino l'URL per cercare un film.</span><span class="sxs-lookup"><span data-stu-id="1d0e3-125">However, you can't expect users to modify the URL to search for a movie.</span></span> <span data-ttu-id="1d0e3-126">In questo passaggio viene aggiunta l'interfaccia utente per filtrare i film.</span><span class="sxs-lookup"><span data-stu-id="1d0e3-126">In this step, UI is added to filter movies.</span></span> <span data-ttu-id="1d0e3-127">Rimuovere il vincolo di route `"{searchString?}"`, se è stato aggiunto.</span><span class="sxs-lookup"><span data-stu-id="1d0e3-127">If you added the route constraint `"{searchString?}"`, remove it.</span></span>

<span data-ttu-id="1d0e3-128">Aprire il file *Pages/Movies/Index.cshtml* e aggiungere il markup `<form>` evidenziato nel codice seguente:</span><span class="sxs-lookup"><span data-stu-id="1d0e3-128">Open the *Pages/Movies/Index.cshtml* file, and add the `<form>` markup highlighted in the following code:</span></span>

[!code-cshtml[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index2.cshtml?highlight=14-19&range=1-22)]

<span data-ttu-id="1d0e3-129">Il tag HTML `<form>` usa l'[helper tag del modulo](xref:mvc/views/working-with-forms#the-form-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="1d0e3-129">The HTML `<form>` tag uses the [Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper).</span></span> <span data-ttu-id="1d0e3-130">Quando il modulo viene inviato, la stringa di filtro verrà inviata alla pagina *Pages/Movies/Index*.</span><span class="sxs-lookup"><span data-stu-id="1d0e3-130">When the form is submitted, the filter string is sent to the *Pages/Movies/Index* page.</span></span> <span data-ttu-id="1d0e3-131">Salvare le modifiche e testare il filtro.</span><span class="sxs-lookup"><span data-stu-id="1d0e3-131">Save the changes and test the filter.</span></span>

![Vista Index con la parola ghost digitata nella casella di testo del filtro del titolo](../../tutorials/razor-pages/search/_static/filter.png)

## <a name="search-by-genre"></a><span data-ttu-id="1d0e3-133">Ricerca in base al genere</span><span class="sxs-lookup"><span data-stu-id="1d0e3-133">Search by genre</span></span>

<span data-ttu-id="1d0e3-134">Aggiungere le proprietà evidenziate seguenti in *Pages/Movies/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="1d0e3-134">Add the following highlighted properties to *Pages/Movies/Index.cshtml.cs*:</span></span>

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_newProps&highlight=11-999)]

<span data-ttu-id="1d0e3-135">`SelectList Genres` contiene l'elenco dei generi.</span><span class="sxs-lookup"><span data-stu-id="1d0e3-135">The `SelectList Genres` contains the list of genres.</span></span> <span data-ttu-id="1d0e3-136">Consente all'utente di selezionare un genere dall'elenco.</span><span class="sxs-lookup"><span data-stu-id="1d0e3-136">This allows the user to select a genre from the list.</span></span>

<span data-ttu-id="1d0e3-137">La proprietà `MovieGenre` contiene il genere specificato selezionate dall'utente, ad esempio, "Western".</span><span class="sxs-lookup"><span data-stu-id="1d0e3-137">The `MovieGenre` property contains the specific genre the user selects (for example, "Western").</span></span>

<span data-ttu-id="1d0e3-138">Aggiornare il metodo `OnGetAsync` con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="1d0e3-138">Update the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SearchGenre)]

<span data-ttu-id="1d0e3-139">Il codice seguente è una query LINQ che recupera tutti i generi dal database.</span><span class="sxs-lookup"><span data-stu-id="1d0e3-139">The following code is a LINQ query that retrieves all the genres from the database.</span></span>

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_LINQ)]

<span data-ttu-id="1d0e3-140">L'elenco `SelectList` di generi viene creato presentando generi distinti.</span><span class="sxs-lookup"><span data-stu-id="1d0e3-140">The `SelectList` of genres is created by projecting the distinct genres.</span></span>

<!-- BUG in OPS
Tag snippet_selectlist's start line '75' should be less than end line '29' when resolving "[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SelectList)]"

There's no start line.

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml.cs?name=snippet_SelectList)]
-->

```csharp
Genres = new SelectList(await genreQuery.Distinct().ToListAsync());
```

### <a name="adding-search-by-genre"></a><span data-ttu-id="1d0e3-141">Aggiunta della funzionalità di ricerca in base al genere</span><span class="sxs-lookup"><span data-stu-id="1d0e3-141">Adding search by genre</span></span>

<span data-ttu-id="1d0e3-142">Aggiornare *Index.cshtml* come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="1d0e3-142">Update *Index.cshtml* as follows:</span></span>

[!code-cshtml[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Pages/Movies/IndexFormGenreNoRating.cshtml?highlight=16-18&range=1-26)]

<span data-ttu-id="1d0e3-143">Eseguire il test dell'app effettuando una ricerca per genere, titolo del film e usando entrambi i filtri.</span><span class="sxs-lookup"><span data-stu-id="1d0e3-143">Test the app by searching by genre, by movie title, and by both.</span></span>
