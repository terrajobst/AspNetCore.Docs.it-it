---
title: Usare SQL Server Local DB e ASP.NET Core
author: rick-anderson
description: Descrive l'utilizzo di SQL Server Local DB e ASP.NET Core.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/07/2017
uid: tutorials/razor-pages/sql
ms.openlocfilehash: 20e2353eb2e453235c2fb04c696a7e3d27bed5bf
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011274"
---
# <a name="work-with-sql-server-localdb-and-aspnet-core"></a><span data-ttu-id="bec8b-103">Usare SQL Server Local DB e ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bec8b-103">Work with SQL Server LocalDB and ASP.NET Core</span></span>

<span data-ttu-id="bec8b-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT) e [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="bec8b-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span> 

<span data-ttu-id="bec8b-105">L'oggetto `MovieContext` gestisce l'attività di connessione al database e di mapping degli oggetti `Movie` ai record di database.</span><span class="sxs-lookup"><span data-stu-id="bec8b-105">The `MovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="bec8b-106">Il contesto del database viene registrato nel contenitore di [inserimento dipendenze](xref:fundamentals/dependency-injection) nel metodo `ConfigureServices` nel file *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="bec8b-106">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=7-8)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Startup.cs?name=snippet_ConfigureServices&highlight=12-13)]

<span data-ttu-id="bec8b-107">Per altre informazioni sui metodi usati in `ConfigureServices`, vedere:</span><span class="sxs-lookup"><span data-stu-id="bec8b-107">For more information on the methods used in `ConfigureServices`, see:</span></span>

* <span data-ttu-id="bec8b-108">[Supporto per il Regolamento generale sulla protezione dei dati (GDPR) dell'Unione Europea in ASP.NET Core](xref:security/gdpr) per `CookiePolicyOptions`.</span><span class="sxs-lookup"><span data-stu-id="bec8b-108">[EU General Data Protection Regulation (GDPR) support in ASP.NET Core](xref:security/gdpr) for `CookiePolicyOptions`.</span></span>
* [<span data-ttu-id="bec8b-109">SetCompatibilityVersion</span><span class="sxs-lookup"><span data-stu-id="bec8b-109">SetCompatibilityVersion</span></span>](xref:mvc/compatibility-version)

::: moniker-end

<span data-ttu-id="bec8b-110">Il sistema di [configurazione](xref:fundamentals/configuration/index) di ASP.NET Core legge la `ConnectionString`.</span><span class="sxs-lookup"><span data-stu-id="bec8b-110">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="bec8b-111">Per lo sviluppo locale, ottiene la stringa di connessione dal file *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="bec8b-111">For local development, it gets the connection string from the *appsettings.json* file.</span></span> <span data-ttu-id="bec8b-112">Il valore del nome per il database (`Database={Database name}`) sarà diverso per il codice generato.</span><span class="sxs-lookup"><span data-stu-id="bec8b-112">The name value for the database (`Database={Database name}`) will be different for your generated code.</span></span> <span data-ttu-id="bec8b-113">Il valore del nome è arbitrario.</span><span class="sxs-lookup"><span data-stu-id="bec8b-113">The name value is arbitrary.</span></span>

[!code-json[](razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=2&range=8-10)]

<span data-ttu-id="bec8b-114">Quando si distribuisce l'app in un server di test o di produzione, è possibile usare una variabile di ambiente o un altro approccio per impostare la stringa di connessione su un SQL Server reale.</span><span class="sxs-lookup"><span data-stu-id="bec8b-114">When you deploy the app to a test or production server, you can use an environment variable or another approach to set the connection string to a real SQL Server.</span></span> <span data-ttu-id="bec8b-115">Per altre informazioni, vedere [Configurazione](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="bec8b-115">See [Configuration](xref:fundamentals/configuration/index) for more information.</span></span>

## <a name="sql-server-express-localdb"></a><span data-ttu-id="bec8b-116">LocalDB di SQL Server Express</span><span class="sxs-lookup"><span data-stu-id="bec8b-116">SQL Server Express LocalDB</span></span>

<span data-ttu-id="bec8b-117">Local DB è una versione leggera del motore di database di SQL Server Express progettata appositamente per lo sviluppo di programmi.</span><span class="sxs-lookup"><span data-stu-id="bec8b-117">LocalDB is a lightweight version of the SQL Server Express Database Engine that's targeted for program development.</span></span> <span data-ttu-id="bec8b-118">Local DB viene avviato su richiesta ed eseguito in modalità utente; non richiede quindi una configurazione complessa.</span><span class="sxs-lookup"><span data-stu-id="bec8b-118">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="bec8b-119">Per impostazione predefinita, il database Local DB crea i file "\*.mdf" nella directory *C:/Users/\<user\>*.</span><span class="sxs-lookup"><span data-stu-id="bec8b-119">By default, LocalDB database creates "\*.mdf" files in the *C:/Users/\<user\>* directory.</span></span>

<a name="ssox"></a>
* <span data-ttu-id="bec8b-120">Dal menu **Visualizzazione** aprire **Esplora oggetti di SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="bec8b-120">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![Menu View](sql/_static/ssox.png)

* <span data-ttu-id="bec8b-122">Fare clic con il pulsante destro del mouse sulla tabella `Movie` e selezionare**Progettazione viste**:</span><span class="sxs-lookup"><span data-stu-id="bec8b-122">Right click on the `Movie` table and select **View Designer**:</span></span>

  ![Menu di scelta rapida aperto per la tabella Movie](sql/_static/design.png)

  ![Tabella Movie aperta nella finestra di progettazione](sql/_static/dv.png)

<span data-ttu-id="bec8b-125">Si noti l'icona a forma di chiave accanto a `ID`.</span><span class="sxs-lookup"><span data-stu-id="bec8b-125">Note the key icon next to `ID`.</span></span> <span data-ttu-id="bec8b-126">Per impostazione predefinita, Entity Framework crea una proprietà denominata `ID` per la chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="bec8b-126">By default, EF creates a property named `ID` for the primary key.</span></span>

* <span data-ttu-id="bec8b-127">Fare clic con il pulsante destro del mouse sulla tabella `Movie` e selezionare**Visualizza dati**:</span><span class="sxs-lookup"><span data-stu-id="bec8b-127">Right click on the `Movie` table and select **View Data**:</span></span>

  ![Tabella Movie aperta con i dati della tabella](sql/_static/vd22.png)

## <a name="seed-the-database"></a><span data-ttu-id="bec8b-129">Specificare il valore di inizializzazione del database</span><span class="sxs-lookup"><span data-stu-id="bec8b-129">Seed the database</span></span>

<span data-ttu-id="bec8b-130">Creare una nuova classe denominata `SeedData` nella cartella *Models*.</span><span class="sxs-lookup"><span data-stu-id="bec8b-130">Create a new class named `SeedData` in the *Models* folder.</span></span> <span data-ttu-id="bec8b-131">Sostituire il codice generato con il seguente:</span><span class="sxs-lookup"><span data-stu-id="bec8b-131">Replace the generated code with the following:</span></span>

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/SeedData.cs?name=snippet_1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Models/SeedData.cs?name=snippet_1)]

::: moniker-end

<span data-ttu-id="bec8b-132">Se sono presenti eventuali film nel database, l'inizializzatore del valore di inizializzazione viene restituito e non vengono aggiunti film.</span><span class="sxs-lookup"><span data-stu-id="bec8b-132">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```
<a name="si"></a>
### <a name="add-the-seed-initializer"></a><span data-ttu-id="bec8b-133">Aggiungere l'inizializzatore del valore di inizializzazione</span><span class="sxs-lookup"><span data-stu-id="bec8b-133">Add the seed initializer</span></span>

