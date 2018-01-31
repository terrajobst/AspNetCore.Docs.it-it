---
title: Utilizzo di SQL Server Local DB e ASP.NET Core
author: rick-anderson
description: Descrive l'utilizzo di SQL Server Local DB e ASP.NET Core.
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/sql
ms.openlocfilehash: 07f024e2e178828c4488adfd866fc6eec3b251dd
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
# <a name="working-with-sql-server-localdb-and-aspnet-core"></a><span data-ttu-id="c2c03-103">Utilizzo di SQL Server Local DB e ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c2c03-103">Working with SQL Server LocalDB and ASP.NET Core</span></span>

<span data-ttu-id="c2c03-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT) e [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="c2c03-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span> 

<span data-ttu-id="c2c03-105">L'oggetto `MovieContext` gestisce l'attività di connessione al database e di mapping degli oggetti `Movie` ai record di database.</span><span class="sxs-lookup"><span data-stu-id="c2c03-105">The `MovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="c2c03-106">Il contesto del database viene registrato nel contenitore di [inserimento dipendenze](xref:fundamentals/dependency-injection) nel metodo `ConfigureServices` nel file *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="c2c03-106">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=7-8)]

<span data-ttu-id="c2c03-107">Il sistema di [configurazione](xref:fundamentals/configuration/index) di ASP.NET Core legge la `ConnectionString`.</span><span class="sxs-lookup"><span data-stu-id="c2c03-107">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="c2c03-108">Per lo sviluppo locale ottiene la stringa di connessione dal file *appSettings.JSON*:</span><span class="sxs-lookup"><span data-stu-id="c2c03-108">For local development, it gets the connection string from the *appsettings.json* file:</span></span>

[!code-json[Main](razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=2&range=8-10)]

<span data-ttu-id="c2c03-109">Quando si distribuisce l'app in un server di test o di produzione, è possibile usare una variabile di ambiente o un altro approccio per impostare la stringa di connessione su un SQL Server reale.</span><span class="sxs-lookup"><span data-stu-id="c2c03-109">When you deploy the app to a test or production server, you can use an environment variable or another approach to set the connection string to a real SQL Server.</span></span> <span data-ttu-id="c2c03-110">Per altre informazioni, vedere [Configurazione](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="c2c03-110">See [Configuration](xref:fundamentals/configuration/index) for more information.</span></span>

## <a name="sql-server-express-localdb"></a><span data-ttu-id="c2c03-111">LocalDB di SQL Server Express</span><span class="sxs-lookup"><span data-stu-id="c2c03-111">SQL Server Express LocalDB</span></span>

<span data-ttu-id="c2c03-112">Local DB è una versione leggera del motore di database di SQL Server Express progettata appositamente per lo sviluppo di programmi.</span><span class="sxs-lookup"><span data-stu-id="c2c03-112">LocalDB is a lightweight version of the SQL Server Express Database Engine that's targeted for program development.</span></span> <span data-ttu-id="c2c03-113">Local DB viene avviato su richiesta ed eseguito in modalità utente; non richiede quindi una configurazione complessa.</span><span class="sxs-lookup"><span data-stu-id="c2c03-113">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="c2c03-114">Per impostazione predefinita, il database Local DB crea i file "\*.mdf" nella directory *C:/Users/\<user\>*.</span><span class="sxs-lookup"><span data-stu-id="c2c03-114">By default, LocalDB database creates "\*.mdf" files in the *C:/Users/\<user\>* directory.</span></span>

<a name="ssox"></a>
* <span data-ttu-id="c2c03-115">Dal menu **Visualizzazione** aprire **Esplora oggetti di SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="c2c03-115">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![Menu View](sql/_static/ssox.png)

* <span data-ttu-id="c2c03-117">Fare clic con il pulsante destro del mouse sulla tabella `Movie` e selezionare**Progettazione viste**:</span><span class="sxs-lookup"><span data-stu-id="c2c03-117">Right click on the `Movie` table and select **View Designer**:</span></span>

  ![Menu di scelta rapida aperto per la tabella Movie](sql/_static/design.png)

  ![Tabella Movie aperta nella finestra di progettazione](sql/_static/dv.png)

<span data-ttu-id="c2c03-120">Si noti l'icona a forma di chiave accanto a `ID`.</span><span class="sxs-lookup"><span data-stu-id="c2c03-120">Note the key icon next to `ID`.</span></span> <span data-ttu-id="c2c03-121">Per impostazione predefinita, Entity Framework crea una proprietà denominata `ID` per la chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="c2c03-121">By default, EF creates a property named `ID` for the primary key.</span></span>

* <span data-ttu-id="c2c03-122">Fare clic con il pulsante destro del mouse sulla tabella `Movie` e selezionare**Visualizza dati**:</span><span class="sxs-lookup"><span data-stu-id="c2c03-122">Right click on the `Movie` table and select **View Data**:</span></span>

  ![Tabella Movie aperta con i dati della tabella](sql/_static/vd22.png)

## <a name="seed-the-database"></a><span data-ttu-id="c2c03-124">Specificare il valore di inizializzazione del database</span><span class="sxs-lookup"><span data-stu-id="c2c03-124">Seed the database</span></span>

<span data-ttu-id="c2c03-125">Creare una nuova classe denominata `SeedData` nella cartella *Models*.</span><span class="sxs-lookup"><span data-stu-id="c2c03-125">Create a new class named `SeedData` in the *Models* folder.</span></span> <span data-ttu-id="c2c03-126">Sostituire il codice generato con il seguente:</span><span class="sxs-lookup"><span data-stu-id="c2c03-126">Replace the generated code with the following:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/SeedData.cs?name=snippet_1)]

<span data-ttu-id="c2c03-127">Se sono presenti eventuali film nel database, l'inizializzatore del valore di inizializzazione viene restituito e non vengono aggiunti film.</span><span class="sxs-lookup"><span data-stu-id="c2c03-127">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```
<a name="si"></a>
### <a name="add-the-seed-initializer"></a><span data-ttu-id="c2c03-128">Aggiungere l'inizializzatore del valore di inizializzazione</span><span class="sxs-lookup"><span data-stu-id="c2c03-128">Add the seed initializer</span></span>

