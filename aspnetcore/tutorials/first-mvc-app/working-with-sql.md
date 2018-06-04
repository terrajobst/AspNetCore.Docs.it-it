---
title: Utilizzare SQL Server Local DB in ASP.NET Core
author: rick-anderson
description: Informazioni sull'uso di SQL Server Local DB in una semplice app ASP.NET Core MVC.
manager: wpickett
ms.author: riande
ms.date: 03/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app/working-with-sql
ms.openlocfilehash: b00d709f3009f63431becf24797269ad5988a20b
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/04/2018
ms.locfileid: "34688374"
---
# <a name="work-with-sql-server-localdb-in-aspnet-core"></a><span data-ttu-id="cf979-103">Utilizzare SQL Server Local DB in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="cf979-103">Work with SQL Server LocalDB in ASP.NET Core</span></span>

<span data-ttu-id="cf979-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="cf979-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="cf979-105">L'oggetto `MvcMovieContext` gestisce l'attività di connessione al database e di mapping degli oggetti `Movie` ai record di database.</span><span class="sxs-lookup"><span data-stu-id="cf979-105">The `MvcMovieContext` object handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="cf979-106">Il contesto del database viene registrato nel contenitore di [inserimento dipendenze](xref:fundamentals/dependency-injection) nel metodo `ConfigureServices` nel file *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="cf979-106">The database context is registered with the [Dependency Injection](xref:fundamentals/dependency-injection) container in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="cf979-107">[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Startup.cs?name=ConfigureServices&highlight=13-99)]</span><span class="sxs-lookup"><span data-stu-id="cf979-107">[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Startup.cs?name=ConfigureServices&highlight=13-99)]</span></span>
::: moniker-end
::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="cf979-108">[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]</span><span class="sxs-lookup"><span data-stu-id="cf979-108">[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]</span></span>
::: moniker-end

<span data-ttu-id="cf979-109">Il sistema di [configurazione](xref:fundamentals/configuration/index) di ASP.NET Core legge la `ConnectionString`.</span><span class="sxs-lookup"><span data-stu-id="cf979-109">The ASP.NET Core [Configuration](xref:fundamentals/configuration/index) system reads the `ConnectionString`.</span></span> <span data-ttu-id="cf979-110">Per lo sviluppo locale ottiene la stringa di connessione dal file *appSettings.JSON*:</span><span class="sxs-lookup"><span data-stu-id="cf979-110">For local development, it gets the connection string from the *appsettings.json* file:</span></span>

[!code-json[](start-mvc/sample/MvcMovie/appsettings.json?highlight=2&range=8-10)]

<span data-ttu-id="cf979-111">Quando si distribuisce l'app in un server di test o di produzione, è possibile usare una variabile di ambiente o un altro approccio per impostare la stringa di connessione su un SQL Server reale.</span><span class="sxs-lookup"><span data-stu-id="cf979-111">When you deploy the app to a test or production server, you can use an environment variable or another approach to set the connection string to a real SQL Server.</span></span> <span data-ttu-id="cf979-112">Per altre informazioni, vedere [Configurazione](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="cf979-112">See [Configuration](xref:fundamentals/configuration/index) for more information.</span></span>

## <a name="sql-server-express-localdb"></a><span data-ttu-id="cf979-113">LocalDB di SQL Server Express</span><span class="sxs-lookup"><span data-stu-id="cf979-113">SQL Server Express LocalDB</span></span>

<span data-ttu-id="cf979-114">Local DB è una versione leggera del motore di database di SQL Server Express progettata appositamente per lo sviluppo di programmi.</span><span class="sxs-lookup"><span data-stu-id="cf979-114">LocalDB is a lightweight version of the SQL Server Express Database Engine that's targeted for program development.</span></span> <span data-ttu-id="cf979-115">Local DB viene avviato su richiesta ed eseguito in modalità utente; non richiede quindi una configurazione complessa.</span><span class="sxs-lookup"><span data-stu-id="cf979-115">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="cf979-116">Per impostazione predefinita, il database Local DB crea i file "\*.mdf" nella directory *C:/Users/\<user\>*.</span><span class="sxs-lookup"><span data-stu-id="cf979-116">By default, LocalDB database creates "\*.mdf" files in the *C:/Users/\<user\>* directory.</span></span>

