<a name="cli"></a>
## <a name="perform-initial-migration"></a><span data-ttu-id="7862f-101">Eseguire la migrazione iniziale</span><span class="sxs-lookup"><span data-stu-id="7862f-101">Perform initial migration</span></span>

<span data-ttu-id="7862f-102">Dalla riga di comando, eseguire i comandi dell'interfaccia della riga di comando di .NET Core seguenti:</span><span class="sxs-lookup"><span data-stu-id="7862f-102">From the command line, run the following .NET Core CLI commands:</span></span>

```console
dotnet ef migrations add InitialCreate
dotnet ef database update
```

<span data-ttu-id="7862f-103">Il comando `dotnet ef migrations add InitialCreate` genera un codice per creare lo schema del database iniziale.</span><span class="sxs-lookup"><span data-stu-id="7862f-103">The `dotnet ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span> <span data-ttu-id="7862f-104">Lo schema si basa sul modello specificato in `DbContext`, nel file *Models/MvcMovieContext.cs*.</span><span class="sxs-lookup"><span data-stu-id="7862f-104">The schema is based on the model specified in the `DbContext` (In the *Models/MvcMovieContext.cs* file).</span></span> <span data-ttu-id="7862f-105">L'argomento `Initial` viene usato per denominare le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="7862f-105">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="7862f-106">È possibile usare qualunque nome, ma per convenzione si sceglie un nome che descrive la migrazione.</span><span class="sxs-lookup"><span data-stu-id="7862f-106">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="7862f-107">Per altre informazioni vedere [Introduzione alle migrazioni](xref:data/ef-mvc/migrations#introduction-to-migrations).</span><span class="sxs-lookup"><span data-stu-id="7862f-107">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="7862f-108">Il comando `dotnet ef database update` esegue il metodo `Up` nel file *Migrations/\<time-stamp>_InitialCreate.cs*, che crea il database.</span><span class="sxs-lookup"><span data-stu-id="7862f-108">The `dotnet ef database update` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file, which creates the database.</span></span>
