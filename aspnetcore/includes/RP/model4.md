Nella tabella seguente sono specificati i parametri dei generatori di codice ASP.NET Core:

| Parametro               | Descrizione|
| ----------------- | ------------ |
| -m  | Nome del modello. |
| -dc  | Contesto dati. |
| -udl | Uso del layout predefinito. |
| -outDir | Percorso relativo della cartella di output per creare le viste. |
| --referenceScriptLibraries | Aggiunge `_ValidationScriptsPartial` per modificare e creare pagine |

Utilizzare il commutatore `h` per ottenere assistenza sul comando `aspnet-codegenerator razorpage`:

```console
dotnet aspnet-codegenerator razorpage -h
```
<a name="test"></a>
### <a name="test-the-app"></a>Eseguire il test dell'applicazione

* Eseguire l'app e accodare `/Movies` all'URL nel browser (`http://localhost:port/movies`).
* Eseguire il test del collegamento **Crea**.

 ![Pagina Crea](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* Eseguire il test dei collegamenti **Modifica**, **Dettagli** e **Elimina**.

Se viene visualizzato l'errore seguente, verificare di aver eseguito le migrazioni e di aver aggiornato il database:

```
An unhandled exception occurred while processing the request.
SqliteException: SQLite Error 1: 'no such table: Movie'.
Microsoft.Data.Sqlite.SqliteException.ThrowExceptionForRC(int rc, sqlite3 db)
```