<!--
[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]


[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_1stSearch)]

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchNull)]

![Index view](~/tutorials/first-mvc-app/search/_static/ghost.png)


[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

--> 

A questo punto quando si invia una ricerca, l'URL contiene la stringa di query della ricerca. La ricerca passerà anche al metodo di azione `HttpGet Index`, anche se si dispone di un metodo `HttpPost Index`.

![Finestra del browser che mostra searchString=ghost nell'URL e i film restituiti, Ghostbusters e Ghostbusters 2, contengono la parola ghost](~/tutorials/first-mvc-app/search/_static/search_get.png)

Il markup seguente mostra la modifica al tag `form`:

```html
<form asp-controller="Movies" asp-action="Index" method="get">
   ```

## <a name="adding-search-by-genre"></a>Aggiunta della funzionalità di ricerca in base al genere

Aggiungere la classe `MovieGenreViewModel` seguente alla cartella *Models*:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieGenreViewModel.cs)]

Il modello di vista movie-genre conterrà:

   * Un elenco di film.
   * `SelectList` contiene l'elenco dei generi. Consente all'utente di selezionare un genere dall'elenco.
   * `MovieGenre` che contiene il genere selezionato.
   * `SearchString`, che contiene il testo immesso dagli utenti nella casella di testo di ricerca.

Sostituire il metodo `Index` in `MoviesController.cs` con il codice seguente:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchGenre)]

Il codice seguente è una query `LINQ` che recupera tutti i generi dal database.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_LINQ)]

L'elenco `SelectList` di generi viene creato presentando generi distinti per evitare che l'elenco di selezione includa generi duplicati.

Quando l'utente cerca l'elemento, viene mantenuto il valore di ricerca nella casella di ricerca. Per mantenere il valore di ricerca, popolare la proprietà `SearchString` con il valore di ricerca. Il valore di ricerca è il parametro `searchString` per l'azione del controller `Index`.

```csharp
movieGenreVM.genres = new SelectList(await genreQuery.Distinct().ToListAsync())
```

## <a name="adding-search-by-genre-to-the-index-view"></a>Aggiunta della funzionalità di ricerca in base al genere alla vista Index

Aggiornare `Index.cshtml` come indicato di seguito:

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexFormGenreNoRating.cshtml?highlight=1,15,16,17,28,31,34,37,43)]

Esaminare l'espressione lambda usata nell'helper HTML seguente:

`@Html.DisplayNameFor(model => model.Movies[0].Title)`
 
Nel codice precedente, l'helper HTML `DisplayNameFor` controlla la proprietà `Title` a cui si fa riferimento nell'espressione lambda per determinare il nome visualizzato. Poiché l'espressione lambda viene controllata e anziché essere valutata, non viene generata una violazione di accesso quando `model`, `model.Movies` o `model.Movies[0]` sono `null` o vuoti. Quando invece l'espressione lambda viene valutata, ad esempio `@Html.DisplayFor(modelItem => item.Title)`, vengono valutati i valori delle proprietà del modello.

Eseguire il test dell'app effettuando una ricerca per genere, titolo del film e usando entrambi i filtri.
