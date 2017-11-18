---
title: Aggiunta di un modello a un'app ASP.NET Core MVC.
author: rick-anderson
description: Aggiungere un modello a una app semplice di ASP.NET Core.
ms.author: riande
ms.date: 09/18/2017
ms.topic: get-started-article
ms.technology: aspnet
keywords: ASP.NET Core, WebAPI, Web API, REST, Mac, Linux, HTTP, Service, HTTP Service,VS Code
ms.prod: asp.net-core
manager: wpickett
ms.assetid: 8dc28498-eeee-4666-b903-b593059e9f39
uid: tutorials/first-mvc-app-xplat/adding-model
ms.openlocfilehash: 70aa344ca4ceafacf53907c925fd595e47104d7e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
[!INCLUDE[adding-model1](../../includes/mvc-intro/adding-model1.md)]

* <span data-ttu-id="1e091-104">Aggiungere una classe alla cartella *Modello* denominata *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="1e091-104">Add a class to the *Models* folder named *Movie.cs*.</span></span>
* <span data-ttu-id="1e091-105">Aggiungere il codice seguente al file *Modelli/Movie.cs*:</span><span class="sxs-lookup"><span data-stu-id="1e091-105">Add the following code to the *Models/Movie.cs* file:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

<span data-ttu-id="1e091-106">Il campo `ID` è richiesto dal database per la chiave primaria.</span><span class="sxs-lookup"><span data-stu-id="1e091-106">The `ID` field is required by the database for the primary key.</span></span> 

<span data-ttu-id="1e091-107">Compilare l'applicazione per verificare di non ottenere errori e di aver aggiunto un **M**odello alla propria app **M**VC.</span><span class="sxs-lookup"><span data-stu-id="1e091-107">Build the app to verify you don't have any errors, and you've finally added a **M**odel to your **M**VC app.</span></span>

## <a name="prepare-the-project-for-scaffolding"></a><span data-ttu-id="1e091-108">Preparare il progetto per lo scaffolding</span><span class="sxs-lookup"><span data-stu-id="1e091-108">Prepare the project for scaffolding</span></span>

- <span data-ttu-id="1e091-109">Aggiungere i pacchetti NuGet evidenziati seguenti al file *MvcMovie.csproj*:</span><span class="sxs-lookup"><span data-stu-id="1e091-109">Add the following highlighted NuGet packages to the *MvcMovie.csproj* file:</span></span>
             
   [!code-csharp[Main](start-mvc/sample/MvcMovie/MvcMovie.csproj?highlight=7,10)]

- <span data-ttu-id="1e091-110">Salvare il file e selezionare **Ripristina** per il messaggio di **informazione** "Dipendenze non risolte".</span><span class="sxs-lookup"><span data-stu-id="1e091-110">Save the file and select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>
- <span data-ttu-id="1e091-111">Creare un file *Models/MvcMovieContext.cs* e aggiungere la classe `MvcMovieContext` seguente:</span><span class="sxs-lookup"><span data-stu-id="1e091-111">Create a *Models/MvcMovieContext.cs* file and add the following `MvcMovieContext` class:</span></span>

   [!code-csharp[Main](start-mvc/sample/MvcMovie/Models/MvcMovieContext.cs)]
   
- <span data-ttu-id="1e091-112">Aprire il file *Startup.cs* file e aggiungere due using:</span><span class="sxs-lookup"><span data-stu-id="1e091-112">Open the *Startup.cs* file and add two usings:</span></span>

   [!code-csharp[Main](start-mvc/sample/MvcMovie/Startup.cs?name=snippet1&highlight=1,2)]

- <span data-ttu-id="1e091-113">Aggiungere il contesto del database al file *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="1e091-113">Add the database context to the *Startup.cs* file:</span></span>

   [!code-csharp[Main](start-mvc/sample/MvcMovie/Startup.cs?name=snippet2&highlight=6-7)]

  <span data-ttu-id="1e091-114">Indica a Entity Framework quali classi di modello sono incluse nel modello di dati.</span><span class="sxs-lookup"><span data-stu-id="1e091-114">This tells Entity Framework which model classes are included in the data model.</span></span> <span data-ttu-id="1e091-115">Si sta definendo un *set di entità* di oggetti filmato, che verranno rappresentati nel database come tabella di un filmato.</span><span class="sxs-lookup"><span data-stu-id="1e091-115">You're defining one *entity set* of Movie objects, which will be represented in the database as a Movie table.</span></span>

