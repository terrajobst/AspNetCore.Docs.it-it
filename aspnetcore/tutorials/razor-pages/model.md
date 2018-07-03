---
title: Aggiungere un modello a un'app Razor Pages in ASP.NET Core
author: rick-anderson
description: Scoprire come aggiungere classi per la gestione dei film in un database tramite Entity Framework Core (EF Core).
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/model
ms.openlocfilehash: 508cca07fa96c20e228d2c55c9fb101f7fc3cb02
ms.sourcegitcommit: 79b756ea03eae77a716f500ef88253ee9b1464d2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/22/2018
ms.locfileid: "36327552"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a>Aggiungere un modello a un'app Razor Pages in ASP.NET Core

::: moniker range=">= aspnetcore-2.1"

[!INCLUDE [model1](~/includes/RP/model1.md)]

## <a name="add-a-data-model"></a>Aggiungere un modello di dati

In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto **RazorPagesMovie** > **Aggiungi** > **Nuova cartella**. Assegnare il nome *Modelli* alla cartella.

Fare clic con il pulsante destro del mouse sulla cartella *Models*. Selezionare **Aggiungi** > **Classe**. Assegnare il nome **Movie** alla classe e aggiungere le proprietà seguenti:

Sostituire il contenuto della classe `Movie` con il codice seguente:

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie21/Models/Movie1.cs?name=snippet)]

## <a name="scaffold-the-movie-model"></a>Eseguire lo scaffolding del modello di filmato

In questa sezione viene eseguito lo scaffolding del modello di filmato. Lo strumento di scaffolding crea quindi le pagine per le operazioni CRUD (creazione, lettura, aggiornamento ed eliminazione) per il modello di filmato.

Creare una cartella *Pages/Movies*:

* In **Esplora soluzioni** fare clic con il pulsante destro del mouse sulla cartella *Pages* > **Aggiungi** > **Nuova cartella**.
* Assegnare il nome *Movies* alla cartella

In **Esplora soluzioni** fare clic con il pulsante destro del mouse sulla cartella *Pages/Movies* > **Aggiungi** > **Nuovo elemento di scaffolding**.

![Immagine relativa alle istruzioni precedenti.](model/_static/sca.png)

Nella finestra di dialogo **Aggiungi scaffolding** selezionare **Pagine Razor che usano Entity Framework (CRUD)** > **Aggiungi**.

![Immagine relativa alle istruzioni precedenti.](model/_static/add_scaffold.png)

Completare la finestra di dialogo **Pagine Razor che usano Entity Framework (CRUD)**:

* Nel menu a discesa **Classe modello** selezionare **Movie (RazorPagesMovie.Models)**.
* Nella riga **Classe contesto di dati** selezionare il segno più **+** e accettare il nome generato **RazorPagesMovie.Models.RazorPagesMovieContext**.
* Nell'elenco a discesa **Classe contesto di dati** selezionare **RazorPagesMovie.Models.RazorPagesMovieContext**
* Selezionare **Aggiungi**.

![Immagine relativa alle istruzioni precedenti.](model/_static/arp.png)

<a name="pmc"></a>
## <a name="perform-initial-migration"></a>Eseguire la migrazione iniziale

In questa sezione viene usata la Console di Gestione pacchetti per:

* Aggiungere una migrazione iniziale.
* Aggiornare il database con la migrazione iniziale.

Dal menu **Strumenti** selezionare **Gestione pacchetti NuGet** >  **Console di Gestione pacchetti**.

  ![Menu della Console di Gestione pacchetti](../first-mvc-app/adding-model/_static/pmc.png)

Nella Console di Gestione pacchetti immettere i comandi seguenti:

```PMC
Add-Migration Initial
Update-Database
```

In alternativa, è possibile usare i comandi dell'interfaccia della riga di comando di .NET Core seguenti:

```console
dotnet ef migrations add Initial
dotnet ef database update
```

Ignorare il messaggio di avviso seguente che sarà risolto nell'esercitazione successiva:

`Microsoft.EntityFrameworkCore.Model.Validation[30000]`

      *No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'.*

Il comando `Add-Migration` genera un codice per creare lo schema del database iniziale. Lo schema è basato sul modello specificato in `RazorPagesMovieContext` (nel file *Data/RazorPagesMovieContext.cs*). L'argomento `Initial` viene usato per denominare le migrazioni. È possibile usare qualunque nome, ma per convenzione si sceglie un nome che descrive la migrazione. Per altre informazioni vedere [Introduzione alle migrazioni](xref:data/ef-mvc/migrations#introduction-to-migrations).

