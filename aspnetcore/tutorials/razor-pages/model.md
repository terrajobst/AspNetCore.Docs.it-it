---
title: Aggiungere un modello a un'app Razor Pages in ASP.NET Core
author: rick-anderson
description: Scoprire come aggiungere classi per la gestione dei film in un database tramite Entity Framework Core (EF Core).
ms.author: riande
monikerRange: '>= aspnetcore-2.2'
ms.date: 12/3/2018
uid: tutorials/razor-pages/model
ms.openlocfilehash: 91fee1db820493be671fecaee3cfb4c1b7df8bd3
ms.sourcegitcommit: 49faca2644590fc081d86db46ea5e29edfc28b7b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/09/2018
ms.locfileid: "53121363"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a>Aggiungere un modello a un'app Razor Pages in ASP.NET Core

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE[](~/includes/rp/download.md)]

In questa sezione si aggiungono alcune classi per la gestione di filmati in un database. Queste classi vengono usate con [Entity Framework Core](/ef/core) (EF Core) per lavorare in un database. EF Core è un framework object-relational mapping (ORM) che semplifica il codice di accesso ai dati.

Le classi di modello sono dette classi POCO (da "plain-old CLR objects") perché non hanno alcuna dipendenza in EF Core. Definiscono le proprietà dei dati archiviati nel database.

[Visualizzare o scaricare](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages-start/sample/) l'esempio.

## <a name="add-a-data-model"></a>Aggiungere un modello di dati

<!-- VS -------------------------->

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Fare clic con il pulsante destro del mouse sul progetto **RazorPagesMovie** > **Aggiungi** > **Nuova cartella**. Assegnare il nome *Modelli* alla cartella.

Fare clic con il pulsante destro del mouse sulla cartella *Models*. Selezionare **Aggiungi** > **Classe**. Denominare la classe **Movie**.

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

<!-- Code -------------------------->

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Aggiungere una cartella denominata *Modelli*.
* Aggiungere una classe alla cartella *Modello* denominata *Movie.cs*.

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

