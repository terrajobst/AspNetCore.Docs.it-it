---
title: Aggiunta di un modello a un'app di pagine Razor in ASP.NET Core
author: rick-anderson
description: Aggiunta di un modello a un'app di pagine Razor in ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 07/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/model
ms.openlocfilehash: 84e5ec27904b564fa6ee29843ceae0bb70754ea7
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/19/2018
---
# <a name="adding-a-model-to-a-razor-pages-app"></a><span data-ttu-id="aa3b0-103">Aggiunta di un modello a un'app di pagine Razor</span><span class="sxs-lookup"><span data-stu-id="aa3b0-103">Adding a model to a Razor Pages app</span></span>

[!INCLUDE[model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="aa3b0-104">Aggiungere un modello di dati</span><span class="sxs-lookup"><span data-stu-id="aa3b0-104">Add a data model</span></span>

<span data-ttu-id="aa3b0-105">In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto **RazorPagesMovie** > **Aggiungi** > **Nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="aa3b0-105">In Solution Explorer, right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="aa3b0-106">Assegnare il nome *Modelli* alla cartella.</span><span class="sxs-lookup"><span data-stu-id="aa3b0-106">Name the folder *Models*.</span></span>

<span data-ttu-id="aa3b0-107">Fare clic con il pulsante destro del mouse sulla cartella *Models*.</span><span class="sxs-lookup"><span data-stu-id="aa3b0-107">Right click the *Models* folder.</span></span> <span data-ttu-id="aa3b0-108">Selezionare **Aggiungi** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="aa3b0-108">Select **Add** > **Class**.</span></span> <span data-ttu-id="aa3b0-109">Assegnare il nome **Movie** alla classe e aggiungere le proprietà seguenti:</span><span class="sxs-lookup"><span data-stu-id="aa3b0-109">Name the class **Movie** and add the following properties:</span></span>

[!INCLUDE[model 2](../../includes/RP/model2.md)]

<a name="cs"></a>
### <a name="add-a-database-connection-string"></a><span data-ttu-id="aa3b0-110">Aggiungere una stringa di connessione del database</span><span class="sxs-lookup"><span data-stu-id="aa3b0-110">Add a database connection string</span></span>

<span data-ttu-id="aa3b0-111">Aggiungere una stringa di connessione al file *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="aa3b0-111">Add a connection string to the *appsettings.json* file.</span></span>

[!code-json[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=8-10)]

<a name="reg"></a>
###  <a name="register-the-database-context"></a><span data-ttu-id="aa3b0-112">Registrare il contesto del database</span><span class="sxs-lookup"><span data-stu-id="aa3b0-112">Register the database context</span></span>

<span data-ttu-id="aa3b0-113">Registrare il contesto del database con il contenitore [Dependency Injection](xref:fundamentals/dependency-injection) (inserimento delle dipendenze) nel file *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="aa3b0-113">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in the *Startup.cs* file.</span></span>

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=3-5,7-9)]

<span data-ttu-id="aa3b0-114">Compilare il progetto per verificare che non ci siano errori.</span><span class="sxs-lookup"><span data-stu-id="aa3b0-114">Build the project to verify you don't have any errors.</span></span>

<a name="pmc"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a><span data-ttu-id="aa3b0-115">Aggiungere gli strumenti di scaffolding ed eseguire la migrazione iniziale</span><span class="sxs-lookup"><span data-stu-id="aa3b0-115">Add scaffold tooling and perform initial migration</span></span>

<span data-ttu-id="aa3b0-116">In questa sezione viene usata la Console di Gestione pacchetti per:</span><span class="sxs-lookup"><span data-stu-id="aa3b0-116">In this section, you use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="aa3b0-117">Aggiungere il pacchetto di generazione codice Web di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="aa3b0-117">Add the Visual Studio web code generation package.</span></span> <span data-ttu-id="aa3b0-118">Questo pacchetto è necessario per eseguire il motore di scaffolding.</span><span class="sxs-lookup"><span data-stu-id="aa3b0-118">This package is required to run the scaffolding engine.</span></span>
* <span data-ttu-id="aa3b0-119">Aggiungere una migrazione iniziale.</span><span class="sxs-lookup"><span data-stu-id="aa3b0-119">Add an initial migration.</span></span>
* <span data-ttu-id="aa3b0-120">Aggiornare il database con la migrazione iniziale.</span><span class="sxs-lookup"><span data-stu-id="aa3b0-120">Update the database with the initial migration.</span></span>

