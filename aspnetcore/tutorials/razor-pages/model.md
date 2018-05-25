---
title: Aggiungere un modello a un'app Razor Pages in ASP.NET Core
author: rick-anderson
description: Scoprire come aggiungere classi per la gestione dei film in un database tramite Entity Framework Core (EF Core).
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 07/27/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/model
ms.openlocfilehash: 80b3aae661342ccde257805c370780cd6f5b4aa4
ms.sourcegitcommit: 24c32648ab0c6f0be15333d7c23c1bf680858c43
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/20/2018
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="45168-103">Aggiungere un modello a un'app Razor Pages in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="45168-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

[!INCLUDE [model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="45168-104">Aggiungere un modello di dati</span><span class="sxs-lookup"><span data-stu-id="45168-104">Add a data model</span></span>

<span data-ttu-id="45168-105">In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto **RazorPagesMovie** > **Aggiungi** > **Nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="45168-105">In Solution Explorer, right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="45168-106">Assegnare il nome *Modelli* alla cartella.</span><span class="sxs-lookup"><span data-stu-id="45168-106">Name the folder *Models*.</span></span>

<span data-ttu-id="45168-107">Fare clic con il pulsante destro del mouse sulla cartella *Models*.</span><span class="sxs-lookup"><span data-stu-id="45168-107">Right click the *Models* folder.</span></span> <span data-ttu-id="45168-108">Selezionare **Aggiungi** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="45168-108">Select **Add** > **Class**.</span></span> <span data-ttu-id="45168-109">Assegnare il nome **Movie** alla classe e aggiungere le proprietà seguenti:</span><span class="sxs-lookup"><span data-stu-id="45168-109">Name the class **Movie** and add the following properties:</span></span>

[!INCLUDE [model 2](../../includes/RP/model2.md)]

<a name="cs"></a>
### <a name="add-a-database-connection-string"></a><span data-ttu-id="45168-110">Aggiungere una stringa di connessione del database</span><span class="sxs-lookup"><span data-stu-id="45168-110">Add a database connection string</span></span>

<span data-ttu-id="45168-111">Aggiungere una stringa di connessione al file *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="45168-111">Add a connection string to the *appsettings.json* file.</span></span>

[!code-json[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=8-10)]

<a name="reg"></a>
###  <a name="register-the-database-context"></a><span data-ttu-id="45168-112">Registrare il contesto del database</span><span class="sxs-lookup"><span data-stu-id="45168-112">Register the database context</span></span>

<span data-ttu-id="45168-113">Registrare il contesto del database con il contenitore [Inserimento dipendenze](xref:fundamentals/dependency-injection) nel [ metodo ConfigureServices della classe Startup](xref:fundamentals/startup#the-startup-class) (*Startup.cs*):</span><span class="sxs-lookup"><span data-stu-id="45168-113">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in the [ConfigureServices method of the Startup class](xref:fundamentals/startup#the-startup-class) (*Startup.cs*):</span></span>

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=3-5,7-9)]

<span data-ttu-id="45168-114">Compilare il progetto per verificare che non ci siano errori.</span><span class="sxs-lookup"><span data-stu-id="45168-114">Build the project to verify you don't have any errors.</span></span>

<a name="pmc"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a><span data-ttu-id="45168-115">Aggiungere gli strumenti di scaffolding ed eseguire la migrazione iniziale</span><span class="sxs-lookup"><span data-stu-id="45168-115">Add scaffold tooling and perform initial migration</span></span>

<span data-ttu-id="45168-116">In questa sezione viene usata la Console di Gestione pacchetti per:</span><span class="sxs-lookup"><span data-stu-id="45168-116">In this section, you use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="45168-117">Aggiungere il pacchetto di generazione codice Web di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="45168-117">Add the Visual Studio web code generation package.</span></span> <span data-ttu-id="45168-118">Questo pacchetto è necessario per eseguire il motore di scaffolding.</span><span class="sxs-lookup"><span data-stu-id="45168-118">This package is required to run the scaffolding engine.</span></span>
* <span data-ttu-id="45168-119">Aggiungere una migrazione iniziale.</span><span class="sxs-lookup"><span data-stu-id="45168-119">Add an initial migration.</span></span>
* <span data-ttu-id="45168-120">Aggiornare il database con la migrazione iniziale.</span><span class="sxs-lookup"><span data-stu-id="45168-120">Update the database with the initial migration.</span></span>

<span data-ttu-id="45168-121">Dal menu **Strumenti** selezionare **Gestione pacchetti NuGet** > **Console di Gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="45168-121">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Menu della Console di Gestione pacchetti](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="45168-123">Nella Console di Gestione pacchetti immettere i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="45168-123">In the PMC, enter the following commands:</span></span>

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design -Version 2.0.3
Add-Migration Initial
Update-Database
```

<span data-ttu-id="45168-124">In alternativa, è possibile usare i comandi dell'interfaccia della riga di comando di .NET Core seguenti:</span><span class="sxs-lookup"><span data-stu-id="45168-124">Alternatively, the following .NET Core CLI commands can be used:</span></span>

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet ef migrations add Initial
dotnet ef database update
```

<span data-ttu-id="45168-125">Il comando `Install-Package` installa gli strumenti necessari a eseguire il motore di scaffolding.</span><span class="sxs-lookup"><span data-stu-id="45168-125">The `Install-Package` command installs the tooling required to run the scaffolding engine.</span></span>

<span data-ttu-id="45168-126">Il comando `Add-Migration` genera un codice per creare lo schema del database iniziale.</span><span class="sxs-lookup"><span data-stu-id="45168-126">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="45168-127">Lo schema è basato sul modello specificato in `DbContext` (nel file *Models/MovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="45168-127">The schema is based on the model specified in the `DbContext` (In the *Models/MovieContext.cs* file).</span></span> <span data-ttu-id="45168-128">L'argomento `Initial` viene usato per denominare le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="45168-128">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="45168-129">È possibile usare qualunque nome, ma per convenzione si sceglie un nome che descrive la migrazione.</span><span class="sxs-lookup"><span data-stu-id="45168-129">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="45168-130">Per altre informazioni vedere [Introduzione alle migrazioni](xref:data/ef-mvc/migrations#introduction-to-migrations).</span><span class="sxs-lookup"><span data-stu-id="45168-130">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="45168-131">Il comando `Update-Database` esegue il metodo `Up` nel file *Migrations/\<time-stamp>_InitialCreate.cs*, che crea il database.</span><span class="sxs-lookup"><span data-stu-id="45168-131">The `Update-Database` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file, which creates the database.</span></span>

[!INCLUDE [model 4windows](../../includes/RP/model4Win.md)]

[!INCLUDE [model 4](../../includes/RP/model4tbl.md)]

<a name="test"></a>
### <a name="test-the-app"></a><span data-ttu-id="45168-132">Eseguire il test dell'app</span><span class="sxs-lookup"><span data-stu-id="45168-132">Test the app</span></span>

* <span data-ttu-id="45168-133">Eseguire l'app e accodare `/Movies` all'URL nel browser (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="45168-133">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>
* <span data-ttu-id="45168-134">Eseguire il test del collegamento **Crea**.</span><span class="sxs-lookup"><span data-stu-id="45168-134">Test the **Create** link.</span></span>

  ![Pagina Crea](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* <span data-ttu-id="45168-136">Eseguire il test dei collegamenti **Modifica**, **Dettagli** e **Elimina**.</span><span class="sxs-lookup"><span data-stu-id="45168-136">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="45168-137">Se viene visualizzata un'eccezione SQL, verificare di avere eseguito le migrazioni e di avere aggiornato il database:</span><span class="sxs-lookup"><span data-stu-id="45168-137">If you get a SQL exception, verify you have run migrations and updated the database:</span></span>

<span data-ttu-id="45168-138">L'esercitazione successiva illustra i file creati tramite scaffolding.</span><span class="sxs-lookup"><span data-stu-id="45168-138">The next tutorial explains the files created by scaffolding.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="45168-139">[Precedente: Introduzione](xref:tutorials/razor-pages/razor-pages-start)
> [Successivo: Pagine Razor create tramite scaffolding](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="45168-139">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>    
