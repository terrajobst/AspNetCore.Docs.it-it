---
title: Aggiunta di un modello a un'app di pagine Razor con Visual Studio per Mac
author: rick-anderson
description: Aggiunta di un modello a un'app di pagine Razor in ASP.NET Core con Visual Studio per Mac
ms.author: riande
manager: wpickett
ms.date: 08/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages-vsc/model
ms.openlocfilehash: 2a0c21ddc9ff44143ab828f8962946138a7ca0fa
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/19/2018
---
# <a name="adding-a-model-to-a-razor-pages-app-in-aspnet-core-with-visual-studio-for-mac"></a><span data-ttu-id="ce8b3-103">Aggiunta di un modello a un'app di pagine Razor in ASP.NET Core con Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="ce8b3-103">Adding a model to a Razor Pages app in ASP.NET Core with Visual Studio for Mac</span></span>

[!INCLUDE[model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="ce8b3-104">Aggiungere un modello di dati</span><span class="sxs-lookup"><span data-stu-id="ce8b3-104">Add a data model</span></span>

* <span data-ttu-id="ce8b3-105">Aggiungere una cartella denominata *Modelli*.</span><span class="sxs-lookup"><span data-stu-id="ce8b3-105">Add a folder named *Models*.</span></span>
* <span data-ttu-id="ce8b3-106">Aggiungere una classe alla cartella *Modello* denominata *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="ce8b3-106">Add a class to the *Models* folder named *Movie.cs*.</span></span>
* <span data-ttu-id="ce8b3-107">Aggiungere il codice seguente al file *Modelli/Movie.cs*:</span><span class="sxs-lookup"><span data-stu-id="ce8b3-107">Add the following code to the *Models/Movie.cs* file:</span></span>

[!INCLUDE[model 2](../../includes/RP/model2.md)]
[!INCLUDE[model 2a](../../includes/RP/model2a.md)]

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-6)]

<span data-ttu-id="ce8b3-108">Compilare il progetto per verificare che non ci siano errori.</span><span class="sxs-lookup"><span data-stu-id="ce8b3-108">Build the project to verify you don't have any errors.</span></span>

### <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="ce8b3-109">Pacchetti NuGet di Entity Framework Core per le migrazioni</span><span class="sxs-lookup"><span data-stu-id="ce8b3-109">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="ce8b3-110">Gli strumenti di Entity Framework per l'interfaccia della riga di comando (CLI) sono disponibili in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span><span class="sxs-lookup"><span data-stu-id="ce8b3-110">The EF tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="ce8b3-111">Per installare questo pacchetto, aggiungerlo alla collezione `DotNetCliToolReference` nel file *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="ce8b3-111">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file.</span></span> <span data-ttu-id="ce8b3-112">**Nota:** è necessario installare questo pacchetto modificando il file *.csproj* ; non è possibile utilizzare il comando `install-package` o la GUI di gestione di pacchetti.</span><span class="sxs-lookup"><span data-stu-id="ce8b3-112">**Note:** You have to install this package by editing the *.csproj* file; you can't use the `install-package` command or the package manager GUI.</span></span>

<span data-ttu-id="ce8b3-113">Modificare il file *RazorPagesMovie.csproj*:</span><span class="sxs-lookup"><span data-stu-id="ce8b3-113">Edit the *RazorPagesMovie.csproj* file:</span></span>

* <span data-ttu-id="ce8b3-114">Selezionare **File** > **Apri file** e selezionare il file *RazorPagesMovie.csproj*.</span><span class="sxs-lookup"><span data-stu-id="ce8b3-114">Select **File** > **Open File**, and then select the *RazorPagesMovie.csproj* file.</span></span>
* <span data-ttu-id="ce8b3-115">Aggiungere il riferimento allo strumento per `Microsoft.EntityFrameworkCore.Tools.DotNet` al secondo **\<ItemGroup >**:</span><span class="sxs-lookup"><span data-stu-id="ce8b3-115">Add tool reference for `Microsoft.EntityFrameworkCore.Tools.DotNet` to the second **\<ItemGroup>**:</span></span>

[!code-xml[Main](../../tutorials/razor-pages/razor-pages-start/snapshot_cli_sample/RazorPagesMovie/RazorPagesMovie.cli.csproj?range=12-16&highlight=4)]

[!INCLUDE[model 3](../../includes/RP/model3.md)]

<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a><span data-ttu-id="ce8b3-116">Eseguire lo scaffolding del modello di filmato</span><span class="sxs-lookup"><span data-stu-id="ce8b3-116">Scaffold the Movie model</span></span>

* <span data-ttu-id="ce8b3-117">Aprire una finestra di comando nella directory del progetto (la directory che contiene i file *Program.cs*, *Startup.cs* e *csproj*).</span><span class="sxs-lookup"><span data-stu-id="ce8b3-117">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="ce8b3-118">Eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="ce8b3-118">Run the following command:</span></span>

<span data-ttu-id="ce8b3-119">**Nota: eseguire il comando riportato di seguito su Windows. Per MacOS e Linux, vedere il comando successivo**</span><span class="sxs-lookup"><span data-stu-id="ce8b3-119">**Note: Run the following command on Windows. For MacOS and Linux, see the next command**</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="ce8b3-120">In MacOS e Linux, eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="ce8b3-120">On MacOS and Linux, run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

<span data-ttu-id="ce8b3-121">Se viene visualizzato l'errore:</span><span class="sxs-lookup"><span data-stu-id="ce8b3-121">If you get the error:</span></span>
  ```
  The process cannot access the file 
 'RazorPagesMovie/bin/Debug/netcoreapp2.0/RazorPagesMovie.dll' 
  because it is being used by another process.
  ```

<span data-ttu-id="ce8b3-122">Chiudere Visual Studio ed eseguire nuovamente il comando.</span><span class="sxs-lookup"><span data-stu-id="ce8b3-122">Exit Visual Studio and run the command again.</span></span>

[!INCLUDE[model 4](../../includes/RP/model4.md)]<span data-ttu-id="ce8b3-123"> L'esercitazione successiva illustra i file creati dallo scaffolding.</span><span class="sxs-lookup"><span data-stu-id="ce8b3-123"> The next tutorial explains the files created by scaffolding.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="ce8b3-124">[Indietro: Introduzione](xref:tutorials/razor-pages-vsc/razor-pages-start)
[Avanti: pagine Razor create tramite scaffolding](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="ce8b3-124">[Previous: Getting Started](xref:tutorials/razor-pages-vsc/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>
