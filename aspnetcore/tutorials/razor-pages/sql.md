---
title: Usare un database e ASP.NET Core
author: rick-anderson
description: Questa sezione illustra l'utilizzo di un database e di ASP.NET Core.
ms.author: riande
ms.date: 12/07/2017
uid: tutorials/razor-pages/sql
ms.openlocfilehash: 3e05f5dbc73c35f1f938346b2eaab8c0fa7d8ab9
ms.sourcegitcommit: d75d8eb26c2cce19876c8d5b65ac8a4b21f625ef
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/19/2019
ms.locfileid: "56410297"
---
# <a name="work-with-a-database-and-aspnet-core"></a><span data-ttu-id="9f26b-103">Usare un database e ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9f26b-103">Work with a database and ASP.NET Core</span></span>

<span data-ttu-id="9f26b-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT) e [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="9f26b-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

<span data-ttu-id="9f26b-105">L'oggetto `RazorPagesMovieContext` gestisce l'attività di connessione al database e di mapping degli oggetti `Movie` ai record di database.</span><span class="sxs-lookup"><span data-stu-id="9f26b-105">The `RazorPagesMovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="9f26b-106">Il contesto del database viene registrato nel contenitore di [inserimento dipendenze](xref:fundamentals/dependency-injection) nel metodo `ConfigureServices` di *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="9f26b-106">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in *Startup.cs*:</span></span>

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9f26b-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9f26b-107">Visual Studio</span></span>](#tab/visual-studio)

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=15-18)]

<!-- Code -------------------------->
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9f26b-108">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9f26b-108">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

<!-- Mac -------------------------->
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="9f26b-109">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="9f26b-109">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

---  
<!-- End of VS tabs -->

<span data-ttu-id="9f26b-110">Per altre informazioni sui metodi usati in `ConfigureServices`, vedere:</span><span class="sxs-lookup"><span data-stu-id="9f26b-110">For more information on the methods used in `ConfigureServices`, see:</span></span>

* <span data-ttu-id="9f26b-111">[Supporto per il Regolamento generale sulla protezione dei dati (GDPR) dell'Unione Europea in ASP.NET Core](xref:security/gdpr) per `CookiePolicyOptions`.</span><span class="sxs-lookup"><span data-stu-id="9f26b-111">[EU General Data Protection Regulation (GDPR) support in ASP.NET Core](xref:security/gdpr) for `CookiePolicyOptions`.</span></span>
* [<span data-ttu-id="9f26b-112">SetCompatibilityVersion</span><span class="sxs-lookup"><span data-stu-id="9f26b-112">SetCompatibilityVersion</span></span>](xref:mvc/compatibility-version)

<span data-ttu-id="9f26b-113">Il sistema di [configurazione](xref:fundamentals/configuration/index) di ASP.NET Core legge la `ConnectionString`.</span><span class="sxs-lookup"><span data-stu-id="9f26b-113">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="9f26b-114">Per lo sviluppo locale, ottiene la stringa di connessione dal file *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="9f26b-114">For local development, it gets the connection string from the *appsettings.json* file.</span></span>

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9f26b-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9f26b-115">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="9f26b-116">Il valore del nome per il database (`Database={Database name}`) sarà diverso per il codice generato.</span><span class="sxs-lookup"><span data-stu-id="9f26b-116">The name value for the database (`Database={Database name}`) will be different for your generated code.</span></span> <span data-ttu-id="9f26b-117">Il valore del nome è arbitrario.</span><span class="sxs-lookup"><span data-stu-id="9f26b-117">The name value is arbitrary.</span></span>

[!code-json[](razor-pages-start/sample/RazorPagesMovie22/appsettings.json)]

<!-- Code -------------------------->
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9f26b-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9f26b-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

<!-- Mac -------------------------->
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="9f26b-119">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="9f26b-119">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!code-json[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings_SQLite.json?highlight=8-10)]

---  
<!-- End of VS tabs -->

<span data-ttu-id="9f26b-120">Quando l'app viene distribuita in un server di test o produzione, è possibile utilizzare una variabile di ambiente per impostare la stringa di connessione su un server di database reale.</span><span class="sxs-lookup"><span data-stu-id="9f26b-120">When the app is deployed to a test or production server, an environment variable can be used to set the connection string to a real database server.</span></span> <span data-ttu-id="9f26b-121">Per altre informazioni, vedere [Configurazione](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="9f26b-121">See [Configuration](xref:fundamentals/configuration/index) for more information.</span></span>

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9f26b-122">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9f26b-122">Visual Studio</span></span>](#tab/visual-studio)

## <a name="sql-server-express-localdb"></a><span data-ttu-id="9f26b-123">LocalDB di SQL Server Express</span><span class="sxs-lookup"><span data-stu-id="9f26b-123">SQL Server Express LocalDB</span></span>

<span data-ttu-id="9f26b-124">Local DB è una versione leggera del motore di database di SQL Server Express appositamente pensato per lo sviluppo di programmi.</span><span class="sxs-lookup"><span data-stu-id="9f26b-124">LocalDB is a lightweight version of the SQL Server Express database engine that's targeted for program development.</span></span> <span data-ttu-id="9f26b-125">Local DB viene avviato su richiesta ed eseguito in modalità utente; non richiede quindi una configurazione complessa.</span><span class="sxs-lookup"><span data-stu-id="9f26b-125">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="9f26b-126">Per impostazione predefinita, Local DB crea file con estensione `*.mdf` nella directory `C:/Users/<user/>`.</span><span class="sxs-lookup"><span data-stu-id="9f26b-126">By default, LocalDB database creates `*.mdf` files in the `C:/Users/<user/>` directory.</span></span>

<a name="ssox"></a>
* <span data-ttu-id="9f26b-127">Dal menu **Visualizzazione** aprire **Esplora oggetti di SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="9f26b-127">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![Menu View](sql/_static/ssox.png)

* <span data-ttu-id="9f26b-129">Fare clic con il pulsante destro del mouse sulla tabella `Movie` e selezionare**Progettazione viste**:</span><span class="sxs-lookup"><span data-stu-id="9f26b-129">Right click on the `Movie` table and select **View Designer**:</span></span>

  ![Menu di scelta rapida aperto per la tabella Movie](sql/_static/design.png)

  ![Tabella Movie aperta nella finestra di progettazione](sql/_static/dv.png)

<span data-ttu-id="9f26b-132">Si noti l'icona a forma di chiave accanto a `ID`.</span><span class="sxs-lookup"><span data-stu-id="9f26b-132">Note the key icon next to `ID`.</span></span> <span data-ttu-id="9f26b-133">Per impostazione predefinita, Entity Framework crea una proprietà denominata `ID` per la chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="9f26b-133">By default, EF creates a property named `ID` for the primary key.</span></span>

* <span data-ttu-id="9f26b-134">Fare clic con il pulsante destro del mouse sulla tabella `Movie` e selezionare**Visualizza dati**:</span><span class="sxs-lookup"><span data-stu-id="9f26b-134">Right click on the `Movie` table and select **View Data**:</span></span>

  <span data-ttu-id="9f26b-135">![Tabella Movie aperta con i dati della tabella](sql/_static/vd22.png)
<!-- Code --------------------------></span><span class="sxs-lookup"><span data-stu-id="9f26b-135">![Movie table open showing table data](sql/_static/vd22.png)
<!-- Code --------------------------></span></span>
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9f26b-136">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9f26b-136">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/rp/sqlite.md)]

<!-- Mac -------------------------->
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="9f26b-137">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="9f26b-137">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/rp/sqlite.md)]

---  
<!-- End of VS tabs -->

## <a name="seed-the-database"></a><span data-ttu-id="9f26b-138">Specificare il valore di inizializzazione del database</span><span class="sxs-lookup"><span data-stu-id="9f26b-138">Seed the database</span></span>

<span data-ttu-id="9f26b-139">Creare una nuova classe denominata `SeedData` nella cartella *Models* usando il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="9f26b-139">Create a new class named `SeedData` in the *Models* folder with the following code:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Models/SeedData.cs?name=snippet_1)]

<span data-ttu-id="9f26b-140">Se sono presenti eventuali film nel database, l'inizializzatore del valore di inizializzazione viene restituito e non vengono aggiunti film.</span><span class="sxs-lookup"><span data-stu-id="9f26b-140">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```
<a name="si"></a>
### <a name="add-the-seed-initializer"></a><span data-ttu-id="9f26b-141">Aggiungere l'inizializzatore del valore di inizializzazione</span><span class="sxs-lookup"><span data-stu-id="9f26b-141">Add the seed initializer</span></span>

