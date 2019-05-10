---
title: Usare SQL in un'app ASP.NET Core MVC
author: rick-anderson
description: Informazioni sull'uso di SQL Server LocalDB o SQLite in un'app ASP.NET Core MVC.
ms.author: riande
ms.date: 03/07/2017
uid: tutorials/first-mvc-app/working-with-sql
ms.openlocfilehash: 983742276f3519b540cd62e4ada6eb5189650aa8
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/27/2019
ms.locfileid: "64884116"
---
# <a name="work-with-sql-in-aspnet-core"></a><span data-ttu-id="b77d4-103">Usare SQL in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b77d4-103">Work with SQL in ASP.NET Core</span></span>

<span data-ttu-id="b77d4-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="b77d4-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="b77d4-105">L'oggetto `MvcMovieContext` gestisce l'attività di connessione al database e di mapping degli oggetti `Movie` ai record di database.</span><span class="sxs-lookup"><span data-stu-id="b77d4-105">The `MvcMovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="b77d4-106">Il contesto del database viene registrato nel contenitore di [inserimento dipendenze](xref:fundamentals/dependency-injection) nel metodo `ConfigureServices` nel file *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="b77d4-106">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b77d4-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b77d4-107">Visual Studio</span></span>](#tab/visual-studio)

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=13-99)]

<span data-ttu-id="b77d4-108">Il sistema di [configurazione](xref:fundamentals/configuration/index) di ASP.NET Core legge la `ConnectionString`.</span><span class="sxs-lookup"><span data-stu-id="b77d4-108">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="b77d4-109">Per lo sviluppo locale ottiene la stringa di connessione dal file *appSettings.JSON*:</span><span class="sxs-lookup"><span data-stu-id="b77d4-109">For local development, it gets the connection string from the *appsettings.json* file:</span></span>

[!code-json[](start-mvc/sample/MvcMovie/appsettings.json?highlight=2&range=8-10)]

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="b77d4-110">Visual Studio Code/Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="b77d4-110">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_UseSqlite&highlight=11-12)]

<span data-ttu-id="b77d4-111">Il sistema di [configurazione](xref:fundamentals/configuration/index) di ASP.NET Core legge la `ConnectionString`.</span><span class="sxs-lookup"><span data-stu-id="b77d4-111">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="b77d4-112">Per lo sviluppo locale ottiene la stringa di connessione dal file *appSettings.JSON*:</span><span class="sxs-lookup"><span data-stu-id="b77d4-112">For local development, it gets the connection string from the *appsettings.json* file:</span></span>

[!code-json[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/appsettingsSQLite.json?highlight=2&range=8-10)]

---

<span data-ttu-id="b77d4-113">Quando si distribuisce l'app in un server di test o di produzione, è possibile usare una variabile di ambiente o un altro approccio per impostare la stringa di connessione su un SQL Server reale.</span><span class="sxs-lookup"><span data-stu-id="b77d4-113">When you deploy the app to a test or production server, you can use an environment variable or another approach to set the connection string to a real SQL Server.</span></span> <span data-ttu-id="b77d4-114">Per altre informazioni, vedere [Configurazione](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="b77d4-114">See [Configuration](xref:fundamentals/configuration/index) for more information.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b77d4-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b77d4-115">Visual Studio</span></span>](#tab/visual-studio)

## <a name="sql-server-express-localdb"></a><span data-ttu-id="b77d4-116">LocalDB di SQL Server Express</span><span class="sxs-lookup"><span data-stu-id="b77d4-116">SQL Server Express LocalDB</span></span>

<span data-ttu-id="b77d4-117">Local DB è una versione leggera del motore di database di SQL Server Express progettata appositamente per lo sviluppo di programmi.</span><span class="sxs-lookup"><span data-stu-id="b77d4-117">LocalDB is a lightweight version of the SQL Server Express Database Engine that's targeted for program development.</span></span> <span data-ttu-id="b77d4-118">Local DB viene avviato su richiesta ed eseguito in modalità utente; non richiede quindi una configurazione complessa.</span><span class="sxs-lookup"><span data-stu-id="b77d4-118">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="b77d4-119">Per impostazione predefinita, il database Local DB crea i file con estensione *mdf* nella directory *C:/Utenti/{utente}*.</span><span class="sxs-lookup"><span data-stu-id="b77d4-119">By default, LocalDB database creates *.mdf* files in the *C:/Users/{user}* directory.</span></span>

* <span data-ttu-id="b77d4-120">Dal menu **Visualizzazione** aprire **Esplora oggetti di SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="b77d4-120">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![Menu View](working-with-sql/_static/ssox.png)

* <span data-ttu-id="b77d4-122">Fare clic con il pulsante destro del mouse sulla tabella `Movie`**> Visualizza finestra di progettazione**</span><span class="sxs-lookup"><span data-stu-id="b77d4-122">Right click on the `Movie` table **> View Designer**</span></span>

  ![Menu di scelta rapida aperto per la tabella Movie](working-with-sql/_static/design.png)

  ![Tabella Movie aperta nella finestra di progettazione](working-with-sql/_static/dv.png)

<span data-ttu-id="b77d4-125">Si noti l'icona a forma di chiave accanto a `ID`.</span><span class="sxs-lookup"><span data-stu-id="b77d4-125">Note the key icon next to `ID`.</span></span> <span data-ttu-id="b77d4-126">Per impostazione predefinita, Entity Framework userà una proprietà denominata `ID` come chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="b77d4-126">By default, EF will make a property named `ID` the primary key.</span></span>

* <span data-ttu-id="b77d4-127">Fare clic con il pulsante destro del mouse sulla tabella `Movie`**> Visualizza dati**</span><span class="sxs-lookup"><span data-stu-id="b77d4-127">Right click on the `Movie` table **> View Data**</span></span>

  ![Menu di scelta rapida aperto per la tabella Movie](working-with-sql/_static/ssox2.png)

  ![Tabella Movie aperta con i dati della tabella](working-with-sql/_static/vd22.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="b77d4-130">Visual Studio Code/Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="b77d4-130">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!INCLUDE[](~/includes/rp/sqlite.md)]
[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

---
<!-- End of VS tabs -->

## <a name="seed-the-database"></a><span data-ttu-id="b77d4-131">Specificare il valore di inizializzazione del database</span><span class="sxs-lookup"><span data-stu-id="b77d4-131">Seed the database</span></span>

<span data-ttu-id="b77d4-132">Creare una nuova classe denominata `SeedData` nella cartella *Models*.</span><span class="sxs-lookup"><span data-stu-id="b77d4-132">Create a new class named `SeedData` in the *Models* folder.</span></span> <span data-ttu-id="b77d4-133">Sostituire il codice generato con il seguente:</span><span class="sxs-lookup"><span data-stu-id="b77d4-133">Replace the generated code with the following:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Models/SeedData.cs?name=snippet_1)]

<span data-ttu-id="b77d4-134">Se sono presenti eventuali film nel database, l'inizializzatore del valore di inizializzazione viene restituito e non vengono aggiunti film.</span><span class="sxs-lookup"><span data-stu-id="b77d4-134">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>

### <a name="add-the-seed-initializer"></a><span data-ttu-id="b77d4-135">Aggiungere l'inizializzatore del valore di inizializzazione</span><span class="sxs-lookup"><span data-stu-id="b77d4-135">Add the seed initializer</span></span>

<span data-ttu-id="b77d4-136">Sostituire il contenuto di *Program.cs* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="b77d4-136">Replace the contents of *Program.cs* with the following code:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Program.cs)]

<span data-ttu-id="b77d4-137">Eseguire il test dell'app</span><span class="sxs-lookup"><span data-stu-id="b77d4-137">Test the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b77d4-138">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b77d4-138">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="b77d4-139">Eliminare tutti i record nel database.</span><span class="sxs-lookup"><span data-stu-id="b77d4-139">Delete all the records in the DB.</span></span> <span data-ttu-id="b77d4-140">È possibile eseguire questa operazione con i collegamenti di eliminazione nel browser o da SSOX.</span><span class="sxs-lookup"><span data-stu-id="b77d4-140">You can do this with the delete links in the browser or from SSOX.</span></span>
* <span data-ttu-id="b77d4-141">Forzare l'inizializzazione dell'app (chiamare i metodi nella classe `Startup`) in modo che venga eseguito il metodo di inizializzazione.</span><span class="sxs-lookup"><span data-stu-id="b77d4-141">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="b77d4-142">Per forzare l'inizializzazione, IIS Express deve essere arrestato e riavviato.</span><span class="sxs-lookup"><span data-stu-id="b77d4-142">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="b77d4-143">È possibile eseguire questa operazione adottando uno degli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="b77d4-143">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="b77d4-144">Fare clic con il pulsante destro del mouse sull'icona dell'area di notifica di IIS Express e toccare **Esci** o **Arresta sito**</span><span class="sxs-lookup"><span data-stu-id="b77d4-144">Right click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**</span></span>

    ![Icona dell'area di notifica di IIS Express](working-with-sql/_static/iisExIcon.png)

    ![Menu di scelta rapida](working-with-sql/_static/stopIIS.png)

    * <span data-ttu-id="b77d4-147">Se Visual Studio è in esecuzione in modalità non di debug, premere F5 per attivare la modalità di debug</span><span class="sxs-lookup"><span data-stu-id="b77d4-147">If you were running VS in non-debug mode, press F5 to run in debug mode</span></span>
    * <span data-ttu-id="b77d4-148">Se Visual Studio è in esecuzione in modalità di debug, arrestare il debugger e premere F5</span><span class="sxs-lookup"><span data-stu-id="b77d4-148">If you were running VS in debug mode, stop the debugger and press F5</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="b77d4-149">Visual Studio Code/Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="b77d4-149">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="b77d4-150">Eliminare tutti i record del database; verrà quindi eseguito il metodo di inizializzazione.</span><span class="sxs-lookup"><span data-stu-id="b77d4-150">Delete all the records in the DB (So the seed method will run).</span></span> <span data-ttu-id="b77d4-151">Arrestare e avviare l'app per inizializzare il database.</span><span class="sxs-lookup"><span data-stu-id="b77d4-151">Stop and start the app to seed the database.</span></span>

---

<span data-ttu-id="b77d4-152">L'app mostra i dati inizializzati.</span><span class="sxs-lookup"><span data-stu-id="b77d4-152">The app shows the seeded data.</span></span>

![App per i film MVC aperta in Microsoft Edge con i dati sui film](working-with-sql/_static/m55.png)

> [!div class="step-by-step"]
> <span data-ttu-id="b77d4-154">[Precedente](adding-model.md)
> [Successivo](controller-methods-views.md)</span><span class="sxs-lookup"><span data-stu-id="b77d4-154">[Previous](adding-model.md)
[Next](controller-methods-views.md)</span></span>
