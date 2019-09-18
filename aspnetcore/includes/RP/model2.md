<a name="dc"></a>

### <a name="add-a-database-context-class"></a><span data-ttu-id="b31ef-101">Aggiungere una classe di contesto dei dati</span><span class="sxs-lookup"><span data-stu-id="b31ef-101">Add a database context class</span></span>

<span data-ttu-id="b31ef-102">Nel progetto RazorPagesMovie creare una nuova cartella denominata *Data*.</span><span class="sxs-lookup"><span data-stu-id="b31ef-102">In the RazorPagesMovie project, create a new folder called *Data*.</span></span> <span data-ttu-id="b31ef-103">Aggiungere la classe `RazorPagesMovieContext` seguente alla cartella *Data*:</span><span class="sxs-lookup"><span data-stu-id="b31ef-103">Add the following `RazorPagesMovieContext` class to the *Data* folder:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

<span data-ttu-id="b31ef-104">Il codice precedente crea una proprietà `DbSet` per il set di entità.</span><span class="sxs-lookup"><span data-stu-id="b31ef-104">The preceding code creates a `DbSet` property for the entity set.</span></span> <span data-ttu-id="b31ef-105">Nella terminologia di Entity Framework, un set di entità corrisponde in genere alla tabella di un database e un'entità corrisponde a una riga della tabella.</span><span class="sxs-lookup"><span data-stu-id="b31ef-105">In Entity Framework terminology, an entity set typically corresponds to a database table, and an entity corresponds to a row in the table.</span></span>

<a name="cs"></a>

### <a name="add-a-database-connection-string"></a><span data-ttu-id="b31ef-106">Aggiungere una stringa di connessione del database</span><span class="sxs-lookup"><span data-stu-id="b31ef-106">Add a database connection string</span></span>

<span data-ttu-id="b31ef-107">Aggiungere una stringa di connessione al file *appsettings.json* come illustrato nel codice evidenziato seguente:</span><span class="sxs-lookup"><span data-stu-id="b31ef-107">Add a connection string to the *appsettings.json* file as shown in the following highlighted code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/appsettings_SQLite.json?highlight=10-12)]

### <a name="add-nuget-packages-and-ef-tools"></a><span data-ttu-id="b31ef-108">Aggiungere i pacchetti NuGet e gli strumenti EF</span><span class="sxs-lookup"><span data-stu-id="b31ef-108">Add NuGet packages and EF tools</span></span>

<span data-ttu-id="b31ef-109">Aprire un terminale per il progetto RazorPagesMovie.</span><span class="sxs-lookup"><span data-stu-id="b31ef-109">Open a terminal for the RazorPagesMovie project.</span></span>  <span data-ttu-id="b31ef-110">Fare clic con il pulsante destro del mouse sul nome del progetto nella barra di progettazione/layout e passare a **Strumenti > Apri** nel terminale.</span><span class="sxs-lookup"><span data-stu-id="b31ef-110">Right click the project name in the design/layout bar and go to **Tools > Open** in Terminal.</span></span> <span data-ttu-id="b31ef-111">Eseguire i comandi seguenti dell'interfaccia della riga di comando di .NET Core nel terminale:</span><span class="sxs-lookup"><span data-stu-id="b31ef-111">Run the following .NET Core CLI commands in the Termial:</span></span>

```dotnetcli
dotnet tool install --global dotnet-ef --version 3.0.0-*
dotnet add package Microsoft.EntityFrameworkCore.SQLite --version 3.0.0-*
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 3.0.0-*
dotnet add package Microsoft.EntityFrameworkCore.Design --version 3.0.0-*
dotnet add package Microsoft.EntityFrameworkCore.SqlServer --version 3.0.0-*
```

<span data-ttu-id="b31ef-112">I comandi precedenti aggiungono al progetto gli strumenti di Entity Framework Core per l'interfaccia della riga di comando di .NET e diversi pacchetti.</span><span class="sxs-lookup"><span data-stu-id="b31ef-112">The preceding commands add Entity Framework Core Tools for the .NET CLI and several packages to the project.</span></span> <span data-ttu-id="b31ef-113">Il pacchetto `Microsoft.VisualStudio.Web.CodeGeneration.Design` è necessario per lo scaffolding.</span><span class="sxs-lookup"><span data-stu-id="b31ef-113">The `Microsoft.VisualStudio.Web.CodeGeneration.Design` package is required for scaffolding.</span></span>

<a name="reg"></a>

### <a name="register-the-database-context"></a><span data-ttu-id="b31ef-114">Registrare il contesto del database</span><span class="sxs-lookup"><span data-stu-id="b31ef-114">Register the database context</span></span>

<span data-ttu-id="b31ef-115">Aggiungere le istruzioni `using` seguenti all'inizio di *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="b31ef-115">Add the following `using` statements at the top of *Startup.cs*:</span></span>

```csharp
using RazorPagesMovie.Models;
using Microsoft.EntityFrameworkCore;
```

<span data-ttu-id="b31ef-116">Registrare il contesto del database con il contenitore di [inserimento delle dipendenze](xref:fundamentals/dependency-injection) in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="b31ef-116">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-9)]

### <a name="add-required-nuget-packages"></a><span data-ttu-id="b31ef-117">Aggiungere i pacchetti NuGet necessari</span><span class="sxs-lookup"><span data-stu-id="b31ef-117">Add required NuGet packages</span></span>

<span data-ttu-id="b31ef-118">Eseguire il comando seguente dell'interfaccia della riga di comando di .NET Core CLI per aggiungere SQLite e CodeGeneration.Design al progetto:</span><span class="sxs-lookup"><span data-stu-id="b31ef-118">Run the following .NET Core CLI command to add SQLite and CodeGeneration.Design  to the project:</span></span>

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.SQLite
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
```

<span data-ttu-id="b31ef-119">Il pacchetto `Microsoft.VisualStudio.Web.CodeGeneration.Design` è necessario per lo scaffolding.</span><span class="sxs-lookup"><span data-stu-id="b31ef-119">The `Microsoft.VisualStudio.Web.CodeGeneration.Design` package is required for scaffolding.</span></span>

<a name="reg"></a>

### <a name="register-the-database-context"></a><span data-ttu-id="b31ef-120">Registrare il contesto del database</span><span class="sxs-lookup"><span data-stu-id="b31ef-120">Register the database context</span></span>

<span data-ttu-id="b31ef-121">Aggiungere le istruzioni `using` seguenti all'inizio di *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="b31ef-121">Add the following `using` statements at the top of *Startup.cs*:</span></span>

```csharp
using RazorPagesMovie.Models;
using Microsoft.EntityFrameworkCore;
```

<span data-ttu-id="b31ef-122">Registrare il contesto del database con il contenitore di [inserimento delle dipendenze](xref:fundamentals/dependency-injection) in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="b31ef-122">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

<span data-ttu-id="b31ef-123">Compilare il progetto per il controllo di eventuali errori.</span><span class="sxs-lookup"><span data-stu-id="b31ef-123">Build the project as a check for errors.</span></span>
::: moniker-end
