<a name="dc"></a>

### <a name="add-a-database-context-class"></a>Aggiungere una classe di contesto dei dati

Nel progetto RazorPagesMovie creare una nuova cartella denominata *Data*. Aggiungere la classe `RazorPagesMovieContext` seguente alla cartella *Data*:

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

Il codice precedente crea una proprietà `DbSet` per il set di entità. Nella terminologia di Entity Framework, un set di entità corrisponde in genere alla tabella di un database e un'entità corrisponde a una riga della tabella.

<a name="cs"></a>

### <a name="add-a-database-connection-string"></a>Aggiungere una stringa di connessione del database

Aggiungere una stringa di connessione al file *appsettings.json* come illustrato nel codice evidenziato seguente:

::: moniker range=">= aspnetcore-3.0"

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/appsettings_SQLite.json?highlight=10-12)]

### <a name="add-nuget-packages-and-ef-tools"></a>Aggiungere i pacchetti NuGet e gli strumenti EF

Aprire un terminale per il progetto RazorPagesMovie.  Fare clic con il pulsante destro del mouse sul nome del progetto nella barra di progettazione/layout e passare a **Strumenti > Apri** nel terminale. Eseguire i comandi seguenti dell'interfaccia della riga di comando di .NET Core nel terminale:

```dotnetcli
dotnet tool install --global dotnet-ef --version 3.0.0-*
dotnet add package Microsoft.EntityFrameworkCore.SQLite --version 3.0.0-*
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 3.0.0-*
dotnet add package Microsoft.EntityFrameworkCore.Design --version 3.0.0-*
dotnet add package Microsoft.EntityFrameworkCore.SqlServer --version 3.0.0-*
```

I comandi precedenti aggiungono al progetto gli strumenti di Entity Framework Core per l'interfaccia della riga di comando di .NET e diversi pacchetti. Il pacchetto `Microsoft.VisualStudio.Web.CodeGeneration.Design` è necessario per lo scaffolding.

<a name="reg"></a>

### <a name="register-the-database-context"></a>Registrare il contesto del database

Aggiungere le istruzioni `using` seguenti all'inizio di *Startup.cs*:

```csharp
using RazorPagesMovie.Models;
using Microsoft.EntityFrameworkCore;
```

Registrare il contesto del database con il contenitore di [inserimento delle dipendenze](xref:fundamentals/dependency-injection) in `Startup.ConfigureServices`.

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-9)]

### <a name="add-required-nuget-packages"></a>Aggiungere i pacchetti NuGet necessari

Eseguire il comando seguente dell'interfaccia della riga di comando di .NET Core CLI per aggiungere SQLite e CodeGeneration.Design al progetto:

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.SQLite
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
```

Il pacchetto `Microsoft.VisualStudio.Web.CodeGeneration.Design` è necessario per lo scaffolding.

<a name="reg"></a>

### <a name="register-the-database-context"></a>Registrare il contesto del database

Aggiungere le istruzioni `using` seguenti all'inizio di *Startup.cs*:

```csharp
using RazorPagesMovie.Models;
using Microsoft.EntityFrameworkCore;
```

Registrare il contesto del database con il contenitore di [inserimento delle dipendenze](xref:fundamentals/dependency-injection) in `Startup.ConfigureServices`.

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

Compilare il progetto per il controllo di eventuali errori.
::: moniker-end
