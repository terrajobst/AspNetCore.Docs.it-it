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
uid: tutorials/razor-pages-mac/model
ms.openlocfilehash: 7b1b2d54e9c68b0a6f2b1355726d0d1cb484f69e
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/19/2018
---
# <a name="adding-a-model-to-a-razor-pages-app-in-aspnet-core-with-visual-studio-for-mac"></a><span data-ttu-id="adaea-103">Aggiunta di un modello a un'app di pagine Razor in ASP.NET Core con Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="adaea-103">Adding a model to a Razor Pages app in ASP.NET Core with Visual Studio for Mac</span></span>

[!INCLUDE[model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="adaea-104">Aggiungere un modello di dati</span><span class="sxs-lookup"><span data-stu-id="adaea-104">Add a data model</span></span>

* <span data-ttu-id="adaea-105">In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto **RazorPagesMovie**, quindi selezionare **Aggiungi** > **Nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="adaea-105">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="adaea-106">Assegnare il nome *Modelli* alla cartella.</span><span class="sxs-lookup"><span data-stu-id="adaea-106">Name the folder *Models*.</span></span>
* <span data-ttu-id="adaea-107">Fare clic con il pulsante destro del mouse sulla cartella *Modelli* e scegliere **Aggiungi** > **Nuovo file**.</span><span class="sxs-lookup"><span data-stu-id="adaea-107">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="adaea-108">Nella finestra di dialogo **Nuovo file**:</span><span class="sxs-lookup"><span data-stu-id="adaea-108">In the **New File** dialog:</span></span>

  * <span data-ttu-id="adaea-109">Selezionare **Generale** nel riquadro a sinistra.</span><span class="sxs-lookup"><span data-stu-id="adaea-109">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="adaea-110">Selezionare **Classe vuota** nel riquadro centrale.</span><span class="sxs-lookup"><span data-stu-id="adaea-110">Select **Empty Class** in the center pain.</span></span>
  * <span data-ttu-id="adaea-111">Denominare la classe **Filmato** e selezionare **Nuovo**.</span><span class="sxs-lookup"><span data-stu-id="adaea-111">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE[model 2](../../includes/RP/model2.md)]
[!INCLUDE[model 2a](../../includes/RP/model2a.md)]

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-6)]

<span data-ttu-id="adaea-112">Fare clic con il pulsante destro del mouse su una riga rossa ondulata, ad esempio `MovieContext` nella riga `services.AddDbContext<MovieContext>(options =>`.</span><span class="sxs-lookup"><span data-stu-id="adaea-112">Right click on a red squiggly line, for example `MovieContext` in the line `services.AddDbContext<MovieContext>(options =>`.</span></span> <span data-ttu-id="adaea-113">Selezionare **Quick Fix > using RazorPagesMovie.Models;** (Correzione rapida > tramite RazorPagesMovie.Models;).</span><span class="sxs-lookup"><span data-stu-id="adaea-113">Select **Quick Fix > using RazorPagesMovie.Models;**.</span></span> <span data-ttu-id="adaea-114">Visual studio aggiunge l'istruzione using.</span><span class="sxs-lookup"><span data-stu-id="adaea-114">Visual studio adds the using statement.</span></span>

<span data-ttu-id="adaea-115">Compilare il progetto per verificare che non ci siano errori.</span><span class="sxs-lookup"><span data-stu-id="adaea-115">Build the project to verify you don't have any errors.</span></span>

![Pagina Crea](model/red.png)

### <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="adaea-117">Pacchetti NuGet di Entity Framework Core per le migrazioni</span><span class="sxs-lookup"><span data-stu-id="adaea-117">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="adaea-118">Gli strumenti di Entity Framework per l'interfaccia della riga di comando (CLI) sono disponibili in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span><span class="sxs-lookup"><span data-stu-id="adaea-118">The EF tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="adaea-119">Per installare questo pacchetto, aggiungerlo alla collezione `DotNetCliToolReference` nel file *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="adaea-119">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file.</span></span> <span data-ttu-id="adaea-120">**Nota:** è necessario installare questo pacchetto modificando il file *.csproj* ; non è possibile utilizzare il comando `install-package` o la GUI di gestione di pacchetti.</span><span class="sxs-lookup"><span data-stu-id="adaea-120">**Note:** You have to install this package by editing the *.csproj* file; you can't use the `install-package` command or the package manager GUI.</span></span>