Il comando `Update-Database` esegue il metodo `Up` nel file *Migrations/{time-stamp}_InitialCreate.cs*, che crea il database.

Se viene visualizzato l'errore:

SqlException: Impossibile aprire il database "RazorPagesMovieContext-GUID" richiesto dall'account di accesso. Accesso non riuscito.
Accesso non riuscito per l'utente "Nome-utente".

Non è stato eseguita la [migrazione](#pmc).
::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!INCLUDE [model1](~/includes/RP/model1.md)]

## <a name="add-a-data-model"></a>Aggiungere un modello di dati

In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto **RazorPagesMovie** > **Aggiungi** > **Nuova cartella**. Assegnare il nome *Modelli* alla cartella.

Fare clic con il pulsante destro del mouse sulla cartella *Models*. Selezionare **Aggiungi** > **Classe**. Assegnare il nome **Movie** alla classe e aggiungere le proprietà seguenti:

[!INCLUDE [model 2](~/includes/RP/model2.md)]

<a name="cs"></a>
### <a name="add-a-database-connection-string"></a>Aggiungere una stringa di connessione del database

Aggiungere una stringa di connessione al file *appsettings.json*.

[!code-json[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=8-10)]

<a name="reg"></a>
###  <a name="register-the-database-context"></a>Registrare il contesto del database

Registrare il contesto del database con il contenitore [Inserimento dipendenze](xref:fundamentals/dependency-injection) nel [ metodo ConfigureServices della classe Startup](xref:fundamentals/startup#the-startup-class) (*Startup.cs*):

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=3-5,7-9)]

Compilare il progetto per verificare che non ci siano errori.

<a name="pmc"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a>Aggiungere gli strumenti di scaffolding ed eseguire la migrazione iniziale

In questa sezione viene usata la Console di Gestione pacchetti per:

* Aggiungere il pacchetto di generazione codice Web di Visual Studio. Questo pacchetto è necessario per eseguire il motore di scaffolding.
* Aggiungere una migrazione iniziale.
* Aggiornare il database con la migrazione iniziale.

Dal menu **Strumenti** selezionare **Gestione pacchetti NuGet** >  **Console di Gestione pacchetti**.

  ![Menu della Console di Gestione pacchetti](../first-mvc-app/adding-model/_static/pmc.png)

Nella Console di Gestione pacchetti immettere i comandi seguenti:

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design -Version 2.0.3
Add-Migration Initial
Update-Database
```

In alternativa, è possibile usare i comandi dell'interfaccia della riga di comando di .NET Core seguenti:

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet ef migrations add Initial
dotnet ef database update
```

Ignorare il messaggio seguente:

    `Microsoft.EntityFrameworkCore.Model.Validation[30000]`

      *No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'*

Sarà risolto nell'esercitazione successiva.

Il comando `Install-Package` installa gli strumenti necessari a eseguire il motore di scaffolding.

Il comando `Add-Migration` genera un codice per creare lo schema del database iniziale. Lo schema è basato sul modello specificato in `DbContext` (nel file *Models/MovieContext.cs*). L'argomento `Initial` viene usato per denominare le migrazioni. È possibile usare qualunque nome, ma per convenzione si sceglie un nome che descrive la migrazione. Per altre informazioni vedere [Introduzione alle migrazioni](xref:data/ef-mvc/migrations#introduction-to-migrations).

Il comando `Update-Database` esegue il metodo `Up` nel file *Migrations/{time-stamp}_InitialCreate.cs*, che crea il database.

[!INCLUDE [model 4windows](~/includes/RP/model4Win.md)]

[!INCLUDE [model 4](~/includes/RP/model4tbl.md)]

::: moniker-end

<a name="test"></a>

### <a name="test-the-app"></a>Eseguire il test dell'app

* Eseguire l'app e accodare `/Movies` all'URL nel browser (`http://localhost:port/movies`).
* Eseguire il test del collegamento **Crea**.

  ![Pagina Crea](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* Eseguire il test dei collegamenti **Modifica**, **Dettagli** e **Elimina**.

Se viene visualizzata un'eccezione SQL, verificare di avere eseguito le migrazioni e di avere aggiornato il database.

L'esercitazione successiva illustra i file creati tramite scaffolding.

> [!div class="step-by-step"]
> [Precedente: Introduzione](xref:tutorials/razor-pages/razor-pages-start)
> [Successivo: Pagine Razor create tramite scaffolding](xref:tutorials/razor-pages/page)    
