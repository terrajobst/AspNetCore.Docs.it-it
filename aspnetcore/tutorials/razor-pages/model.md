---
title: Aggiunta di un modello a un'app di pagine Razor in ASP.NET Core
author: rick-anderson
description: Aggiunta di un modello a un'app di pagine Razor in ASP.NET Core
keywords: ASP.NET Core, pagine Razor, Razor, MVC
ms.author: riande
manager: wpickett
ms.date: 07/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/model
ms.openlocfilehash: 195128f670223f15e1679c51fb6354c1aa527c01
ms.sourcegitcommit: 3ba32b2b6425ed94604cb0f681db0d5bb5f8ad58
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/28/2017
---
# <a name="adding-a-model-to-a-razor-pages-app"></a><span data-ttu-id="b0a15-104">Aggiunta di un modello a un'app di pagine Razor</span><span class="sxs-lookup"><span data-stu-id="b0a15-104">Adding a model to a Razor Pages app</span></span>

[!INCLUDE[model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="b0a15-105">Aggiungere un modello di dati</span><span class="sxs-lookup"><span data-stu-id="b0a15-105">Add a data model</span></span>

<span data-ttu-id="b0a15-106">In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto **RazorPagesMovie** > **Aggiungi** > **Nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="b0a15-106">In Solution Explorer, right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="b0a15-107">Assegnare il nome *Modelli* alla cartella.</span><span class="sxs-lookup"><span data-stu-id="b0a15-107">Name the folder *Models*.</span></span>

<span data-ttu-id="b0a15-108">Fare clic con il pulsante destro del mouse sulla cartella *Models*.</span><span class="sxs-lookup"><span data-stu-id="b0a15-108">Right click the *Models* folder.</span></span> <span data-ttu-id="b0a15-109">Selezionare **Aggiungi** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="b0a15-109">Select **Add** > **Class**.</span></span> <span data-ttu-id="b0a15-110">Assegnare il nome **Movie** alla classe e aggiungere le proprietà seguenti:</span><span class="sxs-lookup"><span data-stu-id="b0a15-110">Name the class **Movie** and add the following properties:</span></span>

[!INCLUDE[model 2](../../includes/RP/model2.md)]

<a name="cs"></a>
### <a name="add-a-database-connection-string"></a><span data-ttu-id="b0a15-111">Aggiungere una stringa di connessione del database</span><span class="sxs-lookup"><span data-stu-id="b0a15-111">Add a database connection string</span></span>

<span data-ttu-id="b0a15-112">Aggiungere una stringa di connessione al file *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="b0a15-112">Add a connection string to the *appsettings.json* file.</span></span>

[!code-json[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=8-10)]

<a name="reg"></a>
###  <a name="register-the-database-context"></a><span data-ttu-id="b0a15-113">Registrare il contesto del database</span><span class="sxs-lookup"><span data-stu-id="b0a15-113">Register the database context</span></span>

<span data-ttu-id="b0a15-114">Registrare il contesto del database con il contenitore [Dependency Injection](xref:fundamentals/dependency-injection) (inserimento delle dipendenze) nel file *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="b0a15-114">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in the *Startup.cs* file.</span></span>

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=3-6)]

<span data-ttu-id="b0a15-115">Compilare il progetto per verificare che non ci siano errori.</span><span class="sxs-lookup"><span data-stu-id="b0a15-115">Build the project to verify you don't have any errors.</span></span>

<a name="pmc"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a><span data-ttu-id="b0a15-116">Aggiungere gli strumenti di scaffolding ed eseguire la migrazione iniziale</span><span class="sxs-lookup"><span data-stu-id="b0a15-116">Add scaffold tooling and perform initial migration</span></span>

<span data-ttu-id="b0a15-117">In questa sezione viene usata la Console di Gestione pacchetti per:</span><span class="sxs-lookup"><span data-stu-id="b0a15-117">In this section, you use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="b0a15-118">Aggiungere il pacchetto di generazione codice Web di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b0a15-118">Add the Visual Studio web code generation package.</span></span> <span data-ttu-id="b0a15-119">Questo pacchetto è necessario per eseguire il motore di scaffolding.</span><span class="sxs-lookup"><span data-stu-id="b0a15-119">This package is required to run the scaffolding engine.</span></span>
* <span data-ttu-id="b0a15-120">Aggiungere una migrazione iniziale.</span><span class="sxs-lookup"><span data-stu-id="b0a15-120">Add an initial migration.</span></span>
* <span data-ttu-id="b0a15-121">Aggiornare il database con la migrazione iniziale.</span><span class="sxs-lookup"><span data-stu-id="b0a15-121">Update the database with the initial migration.</span></span>

<span data-ttu-id="b0a15-122">Dal menu **Strumenti** selezionare **Gestione pacchetti NuGet** > **Console di Gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="b0a15-122">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![Menu della Console di Gestione pacchetti](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="b0a15-124">In PMC, immettere i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="b0a15-124">In the PMC, enter the following commands:</span></span>

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design -Version 2.0.0
Add-Migration Initial
Update-Database
```

<span data-ttu-id="b0a15-125">Il comando `Install-Package` installa gli strumenti necessari a eseguire il motore di scaffolding.</span><span class="sxs-lookup"><span data-stu-id="b0a15-125">The `Install-Package` command installs the tooling required to run the scaffolding engine.</span></span>

<span data-ttu-id="b0a15-126">Il comando `Add-Migration` genera un codice per creare lo schema del database iniziale.</span><span class="sxs-lookup"><span data-stu-id="b0a15-126">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="b0a15-127">Lo schema è basato sul modello specificato in `DbContext` (nel file *Models/MovieContext.cs*).</span><span class="sxs-lookup"><span data-stu-id="b0a15-127">The schema is based on the model specified in the `DbContext` (In the *Models/MovieContext.cs* file).</span></span> <span data-ttu-id="b0a15-128">L'argomento `Initial` viene usato per denominare le migrazioni.</span><span class="sxs-lookup"><span data-stu-id="b0a15-128">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="b0a15-129">È possibile usare qualunque nome, ma per convenzione si sceglie un nome che descrive la migrazione.</span><span class="sxs-lookup"><span data-stu-id="b0a15-129">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="b0a15-130">Per altre informazioni vedere [Introduzione alle migrazioni](xref:data/ef-mvc/migrations#introduction-to-migrations).</span><span class="sxs-lookup"><span data-stu-id="b0a15-130">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="b0a15-131">Il comando `Update-Database` esegue il metodo `Up` nel file *Migrations/\<time-stamp>_InitialCreate.cs*, che crea il database.</span><span class="sxs-lookup"><span data-stu-id="b0a15-131">The `Update-Database` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file, which creates the database.</span></span>

[!INCLUDE[model 4windows](../../includes/RP/model4Win.md)]

[!INCLUDE[model 4](../../includes/RP/model4.md)]

<span data-ttu-id="b0a15-132">L'esercitazione successiva illustra i file creati dallo scaffolding.</span><span class="sxs-lookup"><span data-stu-id="b0a15-132">The next tutorial explains the files created by scaffolding.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="b0a15-133">[Indietro: Introduzione](xref:tutorials/razor-pages/razor-pages-start)
[Avanti: pagine Razor create tramite scaffolding](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="b0a15-133">[Previous: Getting Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>    
