# <a name="adding-a-new-field"></a>Aggiunta di un nuovo campo

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

In questa esercitazione si aggiungerà un nuovo campo alla tabella `Movies`. Verrà eliminato il database e verrà creato un nuovo database quando si modifica lo schema (aggiunta di un nuovo campo). Questo flusso di lavoro funziona correttamente già in fase di sviluppo, se non sono disponibili dati di produzione da conservare.

Dopo che l'app è stata distribuita e i dati disponibili devono essere conservati, non è possibile eliminare il database quando è necessario modificare lo schema. [Migrazioni Code First](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) di Entity Framework consente di aggiornare lo schema e migrare il database senza perdere i dati. Si tratta di una funzionalità comune quando si usa SQL Server; SQLlite non supporta molte operazioni dello schema di migrazione e sono possibili solo migrazioni molto semplici. Per altre informazioni vedere [SQLite Limitations](https://docs.microsoft.com/ef/core/providers/sqlite/limitations) (Limitazioni di SQLite).

## <a name="adding-a-rating-property-to-the-movie-model"></a>Aggiunta di una proprietà Rating al modello Movie

Aprire il file *Models/Movie.cs* e aggiungere una proprietà `Rating`:

[!code-csharp[Principale](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]

Poiché è stato aggiunto un nuovo campo alla classe `Movie`, è necessario anche aggiornare l'elenco di elementi di associazione in modo da includere questa nuova proprietà. In *MoviesController.cs* aggiornare l'attributo `[Bind]` per i metodi di azione `Create` e `Edit` in modo da includere la proprietà `Rating`:

```csharp
[Bind("ID,Title,ReleaseDate,Genre,Price,Rating")]
   ```

È necessario anche aggiornare i modelli di vista per visualizzare, creare e modificare la nuova proprietà `Rating` nella vista del browser.

Modificare il file */Views/Movies/Index.cshtml* e aggiungere un campo `Rating`:

[!code-HTML[Principale](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexGenreRating.cshtml?highlight=17,39&range=24-64)]

Aggiornare */Views/Movies/Create.cshtml* con un campo `Rating`.

L'app non funzionerà finché non si aggiorna il database in modo da includere il nuovo campo. Se si esegue l'app ora, verrà visualizzato il seguente errore `SqliteException`:

```
SqliteException: SQLite Error 1: 'no such column: m.Rating'.
```

Questo errore viene visualizzato perché la classe del modello Movie aggiornata è diversa rispetto allo schema della tabella Movie nel database esistente. Nella tabella del database non è presente una colonna `Rating`.

Per correggere questo errore, esistono alcuni approcci:

1. Eliminare il database e fare in modo che Entity Framework crei di nuovo automaticamente il database in base al nuovo schema di classi del modello. Con questo approccio si perdono i dati esistenti nel database e non è possibile quindi adottare questo approccio con un database di produzione. Un modo efficace per sviluppare un'app consiste nell'usare un inizializzatore per inizializzare automaticamente un database con i dati di test.

2. Modificare manualmente lo schema del database esistente in modo che corrisponda alle classi del modello. Il vantaggio di questo approccio è che i dati vengono mantenuti. È possibile apportare questa modifica manualmente o creando uno script di modifica del database.

3. Usare Migrazioni Code First per aggiornare lo schema del database.

In questa esercitazione il database verrà eliminato e verrà creato di nuovo quando si modifica lo schema. Per eliminare il database, eseguire il comando seguente da un terminale:

`dotnet ef database drop`

Aggiornare la classe `SeedData` in modo che fornisca un valore per la nuova colonna. Di seguito viene illustrata una modifica di esempio, ma si apporterà questa modifica per ogni `new Movie`.

[!code-csharp[Principale](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/SeedDataRating.cs?name=snippet1&highlight=6)]

Aggiungere il campo `Rating` alla vista `Edit`, `Details` e `Delete`.

Eseguire l'app e verificare che sia possibile creare/modificare/visualizzare i film con un campo `Rating`. modelli.
