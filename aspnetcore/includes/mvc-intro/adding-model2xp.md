<a name="cli"></a>
## <a name="perform-initial-migration"></a>Eseguire la migrazione iniziale

Dalla riga di comando, eseguire i comandi dell'interfaccia della riga di comando di .NET Core seguenti:

```console
dotnet ef migrations add InitialCreate
dotnet ef database update
```

Il comando `dotnet ef migrations add InitialCreate` genera un codice per creare lo schema del database iniziale. Lo schema si basa sul modello specificato in `DbContext`, nel file *Models/MvcMovieContext.cs*. L'argomento `Initial` viene usato per denominare le migrazioni. È possibile usare qualunque nome, ma per convenzione si sceglie un nome che descrive la migrazione. Per altre informazioni vedere [Introduzione alle migrazioni](xref:data/ef-mvc/migrations#introduction-to-migrations).

Il comando `dotnet ef database update` esegue il metodo `Up` nel file *Migrations/\<time-stamp>_InitialCreate.cs*, che crea il database.