<span data-ttu-id="9f26b-142">In *Program.cs* modificare il metodo `Main` per eseguire le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="9f26b-142">In *Program.cs*, modify the `Main` method to do the following:</span></span>

* <span data-ttu-id="9f26b-143">Ottenere un'istanza del contesto di database dal contenitore di inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="9f26b-143">Get a DB context instance from the dependency injection container.</span></span>
* <span data-ttu-id="9f26b-144">Chiamare il metodo di inizializzazione, passandolo al contesto.</span><span class="sxs-lookup"><span data-stu-id="9f26b-144">Call the seed method, passing to it the context.</span></span>
* <span data-ttu-id="9f26b-145">Eliminare il contesto dopo che il metodo di inizializzazione è stato completato.</span><span class="sxs-lookup"><span data-stu-id="9f26b-145">Dispose the context when the seed method completes.</span></span>

<span data-ttu-id="9f26b-146">Il codice seguente illustra il file *Program.cs* aggiornato.</span><span class="sxs-lookup"><span data-stu-id="9f26b-146">The following code shows the updated *Program.cs* file.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Program.cs)]

<span data-ttu-id="9f26b-147">Un'app di produzione non chiamerà `Database.Migrate`.</span><span class="sxs-lookup"><span data-stu-id="9f26b-147">A production app would not call `Database.Migrate`.</span></span> <span data-ttu-id="9f26b-148">Sarà aggiunto al codice precedente per evitare l'eccezione seguente quando `Update-Database` non è stato eseguito:</span><span class="sxs-lookup"><span data-stu-id="9f26b-148">It's added to the preceding code to prevent the following exception when `Update-Database` has not been run:</span></span>

