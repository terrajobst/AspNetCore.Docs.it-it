---
title: Aggiungere un modello a un'app Razor Pages in ASP.NET Core con Visual Studio per Mac
author: rick-anderson
description: Informazioni su come aggiungere un modello a un'app Razor Pages in ASP.NET Core con Visual Studio per Mac.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/27/2017
uid: tutorials/razor-pages-mac/model
ms.openlocfilehash: 3ca6c9b9988b8335116b7248c6c4a89997d02b14
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38186758"
---
# <a name="add-a-model-to-an-aspnet-core-razor-pages-app-with-visual-studio-for-mac"></a><span data-ttu-id="2abe3-103">Aggiungere un modello a un'app Razor Pages in ASP.NET Core con Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="2abe3-103">Add a model to an ASP.NET Core Razor Pages app with Visual Studio for Mac</span></span>

[!INCLUDE [model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="2abe3-104">Aggiungere un modello di dati</span><span class="sxs-lookup"><span data-stu-id="2abe3-104">Add a data model</span></span>

* <span data-ttu-id="2abe3-105">In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto **RazorPagesMovie**, quindi selezionare **Aggiungi** > **Nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="2abe3-105">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="2abe3-106">Assegnare il nome *Modelli* alla cartella.</span><span class="sxs-lookup"><span data-stu-id="2abe3-106">Name the folder *Models*.</span></span>
* <span data-ttu-id="2abe3-107">Fare clic con il pulsante destro del mouse sulla cartella *Modelli* e scegliere **Aggiungi** > **Nuovo file**.</span><span class="sxs-lookup"><span data-stu-id="2abe3-107">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="2abe3-108">Nella finestra di dialogo **Nuovo file**:</span><span class="sxs-lookup"><span data-stu-id="2abe3-108">In the **New File** dialog:</span></span>

  * <span data-ttu-id="2abe3-109">Selezionare **Generale** nel riquadro a sinistra.</span><span class="sxs-lookup"><span data-stu-id="2abe3-109">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="2abe3-110">Selezionare **Classe vuota** nel riquadro centrale.</span><span class="sxs-lookup"><span data-stu-id="2abe3-110">Select **Empty Class** in the center pain.</span></span>
  * <span data-ttu-id="2abe3-111">Denominare la classe **Filmato** e selezionare **Nuovo**.</span><span class="sxs-lookup"><span data-stu-id="2abe3-111">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 2](../../includes/RP/model2.md)]

[!INCLUDE [model 2a](../../includes/RP/model2a.md)]

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-6)]

<span data-ttu-id="2abe3-112">Fare clic con il pulsante destro del mouse su una riga rossa ondulata, ad esempio `MovieContext` nella riga `services.AddDbContext<MovieContext>(options =>`.</span><span class="sxs-lookup"><span data-stu-id="2abe3-112">Right click on a red squiggly line, for example `MovieContext` in the line `services.AddDbContext<MovieContext>(options =>`.</span></span> <span data-ttu-id="2abe3-113">Selezionare **Quick Fix > using RazorPagesMovie.Models;** (Correzione rapida > tramite RazorPagesMovie.Models;).</span><span class="sxs-lookup"><span data-stu-id="2abe3-113">Select **Quick Fix > using RazorPagesMovie.Models;**.</span></span> <span data-ttu-id="2abe3-114">Visual studio aggiunge l'istruzione using.</span><span class="sxs-lookup"><span data-stu-id="2abe3-114">Visual studio adds the using statement.</span></span>

<span data-ttu-id="2abe3-115">Compilare il progetto per verificare che non ci siano errori.</span><span class="sxs-lookup"><span data-stu-id="2abe3-115">Build the project to verify you don't have any errors.</span></span>

![Pagina Crea](model/red.png)

### <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="2abe3-117">Pacchetti NuGet di Entity Framework Core per le migrazioni</span><span class="sxs-lookup"><span data-stu-id="2abe3-117">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="2abe3-118">Gli strumenti di Entity Framework per l'interfaccia della riga di comando (CLI) sono disponibili in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span><span class="sxs-lookup"><span data-stu-id="2abe3-118">The EF tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="2abe3-119">Fare clic sul collegamento [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet) per ottenere il numero di versione da usare.</span><span class="sxs-lookup"><span data-stu-id="2abe3-119">Click on the [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet) link to get the version number to use.</span></span> <span data-ttu-id="2abe3-120">Per installare questo pacchetto, aggiungerlo alla collezione `DotNetCliToolReference` nel file *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="2abe3-120">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file.</span></span> <span data-ttu-id="2abe3-121">**Nota:** è necessario installare questo pacchetto modificando il file *.csproj* ; non è possibile utilizzare il comando `install-package` o la GUI di gestione di pacchetti.</span><span class="sxs-lookup"><span data-stu-id="2abe3-121">**Note:** You have to install this package by editing the *.csproj* file; you can't use the `install-package` command or the package manager GUI.</span></span>