<span data-ttu-id="bec8b-134">In *Program.cs* modificare il metodo `Main` per eseguire le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="bec8b-134">In *Program.cs*, modify the `Main` method to do the following:</span></span>

* <span data-ttu-id="bec8b-135">Ottenere un'istanza del contesto di database dal contenitore di inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="bec8b-135">Get a DB context instance from the dependency injection container.</span></span>
* <span data-ttu-id="bec8b-136">Chiamare il metodo di inizializzazione, passandolo al contesto.</span><span class="sxs-lookup"><span data-stu-id="bec8b-136">Call the seed method, passing to it the context.</span></span>
* <span data-ttu-id="bec8b-137">Eliminare il contesto dopo che il metodo di inizializzazione è stato completato.</span><span class="sxs-lookup"><span data-stu-id="bec8b-137">Dispose the context when the seed method completes.</span></span>

<span data-ttu-id="bec8b-138">Il codice seguente illustra il file *Program.cs* aggiornato.</span><span class="sxs-lookup"><span data-stu-id="bec8b-138">The following code shows the updated *Program.cs* file.</span></span>

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Program.cs)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Program.cs)]

::: moniker-end

<span data-ttu-id="bec8b-139">Un'app di produzione non chiamerà `Database.Migrate`.</span><span class="sxs-lookup"><span data-stu-id="bec8b-139">A production app would not call `Database.Migrate`.</span></span> <span data-ttu-id="bec8b-140">Sarà aggiunto al codice precedente per evitare l'eccezione seguente quando `Update-Database` non è stato eseguito:</span><span class="sxs-lookup"><span data-stu-id="bec8b-140">It's added to the preceding code to prevent the following exception when `Update-Database` has not been run:</span></span>