<span data-ttu-id="aa3b0-121">Dal menu **Strumenti** selezionare **Gestione pacchetti NuGet** > **Console di Gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="aa3b0-121">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Menu della Console di Gestione pacchetti](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="aa3b0-123">In PMC, immettere i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="aa3b0-123">In the PMC, enter the following commands:</span></span>

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design
Add-Migration Initial
Update-Database
```

<span data-ttu-id="aa3b0-124">Il comando `Install-Package` installa gli strumenti necessari a eseguire il motore di scaffolding.</span><span class="sxs-lookup"><span data-stu-id="aa3b0-124">The `Install-Package` command installs the tooling required to run the scaffolding engine.</span></span>

<span data-ttu-id="aa3b0-125">Il comando `Add-Migration` genera un codice per creare lo schema del database iniziale.</span><span class="sxs-lookup"><span data-stu-id="aa3b0-125">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="aa3b0-126">Lo schema è basato sul modello specificato in `DbContext` (nel file *Models/MovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="aa3b0-126">The schema is based on the model specified in the `DbContext` (In the *Models/MovieContext.cs* file).</span></span> <span data-ttu-id="aa3b0-127">L'argomento `Initial` viene usato per denominare le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="aa3b0-127">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="aa3b0-128">È possibile usare qualunque nome, ma per convenzione si sceglie un nome che descrive la migrazione.</span><span class="sxs-lookup"><span data-stu-id="aa3b0-128">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="aa3b0-129">Per altre informazioni vedere [Introduzione alle migrazioni](xref:data/ef-mvc/migrations#introduction-to-migrations).</span><span class="sxs-lookup"><span data-stu-id="aa3b0-129">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="aa3b0-130">Il comando `Update-Database` esegue il metodo `Up` nel file *Migrations/\<time-stamp>_InitialCreate.cs*, che crea il database.</span><span class="sxs-lookup"><span data-stu-id="aa3b0-130">The `Update-Database` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file, which creates the database.</span></span>

[!INCLUDE[model 4windows](../../includes/RP/model4Win.md)]

[!INCLUDE[model 4](../../includes/RP/model4tbl.md)]

<a name="test"></a>
### <a name="test-the-app"></a><span data-ttu-id="aa3b0-131">Eseguire il test dell'app</span><span class="sxs-lookup"><span data-stu-id="aa3b0-131">Test the app</span></span>

* <span data-ttu-id="aa3b0-132">Eseguire l'app e accodare `/Movies` all'URL nel browser (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="aa3b0-132">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>
* <span data-ttu-id="aa3b0-133">Eseguire il test del collegamento **Crea**.</span><span class="sxs-lookup"><span data-stu-id="aa3b0-133">Test the **Create** link.</span></span>

 ![Pagina Crea](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* <span data-ttu-id="aa3b0-135">Eseguire il test dei collegamenti **Modifica**, **Dettagli** e **Elimina**.</span><span class="sxs-lookup"><span data-stu-id="aa3b0-135">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="aa3b0-136">Se viene visualizzata un'eccezione SQL, verificare di avere eseguito le migrazioni e di avere aggiornato il database:</span><span class="sxs-lookup"><span data-stu-id="aa3b0-136">If you get a SQL exception, verify you have run migrations and updated the database:</span></span>

<span data-ttu-id="aa3b0-137">L'esercitazione successiva illustra i file creati tramite scaffolding.</span><span class="sxs-lookup"><span data-stu-id="aa3b0-137">The next tutorial explains the files created by scaffolding.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="aa3b0-138">[Indietro: Introduzione](xref:tutorials/razor-pages/razor-pages-start)
[Avanti: pagine Razor create tramite scaffolding](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="aa3b0-138">[Previous: Getting Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>    
