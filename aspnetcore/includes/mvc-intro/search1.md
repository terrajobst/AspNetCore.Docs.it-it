# <a name="add-search-to-an-aspnet-core-mvc-app"></a>Aggiungere la funzionalità di ricerca a un'app ASP.NET Core MVC

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

In questa sezione si aggiunge una funzionalità di ricerca al metodo di azione `Index` che consente di ricercare i film in base al *genere* o al *nome*.

Aggiornare il metodo `Index` con il codice seguente:
<!--
[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]
-->

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_1stSearch)]

La prima riga del metodo di azione `Index` crea una query [LINQ](/dotnet/standard/using-linq) per selezionare i film:

```csharp
var movies = from m in _context.Movie
             select m;
```

La query viene *solo* definita in questo punto, ma **non** viene eseguita nel database.

Se il parametro `searchString` contiene una stringa, la query dei film viene modificata per filtrare in base al valore della stringa di ricerca:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchNull2)]

Il codice `s => s.Title.Contains()` precedente è un'[espressione lambda](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions). Le espressioni lambda vengono usate nelle query [LINQ](/dotnet/standard/using-linq) basate sul metodo come argomenti dei metodi di operatori di query standard, quali il metodo [Where](/dotnet/api/system.linq.enumerable.where) o `Contains` (usato nel codice precedente). Le query LINQ non vengono eseguite al momento della definizione o della modifica chiamando un metodo, ad esempio `Where`, `Contains` o `OrderBy`. L'esecuzione della query viene invece posticipata.  Per esecuzione posticipata si intende che la valutazione di un'espressione viene ritardata finché non viene eseguita l'iterazione del valore o non viene chiamato il metodo `ToListAsync`. Per altre informazioni sull'esecuzione posticipata di query, vedere [Esecuzione di query](/dotnet/framework/data/adonet/ef/language-reference/query-execution).

Nota: il metodo [Contains](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) viene eseguito sul database, non nel codice C# indicato in precedenza. La distinzione tra maiuscole/minuscole nella query dipende dal database e dalle regole di confronto. In SQL Server, [Contains](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) esegue il mapping a [SQL LIKE](/sql/t-sql/language-elements/like-transact-sql), che fa distinzione tra maiuscole e minuscole. In SQLite, con le regole di confronto predefinite si fa distinzione tra maiuscole e minuscole.

Passare a `/Movies/Index`. Accodare una stringa di query, ad esempio `?searchString=Ghost`, all'URL. Vengono visualizzati i film filtrati.

![Vista Index](~/tutorials/first-mvc-app/search/_static/ghost.png)

Se si modifica la firma del metodo `Index` in modo che contenga un parametro denominato `id`, il parametro `id` corrisponderà al segnaposto `{id}` facoltativo per le route predefinite impostate in *Startup.cs*.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]
