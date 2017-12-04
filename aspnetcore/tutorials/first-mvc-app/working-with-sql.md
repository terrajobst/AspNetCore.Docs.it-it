---
title: Utilizzo di SQL Server Local DB
author: rick-anderson
description: Utilizzo di SQL Server Local DB con un'app MVC semplice
keywords: ASP.NET Core, SQL Server Local DB, SQL Server, Local DB
ms.author: riande
manager: wpickett
ms.date: 03/07/2017
ms.topic: get-started-article
ms.assetid: ff8fd9b8-7c98-424d-8641-7524e23bf541
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/working-with-sql
ms.openlocfilehash: 67894d05bfd44b0406d10cbbe30ddfdaf0d66636
ms.sourcegitcommit: 8f42ab93402c1b8044815e1e48d0bb84c81f8b59
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/29/2017
---
# <a name="working-with-sql-server-localdb"></a><span data-ttu-id="e3c65-104">Utilizzo di SQL Server Local DB</span><span class="sxs-lookup"><span data-stu-id="e3c65-104">Working with SQL Server LocalDB</span></span>

<span data-ttu-id="e3c65-105">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="e3c65-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="e3c65-106">L'oggetto `MvcMovieContext` gestisce l'attività di connessione al database e di mapping degli oggetti `Movie` ai record di database.</span><span class="sxs-lookup"><span data-stu-id="e3c65-106">The `MvcMovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="e3c65-107">Il contesto del database viene registrato nel contenitore di [inserimento dipendenze](xref:fundamentals/dependency-injection) nel metodo `ConfigureServices` nel file *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="e3c65-107">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]

<span data-ttu-id="e3c65-108">Il sistema di [configurazione](xref:fundamentals/configuration/index) di ASP.NET Core legge la `ConnectionString`.</span><span class="sxs-lookup"><span data-stu-id="e3c65-108">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="e3c65-109">Per lo sviluppo locale ottiene la stringa di connessione dal file *appSettings.JSON*:</span><span class="sxs-lookup"><span data-stu-id="e3c65-109">For local development, it gets the connection string from the *appsettings.json* file:</span></span>

[!code-json[Main](start-mvc/sample/MvcMovie/appsettings.json?highlight=2&range=8-10)]

<span data-ttu-id="e3c65-110">Quando si distribuisce l'app in un server di test o di produzione, è possibile usare una variabile di ambiente o un altro approccio per impostare la stringa di connessione su un SQL Server reale.</span><span class="sxs-lookup"><span data-stu-id="e3c65-110">When you deploy the app to a test or production server, you can use an environment variable or another approach to set the connection string to a real SQL Server.</span></span> <span data-ttu-id="e3c65-111">Per altre informazioni, vedere [Configurazione](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="e3c65-111">See [Configuration](xref:fundamentals/configuration/index) for more information.</span></span>

## <a name="sql-server-express-localdb"></a><span data-ttu-id="e3c65-112">LocalDB di SQL Server Express</span><span class="sxs-lookup"><span data-stu-id="e3c65-112">SQL Server Express LocalDB</span></span>

<span data-ttu-id="e3c65-113">Local DB è una versione leggera del motore di database di SQL Server Express appositamente pensato per lo sviluppo di programmi.</span><span class="sxs-lookup"><span data-stu-id="e3c65-113">LocalDB is a lightweight version of the SQL Server Express Database Engine that is targeted for program development.</span></span> <span data-ttu-id="e3c65-114">Local DB viene avviato su richiesta ed eseguito in modalità utente; non richiede quindi una configurazione complessa.</span><span class="sxs-lookup"><span data-stu-id="e3c65-114">LocalDB starts on demand and runs in user mode, so there is no complex configuration.</span></span> <span data-ttu-id="e3c65-115">Per impostazione predefinita, il database Local DB crea i file "\*.mdf" nella directory *C:/Users/\<user\>*.</span><span class="sxs-lookup"><span data-stu-id="e3c65-115">By default, LocalDB database creates "\*.mdf" files in the *C:/Users/\<user\>* directory.</span></span>

* <span data-ttu-id="e3c65-116">Dal menu **Visualizzazione** aprire **Esplora oggetti di SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="e3c65-116">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![Menu View](working-with-sql/_static/ssox.png)

* <span data-ttu-id="e3c65-118">Fare clic con il pulsante destro del mouse sulla tabella `Movie`**> Visualizza finestra di progettazione**</span><span class="sxs-lookup"><span data-stu-id="e3c65-118">Right click on the `Movie` table **> View Designer**</span></span>

  ![Menu di scelta rapida aperto per la tabella Movie](working-with-sql/_static/design.png)

  ![Tabella Movie aperta nella finestra di progettazione](working-with-sql/_static/dv.png)

<span data-ttu-id="e3c65-121">Si noti l'icona a forma di chiave accanto a `ID`.</span><span class="sxs-lookup"><span data-stu-id="e3c65-121">Note the key icon next to `ID`.</span></span> <span data-ttu-id="e3c65-122">Per impostazione predefinita, Entity Framework userà una proprietà denominata `ID` come chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="e3c65-122">By default, EF will make a property named `ID` the primary key.</span></span>