<span data-ttu-id="2abe3-122">Per modificare un file *.csproj*:</span><span class="sxs-lookup"><span data-stu-id="2abe3-122">To edit a *.csproj* file:</span></span>

* <span data-ttu-id="2abe3-123">Selezionare **File** > **Apri** e selezionare il file *csproj*.</span><span class="sxs-lookup"><span data-stu-id="2abe3-123">Select **File** > **Open**, and then select the *.csproj* file.</span></span>
* <span data-ttu-id="2abe3-124">Selezionare **Opzioni**.</span><span class="sxs-lookup"><span data-stu-id="2abe3-124">Select **Options**.</span></span>
* <span data-ttu-id="2abe3-125">Modificare **Apri con** in **Editor codice sorgente**.</span><span class="sxs-lookup"><span data-stu-id="2abe3-125">Change **Open with** to **Source Code Editor**.</span></span>

![Modificare il file csproj](model/csproj.png)

<span data-ttu-id="2abe3-127">Aggiungere il riferimento allo strumento `Microsoft.EntityFrameworkCore.Tools.DotNet` al secondo **\<ItemGroup>**:</span><span class="sxs-lookup"><span data-stu-id="2abe3-127">Add the `Microsoft.EntityFrameworkCore.Tools.DotNet` tool reference to the second **\<ItemGroup>**.:</span></span>

[!code-xml[](../../tutorials/razor-pages/razor-pages-start/snapshot_cli_sample/RazorPagesMovie/RazorPagesMovie.cli.csproj?highlight=10)]

<span data-ttu-id="2abe3-128">I numeri di versione indicati nel codice seguente erano corretti al momento della scrittura del presente articolo.</span><span class="sxs-lookup"><span data-stu-id="2abe3-128">The version numbers shown in the following code were correct at the time of writing.</span></span>

[!INCLUDE [model3](../../includes/RP/model3.md)]

[!INCLUDE [model 4x](../../includes/RP/model4x.md)]

[!INCLUDE [model 4 exit](../../includes/RP/model4exit.md)]

[!INCLUDE [model 4](../../includes/RP/model4.md)]

### <a name="add-the-pagesmovies-files-to-the-project"></a><span data-ttu-id="2abe3-129">Aggiungere i file di pagine/filmati al progetto</span><span class="sxs-lookup"><span data-stu-id="2abe3-129">Add the Pages/Movies files to the project</span></span>

* <span data-ttu-id="2abe3-130">In Visual Studio, fare clic con il pulsante destro del mouse sulla cartella *Pagine* e selezionare **Aggiungi > Aggiungi cartella esistente**.</span><span class="sxs-lookup"><span data-stu-id="2abe3-130">In Visual Studio, Right-click the *Pages* folder and select **Add > Add existing Folder**.</span></span>
* <span data-ttu-id="2abe3-131">Selezionare la cartella *Filmati*.</span><span class="sxs-lookup"><span data-stu-id="2abe3-131">Select the *Movies* folder.</span></span>
* <span data-ttu-id="2abe3-132">Nella finestra di dialogo *Scegli i file da includere nel progetto* selezionare **Includi tutto**.</span><span class="sxs-lookup"><span data-stu-id="2abe3-132">In the *Choose files to include in the project* dialog, select **Include All**.</span></span>

<span data-ttu-id="2abe3-133">L'esercitazione successiva illustra i file creati tramite scaffolding.</span><span class="sxs-lookup"><span data-stu-id="2abe3-133">The next tutorial explains the files created by scaffolding.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="2abe3-134">[Precedente: Introduzione](xref:tutorials/razor-pages-mac/razor-pages-start)
> [Successivo: Pagine Razor create tramite scaffolding](xref:tutorials/razor-pages-mac/page)</span><span class="sxs-lookup"><span data-stu-id="2abe3-134">[Previous: Get Started](xref:tutorials/razor-pages-mac/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages-mac/page)</span></span>
