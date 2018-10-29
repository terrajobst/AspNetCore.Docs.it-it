<!--
[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]


[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_1stSearch)]

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchNull)]

![Index view](~/tutorials/first-mvc-app/search/_static/ghost.png)


[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

--> 

<span data-ttu-id="2b933-101">A questo punto quando si invia una ricerca, l'URL contiene la stringa di query della ricerca.</span><span class="sxs-lookup"><span data-stu-id="2b933-101">Now when you submit a search, the URL contains the search query string.</span></span> <span data-ttu-id="2b933-102">La ricerca passerà anche al metodo di azione `HttpGet Index`, anche se si dispone di un metodo `HttpPost Index`.</span><span class="sxs-lookup"><span data-stu-id="2b933-102">Searching will also go to the `HttpGet Index` action method, even if you have a `HttpPost Index` method.</span></span>

![Finestra del browser che mostra searchString=ghost nell'URL e i film restituiti, Ghostbusters e Ghostbusters 2, contengono la parola ghost](~/tutorials/first-mvc-app/search/_static/search_get.png)

<span data-ttu-id="2b933-104">Il markup seguente mostra la modifica al tag `form`:</span><span class="sxs-lookup"><span data-stu-id="2b933-104">The following markup shows the change to the `form` tag:</span></span>

```html
<form asp-controller="Movies" asp-action="Index" method="get">
   ```

## <a name="adding-search-by-genre"></a><span data-ttu-id="2b933-105">Aggiunta della funzionalità di ricerca in base al genere</span><span class="sxs-lookup"><span data-stu-id="2b933-105">Adding Search by genre</span></span>

<span data-ttu-id="2b933-106">Aggiungere la classe `MovieGenreViewModel` seguente alla cartella *Models*:</span><span class="sxs-lookup"><span data-stu-id="2b933-106">Add the following `MovieGenreViewModel` class to the *Models* folder:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieGenreViewModel.cs)]

<span data-ttu-id="2b933-107">Il modello di vista movie-genre conterrà:</span><span class="sxs-lookup"><span data-stu-id="2b933-107">The movie-genre view model will contain:</span></span>

   * <span data-ttu-id="2b933-108">Un elenco di film.</span><span class="sxs-lookup"><span data-stu-id="2b933-108">A list of movies.</span></span>
   * <span data-ttu-id="2b933-109">`SelectList` contiene l'elenco dei generi.</span><span class="sxs-lookup"><span data-stu-id="2b933-109">A `SelectList` containing the list of genres.</span></span> <span data-ttu-id="2b933-110">Consente all'utente di selezionare un genere dall'elenco.</span><span class="sxs-lookup"><span data-stu-id="2b933-110">This allows the user to select a genre from the list.</span></span>
   * <span data-ttu-id="2b933-111">`MovieGenre` che contiene il genere selezionato.</span><span class="sxs-lookup"><span data-stu-id="2b933-111">`MovieGenre`, which contains the selected genre.</span></span>
   * <span data-ttu-id="2b933-112">`SearchString`, che contiene il testo immesso dagli utenti nella casella di testo di ricerca.</span><span class="sxs-lookup"><span data-stu-id="2b933-112">`SearchString`, which contains the text users enter in the search text box.</span></span>

<span data-ttu-id="2b933-113">Sostituire il metodo `Index` in `MoviesController.cs` con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="2b933-113">Replace the `Index` method in `MoviesController.cs` with the following code:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchGenre)]

<span data-ttu-id="2b933-114">Il codice seguente è una query `LINQ` che recupera tutti i generi dal database.</span><span class="sxs-lookup"><span data-stu-id="2b933-114">The following code is a `LINQ` query that retrieves all the genres from the database.</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_LINQ)]

<span data-ttu-id="2b933-115">L'elenco `SelectList` di generi viene creato presentando generi distinti per evitare che l'elenco di selezione includa generi duplicati.</span><span class="sxs-lookup"><span data-stu-id="2b933-115">The `SelectList` of genres is created by projecting the distinct genres (we don't want our select list to have duplicate genres).</span></span>

<span data-ttu-id="2b933-116">Quando l'utente cerca l'elemento, viene mantenuto il valore di ricerca nella casella di ricerca.</span><span class="sxs-lookup"><span data-stu-id="2b933-116">When the user searches for the item, the search value is retained in the search box.</span></span> <span data-ttu-id="2b933-117">Per mantenere il valore di ricerca, popolare la proprietà `SearchString` con il valore di ricerca.</span><span class="sxs-lookup"><span data-stu-id="2b933-117">To retain the search value,  populate the `SearchString` property with the search value.</span></span> <span data-ttu-id="2b933-118">Il valore di ricerca è il parametro `searchString` per l'azione del controller `Index`.</span><span class="sxs-lookup"><span data-stu-id="2b933-118">The search value is the `searchString` parameter for the `Index` controller action.</span></span>

```csharp
movieGenreVM.genres = new SelectList(await genreQuery.Distinct().ToListAsync())
```

## <a name="adding-search-by-genre-to-the-index-view"></a><span data-ttu-id="2b933-119">Aggiunta della funzionalità di ricerca in base al genere alla vista Index</span><span class="sxs-lookup"><span data-stu-id="2b933-119">Adding search by genre to the Index view</span></span>

<span data-ttu-id="2b933-120">Aggiornare `Index.cshtml` come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="2b933-120">Update `Index.cshtml` as follows:</span></span>

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexFormGenreNoRating.cshtml?highlight=1,15,16,17,28,31,34,37,43)]

<span data-ttu-id="2b933-121">Esaminare l'espressione lambda usata nell'helper HTML seguente:</span><span class="sxs-lookup"><span data-stu-id="2b933-121">Examine the lambda expression used in the following HTML Helper:</span></span>

`@Html.DisplayNameFor(model => model.Movies[0].Title)`
 
<span data-ttu-id="2b933-122">Nel codice precedente, l'helper HTML `DisplayNameFor` controlla la proprietà `Title` a cui si fa riferimento nell'espressione lambda per determinare il nome visualizzato.</span><span class="sxs-lookup"><span data-stu-id="2b933-122">In the preceding code, the `DisplayNameFor` HTML Helper inspects the `Title` property referenced in the lambda expression to determine the display name.</span></span> <span data-ttu-id="2b933-123">Poiché l'espressione lambda viene controllata e anziché essere valutata, non viene generata una violazione di accesso quando `model`, `model.Movies` o `model.Movies[0]` sono `null` o vuoti.</span><span class="sxs-lookup"><span data-stu-id="2b933-123">Since the lambda expression is inspected rather than evaluated, you don't receive an access violation when `model`, `model.Movies`, or `model.Movies[0]` are `null` or empty.</span></span> <span data-ttu-id="2b933-124">Quando invece l'espressione lambda viene valutata, ad esempio `@Html.DisplayFor(modelItem => item.Title)`, vengono valutati i valori delle proprietà del modello.</span><span class="sxs-lookup"><span data-stu-id="2b933-124">When the lambda expression is evaluated (for example, `@Html.DisplayFor(modelItem => item.Title)`), the model's property values are evaluated.</span></span>

<span data-ttu-id="2b933-125">Eseguire il test dell'app effettuando una ricerca per genere, titolo del film e usando entrambi i filtri.</span><span class="sxs-lookup"><span data-stu-id="2b933-125">Test the app by searching by genre, by movie title, and by both.</span></span>
