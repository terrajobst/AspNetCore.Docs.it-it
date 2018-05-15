---
title: Aggiungere un modello a un'app ASP.NET Core MVC
author: rick-anderson
description: Aggiungere un modello a una app semplice di ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 12/8/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app/adding-model
ms.openlocfilehash: 4204d4e2d474db51692d42751a9f82373e9f0c0d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
# <a name="add-a-model-to-an-aspnet-core-mvc-app"></a><span data-ttu-id="f10d2-103">Aggiungere un modello a un'app ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="f10d2-103">Add a model to an ASP.NET Core MVC app</span></span>

[!INCLUDE [adding-model](../../includes/mvc-intro/adding-model1.md)]

<span data-ttu-id="f10d2-104">Nota: i modelli ASP.NET Core 2.0 contengono la cartella *Models*.</span><span class="sxs-lookup"><span data-stu-id="f10d2-104">Note: The ASP.NET Core 2.0 templates contain the *Models* folder.</span></span>

<span data-ttu-id="f10d2-105">Fare clic con il pulsante destro del mouse sulla cartella *Models* > **Aggiungi** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="f10d2-105">Right-click the *Models* folder > **Add** > **Class**.</span></span> <span data-ttu-id="f10d2-106">Assegnare il nome **Movie** alla classe e aggiungere le proprietà seguenti:</span><span class="sxs-lookup"><span data-stu-id="f10d2-106">Name the class **Movie** and add the following properties:</span></span>

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

<span data-ttu-id="f10d2-107">Il campo `ID` è richiesto dal database per la chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="f10d2-107">The `ID` field is required by the database for the primary key.</span></span> 

<span data-ttu-id="f10d2-108">Compilare il progetto per verificare che non ci siano errori.</span><span class="sxs-lookup"><span data-stu-id="f10d2-108">Build the project to verify you don't have any errors.</span></span> <span data-ttu-id="f10d2-109">L'app **M**VC dispone ora di un **M**odello.</span><span class="sxs-lookup"><span data-stu-id="f10d2-109">You now have a **M**odel in your **M**VC app.</span></span>

## <a name="scaffolding-a-controller"></a><span data-ttu-id="f10d2-110">Eseguire lo scaffolding di un controller</span><span class="sxs-lookup"><span data-stu-id="f10d2-110">Scaffolding a controller</span></span>

<span data-ttu-id="f10d2-111">In **Esplora soluzioni** fare clic con il pulsante destro del mouse sulla cartella *Controllers*> **Aggiungi > Controller**.</span><span class="sxs-lookup"><span data-stu-id="f10d2-111">In **Solution Explorer**, right-click the *Controllers* folder **> Add > Controller**.</span></span>

![vista del passaggio precedente](adding-model/_static/add_controller.png)

<span data-ttu-id="f10d2-113">Se viene visualizzata la finestra di dialogo **Aggiungi dipendenze MVC**:</span><span class="sxs-lookup"><span data-stu-id="f10d2-113">If the **Add MVC Dependencies** dialog appears:</span></span>

