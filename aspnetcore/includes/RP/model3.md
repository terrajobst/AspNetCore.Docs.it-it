<a name="cli"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a><span data-ttu-id="2e3e4-101">Aggiungere gli strumenti di scaffolding ed eseguire la migrazione iniziale</span><span class="sxs-lookup"><span data-stu-id="2e3e4-101">Add scaffold tooling and perform initial migration</span></span>

<span data-ttu-id="2e3e4-102">Dalla riga di comando, eseguire i comandi dell'interfaccia della riga di comando di .NET Core seguenti:</span><span class="sxs-lookup"><span data-stu-id="2e3e4-102">From the command line, run the following .NET Core CLI commands:</span></span>

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet ef migrations add InitialCreate
dotnet ef database update
```

<span data-ttu-id="2e3e4-103">Il comando `add package` installa gli strumenti necessari per eseguire il motore di scaffolding.</span><span class="sxs-lookup"><span data-stu-id="2e3e4-103">The `add package` command installs the tooling required to run the scaffolding engine.</span></span>

<span data-ttu-id="2e3e4-104">Il comando `ef migrations add InitialCreate` genera un codice per creare lo schema del database iniziale.</span><span class="sxs-lookup"><span data-stu-id="2e3e4-104">The `ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span> <span data-ttu-id="2e3e4-105">Lo schema è basato sul modello specificato in `DbContext` (nel file *Models/MovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="2e3e4-105">The schema is based on the model specified in the `DbContext` (In the *Models/MovieContext.cs* file).</span></span> <span data-ttu-id="2e3e4-106">L'argomento `Initial` viene utilizzato per denominare le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="2e3e4-106">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="2e3e4-107">È possibile usare qualunque nome, ma per convenzione si sceglie un nome che descrive la migrazione.</span><span class="sxs-lookup"><span data-stu-id="2e3e4-107">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="2e3e4-108">Per altre informazioni vedere [Introduzione alle migrazioni](xref:data/ef-mvc/migrations#introduction-to-migrations).</span><span class="sxs-lookup"><span data-stu-id="2e3e4-108">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="2e3e4-109">Il comando `ef database update` esegue il metodo `Up` nel file *Migrations/\<time-stamp>_InitialCreate.cs*, che crea il database.</span><span class="sxs-lookup"><span data-stu-id="2e3e4-109">The `ef database update` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file, which creates the database.</span></span>