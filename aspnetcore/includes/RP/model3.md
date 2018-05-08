<a name="cli"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a>Aggiungere gli strumenti di scaffolding ed eseguire la migrazione iniziale

Dalla riga di comando, eseguire i comandi dell'interfaccia della riga di comando di .NET Core seguenti:

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet ef migrations add InitialCreate
dotnet ef database update
```

Il comando `add package` installa gli strumenti necessari per eseguire il motore di scaffolding.

Il comando `ef migrations add InitialCreate` genera un codice per creare lo schema del database iniziale. Lo schema è basato sul modello specificato in `DbContext` (nel file *Models/MovieContext.cs*). L'argomento `InitialCreate` viene usato per denominare le migrazioni. È possibile usare qualunque nome, ma per convenzione si sceglie un nome che descrive la migrazione. Per altre informazioni vedere [Introduzione alle migrazioni](xref:data/ef-mvc/migrations#introduction-to-migrations).

Il comando `ef database update` esegue il metodo `Up` nel file *Migrations/\<time-stamp>_InitialCreate.cs*, che crea il database.
