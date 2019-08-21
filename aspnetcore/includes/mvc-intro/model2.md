::: moniker range=">= aspnetcore-3.0"

<a name="dc"></a>

<span data-ttu-id="7b787-101">Creare una cartella *Data*.</span><span class="sxs-lookup"><span data-stu-id="7b787-101">Create a *Data* folder.</span></span>

<span data-ttu-id="7b787-102">Aggiungere la classe `MvcMovieContext` seguente alla cartella *Data*:</span><span class="sxs-lookup"><span data-stu-id="7b787-102">Add the following `MvcMovieContext` class to the *Data* folder:</span></span>  

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/zDocOnly/MvcMovieContext.cs?name=snippet)]

<span data-ttu-id="7b787-103">Il codice precedente crea una proprietà `DbSet` per il set di entità.</span><span class="sxs-lookup"><span data-stu-id="7b787-103">The preceding code creates a `DbSet` property for the entity set.</span></span> <span data-ttu-id="7b787-104">Nella terminologia di Entity Framework, un set di entità corrisponde in genere alla tabella di un database e un'entità corrisponde a una riga della tabella.</span><span class="sxs-lookup"><span data-stu-id="7b787-104">In Entity Framework terminology, an entity set typically corresponds to a database table, and an entity corresponds to a row in the table.</span></span>

<a name="cs"></a>

### <a name="add-a-database-connection-string"></a><span data-ttu-id="7b787-105">Aggiungere una stringa di connessione del database</span><span class="sxs-lookup"><span data-stu-id="7b787-105">Add a database connection string</span></span>

<span data-ttu-id="7b787-106">Aggiungere una stringa di connessione al file *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="7b787-106">Add a connection string to the *appsettings.json* file:</span></span>

[!code-json[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/appsettings_SQLite.json?highlight=10-12)]

### <a name="add-nuget-packages-and-ef-tools"></a><span data-ttu-id="7b787-107">Aggiungere i pacchetti NuGet e gli strumenti EF</span><span class="sxs-lookup"><span data-stu-id="7b787-107">Add NuGet packages and EF tools</span></span>

<span data-ttu-id="7b787-108">Eseguire i seguenti comandi dell'interfaccia della riga di comando di .NET Core:</span><span class="sxs-lookup"><span data-stu-id="7b787-108">Run the following .NET Core CLI commands:</span></span>

```console
dotnet tool install --global dotnet-ef --version 3.0.0-*
dotnet add package Microsoft.EntityFrameworkCore.SQLite --version 3.0.0-*
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 3.0.0-*
dotnet add package Microsoft.EntityFrameworkCore.Design --version 3.0.0-*
dotnet add package Microsoft.EntityFrameworkCore.SqlServer --version 3.0.0-*
```

<span data-ttu-id="7b787-109">I comandi precedenti aggiungono al progetto gli strumenti di Entity Framework Core per l'interfaccia della riga di comando di .NET e diversi pacchetti.</span><span class="sxs-lookup"><span data-stu-id="7b787-109">The preceding commands add Entity Framework Core Tools for the .NET CLI and several packages to the project.</span></span> <span data-ttu-id="7b787-110">Il pacchetto `Microsoft.VisualStudio.Web.CodeGeneration.Design` è necessario per lo scaffolding.</span><span class="sxs-lookup"><span data-stu-id="7b787-110">The `Microsoft.VisualStudio.Web.CodeGeneration.Design` package is required for scaffolding.</span></span>

<a name="reg"></a>

### <a name="register-the-database-context"></a><span data-ttu-id="7b787-111">Registrare il contesto del database</span><span class="sxs-lookup"><span data-stu-id="7b787-111">Register the database context</span></span>

<span data-ttu-id="7b787-112">Aggiungere le istruzioni `using` seguenti all'inizio di *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="7b787-112">Add the following `using` statements at the top of *Startup.cs*:</span></span>

