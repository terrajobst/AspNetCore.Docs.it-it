---
title: Aggiunta di un modello a un'app di pagine Razor con Visual Studio per Mac
author: rick-anderson
description: Aggiunta di un modello a un'app di pagine Razor in ASP.NET Core con Visual Studio per Mac
keywords: ASP.NET Core, pagine Razor, Razor, MVC, modello
ms.author: riande
manager: wpickett
ms.date: 8/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages-mac/model
ms.openlocfilehash: b234eb93fbca1f4c83712990712b86e9941968fd
ms.sourcegitcommit: d9ec19e5452af83648074db5d96c0a0f4f9e7f9a
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/29/2017
---
# <a name="adding-a-model-to-a-razor-pages-app-in-aspnet-core-with-visual-studio-for-mac"></a><span data-ttu-id="04cd3-104">Aggiunta di un modello a un'app di pagine Razor in ASP.NET Core con Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="04cd3-104">Adding a model to a Razor Pages app in ASP.NET Core with Visual Studio for Mac</span></span>

[!INCLUDE[model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="04cd3-105">Aggiungere un modello di dati</span><span class="sxs-lookup"><span data-stu-id="04cd3-105">Add a data model</span></span>

* <span data-ttu-id="04cd3-106">In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto **RazorPagesMovie**, quindi selezionare **Aggiungi** > **Nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="04cd3-106">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="04cd3-107">Assegnare il nome *Modelli* alla cartella.</span><span class="sxs-lookup"><span data-stu-id="04cd3-107">Name the folder *Models*.</span></span>
* <span data-ttu-id="04cd3-108">Fare clic con il pulsante destro del mouse sulla cartella *Modelli* e scegliere **Aggiungi** > **Nuovo file**.</span><span class="sxs-lookup"><span data-stu-id="04cd3-108">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="04cd3-109">Nella finestra di dialogo **Nuovo file**:</span><span class="sxs-lookup"><span data-stu-id="04cd3-109">In the **New File** dialog:</span></span>

  * <span data-ttu-id="04cd3-110">Selezionare **Generale** nel riquadro a sinistra.</span><span class="sxs-lookup"><span data-stu-id="04cd3-110">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="04cd3-111">Selezionare **Classe vuota** nel riquadro centrale.</span><span class="sxs-lookup"><span data-stu-id="04cd3-111">Select **Empty Class** in the center pain.</span></span>
  * <span data-ttu-id="04cd3-112">Denominare la classe **Filmato** e selezionare **Nuovo**.</span><span class="sxs-lookup"><span data-stu-id="04cd3-112">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE[model 2](../../includes/RP/model2.md)]
[!INCLUDE[model 2a](../../includes/RP/model2a.md)]

<span data-ttu-id="04cd3-113">[!code-csharp[Principale](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-6)]</span><span class="sxs-lookup"><span data-stu-id="04cd3-113">[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-6)]</span></span>

<span data-ttu-id="04cd3-114">Fare clic con il pulsante destro del mouse su una riga rossa ondulata, ad esempio `MovieContext` nella riga `services.AddDbContext<MovieContext>(options =>`.</span><span class="sxs-lookup"><span data-stu-id="04cd3-114">Right click on a red squiggly line, for example `MovieContext` in the line `services.AddDbContext<MovieContext>(options =>`.</span></span> <span data-ttu-id="04cd3-115">Selezionare **Quick Fix > using RazorPagesMovie.Models;** (Correzione rapida > tramite RazorPagesMovie.Models;).</span><span class="sxs-lookup"><span data-stu-id="04cd3-115">Select **Quick Fix > using RazorPagesMovie.Models;**.</span></span> <span data-ttu-id="04cd3-116">Visual studio aggiunge l'istruzione using.</span><span class="sxs-lookup"><span data-stu-id="04cd3-116">Visual studio adds the using statement.</span></span>

<span data-ttu-id="04cd3-117">Compilare il progetto per verificare che non ci siano errori.</span><span class="sxs-lookup"><span data-stu-id="04cd3-117">Build the project to verify you don't have any errors.</span></span>

![Pagina Crea](model/red.png)