* <span data-ttu-id="f10d2-114">[Aggiornare Visual Studio alla versione più recente](https://www.visualstudio.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="f10d2-114">[Update Visual Studio to the latest version](https://www.visualstudio.com/downloads/).</span></span> <span data-ttu-id="f10d2-115">Questa finestra di dialogo viene visualizzata nelle versioni di Visual Studio precedenti alla 15.5.</span><span class="sxs-lookup"><span data-stu-id="f10d2-115">Visual Studio versions prior to 15.5 show this dialog.</span></span>
* <span data-ttu-id="f10d2-116">Se non è possibile eseguire l'aggiornamento, selezionare **Aggiungi** ed eseguire di nuovo i passaggi per l'aggiunta del controller.</span><span class="sxs-lookup"><span data-stu-id="f10d2-116">If you can't update, select **ADD**, and then follow the add controller steps again.</span></span>

<span data-ttu-id="f10d2-117">Nella finestra di dialogo **Aggiungi scaffolding** toccare **Controller MVC con visualizzazioni, che usa Entity Framework**.</span><span class="sxs-lookup"><span data-stu-id="f10d2-117">In the **Add Scaffold** dialog, tap **MVC Controller with views, using Entity Framework > Add**.</span></span>

![Finestra di dialogo Aggiungi scaffolding](adding-model/_static/add_scaffold2.png)

<span data-ttu-id="f10d2-119">Completare la finestra di dialogo **Aggiungi controller**:</span><span class="sxs-lookup"><span data-stu-id="f10d2-119">Complete the **Add Controller** dialog:</span></span>

* <span data-ttu-id="f10d2-120">**Classe di modello:** *Movie (MvcMovie.Models)*</span><span class="sxs-lookup"><span data-stu-id="f10d2-120">**Model class:** *Movie (MvcMovie.Models)*</span></span>
* <span data-ttu-id="f10d2-121">**Classe del contesto dati:** selezionare l'icona **+** e aggiungere il valore predefinito **MvcMovie.Models.MvcMovieContext**</span><span class="sxs-lookup"><span data-stu-id="f10d2-121">**Data context class:** Select the **+** icon and add the default **MvcMovie.Models.MvcMovieContext**</span></span>

![Aggiungere il contesto dati](adding-model/_static/dc.png)

* <span data-ttu-id="f10d2-123">**Viste:** mantenere il valore predefinito di ogni opzione selezionata</span><span class="sxs-lookup"><span data-stu-id="f10d2-123">**Views:** Keep the default of each option checked</span></span>
* <span data-ttu-id="f10d2-124">**Nome del controller:** mantenere il valore predefinito *MoviesController*</span><span class="sxs-lookup"><span data-stu-id="f10d2-124">**Controller name:** Keep the default *MoviesController*</span></span>
* <span data-ttu-id="f10d2-125">Toccare **Aggiungi**</span><span class="sxs-lookup"><span data-stu-id="f10d2-125">Tap **Add**</span></span>

![Finestra di dialogo Aggiungi controller](adding-model/_static/add_controller2.png)

<span data-ttu-id="f10d2-127">Visual Studio crea:</span><span class="sxs-lookup"><span data-stu-id="f10d2-127">Visual Studio creates:</span></span>

* <span data-ttu-id="f10d2-128">Una [classe del contesto di database](xref:data/ef-mvc/intro#create-the-database-context) Entity Framework Core (*Data/MvcMovieContext.cs*)</span><span class="sxs-lookup"><span data-stu-id="f10d2-128">An Entity Framework Core [database context class](xref:data/ef-mvc/intro#create-the-database-context) (*Data/MvcMovieContext.cs*)</span></span>
* <span data-ttu-id="f10d2-129">Un controller di film (*Controllers/MoviesController.cs*)</span><span class="sxs-lookup"><span data-stu-id="f10d2-129">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="f10d2-130">File di vista Razor per le pagine Create (Crea), Delete (Elimina), Details (Dettagli), Edit (Modifica) e Index (Indice) (<em>Views/Movies/&ast;.cshtml</em>)</span><span class="sxs-lookup"><span data-stu-id="f10d2-130">Razor view files for Create, Delete, Details, Edit, and Index pages (<em>Views/Movies/&ast;.cshtml</em>)</span></span>

<span data-ttu-id="f10d2-131">La creazione automatica del contesto di database e di viste e metodi di azione [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, delete) è nota come *scaffolding*.</span><span class="sxs-lookup"><span data-stu-id="f10d2-131">The automatic creation of the database context and [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views is known as *scaffolding*.</span></span> <span data-ttu-id="f10d2-132">Sarà presto disponibile un'applicazione Web completamente funzionale che consente di gestire un database di film.</span><span class="sxs-lookup"><span data-stu-id="f10d2-132">You'll soon have a fully functional web application that lets you manage a movie database.</span></span>

<span data-ttu-id="f10d2-133">Se si esegue l'app e si fa clic sul collegamento **Mvc Movie**, viene visualizzato un errore simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="f10d2-133">If you run the app and click on the **Mvc Movie** link, you get an error similar to the following:</span></span>

```
An unhandled exception occurred while processing the request.

SqlException: Cannot open database "MvcMovieContext-<GUID removed>" requested by the login. The login failed.
Login failed for user 'Rick'.

System.Data.SqlClient.SqlInternalConnectionTds..ctor(DbConnectionPoolIdentity identity, SqlConnectionString 
```

<span data-ttu-id="f10d2-134">È necessario creare il database e si userà la funzionalità di [migrazioni](xref:data/ef-mvc/migrations) di Entity Framework Core a tale scopo.</span><span class="sxs-lookup"><span data-stu-id="f10d2-134">You need to create the database, and you'll use the EF Core [Migrations](xref:data/ef-mvc/migrations) feature to do that.</span></span> <span data-ttu-id="f10d2-135">Le migrazioni consentono di creare un database che corrisponde al modello di dati e aggiornare lo schema del database quando cambia il modello di dati.</span><span class="sxs-lookup"><span data-stu-id="f10d2-135">Migrations lets you create a database that matches your data model and update the database schema when your data model changes.</span></span>

## <a name="add-ef-tooling-and-perform-initial-migration"></a><span data-ttu-id="f10d2-136">Aggiungere gli strumenti di Entity Framework ed eseguire la migrazione iniziale</span><span class="sxs-lookup"><span data-stu-id="f10d2-136">Add EF tooling and perform initial migration</span></span>

<span data-ttu-id="f10d2-137">In questa sezione viene usata la Console di Gestione pacchetti per:</span><span class="sxs-lookup"><span data-stu-id="f10d2-137">In this section you'll use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="f10d2-138">Aggiungere il pacchetto degli strumenti di Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="f10d2-138">Add the Entity Framework Core Tools package.</span></span> <span data-ttu-id="f10d2-139">Questo pacchetto è necessario per aggiungere le migrazioni e aggiornare il database.</span><span class="sxs-lookup"><span data-stu-id="f10d2-139">This package is required to add migrations and update the database.</span></span>
* <span data-ttu-id="f10d2-140">Aggiungere una migrazione iniziale.</span><span class="sxs-lookup"><span data-stu-id="f10d2-140">Add an initial migration.</span></span>
* <span data-ttu-id="f10d2-141">Aggiornare il database con la migrazione iniziale.</span><span class="sxs-lookup"><span data-stu-id="f10d2-141">Update the database with the initial migration.</span></span>

<span data-ttu-id="f10d2-142">Dal menu **Strumenti** selezionare **Gestione pacchetti NuGet > Console di Gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="f10d2-142">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>

<!-- following image shared with uid: tutorials/razor-pages/model -->
  ![Menu della Console di Gestione pacchetti](adding-model/_static/pmc.png)

<span data-ttu-id="f10d2-144">Nella Console di Gestione pacchetti immettere i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="f10d2-144">In the PMC, enter the following commands:</span></span>

``` PMC
Install-Package Microsoft.EntityFrameworkCore.Tools
Add-Migration Initial
Update-Database
```

<span data-ttu-id="f10d2-145">**Nota:** se viene visualizzato un errore con il comando `Install-Package`, aprire Gestione pacchetti NuGet e cercare il pacchetto `Microsoft.EntityFrameworkCore.Tools`.</span><span class="sxs-lookup"><span data-stu-id="f10d2-145">**Note:** If you receive an error with the `Install-Package` command, open NuGet Package Manager and search for the `Microsoft.EntityFrameworkCore.Tools` package.</span></span> <span data-ttu-id="f10d2-146">In questo modo è possibile installare il pacchetto o verificare se è già installato.</span><span class="sxs-lookup"><span data-stu-id="f10d2-146">This allows you to install the package or check if it's already installed.</span></span> <span data-ttu-id="f10d2-147">In alternativa, in caso di problemi con la Console di Gestione pacchetti vedere l'[approccio con l'interfaccia della riga di comando](#cli).</span><span class="sxs-lookup"><span data-stu-id="f10d2-147">Alternatively, see the [CLI approach](#cli) if you have problems with the PMC.</span></span>

<span data-ttu-id="f10d2-148">Il comando `Add-Migration` crea un codice per creare lo schema del database iniziale.</span><span class="sxs-lookup"><span data-stu-id="f10d2-148">The `Add-Migration` command creates code to create the initial database schema.</span></span> <span data-ttu-id="f10d2-149">Lo schema è basato sul modello specificato in `DbContext` (nel file *Data/MvcMovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="f10d2-149">The schema is based on the model specified in the `DbContext`(In the *Data/MvcMovieContext.cs* file).</span></span> <span data-ttu-id="f10d2-150">L'argomento `Initial` viene usato per denominare le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="f10d2-150">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="f10d2-151">È possibile usare qualunque nome, ma per convenzione si sceglie un nome che descrive la migrazione.</span><span class="sxs-lookup"><span data-stu-id="f10d2-151">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="f10d2-152">Per altre informazioni vedere [Introduzione alle migrazioni](xref:data/ef-mvc/migrations#introduction-to-migrations).</span><span class="sxs-lookup"><span data-stu-id="f10d2-152">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="f10d2-153">Il comando `Update-Database` esegue il metodo `Up` nel file *Migrations/\<time-stamp>_Initial.cs*, che crea il database.</span><span class="sxs-lookup"><span data-stu-id="f10d2-153">The `Update-Database` command runs the `Up` method in the *Migrations/\<time-stamp>_Initial.cs* file, which creates the database.</span></span>

<a name="cli"></a> <span data-ttu-id="f10d2-154">È possibile eseguire la procedura descritta in precedenza tramite l'interfaccia della riga di comando anziché la Console di Gestione pacchetti:</span><span class="sxs-lookup"><span data-stu-id="f10d2-154">You can perform the preceeding steps using the command-line interface (CLI) rather than the PMC:</span></span>

* <span data-ttu-id="f10d2-155">Aggiungere gli [strumenti di Entity Framework Core](xref:data/ef-mvc/migrations#entity-framework-core-nuget-packages-for-migrations) nel file *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="f10d2-155">Add [EF Core tooling](xref:data/ef-mvc/migrations#entity-framework-core-nuget-packages-for-migrations) to the *.csproj* file.</span></span>
* <span data-ttu-id="f10d2-156">Dalla console (nella directory del progetto) eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="f10d2-156">Run the following commands from the console (in the project directory):</span></span>

  ```console
  dotnet ef migrations add Initial
  dotnet ef database update
  ```     
  
  <span data-ttu-id="f10d2-157">Se si esegue l'app e viene visualizzato l'errore:</span><span class="sxs-lookup"><span data-stu-id="f10d2-157">If you run the app and get the error:</span></span>
  
  ```text
  SqlException: Cannot open database "Movie" requested by the login.
  The login failed.
  Login failed for user 'user name'.
  ```

<span data-ttu-id="f10d2-158">Probabilmente non è stato eseguito il comando ` dotnet ef database update`.</span><span class="sxs-lookup"><span data-stu-id="f10d2-158">You probably have not run ` dotnet ef database update`.</span></span>
  
[!INCLUDE [adding-model](../../includes/mvc-intro/adding-model3.md)]

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]

[!INCLUDE [adding-model](../../includes/mvc-intro/adding-model4.md)]

![Menu di scelta rapida IntelliSense su un elemento modello contenente le proprietà disponibili per ID, prezzo, data di rilascio e titolo](adding-model/_static/ints.png)

## <a name="additional-resources"></a><span data-ttu-id="f10d2-160">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="f10d2-160">Additional resources</span></span>

* [<span data-ttu-id="f10d2-161">Helper tag</span><span class="sxs-lookup"><span data-stu-id="f10d2-161">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="f10d2-162">Globalizzazione e localizzazione</span><span class="sxs-lookup"><span data-stu-id="f10d2-162">Globalization and localization</span></span>](xref:fundamentals/localization)

> [!div class="step-by-step"]
> <span data-ttu-id="f10d2-163">[Precedente - Aggiunta di una vista](adding-view.md)
> [Successivo - Utilizzo del linguaggio SQL](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="f10d2-163">[Previous Adding a View](adding-view.md)
[Next Working with SQL](working-with-sql.md)</span></span>  