* <span data-ttu-id="cf979-117">Dal menu **Visualizzazione** aprire **Esplora oggetti di SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="cf979-117">From the **View** menu, open **SQL Server Object Explorer** (SSOX).</span></span>

  ![Menu View](working-with-sql/_static/ssox.png)

* <span data-ttu-id="cf979-119">Fare clic con il pulsante destro del mouse sulla tabella `Movie`**> Visualizza finestra di progettazione**</span><span class="sxs-lookup"><span data-stu-id="cf979-119">Right click on the `Movie` table **> View Designer**</span></span>

  ![Menu di scelta rapida aperto per la tabella Movie](working-with-sql/_static/design.png)

  ![Tabella Movie aperta nella finestra di progettazione](working-with-sql/_static/dv.png)

<span data-ttu-id="cf979-122">Si noti l'icona a forma di chiave accanto a `ID`.</span><span class="sxs-lookup"><span data-stu-id="cf979-122">Note the key icon next to `ID`.</span></span> <span data-ttu-id="cf979-123">Per impostazione predefinita, Entity Framework userà una proprietà denominata `ID` come chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="cf979-123">By default, EF will make a property named `ID` the primary key.</span></span>

* <span data-ttu-id="cf979-124">Fare clic con il pulsante destro del mouse sulla tabella `Movie`**> Visualizza dati**</span><span class="sxs-lookup"><span data-stu-id="cf979-124">Right click on the `Movie` table **> View Data**</span></span>

  ![Menu di scelta rapida aperto per la tabella Movie](working-with-sql/_static/ssox2.png)

  ![Tabella Movie aperta con i dati della tabella](working-with-sql/_static/vd22.png)

## <a name="seed-the-database"></a><span data-ttu-id="cf979-127">Specificare il valore di inizializzazione del database</span><span class="sxs-lookup"><span data-stu-id="cf979-127">Seed the database</span></span>

<span data-ttu-id="cf979-128">Creare una nuova classe denominata `SeedData` nella cartella *Models*.</span><span class="sxs-lookup"><span data-stu-id="cf979-128">Create a new class named `SeedData` in the *Models* folder.</span></span> <span data-ttu-id="cf979-129">Sostituire il codice generato con il seguente:</span><span class="sxs-lookup"><span data-stu-id="cf979-129">Replace the generated code with the following:</span></span>

[!code-csharp[](start-mvc/sample/MvcMovie/Models/SeedData.cs?name=snippet_1)]

<span data-ttu-id="cf979-130">Se sono presenti eventuali film nel database, l'inizializzatore del valore di inizializzazione viene restituito e non vengono aggiunti film.</span><span class="sxs-lookup"><span data-stu-id="cf979-130">If there are any movies in the DB, the seed initializer returns and no movies are added.</span></span>

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

<a name="si"></a>
### <a name="add-the-seed-initializer"></a><span data-ttu-id="cf979-131">Aggiungere l'inizializzatore del valore di inizializzazione</span><span class="sxs-lookup"><span data-stu-id="cf979-131">Add the seed initializer</span></span>

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="cf979-132">[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Program.cs)]</span><span class="sxs-lookup"><span data-stu-id="cf979-132">[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie21/Program.cs)]</span></span>
::: moniker-end
::: moniker range="<= aspnetcore-2.0"

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="cf979-133">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="cf979-133">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="cf979-134">Aggiungere l'inizializzatore del valore di inizializzazione al metodo `Main` nel file *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="cf979-134">Add the seed initializer to the `Main` method in the *Program.cs* file:</span></span>