### <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="04cd3-119">Pacchetti NuGet di Entity Framework Core per le migrazioni</span><span class="sxs-lookup"><span data-stu-id="04cd3-119">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="04cd3-120">Gli strumenti di Entity Framework per l'interfaccia della riga di comando (CLI) sono disponibili in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span><span class="sxs-lookup"><span data-stu-id="04cd3-120">The EF tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="04cd3-121">Per installare questo pacchetto, aggiungerlo alla collezione `DotNetCliToolReference` nel file *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="04cd3-121">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file.</span></span> <span data-ttu-id="04cd3-122">**Nota:** è necessario installare questo pacchetto modificando il file *.csproj* ; non è possibile utilizzare il comando `install-package` o la GUI di gestione di pacchetti.</span><span class="sxs-lookup"><span data-stu-id="04cd3-122">**Note:** You have to install this package by editing the *.csproj* file; you can't use the `install-package` command or the package manager GUI.</span></span>

<span data-ttu-id="04cd3-123">Per modificare un file *.csproj*:</span><span class="sxs-lookup"><span data-stu-id="04cd3-123">To edit a *.csproj* file:</span></span>

* <span data-ttu-id="04cd3-124">Selezionare **File > Apri**, quindi selezionare il file *csproj*.</span><span class="sxs-lookup"><span data-stu-id="04cd3-124">Select **File > Open**, and then select the *.csproj* file.</span></span>
* <span data-ttu-id="04cd3-125">Selezionare **Opzioni**.</span><span class="sxs-lookup"><span data-stu-id="04cd3-125">Select **Options**.</span></span>
* <span data-ttu-id="04cd3-126">Modificare **Apri con** in **Editor codice sorgente**.</span><span class="sxs-lookup"><span data-stu-id="04cd3-126">Change **Open with** to **Source Code Editor**.</span></span>

![Modificare il file csproj](model/csproj.png)

<span data-ttu-id="04cd3-128">Il codice seguente illustra il file *csproj* aggiornato.</span><span class="sxs-lookup"><span data-stu-id="04cd3-128">The following code shows the updated *csproj* file.</span></span>

[!code-xml[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/RazorPagesMovie.cli.csproj?highlight=10)]

[!INCLUDE[model3](../../includes/RP/model3.md)]
[!INCLUDE[model 4x](../../includes/RP/model4x.md)]

[!INCLUDE[model 4 exit](../../includes/RP/model4exit.md)]

[!INCLUDE[model 4](../../includes/RP/model4.md)]

### <a name="add-the-pagesmovies-files-to-the-project"></a><span data-ttu-id="04cd3-129">Aggiungere i file di pagine/filmati al progetto</span><span class="sxs-lookup"><span data-stu-id="04cd3-129">Add the Pages/Movies files to the project</span></span>

* <span data-ttu-id="04cd3-130">In Visual Studio, fare clic con il pulsante destro del mouse sulla cartella *Pagine* e selezionare **Aggiungi > Aggiungi cartella esistente**.</span><span class="sxs-lookup"><span data-stu-id="04cd3-130">In Visual Studio, Right-click the *Pages* folder and select **Add > Add existing Folder**.</span></span>
* <span data-ttu-id="04cd3-131">Selezionare la cartella *Filmati*.</span><span class="sxs-lookup"><span data-stu-id="04cd3-131">Select the *Movies* folder.</span></span>
* <span data-ttu-id="04cd3-132">Nella finestra di dialogo *Scegli i file da includere nel progetto*, selezionare **Includi tutto**.</span><span class="sxs-lookup"><span data-stu-id="04cd3-132">In the *Chosse files to include in the project* dialog, select **Include All**.</span></span>

<span data-ttu-id="04cd3-133">L'esercitazione successiva illustra i file creati tramite scaffolding.</span><span class="sxs-lookup"><span data-stu-id="04cd3-133">The next tutorial explains the files created by scaffolding.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="04cd3-134">[Indietro: Introduzione](xref:tutorials/razor-pages-mac/razor-pages-start)
[Avanti: pagine Razor create tramite scaffolding](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="04cd3-134">[Previous: Getting Started](xref:tutorials/razor-pages-mac/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>
