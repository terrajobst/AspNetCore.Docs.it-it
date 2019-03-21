---
ms.openlocfilehash: 82c0f3076e846045ca85697e150ff0ab9be75dba
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/19/2019
ms.locfileid: "58214487"
---
<a name="dc"></a>

Aggiungere la classe `MvcMovieContext` seguente alla cartella *Models*:  

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Data/MvcMovieContext.cs)]

Il codice precedente crea una proprietà `DbSet` per il set di entità. Nella terminologia di Entity Framework, un set di entità corrisponde in genere alla tabella di un database e un'entità corrisponde a una riga della tabella.

<a name="cs"></a>

### <a name="add-a-database-connection-string"></a>Aggiungere una stringa di connessione del database

Aggiungere una stringa di connessione al file *appsettings.json*:

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

### <a name="add-required-nuget-packages"></a>Aggiungere i pacchetti NuGet necessari

Eseguire il comando seguente dell'interfaccia della riga di comando di .NET Core CLI per aggiungere SQLite e CodeGeneration.Design al progetto:

```console
dotnet add package Microsoft.EntityFrameworkCore.SQLite
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
```

Il pacchetto `Microsoft.VisualStudio.Web.CodeGeneration.Design` è necessario per lo scaffolding.

<a name="reg"></a>

### <a name="register-the-database-context"></a>Registrare il contesto del database

Aggiungere le istruzioni `using` seguenti all'inizio di *Startup.cs*:

```csharp
using MvcMovie.Models;
using Microsoft.EntityFrameworkCore;
```

Registrare il contesto del database con il contenitore di [inserimento delle dipendenze](xref:fundamentals/dependency-injection) in `Startup.ConfigureServices`.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

Compilare il progetto per il controllo di eventuali errori.