```csharp
using MvcMovie.Data;
using Microsoft.EntityFrameworkCore;
```

<span data-ttu-id="7b787-113">Registrare il contesto del database con il contenitore di [inserimento delle dipendenze](xref:fundamentals/dependency-injection) in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="7b787-113">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Startup.cs?name=snippet_UseSqlite&highlight=6-7)]

<span data-ttu-id="7b787-114">Compilare il progetto per controllare se sono presenti errori del compilatore.</span><span class="sxs-lookup"><span data-stu-id="7b787-114">Build the project as a check for compiler errors.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="7b787-115">Aggiungere la classe `MvcMovieContext` seguente alla cartella *Models*:</span><span class="sxs-lookup"><span data-stu-id="7b787-115">Add the following `MvcMovieContext` class to the *Models* folder:</span></span>  

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Data/MvcMovieContext.cs)]

<span data-ttu-id="7b787-116">Il codice precedente crea una proprietà `DbSet` per il set di entità.</span><span class="sxs-lookup"><span data-stu-id="7b787-116">The preceding code creates a `DbSet` property for the entity set.</span></span> <span data-ttu-id="7b787-117">Nella terminologia di Entity Framework, un set di entità corrisponde in genere alla tabella di un database e un'entità corrisponde a una riga della tabella.</span><span class="sxs-lookup"><span data-stu-id="7b787-117">In Entity Framework terminology, an entity set typically corresponds to a database table, and an entity corresponds to a row in the table.</span></span>

<a name="cs"></a>

### <a name="add-a-database-connection-string"></a><span data-ttu-id="7b787-118">Aggiungere una stringa di connessione del database</span><span class="sxs-lookup"><span data-stu-id="7b787-118">Add a database connection string</span></span>

<span data-ttu-id="7b787-119">Aggiungere una stringa di connessione al file *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="7b787-119">Add a connection string to the *appsettings.json* file:</span></span>

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

### <a name="add-required-nuget-packages"></a><span data-ttu-id="7b787-120">Aggiungere i pacchetti NuGet necessari</span><span class="sxs-lookup"><span data-stu-id="7b787-120">Add required NuGet packages</span></span>

<span data-ttu-id="7b787-121">Eseguire il comando seguente dell'interfaccia della riga di comando di .NET Core CLI per aggiungere SQLite e CodeGeneration.Design al progetto:</span><span class="sxs-lookup"><span data-stu-id="7b787-121">Run the following .NET Core CLI command to add SQLite and CodeGeneration.Design  to the project:</span></span>

```console
dotnet add package Microsoft.EntityFrameworkCore.SQLite
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
```

<span data-ttu-id="7b787-122">Il pacchetto `Microsoft.VisualStudio.Web.CodeGeneration.Design` è necessario per lo scaffolding.</span><span class="sxs-lookup"><span data-stu-id="7b787-122">The `Microsoft.VisualStudio.Web.CodeGeneration.Design` package is required for scaffolding.</span></span>

<a name="reg"></a>

### <a name="register-the-database-context"></a><span data-ttu-id="7b787-123">Registrare il contesto del database</span><span class="sxs-lookup"><span data-stu-id="7b787-123">Register the database context</span></span>

<span data-ttu-id="7b787-124">Aggiungere le istruzioni `using` seguenti all'inizio di *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="7b787-124">Add the following `using` statements at the top of *Startup.cs*:</span></span>

```csharp
using MvcMovie.Models;
using Microsoft.EntityFrameworkCore;
```

<span data-ttu-id="7b787-125">Registrare il contesto del database con il contenitore di [inserimento delle dipendenze](xref:fundamentals/dependency-injection) in `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="7b787-125">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

<span data-ttu-id="7b787-126">Compilare il progetto per il controllo di eventuali errori.</span><span class="sxs-lookup"><span data-stu-id="7b787-126">Build the project as a check for errors.</span></span>
::: moniker-end