<span data-ttu-id="c2c03-129">Aggiungere l'inizializzatore del valore di inizializzazione alla fine del metodo `Main` nel file *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="c2c03-129">Add the seed initializer to the end of the `Main` method in the *Program.cs* file:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Program.cs)]

<span data-ttu-id="c2c03-130">Eseguire il test dell'app</span><span class="sxs-lookup"><span data-stu-id="c2c03-130">Test the app</span></span>

* <span data-ttu-id="c2c03-131">Eliminare tutti i record nel database.</span><span class="sxs-lookup"><span data-stu-id="c2c03-131">Delete all the records in the DB.</span></span> <span data-ttu-id="c2c03-132">È possibile eseguire questa operazione con i collegamenti di eliminazione nel browser o da [SSOX](xref:tutorials/razor-pages/new-field#ssox)</span><span class="sxs-lookup"><span data-stu-id="c2c03-132">You can do this with the delete links in the browser or from [SSOX](xref:tutorials/razor-pages/new-field#ssox)</span></span>
* <span data-ttu-id="c2c03-133">Forzare l'inizializzazione dell'app (chiamare i metodi nella classe `Startup`) in modo che venga eseguito il metodo di inizializzazione.</span><span class="sxs-lookup"><span data-stu-id="c2c03-133">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="c2c03-134">Per forzare l'inizializzazione, IIS Express deve essere arrestato e riavviato.</span><span class="sxs-lookup"><span data-stu-id="c2c03-134">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="c2c03-135">È possibile eseguire questa operazione adottando uno degli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="c2c03-135">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="c2c03-136">Fare clic con il pulsante destro del mouse sull'icona dell'area di notifica di IIS Express e toccare **Esci** o **Arresta sito**:</span><span class="sxs-lookup"><span data-stu-id="c2c03-136">Right click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**:</span></span>

    ![Icona dell'area di notifica di IIS Express](../first-mvc-app/working-with-sql/_static/iisExIcon.png)

    ![Menu di scelta rapida](sql/_static/stopIIS.png)

   * <span data-ttu-id="c2c03-139">Se Visual Studio è in esecuzione in modalità non di debug, premere F5 per attivare la modalità di debug.</span><span class="sxs-lookup"><span data-stu-id="c2c03-139">If you were running VS in non-debug mode, press F5 to run in debug mode.</span></span>
   * <span data-ttu-id="c2c03-140">Se Visual Studio è in esecuzione in modalità di debug, arrestare il debugger e premere F5.</span><span class="sxs-lookup"><span data-stu-id="c2c03-140">If you were running VS in debug mode, stop the debugger and press F5.</span></span>
   
<span data-ttu-id="c2c03-141">L'app visualizza i dati sottoposti a seed:</span><span class="sxs-lookup"><span data-stu-id="c2c03-141">The app shows the seeded data:</span></span>

![App per i film aperta in Chrome con i dati sui film](sql/_static/m55.png)

<span data-ttu-id="c2c03-143">L'esercitazione successiva consentirà di pulire la presentazione dei dati.</span><span class="sxs-lookup"><span data-stu-id="c2c03-143">The next tutorial will clean up the presentation of the data.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="c2c03-144">[Articolo precedente: Pagine Razor di scaffolding](xref:tutorials/razor-pages/page)
[Articolo successivo: Aggiornamento delle pagine](xref:tutorials/razor-pages/da1)</span><span class="sxs-lookup"><span data-stu-id="c2c03-144">[Previous: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)
[Next: Updating the pages](xref:tutorials/razor-pages/da1)</span></span>
