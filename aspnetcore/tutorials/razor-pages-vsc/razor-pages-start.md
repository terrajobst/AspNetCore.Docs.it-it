---
title: Introduzione a pagine Razor ASP.NET Core in Visual Studio Code
author: rick-anderson
description: Informazioni di base sulla compilazione di un'app Web pagine Razor ASP.NET Core con Visual Studio Code.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/27/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages-vsc/razor-pages-start
ms.openlocfilehash: 17ab52b80a40f6204e2bf2cf9048071c55c0a708
ms.sourcegitcommit: 63fb07fb3f71b32daf2c9466e132f2e7cc617163
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/10/2018
ms.locfileid: "35252217"
---
# <a name="get-started-with-aspnet-core-razor-pages-in-visual-studio-code"></a><span data-ttu-id="84e1f-103">Introduzione a pagine Razor ASP.NET Core in Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="84e1f-103">Get started with ASP.NET Core Razor Pages in Visual Studio Code</span></span>

<span data-ttu-id="84e1f-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="84e1f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="84e1f-105">Questa esercitazione illustra le nozioni di base della creazione di un'app Web pagine Razor di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="84e1f-105">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="84e1f-106">Si consiglia di leggere [Introduzione all'uso di pagine Razor](xref:mvc/razor-pages/index) prima di iniziare questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="84e1f-106">We recommend you complete [Introduction to Razor Pages](xref:mvc/razor-pages/index) before starting this tutorial.</span></span> <span data-ttu-id="84e1f-107">Pagine Razor è il modo consigliato per creare l'interfaccia utente per applicazioni Web in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="84e1f-107">Razor Pages is the recommended way to build UI for web applications in ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="84e1f-108">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="84e1f-108">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs-vscode.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="84e1f-109">Creare un'app Web Razor</span><span class="sxs-lookup"><span data-stu-id="84e1f-109">Create a Razor web app</span></span>

<span data-ttu-id="84e1f-110">Da un terminale eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="84e1f-110">From a terminal, run the following commands:</span></span>

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new webapp -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

::: moniker-end

<span data-ttu-id="84e1f-112">I comandi precedenti usano [.NET Core CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet) per creare ed eseguire un progetto di pagine Razor.</span><span class="sxs-lookup"><span data-stu-id="84e1f-112">The preceding commands use the [.NET Core CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet) to create and run a Razor Pages project.</span></span> <span data-ttu-id="84e1f-113">Aprire un browser all'indirizzo http://localhost:5000 per visualizzare l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="84e1f-113">Open a browser to http://localhost:5000 to view the application.</span></span>

![Pagina Home o di indice](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE [razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a><span data-ttu-id="84e1f-115">Aprire il progetto</span><span class="sxs-lookup"><span data-stu-id="84e1f-115">Open the project</span></span>

<span data-ttu-id="84e1f-116">Premere Ctrl + C per arrestare l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="84e1f-116">Press Ctrl+C to shut down the application.</span></span>

<span data-ttu-id="84e1f-117">Da Visual Studio Code (VS Code), selezionare **File > Apri cartella**, quindi selezionare la cartella *RazorPagesMovie*.</span><span class="sxs-lookup"><span data-stu-id="84e1f-117">From Visual Studio Code (VS Code), select **File > Open Folder**, and then select the *RazorPagesMovie* folder.</span></span>

- <span data-ttu-id="84e1f-118">Selezionare **Yes** (Sì) nel messaggio di **avviso** indicante che le risorse necessarie per la compilazione e il debug mancano in "RazorPagesMovie"</span><span class="sxs-lookup"><span data-stu-id="84e1f-118">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'RazorPagesMovie'.</span></span> <span data-ttu-id="84e1f-119">e se si vuole aggiungerle.</span><span class="sxs-lookup"><span data-stu-id="84e1f-119">Add them?"</span></span>
- <span data-ttu-id="84e1f-120">Selezionare **Ripristina** per il messaggio di **informazione** "Dipendenze non risolte".</span><span class="sxs-lookup"><span data-stu-id="84e1f-120">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="84e1f-121">Avviare l'app</span><span class="sxs-lookup"><span data-stu-id="84e1f-121">Launch the app</span></span>

<span data-ttu-id="84e1f-122">Premere CTRL+F5 per avviare l'app senza debug.</span><span class="sxs-lookup"><span data-stu-id="84e1f-122">Press Ctrl+F5 to start the app without debugging.</span></span> <span data-ttu-id="84e1f-123">In alternativa, scegliere **Avvia senza eseguire debug** dal menu **Debug**.</span><span class="sxs-lookup"><span data-stu-id="84e1f-123">Alternatively, from the **Debug** menu, select **Start Without Debugging**.</span></span>

<span data-ttu-id="84e1f-124">Nella prossima esercitazione viene aggiunto un modello al progetto.</span><span class="sxs-lookup"><span data-stu-id="84e1f-124">In the next tutorial, we add a model to the project.</span></span> 

> [!div class="step-by-step"]
> [<span data-ttu-id="84e1f-125">Avanti: Aggiunta di un modello</span><span class="sxs-lookup"><span data-stu-id="84e1f-125">Next: Adding a model</span></span>](xref:tutorials/razor-pages-vsc/model)  
