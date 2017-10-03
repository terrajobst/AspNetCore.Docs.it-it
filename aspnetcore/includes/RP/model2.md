Aggiungere le proprietà seguenti alla classe `Movie`:

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieNoEF.cs?name=snippet_MovieNoEF)]

Il campo `ID` è richiesto dal database per la chiave primaria.

<a name="dc"></a>
### <a name="add-a-database-context-class"></a>Aggiungere una classe di contesto dei dati

Aggiungere una classe derivata `DbContext` denominata *MovieContext.cs* alla cartella *Modelli*.

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieContext.cs?range=1-12,14-17,19-21)]

Il codice precedente crea una proprietà `DbSet` per il set di entità. Nella terminologia di Entity Framework, un set di entità corrisponde in genere alla tabella di un database e un'entità corrisponde a una riga della tabella. Il nome della proprietà `DbSet` è `Movies`. Poiché il database usa nomi singolari, l'esempio esegue l'override di `OnModelCreating` per usare la forma singolare (`Movie`) per il nome della tabella.
