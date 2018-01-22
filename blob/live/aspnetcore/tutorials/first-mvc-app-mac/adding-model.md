---
title: Aggiungere un modello a un'app ASP.NET Core MVC
author: rick-anderson
description: Aggiungere un modello a una app semplice di ASP.NET Core.
ms.author: riande
manager: wpickett
ms.devlang: csharp
ms.date: 09/22/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: .net-core
uid: tutorials/first-mvc-app-mac/adding-model
ms.openlocfilehash: 3a7db5e435fe72150feb1e0ec905b6f6adc16f2c
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/19/2018
---
[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model1.md)]

* <span data-ttu-id="c8ce6-103">Fare clic con il pulsante destro del mouse sulla cartella *Modelli* e scegliere **Aggiungi** > **Nuovo file**.</span><span class="sxs-lookup"><span data-stu-id="c8ce6-103">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span> 
* <span data-ttu-id="c8ce6-104">Nella finestra di dialogo **Nuovo file**:</span><span class="sxs-lookup"><span data-stu-id="c8ce6-104">In the **New File** dialog:</span></span>

  * <span data-ttu-id="c8ce6-105">Selezionare **Generale** nel riquadro a sinistra.</span><span class="sxs-lookup"><span data-stu-id="c8ce6-105">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="c8ce6-106">Selezionare **Classe vuota** nel riquadro centrale.</span><span class="sxs-lookup"><span data-stu-id="c8ce6-106">Select **Empty Class** in the center pain.</span></span>
  * <span data-ttu-id="c8ce6-107">Denominare la classe **Filmato** e selezionare **Nuovo**.</span><span class="sxs-lookup"><span data-stu-id="c8ce6-107">Name the class **Movie** and select **New**.</span></span>

<span data-ttu-id="c8ce6-108">Aggiungere le proprietà seguenti alla classe `Movie`:</span><span class="sxs-lookup"><span data-stu-id="c8ce6-108">Add the following properties to the `Movie` class:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

<span data-ttu-id="c8ce6-109">Il campo `ID` è richiesto dal database per la chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="c8ce6-109">The `ID` field is required by the database for the primary key.</span></span>

<span data-ttu-id="c8ce6-110">Compilare il progetto per verificare che non ci siano errori.</span><span class="sxs-lookup"><span data-stu-id="c8ce6-110">Build the project to verify you don't have any errors.</span></span> <span data-ttu-id="c8ce6-111">L'app **M**VC dispone ora di un **M**odello.</span><span class="sxs-lookup"><span data-stu-id="c8ce6-111">You now have a **M**odel in your **M**VC app.</span></span>

## <a name="prepare-the-project-for-scaffolding"></a><span data-ttu-id="c8ce6-112">Preparare il progetto per lo scaffolding</span><span class="sxs-lookup"><span data-stu-id="c8ce6-112">Prepare the project for scaffolding</span></span>

- <span data-ttu-id="c8ce6-113">Fare clic con il pulsante destro del mouse sul file di progetto, quindi selezionare **Strumenti > Modifica File**.</span><span class="sxs-lookup"><span data-stu-id="c8ce6-113">Right click on the project file, and then select **Tools > Edit File**.</span></span>

  ![vista del passaggio precedente](adding-model/_static/1.png)

- <span data-ttu-id="c8ce6-115">Aggiungere i pacchetti NuGet evidenziati seguenti al file *MvcMovie.csproj*:</span><span class="sxs-lookup"><span data-stu-id="c8ce6-115">Add the following highlighted NuGet packages to the *MvcMovie.csproj* file:</span></span>
             
  [!code-csharp[Main](../first-mvc-app-xplat/start-mvc/sample/MvcMovie/MvcMovie.csproj?highlight=7,10)]

- <span data-ttu-id="c8ce6-116">Salvare il file.</span><span class="sxs-lookup"><span data-stu-id="c8ce6-116">Save the file.</span></span>

- <span data-ttu-id="c8ce6-117">Creare un file *Models/MvcMovieContext.cs* e aggiungere la classe `MvcMovieContext` seguente: [!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Models/MvcMovieContext.cs)]</span><span class="sxs-lookup"><span data-stu-id="c8ce6-117">Create a *Models/MvcMovieContext.cs* file and add the following `MvcMovieContext` class:  [!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Models/MvcMovieContext.cs)]</span></span>
   