- <span data-ttu-id="1e091-116">Compilare il progetto per verificare che non siano presenti errori.</span><span class="sxs-lookup"><span data-stu-id="1e091-116">Build the project to verify there are no errors.</span></span>

## <a name="scaffold-the-moviecontroller"></a><span data-ttu-id="1e091-117">Eseguire lo scaffolding di MovieController</span><span class="sxs-lookup"><span data-stu-id="1e091-117">Scaffold the MovieController</span></span>

<span data-ttu-id="1e091-118">Aprire una finestra terminale nella cartella di progetto ed eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="1e091-118">Open a terminal window in the project folder and run the following commands:</span></span>

```
dotnet restore
dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries 
```

> [!NOTE]
> <span data-ttu-id="1e091-119">Se si verifica un errore durante l'esecuzione del comando di scaffolding, vedere [l'argomento 444 nell'archivio relativo allo scaffolding](https://github.com/aspnet/scaffolding/issues/444) per una soluzione alternativa.</span><span class="sxs-lookup"><span data-stu-id="1e091-119">If you get an error when the scaffolding command runs, see [issue 444 in the scaffolding repository](https://github.com/aspnet/scaffolding/issues/444) for a workaround.</span></span>

<span data-ttu-id="1e091-120">Il motore di scaffolding crea:</span><span class="sxs-lookup"><span data-stu-id="1e091-120">The scaffolding engine creates the following:</span></span>

* <span data-ttu-id="1e091-121">Un controller di filmati (*Controllers/MoviesController.cs*)</span><span class="sxs-lookup"><span data-stu-id="1e091-121">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="1e091-122">File di vista Razor per le pagine Create, Delete, Details, Edit e Index (*Views/Movies/\*.cshtml*)</span><span class="sxs-lookup"><span data-stu-id="1e091-122">Razor view files for Create, Delete, Details, Edit and Index pages (*Views/Movies/\*.cshtml*)</span></span>

<span data-ttu-id="1e091-123">La creazione automatica di metodi di azione e visualizzazioni [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, delete) è nota come *scaffolding*.</span><span class="sxs-lookup"><span data-stu-id="1e091-123">The automatic creation of [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views is known as *scaffolding*.</span></span> <span data-ttu-id="1e091-124">Sarà presto disponibile un'applicazione Web completamente funzionale che consente di gestire un database di filmati.</span><span class="sxs-lookup"><span data-stu-id="1e091-124">You'll soon have a fully functional web application that lets you manage a movie database.</span></span>

[!INCLUDE[adding-model 2x](../../includes/mvc-intro/adding-model2xp.md)]

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model3.md)]

<span data-ttu-id="1e091-125">È ora disponibile un database e alcune pagine per visualizzare, modificare, aggiornare ed eliminare dati.</span><span class="sxs-lookup"><span data-stu-id="1e091-125">You now have a database and pages to display, edit, update and delete data.</span></span> <span data-ttu-id="1e091-126">Nella prossima esercitazione verrà utilizzato il database.</span><span class="sxs-lookup"><span data-stu-id="1e091-126">In the next tutorial, we'll work with the database.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="1e091-127">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="1e091-127">Additional resources</span></span>

* [<span data-ttu-id="1e091-128">Helper tag</span><span class="sxs-lookup"><span data-stu-id="1e091-128">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="1e091-129">Globalizzazione e localizzazione</span><span class="sxs-lookup"><span data-stu-id="1e091-129">Globalization and localization</span></span>](xref:fundamentals/localization)

>[!div class="step-by-step"]
<span data-ttu-id="1e091-130">[Indietro - Aggiunta di una visualizzazione](adding-view.md)
[Avanti - Uso di SQLite](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="1e091-130">[Previous - Add a view](adding-view.md)
[Next - Working with SQLite](working-with-sql.md)</span></span>