<!-- Mac -------------------------->
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio per Mac](#tab/visual-studio-mac)

* In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto **RazorPagesMovie**, quindi selezionare **Aggiungi** > **Nuova cartella**. Assegnare il nome *Modelli* alla cartella.
* Fare clic con il pulsante destro del mouse sulla cartella *Modelli* e scegliere **Aggiungi** > **Nuovo file**.
* Nella finestra di dialogo **Nuovo file**:

  * Selezionare **Generale** nel riquadro a sinistra.
  * Selezionare **Classe vuota** nel riquadro centrale.
  * Denominare la classe **Filmato** e selezionare **Nuovo**.

[!INCLUDE [model 1b](~/includes/RP/model1b.md)]

[!INCLUDE [model 2](~/includes/RP/model2.md)]

<!-- End of VS tabs -->

---

Compilare il progetto per verificare che non siano presenti errori di compilazione.

## <a name="scaffold-the-movie-model"></a>Eseguire lo scaffolding del modello di filmato

In questa sezione viene eseguito lo scaffolding del modello di filmato. Lo strumento di scaffolding crea quindi le pagine per le operazioni CRUD (creazione, lettura, aggiornamento ed eliminazione) per il modello di filmato.

<!-- VS -------------------------->

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Creare una cartella *Pages/Movies*:

* Fare clic con il pulsante destro del mouse sulla cartella *Pages* > **Aggiungi** > **Nuova cartella**.
* Assegnare il nome *Movies* alla cartella

Fare clic con il pulsante destro del mouse sulla cartella *Pages* > **Aggiungi** > **Nuovo elemento di scaffolding**.

![Immagine relativa alle istruzioni precedenti.](model/_static/sca.png)

Nella finestra di dialogo **Aggiungi scaffolding** selezionare **Pagine Razor che usano Entity Framework (CRUD)** > **Aggiungi**.

![Immagine relativa alle istruzioni precedenti.](model/_static/add_scaffold.png)

Completare la finestra di dialogo **Pagine Razor che usano Entity Framework (CRUD)**:

* Nel menu a discesa **Classe modello** selezionare **Movie (RazorPagesMovie.Models)**.
* Nella riga **Classe contesto di dati** selezionare il segno più **+** e accettare il nome generato **RazorPagesMovie.Models.RazorPagesMovieContext**.
* Selezionare **Aggiungi**.

![Immagine relativa alle istruzioni precedenti.](model/_static/arp.png)

Il file *appsettings.json* è stato aggiornato con la stringa di connessione usata per connettersi a un database locale.

<!-- Code -------------------------->

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* Aprire una finestra di comando nella directory del progetto (la directory che contiene i file *Program.cs*, *Startup.cs* e *csproj*).
* Installare lo strumento di scaffolding:

  ```console
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* **Per Windows**: eseguire il comando seguente:

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* **Per MacOS e Linux**: eseguire il comando seguente:

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

<!-- Mac -------------------------->

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio per Mac](#tab/visual-studio-mac)

* Aprire una finestra di comando nella directory del progetto (la directory che contiene i file *Program.cs*, *Startup.cs* e *csproj*).
* Eseguire il comando seguente:

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc RazorPagesMovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold gen params](~/includes/RP/model4.md)]

---

Il processo di scaffolding crea e aggiorna i file seguenti:

### <a name="files-created"></a>File creati

* *Pages/Movies*: pagine Create (Crea), Delete (Elimina), Details (Dettagli), Edit (Modifica) e Index (Indice).
* *Data/RazorPagesMovieContext.cs*

### <a name="file-updated"></a>File aggiornato

* *Startup.cs*

I file creati e aggiornati sono illustrati nella sezione successiva.

<a name="pmc"></a>

## <a name="initial-migration"></a>Migrazione iniziale

<!-- VS -------------------------->

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

<!-- VS -------------------------->

In questa sezione viene usata la Console di Gestione pacchetti (PMC) per:

* Aggiungere una migrazione iniziale.
* Aggiornare il database con la migrazione iniziale.

Dal menu **Strumenti** selezionare **Gestione pacchetti NuGet** > **Console di Gestione pacchetti**.

  ![Menu della Console di Gestione pacchetti](../first-mvc-app/adding-model/_static/pmc.png)

Nella Console di Gestione pacchetti immettere i comandi seguenti:

```PMC
Add-Migration Initial
Update-Database
```

<!-- Code -------------------------->

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

<!-- Mac -------------------------->

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio per Mac](#tab/visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

---  
<!-- End of VS tabs -->

Il comando `ef migrations add InitialCreate` genera un codice per creare lo schema del database iniziale. Lo schema è basato sul modello specificato in `DbContext` (nel file *Models/RazorPagesMovieContext.cs*). L'argomento `InitialCreate` viene usato per denominare le migrazioni. È possibile usare qualsiasi nome, ma per convenzione viene selezionato un nome che descrive la migrazione.

Il comando `ef database update` esegue il metodo `Up` nel file *Migrations/\<time-stamp>_InitialCreate.cs*. Il metodo `Up` crea il database.

<!-- VS -------------------------->

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

## <a name="examine-the-context-registered-with-dependency-injection"></a>Esaminare il contesto registrato con l'inserimento di dipendenze

ASP.NET Core viene compilato con l'[inserimento di dipendenze](xref:fundamentals/dependency-injection). I servizi, ad esempio il contesto di database di Entity Framework Core, vengono registrati con l'inserimento delle dipendenze durante l'avvio dell'applicazione. Questi servizi vengono quindi offerti ai componenti per cui sono necessari (ad esempio Razor Pages) tramite i parametri del costruttore. Più avanti nell'esercitazione viene illustrato il codice del costruttore che ottiene un'istanza del contesto di database.

Lo strumento di scaffolding ha creato automaticamente un contesto del database e lo ha registrato con il contenitore di inserimento delle dipendenze.

Esaminare il metodo `Startup.ConfigureServices`. La riga evidenziata è stata aggiunta dallo scaffolder:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

`RazorPagesMovieContext` coordina la funzionalità di EF Core (Create, Read, Update, Delete e così via) per il modello `Movie`. Il contesto dei dati (`RazorPagesMovieContext`) è derivato da [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext). Il contesto dei dati specifica le entità incluse nel modello di dati.

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Data/RazorPagesMovieContext.cs)]

Il codice precedente crea una proprietà [DbSet/\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) per il set di entità. Nella terminologia di Entity Framework, un set di entità corrisponde in genere alla tabella di un database. Un'entità corrisponde a una riga nella tabella.

Il nome della stringa di connessione viene passato al contesto chiamando un metodo in un oggetto [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions). Per lo sviluppo locale, il [sistema di configurazione di ASP.NET Core](xref:fundamentals/configuration/index) legge la stringa di connessione dal file *appsettings.json*.
<!-- Code -------------------------->

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

<!-- Mac -------------------------->

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio per Mac](#tab/visual-studio-mac)

<!-- End of VS tabs -->

---

Il comando `Add-Migration` genera un codice per creare lo schema del database iniziale. Lo schema è basato sul modello specificato in `RazorPagesMovieContext` (nel file *Data/RazorPagesMovieContext.cs*). L'argomento `Initial` viene usato per denominare le migrazioni. È possibile usare qualsiasi nome, ma per convenzione viene usato un nome che descrive la migrazione. Per altre informazioni vedere [Introduzione alle migrazioni](xref:data/ef-mvc/migrations#introduction-to-migrations).

Il comando `Update-Database` esegue il metodo `Up` nel file *Migrations/{time-stamp}_InitialCreate.cs*, che crea il database.

<a name="test"></a>

### <a name="test-the-app"></a>Eseguire il test dell'app

* Eseguire l'app e accodare `/Movies` all'URL nel browser (`http://localhost:port/movies`).

Se viene visualizzato l'errore:

```console
SqlException: Cannot open database "RazorPagesMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

Non è stato eseguita la [migrazione](#pmc).

* Eseguire il test del collegamento **Crea**.

  ![Pagina Crea](model/_static/conan.png)
  
  > [!NOTE]
  > Potrebbe non essere possibile immettere virgole decimali nel campo `Price`. Per supportare la [convalida jQuery](https://jqueryvalidation.org/) per impostazioni locali diverse dall'inglese che usano la virgola (",") come separatore decimale e per formati di data diversi da quello dell'inglese (Stati Uniti), è necessario globalizzare l'app. Per istruzioni sulla globalizzazione, vedere [questo problema su GitHub](https://github.com/aspnet/Docs/issues/4076#issuecomment-326590420).

* Eseguire il test dei collegamenti **Modifica**, **Dettagli** e **Elimina**.

L'esercitazione successiva illustra i file creati tramite scaffolding.

> [!div class="step-by-step"]
> [Precedente: Introduzione](xref:tutorials/razor-pages/razor-pages-start)
> [Successivo: Pagine Razor create tramite scaffolding](xref:tutorials/razor-pages/page)
