<a name="dc"></a>

### <a name="add-a-database-context-class"></a><span data-ttu-id="42c5b-101">Aggiungere una classe di contesto dei dati</span><span class="sxs-lookup"><span data-stu-id="42c5b-101">Add a database context class</span></span>

<span data-ttu-id="42c5b-102">Nel progetto RazorPagesMovie creare una nuova cartella denominata *Data*.</span><span class="sxs-lookup"><span data-stu-id="42c5b-102">In the RazorPagesMovie project, create a new folder called *Data*.</span></span> <span data-ttu-id="42c5b-103">Aggiungere la classe `RazorPagesMovieContext` seguente alla cartella *Data*:</span><span class="sxs-lookup"><span data-stu-id="42c5b-103">Add the following `RazorPagesMovieContext` class to the *Data* folder:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="42c5b-104">Il codice precedente crea una proprietà `DbSet` per il set di entità.</span><span class="sxs-lookup"><span data-stu-id="42c5b-104">The preceding code creates a `DbSet` property for the entity set.</span></span> <span data-ttu-id="42c5b-105">Nella terminologia di Entity Framework, un set di entità corrisponde in genere alla tabella di un database e un'entità corrisponde a una riga della tabella.</span><span class="sxs-lookup"><span data-stu-id="42c5b-105">In Entity Framework terminology, an entity set typically corresponds to a database table, and an entity corresponds to a row in the table.</span></span>

<a name="cs"></a>

### <a name="add-a-database-connection-string"></a><span data-ttu-id="42c5b-106">Aggiungere una stringa di connessione del database</span><span class="sxs-lookup"><span data-stu-id="42c5b-106">Add a database connection string</span></span>

<span data-ttu-id="42c5b-107">Aggiungere una stringa di connessione al file *appsettings.json* come illustrato nel codice evidenziato seguente:</span><span class="sxs-lookup"><span data-stu-id="42c5b-107">Add a connection string to the *appsettings.json* file as shown in the following highlighted code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/appsettings_SQLite.json?highlight=10-12)]

### <a name="add-nuget-packages-and-ef-tools"></a><span data-ttu-id="42c5b-108">Aggiungere i pacchetti NuGet e gli strumenti EF</span><span class="sxs-lookup"><span data-stu-id="42c5b-108">Add NuGet packages and EF tools</span></span>

[!INCLUDE[](~/includes/add-EF-NuGet-SQLite-CLI.md)]

<a name="reg"></a>

### <a name="register-the-database-context"></a><span data-ttu-id="42c5b-109">Registrare il contesto del database</span><span class="sxs-lookup"><span data-stu-id="42c5b-109">Register the database context</span></span>

<span data-ttu-id="42c5b-110">Aggiungere le istruzioni `using` seguenti all'inizio di *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="42c5b-110">Add the following `using` statements at the top of *Startup.cs*:</span></span>

```csharp
using RazorPagesMovie.Models;
using Microsoft.EntityFrameworkCore;
```

<span data-ttu-id="42c5b-111">Registrare il contesto del database con il contenitore di [inserimento delle dipendenze](xref:fundamentals/dependency-injection) in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="42c5b-111">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-9)]

### <a name="add-required-nuget-packages"></a><span data-ttu-id="42c5b-112">Aggiungere i pacchetti NuGet necessari</span><span class="sxs-lookup"><span data-stu-id="42c5b-112">Add required NuGet packages</span></span>

<span data-ttu-id="42c5b-113">Eseguire il comando interfaccia della riga di comando di .NET Core seguente per aggiungere SQLite e CodeGeneration. Design al progetto:</span><span class="sxs-lookup"><span data-stu-id="42c5b-113">Run the following .NET Core CLI command to add SQLite and CodeGeneration.Design to the project:</span></span>

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.SQLite
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
```

<span data-ttu-id="42c5b-114">Il pacchetto `Microsoft.VisualStudio.Web.CodeGeneration.Design` è necessario per lo scaffolding.</span><span class="sxs-lookup"><span data-stu-id="42c5b-114">The `Microsoft.VisualStudio.Web.CodeGeneration.Design` package is required for scaffolding.</span></span>

<a name="reg"></a>

### <a name="register-the-database-context"></a><span data-ttu-id="42c5b-115">Registrare il contesto del database</span><span class="sxs-lookup"><span data-stu-id="42c5b-115">Register the database context</span></span>

<span data-ttu-id="42c5b-116">Aggiungere le istruzioni `using` seguenti all'inizio di *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="42c5b-116">Add the following `using` statements at the top of *Startup.cs*:</span></span>

```csharp
using RazorPagesMovie.Models;
using Microsoft.EntityFrameworkCore;
```

<span data-ttu-id="42c5b-117">Registrare il contesto del database con il contenitore di [inserimento delle dipendenze](xref:fundamentals/dependency-injection) in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="42c5b-117">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

<span data-ttu-id="42c5b-118">Compilare il progetto per il controllo di eventuali errori.</span><span class="sxs-lookup"><span data-stu-id="42c5b-118">Build the project as a check for errors.</span></span>

::: moniker-end