* <span data-ttu-id="e3c65-123">Fare clic con il pulsante destro del mouse sulla tabella `Movie`**> Visualizza dati**</span><span class="sxs-lookup"><span data-stu-id="e3c65-123">Right click on the `Movie` table **> View Data**</span></span>

  ![Menu di scelta rapida aperto per la tabella Movie](working-with-sql/_static/ssox2.png)

  ![Tabella Movie aperta con i dati della tabella](working-with-sql/_static/vd22.png)

## <a name="seed-the-database"></a><span data-ttu-id="e3c65-126">Specificare il valore di inizializzazione del database</span><span class="sxs-lookup"><span data-stu-id="e3c65-126">Seed the database</span></span>

<span data-ttu-id="e3c65-127">Creare una nuova classe denominata `SeedData` nella cartella *Models*.</span><span class="sxs-lookup"><span data-stu-id="e3c65-127">Create a new class named `SeedData` in the *Models* folder.</span></span> <span data-ttu-id="e3c65-128">Sostituire il codice generato con il seguente:</span><span class="sxs-lookup"><span data-stu-id="e3c65-128">Replace the generated code with the following:</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Models/SeedData.cs?name=snippet_1)]

<span data-ttu-id="e3c65-129">Se sono presenti eventuali film nel database, l'inizializzatore del valore di inizializzazione viene restituito e non vengono aggiunti film.</span><span class="sxs-lookup"><span data-stu-id="e3c65-129">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>
### <a name="add-the-seed-initializer"></a><span data-ttu-id="e3c65-130">Aggiungere l'inizializzatore del valore di inizializzazione</span><span class="sxs-lookup"><span data-stu-id="e3c65-130">Add the seed initializer</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e3c65-131">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e3c65-131">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="e3c65-132">Aggiungere l'inizializzatore del valore di inizializzazione al metodo `Main` nel file *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="e3c65-132">Add the seed initializer to the `Main` method in the *Program.cs* file:</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Program.cs?highlight=6,14-32)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e3c65-133">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e3c65-133">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="e3c65-134">Aggiungere l'inizializzatore del valore di inizializzazione alla fine del metodo `Configure` nel file *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="e3c65-134">Add the seed initializer to the end of the `Configure` method in the *Startup.cs* file.</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Startup.cs?highlight=9&name=snippet_seed)]

---

<span data-ttu-id="e3c65-135">Eseguire il test dell'app</span><span class="sxs-lookup"><span data-stu-id="e3c65-135">Test the app</span></span>

* <span data-ttu-id="e3c65-136">Eliminare tutti i record nel database.</span><span class="sxs-lookup"><span data-stu-id="e3c65-136">Delete all the records in the DB.</span></span> <span data-ttu-id="e3c65-137">È possibile eseguire questa operazione con i collegamenti di eliminazione nel browser o da SSOX.</span><span class="sxs-lookup"><span data-stu-id="e3c65-137">You can do this with the delete links in the browser or from SSOX.</span></span>
* <span data-ttu-id="e3c65-138">Forzare l'inizializzazione dell'app (chiamare i metodi nella classe `Startup`) in modo che venga eseguito il metodo di inizializzazione.</span><span class="sxs-lookup"><span data-stu-id="e3c65-138">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="e3c65-139">Per forzare l'inizializzazione, IIS Express deve essere arrestato e riavviato.</span><span class="sxs-lookup"><span data-stu-id="e3c65-139">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="e3c65-140">È possibile eseguire questa operazione adottando uno degli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="e3c65-140">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="e3c65-141">Fare clic con il pulsante destro del mouse sull'icona dell'area di notifica di IIS Express e toccare **Esci** o **Arresta sito**</span><span class="sxs-lookup"><span data-stu-id="e3c65-141">Right click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**</span></span>

    ![Icona dell'area di notifica di IIS Express](working-with-sql/_static/iisExIcon.png)

    ![Menu di scelta rapida](working-with-sql/_static/stopIIS.png)

   * <span data-ttu-id="e3c65-144">Se Visual Studio è in esecuzione in modalità non di debug, premere F5 per attivare la modalità di debug</span><span class="sxs-lookup"><span data-stu-id="e3c65-144">If you were running VS in non-debug mode, press F5 to run in debug mode</span></span>
   * <span data-ttu-id="e3c65-145">Se Visual Studio è in esecuzione in modalità di debug, arrestare il debugger e premere F5</span><span class="sxs-lookup"><span data-stu-id="e3c65-145">If you were running VS in debug mode, stop the debugger and press F5</span></span>
   
<span data-ttu-id="e3c65-146">L'app mostra i dati inizializzati.</span><span class="sxs-lookup"><span data-stu-id="e3c65-146">The app shows the seeded data.</span></span>

![App per i film MVC aperta in Microsoft Edge con i dati sui film](working-with-sql/_static/m55.png)

>[!div class="step-by-step"]
<span data-ttu-id="e3c65-148">[Precedente](adding-model.md)
[Successivo](controller-methods-views.md)</span><span class="sxs-lookup"><span data-stu-id="e3c65-148">[Previous](adding-model.md)
[Next](controller-methods-views.md)</span></span>  