<span data-ttu-id="bec8b-141">SqlException: Impossibile aprire il database "RazorPagesMovieContext-21" richiesto dall'account di accesso.</span><span class="sxs-lookup"><span data-stu-id="bec8b-141">SqlException: Cannot open database "RazorPagesMovieContext-21" requested by the login.</span></span> <span data-ttu-id="bec8b-142">Accesso non riuscito.</span><span class="sxs-lookup"><span data-stu-id="bec8b-142">The login failed.</span></span>
<span data-ttu-id="bec8b-143">Accesso non riuscito per l'utente "nome-utente".</span><span class="sxs-lookup"><span data-stu-id="bec8b-143">Login failed for user 'user name'.</span></span>

### <a name="test-the-app"></a><span data-ttu-id="bec8b-144">Eseguire il test dell'app</span><span class="sxs-lookup"><span data-stu-id="bec8b-144">Test the app</span></span>

* <span data-ttu-id="bec8b-145">Eliminare tutti i record nel database.</span><span class="sxs-lookup"><span data-stu-id="bec8b-145">Delete all the records in the DB.</span></span> <span data-ttu-id="bec8b-146">È possibile eseguire questa operazione con i collegamenti di eliminazione nel browser o da [SSOX](xref:tutorials/razor-pages/new-field#ssox)</span><span class="sxs-lookup"><span data-stu-id="bec8b-146">You can do this with the delete links in the browser or from [SSOX](xref:tutorials/razor-pages/new-field#ssox)</span></span>
* <span data-ttu-id="bec8b-147">Forzare l'inizializzazione dell'app (chiamare i metodi nella classe `Startup`) in modo che venga eseguito il metodo di inizializzazione.</span><span class="sxs-lookup"><span data-stu-id="bec8b-147">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="bec8b-148">Per forzare l'inizializzazione, IIS Express deve essere arrestato e riavviato.</span><span class="sxs-lookup"><span data-stu-id="bec8b-148">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="bec8b-149">È possibile eseguire questa operazione adottando uno degli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="bec8b-149">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="bec8b-150">Fare clic con il pulsante destro del mouse sull'icona dell'area di notifica di IIS Express e toccare **Esci** o **Arresta sito**:</span><span class="sxs-lookup"><span data-stu-id="bec8b-150">Right click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**:</span></span>

    ![Icona dell'area di notifica di IIS Express](../first-mvc-app/working-with-sql/_static/iisExIcon.png)

    ![Menu di scelta rapida](sql/_static/stopIIS.png)

    * <span data-ttu-id="bec8b-153">Se Visual Studio è in esecuzione in modalità non di debug, premere F5 per attivare la modalità di debug.</span><span class="sxs-lookup"><span data-stu-id="bec8b-153">If you were running VS in non-debug mode, press F5 to run in debug mode.</span></span>
    * <span data-ttu-id="bec8b-154">Se Visual Studio è in esecuzione in modalità di debug, arrestare il debugger e premere F5.</span><span class="sxs-lookup"><span data-stu-id="bec8b-154">If you were running VS in debug mode, stop the debugger and press F5.</span></span>
   
<span data-ttu-id="bec8b-155">L'app visualizza i dati sottoposti a seed:</span><span class="sxs-lookup"><span data-stu-id="bec8b-155">The app shows the seeded data:</span></span>

![App per i film aperta in Chrome con i dati sui film](sql/_static/m55.png)

<span data-ttu-id="bec8b-157">L'esercitazione successiva consentirà di pulire la presentazione dei dati.</span><span class="sxs-lookup"><span data-stu-id="bec8b-157">The next tutorial will clean up the presentation of the data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="bec8b-158">[Articolo precedente: Pagine Razor di scaffolding](xref:tutorials/razor-pages/page)
> [Articolo successivo: Aggiornamento delle pagine](xref:tutorials/razor-pages/da1)</span><span class="sxs-lookup"><span data-stu-id="bec8b-158">[Previous: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)
[Next: Updating the pages](xref:tutorials/razor-pages/da1)</span></span>
