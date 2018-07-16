<!--
[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]


[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_1stSearch)]

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchNull)]

![Index view](~/tutorials/first-mvc-app/search/_static/ghost.png)


[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

--> 

<span data-ttu-id="1f92c-101">A questo punto quando si invia una ricerca, l'URL contiene la stringa di query della ricerca.</span><span class="sxs-lookup"><span data-stu-id="1f92c-101">Now when you submit a search, the URL contains the search query string.</span></span> <span data-ttu-id="1f92c-102">La ricerca passerà anche al metodo di azione `HttpGet Index`, anche se si dispone di un metodo `HttpPost Index`.</span><span class="sxs-lookup"><span data-stu-id="1f92c-102">Searching will also go to the `HttpGet Index` action method, even if you have a `HttpPost Index` method.</span></span>

![Finestra del browser che mostra searchString=ghost nell'URL e i film restituiti, Ghostbusters e Ghostbusters 2, contengono la parola ghost](~/tutorials/first-mvc-app/search/_static/search_get.png)

<span data-ttu-id="1f92c-104">Il markup seguente mostra la modifica al tag `form`:</span><span class="sxs-lookup"><span data-stu-id="1f92c-104">The following markup shows the change to the `form` tag:</span></span>

```html
<form asp-controller="Movies" asp-action="Index" method="get">
   ```

## <a name="adding-search-by-genre"></a><span data-ttu-id="1f92c-105">Aggiunta della funzionalità di ricerca in base al genere</span><span class="sxs-lookup"><span data-stu-id="1f92c-105">Adding Search by genre</span></span>

<span data-ttu-id="1f92c-106">Aggiungere la classe `MovieGenreViewModel` seguente alla cartella *Models*:</span><span class="sxs-lookup"><span data-stu-id="1f92c-106">Add the following `MovieGenreViewModel` class to the *Models* folder:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieGenreViewModel.cs)]

<span data-ttu-id="1f92c-107">Il modello di vista movie-genre conterrà:</span><span class="sxs-lookup"><span data-stu-id="1f92c-107">The movie-genre view model will contain:</span></span>

   * <span data-ttu-id="1f92c-108">Un elenco di film.</span><span class="sxs-lookup"><span data-stu-id="1f92c-108">A list of movies.</span></span>
   * <span data-ttu-id="1f92c-109">`SelectList` contiene l'elenco dei generi.</span><span class="sxs-lookup"><span data-stu-id="1f92c-109">A `SelectList` containing the list of genres.</span></span> <span data-ttu-id="1f92c-110">Consentirà all'utente di selezionare un genere dall'elenco.</span><span class="sxs-lookup"><span data-stu-id="1f92c-110">This will allow the user to select a genre from the list.</span></span>
   * <span data-ttu-id="1f92c-111">`movieGenre` che contiene il genere selezionato.</span><span class="sxs-lookup"><span data-stu-id="1f92c-111">`movieGenre`, which contains the selected genre.</span></span>

<span data-ttu-id="1f92c-112">Sostituire il metodo `Index` in `MoviesController.cs` con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="1f92c-112">Replace the `Index` method in `MoviesController.cs` with the following code:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchGenre)]

<span data-ttu-id="1f92c-113">Il codice seguente è una query `LINQ` che recupera tutti i generi dal database.</span><span class="sxs-lookup"><span data-stu-id="1f92c-113">The following code is a `LINQ` query that retrieves all the genres from the database.</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_LINQ)]

<span data-ttu-id="1f92c-114">L'elenco `SelectList` di generi viene creato presentando generi distinti per evitare che l'elenco di selezione includa generi duplicati.</span><span class="sxs-lookup"><span data-stu-id="1f92c-114">The `SelectList` of genres is created by projecting the distinct genres (we don't want our select list to have duplicate genres).</span></span>

```csharp
movieGenreVM.genres = new SelectList(await genreQuery.Distinct().ToListAsync())
   ```

## <a name="adding-search-by-genre-to-the-index-view"></a><span data-ttu-id="1f92c-115">Aggiunta della funzionalità di ricerca in base al genere alla vista Index</span><span class="sxs-lookup"><span data-stu-id="1f92c-115">Adding search by genre to the Index view</span></span>

<span data-ttu-id="1f92c-116">Aggiornare `Index.cshtml` come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="1f92c-116">Update `Index.cshtml` as follows:</span></span>

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexFormGenreNoRating.cshtml?highlight=1,15,16,17,28,31,34,37,43)]

<span data-ttu-id="1f92c-117">Esaminare l'espressione lambda usata nell'helper HTML seguente:</span><span class="sxs-lookup"><span data-stu-id="1f92c-117">Examine the lambda expression used in the following HTML Helper:</span></span>

`@Html.DisplayNameFor(model => model.movies[0].Title)`
 
<span data-ttu-id="1f92c-118">Nel codice precedente, l'helper HTML `DisplayNameFor` controlla la proprietà `Title` a cui si fa riferimento nell'espressione lambda per determinare il nome visualizzato.</span><span class="sxs-lookup"><span data-stu-id="1f92c-118">In the preceding code, the `DisplayNameFor` HTML Helper inspects the `Title` property referenced in the lambda expression to determine the display name.</span></span> <span data-ttu-id="1f92c-119">Poiché l'espressione lambda viene controllata e anziché essere valutata, non viene generata una violazione di accesso quando `model`, `model.movies` o `model.movies[0]` sono `null` o vuoti.</span><span class="sxs-lookup"><span data-stu-id="1f92c-119">Since the lambda expression is inspected rather than evaluated, you don't receive an access violation when `model`, `model.movies`, or `model.movies[0]` are `null` or empty.</span></span> <span data-ttu-id="1f92c-120">Quando invece l'espressione lambda viene valutata, ad esempio `@Html.DisplayFor(modelItem => item.Title)`, vengono valutati i valori delle proprietà del modello.</span><span class="sxs-lookup"><span data-stu-id="1f92c-120">When the lambda expression is evaluated (for example, `@Html.DisplayFor(modelItem => item.Title)`), the model's property values are evaluated.</span></span>

<span data-ttu-id="1f92c-121">Eseguire il test dell'app effettuando una ricerca per genere, titolo del film e usando entrambi i filtri.</span><span class="sxs-lookup"><span data-stu-id="1f92c-121">Test the app by searching by genre, by movie title, and by both.</span></span>