<span data-ttu-id="9f26b-149">SqlException: Impossibile aprire il database "RazorPagesMovieContext-21" richiesto dall'account di accesso.</span><span class="sxs-lookup"><span data-stu-id="9f26b-149">SqlException: Cannot open database "RazorPagesMovieContext-21" requested by the login.</span></span> <span data-ttu-id="9f26b-150">Accesso non riuscito.</span><span class="sxs-lookup"><span data-stu-id="9f26b-150">The login failed.</span></span>
<span data-ttu-id="9f26b-151">Accesso non riuscito per l'utente "nome-utente".</span><span class="sxs-lookup"><span data-stu-id="9f26b-151">Login failed for user 'user name'.</span></span>

### <a name="test-the-app"></a><span data-ttu-id="9f26b-152">Eseguire il test dell'app</span><span class="sxs-lookup"><span data-stu-id="9f26b-152">Test the app</span></span>

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9f26b-153">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9f26b-153">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="9f26b-154">Eliminare tutti i record nel database.</span><span class="sxs-lookup"><span data-stu-id="9f26b-154">Delete all the records in the DB.</span></span> <span data-ttu-id="9f26b-155">È possibile eseguire questa operazione con i collegamenti di eliminazione nel browser o da [SSOX](xref:tutorials/razor-pages/new-field#ssox)</span><span class="sxs-lookup"><span data-stu-id="9f26b-155">You can do this with the delete links in the browser or from [SSOX](xref:tutorials/razor-pages/new-field#ssox)</span></span>
* <span data-ttu-id="9f26b-156">Forzare l'inizializzazione dell'app (chiamare i metodi nella classe `Startup`) in modo che venga eseguito il metodo di inizializzazione.</span><span class="sxs-lookup"><span data-stu-id="9f26b-156">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="9f26b-157">Per forzare l'inizializzazione, IIS Express deve essere arrestato e riavviato.</span><span class="sxs-lookup"><span data-stu-id="9f26b-157">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="9f26b-158">È possibile eseguire questa operazione adottando uno degli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="9f26b-158">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="9f26b-159">Fare clic con il pulsante destro del mouse sull'icona dell'area di notifica di IIS Express e toccare **Esci** o **Arresta sito**:</span><span class="sxs-lookup"><span data-stu-id="9f26b-159">Right click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**:</span></span>

    ![Icona dell'area di notifica di IIS Express](../first-mvc-app/working-with-sql/_static/iisExIcon.png)

    ![Menu di scelta rapida](sql/_static/stopIIS.png)

    * <span data-ttu-id="9f26b-162">Se Visual Studio è in esecuzione in modalità non di debug, premere F5 per attivare la modalità di debug.</span><span class="sxs-lookup"><span data-stu-id="9f26b-162">If you were running VS in non-debug mode, press F5 to run in debug mode.</span></span>
    * <span data-ttu-id="9f26b-163">Se Visual Studio è in esecuzione in modalità di debug, arrestare il debugger e premere F5.</span><span class="sxs-lookup"><span data-stu-id="9f26b-163">If you were running VS in debug mode, stop the debugger and press F5.</span></span>

<!-- Code -------------------------->
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9f26b-164">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9f26b-164">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="9f26b-165">Eliminare tutti i record del database; verrà quindi eseguito il metodo di inizializzazione.</span><span class="sxs-lookup"><span data-stu-id="9f26b-165">Delete all the records in the DB (So the seed method will run).</span></span> <span data-ttu-id="9f26b-166">Arrestare e avviare l'app per inizializzare il database.</span><span class="sxs-lookup"><span data-stu-id="9f26b-166">Stop and start the app to seed the database.</span></span>

<span data-ttu-id="9f26b-167">L'app mostra i dati inizializzati.</span><span class="sxs-lookup"><span data-stu-id="9f26b-167">The app shows the seeded data.</span></span>

<!-- Mac -------------------------->
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="9f26b-168">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="9f26b-168">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="9f26b-169">Eliminare tutti i record del database; verrà quindi eseguito il metodo di inizializzazione.</span><span class="sxs-lookup"><span data-stu-id="9f26b-169">Delete all the records in the DB (So the seed method will run).</span></span> <span data-ttu-id="9f26b-170">Arrestare e avviare l'app per inizializzare il database.</span><span class="sxs-lookup"><span data-stu-id="9f26b-170">Stop and start the app to seed the database.</span></span>

<span data-ttu-id="9f26b-171">L'app mostra i dati inizializzati.</span><span class="sxs-lookup"><span data-stu-id="9f26b-171">The app shows the seeded data.</span></span>

---  
<!-- End of VS tabs -->


   
<span data-ttu-id="9f26b-172">L'app visualizza i dati sottoposti a seed:</span><span class="sxs-lookup"><span data-stu-id="9f26b-172">The app shows the seeded data:</span></span>

![App per i film aperta in Chrome con i dati sui film](sql/_static/m55.png)

<span data-ttu-id="9f26b-174">L'esercitazione successiva consentirà di pulire la presentazione dei dati.</span><span class="sxs-lookup"><span data-stu-id="9f26b-174">The next tutorial will clean up the presentation of the data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="9f26b-175">[Precedente: Pagine Razor create tramite scaffolding](xref:tutorials/razor-pages/page)
> [Successiva: Aggiornamento delle pagine](xref:tutorials/razor-pages/da1)</span><span class="sxs-lookup"><span data-stu-id="9f26b-175">[Previous: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)
[Next: Updating the pages](xref:tutorials/razor-pages/da1)</span></span>
