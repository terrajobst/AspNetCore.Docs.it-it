# <a name="working-with-sqlite-in-an-aspnet-core-mvc-project"></a>Utilizzo di SQLite in un progetto ASP.NET Core MVC

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

L'oggetto `MvcMovieContext` gestisce l'attività di connessione al database e di mapping degli oggetti `Movie` ai record di database. Il contesto del database viene registrato nel contenitore di [inserimento dipendenze](xref:fundamentals/dependency-injection) nel metodo `ConfigureServices` nel file *Startup.cs*:

[!code-csharp[](~/tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet2&highlight=6-8)]

## <a name="sqlite"></a>SQLite

Nel sito Web di [SQLite](https://www.sqlite.org/) si dichiara:

> SQLite è un motore di database SQL autonomo, a elevata affidabilità, incorporato, completo e di dominio pubblico. SQLite è il motore di database più usato in tutto il mondo.

Per gestire e visualizzare un database SQLite, sono disponibili numerosi strumenti di terze parti che è possibile scaricare. L'immagine seguente è stata tratta da [DB Browser per SQLite](http://sqlitebrowser.org/). Se si preferisce uno strumento SQLite in particolare, aggiungere un commento sui motivi della preferenza.

![DB Browser for SQLite con il database dei film](~/tutorials/first-mvc-app-xplat/working-with-sql/_static/dbb.png)

## <a name="seed-the-database"></a>Specificare il valore di inizializzazione del database

Creare una nuova classe denominata `SeedData` nella cartella *Models*. Sostituire il codice generato con il seguente:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/SeedData.cs?name=snippet_1)]

Se sono presenti film nel database, l'inizializzatore del valore di inizializzazione viene restituito.

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>
### <a name="add-the-seed-initializer"></a>Aggiungere l'inizializzatore del valore di inizializzazione

Aggiungere l'inizializzatore del valore di inizializzazione al metodo `Main` nel file *Program.cs*:

::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Program.cs)]
::: moniker-end
::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Program.cs?highlight=6,16-32)]
::: moniker-end

### <a name="test-the-app"></a>Eseguire il test dell'app

Eliminare tutti i record del database; verrà quindi eseguito il metodo di inizializzazione. Arrestare e avviare l'app per inizializzare il database.
   
L'app mostra i dati inizializzati.

![Browser aperto sull'app per i film MVC con i dati dei film](~/tutorials/first-mvc-app/working-with-sql/_static/m55.png)