<span data-ttu-id="cf979-135">[!code-csharp[](start-mvc/sample/MvcMovie/Program.cs?highlight=6,14-32)]</span><span class="sxs-lookup"><span data-stu-id="cf979-135">[!code-csharp[](start-mvc/sample/MvcMovie/Program.cs?highlight=6,14-32)]</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="cf979-136">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="cf979-136">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="cf979-137">Aggiungere l'inizializzatore del valore di inizializzazione alla fine del metodo `Configure` nel file *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="cf979-137">Add the seed initializer to the end of the `Configure` method in the *Startup.cs* file.</span></span>

<span data-ttu-id="cf979-138">[!code-csharp[](start-mvc/sample/MvcMovie/Startup.cs?highlight=9&name=snippet_seed)]</span><span class="sxs-lookup"><span data-stu-id="cf979-138">[!code-csharp[](start-mvc/sample/MvcMovie/Startup.cs?highlight=9&name=snippet_seed)]</span></span>

---
::: moniker-end

<span data-ttu-id="cf979-139">Eseguire il test dell'app</span><span class="sxs-lookup"><span data-stu-id="cf979-139">Test the app</span></span>

* <span data-ttu-id="cf979-140">Eliminare tutti i record nel database.</span><span class="sxs-lookup"><span data-stu-id="cf979-140">Delete all the records in the DB.</span></span> <span data-ttu-id="cf979-141">È possibile eseguire questa operazione con i collegamenti di eliminazione nel browser o da SSOX.</span><span class="sxs-lookup"><span data-stu-id="cf979-141">You can do this with the delete links in the browser or from SSOX.</span></span>
* <span data-ttu-id="cf979-142">Forzare l'inizializzazione dell'app (chiamare i metodi nella classe `Startup`) in modo che venga eseguito il metodo di inizializzazione.</span><span class="sxs-lookup"><span data-stu-id="cf979-142">Force the app to initialize (call the methods in the `Startup` class) so the seed method runs.</span></span> <span data-ttu-id="cf979-143">Per forzare l'inizializzazione, IIS Express deve essere arrestato e riavviato.</span><span class="sxs-lookup"><span data-stu-id="cf979-143">To force initialization, IIS Express must be stopped and restarted.</span></span> <span data-ttu-id="cf979-144">È possibile eseguire questa operazione adottando uno degli approcci seguenti:</span><span class="sxs-lookup"><span data-stu-id="cf979-144">You can do this with any of the following approaches:</span></span>

  * <span data-ttu-id="cf979-145">Fare clic con il pulsante destro del mouse sull'icona dell'area di notifica di IIS Express e toccare **Esci** o **Arresta sito**</span><span class="sxs-lookup"><span data-stu-id="cf979-145">Right click the IIS Express system tray icon in the notification area and tap **Exit** or **Stop Site**</span></span>

    ![Icona dell'area di notifica di IIS Express](working-with-sql/_static/iisExIcon.png)

    ![Menu di scelta rapida](working-with-sql/_static/stopIIS.png)

    * <span data-ttu-id="cf979-148">Se Visual Studio è in esecuzione in modalità non di debug, premere F5 per attivare la modalità di debug</span><span class="sxs-lookup"><span data-stu-id="cf979-148">If you were running VS in non-debug mode, press F5 to run in debug mode</span></span>
    * <span data-ttu-id="cf979-149">Se Visual Studio è in esecuzione in modalità di debug, arrestare il debugger e premere F5</span><span class="sxs-lookup"><span data-stu-id="cf979-149">If you were running VS in debug mode, stop the debugger and press F5</span></span>

<span data-ttu-id="cf979-150">L'app mostra i dati inizializzati.</span><span class="sxs-lookup"><span data-stu-id="cf979-150">The app shows the seeded data.</span></span>

![App per i film MVC aperta in Microsoft Edge con i dati sui film](working-with-sql/_static/m55.png)

> [!div class="step-by-step"]
> <span data-ttu-id="cf979-152">[Precedente](adding-model.md)
> [Successivo](controller-methods-views.md)</span><span class="sxs-lookup"><span data-stu-id="cf979-152">[Previous](adding-model.md)
[Next](controller-methods-views.md)</span></span>  
