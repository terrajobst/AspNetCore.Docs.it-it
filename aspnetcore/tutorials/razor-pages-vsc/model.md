---
title: Aggiungere un modello a un'app Razor Pages in ASP.NET Core con Visual Studio Code
author: rick-anderson
description: Informazioni su come aggiungere un modello a un'app Razor Pages in ASP.NET Core con Visual Studio Code.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/27/2017
uid: tutorials/razor-pages-vsc/model
ms.openlocfilehash: bf7566d1be0d7b520ab329ed5490e11f11240886
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276075"
---
# <a name="add-a-model-to-an-aspnet-core-razor-pages-app-with-visual-studio-code"></a><span data-ttu-id="2d6d0-103">Aggiungere un modello a un'app Razor Pages in ASP.NET Core con Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2d6d0-103">Add a model to an ASP.NET Core Razor Pages app with Visual Studio Code</span></span>

[!INCLUDE [model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="2d6d0-104">Aggiungere un modello di dati</span><span class="sxs-lookup"><span data-stu-id="2d6d0-104">Add a data model</span></span>

* <span data-ttu-id="2d6d0-105">Aggiungere una cartella denominata *Modelli*.</span><span class="sxs-lookup"><span data-stu-id="2d6d0-105">Add a folder named *Models*.</span></span>
* <span data-ttu-id="2d6d0-106">Aggiungere una classe alla cartella *Modello* denominata *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="2d6d0-106">Add a class to the *Models* folder named *Movie.cs*.</span></span>
* <span data-ttu-id="2d6d0-107">Aggiungere il codice seguente al file *Modelli/Movie.cs*:</span><span class="sxs-lookup"><span data-stu-id="2d6d0-107">Add the following code to the *Models/Movie.cs* file:</span></span>

[!INCLUDE [model 2](../../includes/RP/model2.md)]

[!INCLUDE [model 2a](../../includes/RP/model2a.md)]

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-6)]

<span data-ttu-id="2d6d0-108">Compilare il progetto per verificare che non ci siano errori.</span><span class="sxs-lookup"><span data-stu-id="2d6d0-108">Build the project to verify you don't have any errors.</span></span>

### <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="2d6d0-109">Pacchetti NuGet di Entity Framework Core per le migrazioni</span><span class="sxs-lookup"><span data-stu-id="2d6d0-109">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="2d6d0-110">Gli strumenti di Entity Framework per l'interfaccia della riga di comando (CLI) sono disponibili in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span><span class="sxs-lookup"><span data-stu-id="2d6d0-110">The EF tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="2d6d0-111">Per installare questo pacchetto, aggiungerlo alla collezione `DotNetCliToolReference` nel file *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="2d6d0-111">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file.</span></span> <span data-ttu-id="2d6d0-112">**Nota:** è necessario installare questo pacchetto modificando il file *.csproj* ; non è possibile utilizzare il comando `install-package` o la GUI di gestione di pacchetti.</span><span class="sxs-lookup"><span data-stu-id="2d6d0-112">**Note:** You have to install this package by editing the *.csproj* file; you can't use the `install-package` command or the package manager GUI.</span></span>

<span data-ttu-id="2d6d0-113">Modificare il file *RazorPagesMovie.csproj*:</span><span class="sxs-lookup"><span data-stu-id="2d6d0-113">Edit the *RazorPagesMovie.csproj* file:</span></span>

* <span data-ttu-id="2d6d0-114">Selezionare **File** > **Apri file** e selezionare il file *RazorPagesMovie.csproj*.</span><span class="sxs-lookup"><span data-stu-id="2d6d0-114">Select **File** > **Open File**, and then select the *RazorPagesMovie.csproj* file.</span></span>
* <span data-ttu-id="2d6d0-115">Aggiungere il riferimento allo strumento per `Microsoft.EntityFrameworkCore.Tools.DotNet` al secondo **\<ItemGroup >**:</span><span class="sxs-lookup"><span data-stu-id="2d6d0-115">Add tool reference for `Microsoft.EntityFrameworkCore.Tools.DotNet` to the second **\<ItemGroup>**:</span></span>

[!code-xml[](../../tutorials/razor-pages/razor-pages-start/snapshot_cli_sample/RazorPagesMovie/RazorPagesMovie.cli.csproj)]

[!INCLUDE [model 3](../../includes/RP/model3.md)]

<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a><span data-ttu-id="2d6d0-116">Eseguire lo scaffolding del modello di filmato</span><span class="sxs-lookup"><span data-stu-id="2d6d0-116">Scaffold the Movie model</span></span>

* <span data-ttu-id="2d6d0-117">Aprire una finestra di comando nella directory del progetto (la directory che contiene i file *Program.cs*, *Startup.cs* e *csproj*).</span><span class="sxs-lookup"><span data-stu-id="2d6d0-117">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="2d6d0-118">Eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="2d6d0-118">Run the following command:</span></span>

<span data-ttu-id="2d6d0-119">**Nota: eseguire il comando riportato di seguito su Windows. Per MacOS e Linux, vedere il comando successivo**</span><span class="sxs-lookup"><span data-stu-id="2d6d0-119">**Note: Run the following command on Windows. For MacOS and Linux, see the next command**</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="2d6d0-120">In MacOS e Linux, eseguire il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="2d6d0-120">On MacOS and Linux, run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

<span data-ttu-id="2d6d0-121">Se viene visualizzato l'errore:</span><span class="sxs-lookup"><span data-stu-id="2d6d0-121">If you get the error:</span></span>
  ```
  The process cannot access the file 
 'RazorPagesMovie/bin/Debug/netcoreapp2.0/RazorPagesMovie.dll' 
  because it is being used by another process.
  ```

<span data-ttu-id="2d6d0-122">Chiudere Visual Studio ed eseguire nuovamente il comando.</span><span class="sxs-lookup"><span data-stu-id="2d6d0-122">Exit Visual Studio and run the command again.</span></span>

[!INCLUDE [model 4](../../includes/RP/model4.md)]

<span data-ttu-id="2d6d0-123">L'esercitazione successiva illustra i file creati tramite scaffolding.</span><span class="sxs-lookup"><span data-stu-id="2d6d0-123">The next tutorial explains the files created by scaffolding.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="2d6d0-124">[Precedente: Introduzione](xref:tutorials/razor-pages-vsc/razor-pages-start)
> [Successivo: Pagine Razor create tramite scaffolding](xref:tutorials/razor-pages-vsc/page)</span><span class="sxs-lookup"><span data-stu-id="2d6d0-124">[Previous: Get Started](xref:tutorials/razor-pages-vsc/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages-vsc/page)</span></span>