<span data-ttu-id="adaea-121">Per modificare un file *.csproj*:</span><span class="sxs-lookup"><span data-stu-id="adaea-121">To edit a *.csproj* file:</span></span>

* <span data-ttu-id="adaea-122">Selezionare **File** > **Apri** e selezionare il file *csproj*.</span><span class="sxs-lookup"><span data-stu-id="adaea-122">Select **File** > **Open**, and then select the *.csproj* file.</span></span>
* <span data-ttu-id="adaea-123">Selezionare **Opzioni**.</span><span class="sxs-lookup"><span data-stu-id="adaea-123">Select **Options**.</span></span>
* <span data-ttu-id="adaea-124">Modificare **Apri con** in **Editor codice sorgente**.</span><span class="sxs-lookup"><span data-stu-id="adaea-124">Change **Open with** to **Source Code Editor**.</span></span>

![Modificare il file csproj](model/csproj.png)

<span data-ttu-id="adaea-126">Aggiungere il riferimento allo strumento `Microsoft.EntityFrameworkCore.Tools.DotNet` al secondo **\<ItemGroup >**:</span><span class="sxs-lookup"><span data-stu-id="adaea-126">Add the `Microsoft.EntityFrameworkCore.Tools.DotNet` tool reference to the second **\<ItemGroup>**:</span></span>

[!code-xml[Main](../../tutorials/razor-pages/razor-pages-start/snapshot_cli_sample/RazorPagesMovie/RazorPagesMovie.cli.csproj?range=12-16&highlight=4)]

[!INCLUDE[model3](../../includes/RP/model3.md)]
[!INCLUDE[model 4x](../../includes/RP/model4x.md)]

[!INCLUDE[model 4 exit](../../includes/RP/model4exit.md)]

[!INCLUDE[model 4](../../includes/RP/model4.md)]

### <a name="add-the-pagesmovies-files-to-the-project"></a><span data-ttu-id="adaea-127">Aggiungere i file di pagine/filmati al progetto</span><span class="sxs-lookup"><span data-stu-id="adaea-127">Add the Pages/Movies files to the project</span></span>

* <span data-ttu-id="adaea-128">In Visual Studio, fare clic con il pulsante destro del mouse sulla cartella *Pagine* e selezionare **Aggiungi > Aggiungi cartella esistente**.</span><span class="sxs-lookup"><span data-stu-id="adaea-128">In Visual Studio, Right-click the *Pages* folder and select **Add > Add existing Folder**.</span></span>
* <span data-ttu-id="adaea-129">Selezionare la cartella *Filmati*.</span><span class="sxs-lookup"><span data-stu-id="adaea-129">Select the *Movies* folder.</span></span>
* <span data-ttu-id="adaea-130">Nella finestra di dialogo *Scegli i file da includere nel progetto*, selezionare **Includi tutto**.</span><span class="sxs-lookup"><span data-stu-id="adaea-130">In the *Chosse files to include in the project* dialog, select **Include All**.</span></span>

<span data-ttu-id="adaea-131">L'esercitazione successiva illustra i file creati tramite scaffolding.</span><span class="sxs-lookup"><span data-stu-id="adaea-131">The next tutorial explains the files created by scaffolding.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="adaea-132">[Indietro: Introduzione](xref:tutorials/razor-pages-mac/razor-pages-start)
[Avanti: pagine Razor create tramite scaffolding](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="adaea-132">[Previous: Getting Started](xref:tutorials/razor-pages-mac/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>