- <span data-ttu-id="c8ce6-118">Aprire il file *Startup.cs* e aggiungere due using: [!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet1&highlight=1,2)]</span><span class="sxs-lookup"><span data-stu-id="c8ce6-118">Open the *Startup.cs* file and add two usings:  [!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet1&highlight=1,2)]</span></span>

- <span data-ttu-id="c8ce6-119">Aggiungere il contesto del database al file *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="c8ce6-119">Add the database context to the *Startup.cs* file:</span></span>

   [!code-csharp[Main](../../tutorials/first-mvc-app-xplat/start-mvc/sample/MvcMovie/Startup.cs?name=snippet2&highlight=6-7)]

  <span data-ttu-id="c8ce6-120">Indica a Entity Framework quali classi di modello sono incluse nel modello di dati.</span><span class="sxs-lookup"><span data-stu-id="c8ce6-120">This tells Entity Framework which model classes are included in the data model.</span></span> <span data-ttu-id="c8ce6-121">Si sta definendo un *set di entità* di oggetti filmato, che verranno rappresentati nel database come tabella di un filmato.</span><span class="sxs-lookup"><span data-stu-id="c8ce6-121">You're defining one *entity set* of Movie objects, which will be represented in the database as a Movie table.</span></span>

- <span data-ttu-id="c8ce6-122">Compilare il progetto per verificare che non siano presenti errori.</span><span class="sxs-lookup"><span data-stu-id="c8ce6-122">Build the project to verify there are no errors.</span></span>

## <a name="scaffold-the-moviecontroller"></a><span data-ttu-id="c8ce6-123">Eseguire lo scaffolding di MovieController</span><span class="sxs-lookup"><span data-stu-id="c8ce6-123">Scaffold the MovieController</span></span>

<span data-ttu-id="c8ce6-124">Aprire una finestra terminale nella cartella di progetto ed eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="c8ce6-124">Open a terminal window in the project folder and run the following commands:</span></span>

```
dotnet restore
dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries 
```
<span data-ttu-id="c8ce6-125">Se viene visualizzato l'errore `No executable found matching command "dotnet-aspnet-codegenerator", verify`:</span><span class="sxs-lookup"><span data-stu-id="c8ce6-125">If you get the error `No executable found matching command "dotnet-aspnet-codegenerator", verify`:</span></span>

 * <span data-ttu-id="c8ce6-126">Si è nella directory del progetto.</span><span class="sxs-lookup"><span data-stu-id="c8ce6-126">You are in the project directory.</span></span> <span data-ttu-id="c8ce6-127">La directory del progetto ha i file *Program.cs*, *Startup.cs* e *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="c8ce6-127">The project directory has the *Program.cs*, *Startup.cs* and *.csproj* files.</span></span>
 * <span data-ttu-id="c8ce6-128">La versione dotnet è 1.1 o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="c8ce6-128">Your dotnet version is 1.1 or higher.</span></span> <span data-ttu-id="c8ce6-129">Eseguire `dotnet` per ottenere la versione.</span><span class="sxs-lookup"><span data-stu-id="c8ce6-129">Run `dotnet` to get the version.</span></span>
 * <span data-ttu-id="c8ce6-130">È stato aggiunto l'elemento `<DotNetCliToolReference>` al [file MvcMovie.csproj](#prepare-the-project-for-scaffolding).</span><span class="sxs-lookup"><span data-stu-id="c8ce6-130">You have added the `<DotNetCliToolReference>` element to the [MvcMovie.csproj file](#prepare-the-project-for-scaffolding).</span></span>
 
