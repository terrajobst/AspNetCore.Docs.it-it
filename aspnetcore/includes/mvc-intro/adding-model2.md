## <a name="add-initial-migration-and-update-the-database"></a>Aggiungere la migrazione iniziale e l'aggiornamento del database

* Aprire un prompt dei comandi e passare alla directory del progetto. (La directory contenente il *Startup.cs* file).

* Eseguire i comandi seguenti dal prompt dei comandi:

  ```console
  dotnet restore
  dotnet ef migrations add Initial
  dotnet ef database update
  ```
  
  [.NET core](http://go.microsoft.com/fwlink/?LinkID=517853) è un'implementazione di più piattaforme di .NET. Ecco cosa questi comandi:

  * `dotnet restore`: Consente di scaricare i pacchetti NuGet specificati nella *csproj* file.
  * `dotnet ef migrations add Initial`Esegue il comando di migrazioni di Entity Framework .NET Core CLI e crea la migrazione iniziale. Il parametro dopo "Aggiungi" è un nome assegnato per la migrazione. Qui si denominano la migrazione "Iniziale" perché è la migrazione del database iniziale. Questa operazione crea il *dati/migrazioni/\<data e ora > _Initial.cs* file contenente i comandi di migrazione per aggiungere il *film* tabella nel database.
  * `dotnet ef database update`Aggiorna il database con la migrazione che appena creato.

Nella prossima esercitazione verrà illustrato sulla stringa di connessione e database. Si apprenderà sulle modifiche al modello di dati nel [aggiungere un campo](xref:tutorials/first-mvc-app/new-field) esercitazione.