<!--
> [!NOTE]
> If you get an error when the scaffolding command runs, see [issue 444 in the scaffolding repository](https://github.com/aspnet/scaffolding/issues/444) for a workaround.
-->

<span data-ttu-id="c8ce6-131">Il motore di scaffolding crea:</span><span class="sxs-lookup"><span data-stu-id="c8ce6-131">The scaffolding engine creates the following:</span></span>

* <span data-ttu-id="c8ce6-132">Un controller di filmati (*Controllers/MoviesController.cs*)</span><span class="sxs-lookup"><span data-stu-id="c8ce6-132">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="c8ce6-133">File di vista Razor per le pagine Create, Delete, Details, Edit e Index (*Views/Movies/\*.cshtml*)</span><span class="sxs-lookup"><span data-stu-id="c8ce6-133">Razor view files for Create, Delete, Details, Edit and Index pages (*Views/Movies/\*.cshtml*)</span></span>

<span data-ttu-id="c8ce6-134">La creazione automatica di metodi di azione e visualizzazioni [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, delete) è nota come *scaffolding*.</span><span class="sxs-lookup"><span data-stu-id="c8ce6-134">The automatic creation of [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views is known as *scaffolding*.</span></span> <span data-ttu-id="c8ce6-135">Sarà presto disponibile un'applicazione Web completamente funzionale che consente di gestire un database di film.</span><span class="sxs-lookup"><span data-stu-id="c8ce6-135">You'll soon have a fully functional web application that lets you manage a movie database.</span></span>

### <a name="add-the-files-to-visual-studio"></a><span data-ttu-id="c8ce6-136">Aggiungere i file a Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c8ce6-136">Add the files to Visual Studio</span></span>

* <span data-ttu-id="c8ce6-137">Aggiungere il file *MovieController.cs* al progetto di Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="c8ce6-137">Add the *MovieController.cs* file to the Visual Studio project:</span></span>

  * <span data-ttu-id="c8ce6-138">Fare clic con il pulsante destro del mouse sulla cartella *Controller* e selezionare **Aggiungi > Aggiungi file**.</span><span class="sxs-lookup"><span data-stu-id="c8ce6-138">Right-click on the *Controllers* folder and select **Add > Add Files**.</span></span>
  * <span data-ttu-id="c8ce6-139">Selezionare il file *MovieController.cs*.</span><span class="sxs-lookup"><span data-stu-id="c8ce6-139">Select the *MovieController.cs* file.</span></span>

* <span data-ttu-id="c8ce6-140">Aggiungere la cartella *Filmati* e le viste:</span><span class="sxs-lookup"><span data-stu-id="c8ce6-140">Add the *Movies* folder and views:</span></span>

  * <span data-ttu-id="c8ce6-141">Fare clic con il pulsante destro del mouse sulla cartella *Viste* e selezionare **Aggiungi > Aggiungi cartella esistente**.</span><span class="sxs-lookup"><span data-stu-id="c8ce6-141">Right-click on the *Views* folder and select **Add > Add Existing Folder**.</span></span>
  * <span data-ttu-id="c8ce6-142">Passare alla cartella *Viste*, selezionare *Viste\Filmati*, quindi selezionare **Apri**.</span><span class="sxs-lookup"><span data-stu-id="c8ce6-142">Navigate to the *Views* folder, select *Views\Movies*, and then select **Open**.</span></span>
  * <span data-ttu-id="c8ce6-143">Nella finestra di dialogo **Select files to add from Movies** (Seleziona file da aggiungere dai filmati), selezionare **Include All** (Includi tutto) e quindi **OK**.</span><span class="sxs-lookup"><span data-stu-id="c8ce6-143">In the **Select files to add from Movies** dialog, select **Include All**, and then **OK**.</span></span>

[!INCLUDE[adding-model 2x](../../includes/mvc-intro/adding-model2xp.md)]

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model3.md)]

<span data-ttu-id="c8ce6-144">È ora disponibile un database e alcune pagine per visualizzare, modificare, aggiornare ed eliminare dati.</span><span class="sxs-lookup"><span data-stu-id="c8ce6-144">You now have a database and pages to display, edit, update and delete data.</span></span> <span data-ttu-id="c8ce6-145">Nella prossima esercitazione verrà utilizzato il database.</span><span class="sxs-lookup"><span data-stu-id="c8ce6-145">In the next tutorial, we'll work with the database.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c8ce6-146">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="c8ce6-146">Additional resources</span></span>

* [<span data-ttu-id="c8ce6-147">Helper tag</span><span class="sxs-lookup"><span data-stu-id="c8ce6-147">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="c8ce6-148">Globalizzazione e localizzazione</span><span class="sxs-lookup"><span data-stu-id="c8ce6-148">Globalization and localization</span></span>](xref:fundamentals/localization)

>[!div class="step-by-step"]
<span data-ttu-id="c8ce6-149">[Precedente - Aggiunta di una vista](adding-view.md)
[Successivo - Utilizzo del linguaggio SQL](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="c8ce6-149">[Previous Adding a View](adding-view.md)
[Next Working with SQL](working-with-sql.md)</span></span>  
