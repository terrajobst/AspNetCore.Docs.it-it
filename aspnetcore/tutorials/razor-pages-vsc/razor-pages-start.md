---
title: Introduzione a pagine Razor ASP.NET Core in Visual Studio Code
author: rick-anderson
description: Informazioni di base sulla compilazione di un'app Web pagine Razor ASP.NET Core con Visual Studio Code.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: tutorials/razor-pages-vsc/razor-pages-start
ms.openlocfilehash: f18d0a8b3ce24c9844b02f8a0b6360f7e1b1bdb7
ms.sourcegitcommit: c43a6f1fe72d7c2db4b5815fd532f2b45d964e07
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/30/2018
ms.locfileid: "50244853"
---
# <a name="get-started-with-aspnet-core-razor-pages-in-visual-studio-code"></a><span data-ttu-id="ce436-103">Introduzione a pagine Razor ASP.NET Core in Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ce436-103">Get started with ASP.NET Core Razor Pages in Visual Studio Code</span></span>

<span data-ttu-id="ce436-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ce436-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="ce436-105">Questa esercitazione illustra le nozioni di base della creazione di un'app Web pagine Razor di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ce436-105">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="ce436-106">Si consiglia di leggere [Introduzione all'uso di pagine Razor](xref:razor-pages/index) prima di iniziare questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="ce436-106">We recommend you complete [Introduction to Razor Pages](xref:razor-pages/index) before starting this tutorial.</span></span> <span data-ttu-id="ce436-107">Pagine Razor è il modo consigliato per creare l'interfaccia utente per applicazioni Web in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ce436-107">Razor Pages is the recommended way to build UI for web applications in ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ce436-108">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="ce436-108">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs-vscode.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="ce436-109">Creare un'app Web Razor</span><span class="sxs-lookup"><span data-stu-id="ce436-109">Create a Razor web app</span></span>

<span data-ttu-id="ce436-110">Da un terminale eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="ce436-110">From a terminal, run the following commands:</span></span>

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new webapp -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

::: moniker-end

<span data-ttu-id="ce436-111">I comandi precedenti usano [.NET Core CLI](/dotnet/core/tools/dotnet) per creare ed eseguire un progetto di pagine Razor.</span><span class="sxs-lookup"><span data-stu-id="ce436-111">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create and run a Razor Pages project.</span></span> <span data-ttu-id="ce436-112">Aprire un browser all'indirizzo http://localhost:5000 per visualizzare l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="ce436-112">Open a browser to http://localhost:5000 to view the application.</span></span>

![Pagina Home o di indice](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE [razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a><span data-ttu-id="ce436-114">Aprire il progetto</span><span class="sxs-lookup"><span data-stu-id="ce436-114">Open the project</span></span>

<span data-ttu-id="ce436-115">Premere Ctrl + C per arrestare l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="ce436-115">Press Ctrl+C to shut down the application.</span></span>

<span data-ttu-id="ce436-116">Da Visual Studio Code (VS Code), selezionare **File > Apri cartella**, quindi selezionare la cartella *RazorPagesMovie*.</span><span class="sxs-lookup"><span data-stu-id="ce436-116">From Visual Studio Code (VS Code), select **File > Open Folder**, and then select the *RazorPagesMovie* folder.</span></span>

- <span data-ttu-id="ce436-117">Selezionare **Yes** (Sì) nel messaggio di **avviso** indicante che le risorse necessarie per la compilazione e il debug mancano in "RazorPagesMovie"</span><span class="sxs-lookup"><span data-stu-id="ce436-117">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'RazorPagesMovie'.</span></span> <span data-ttu-id="ce436-118">e se si vuole aggiungerle.</span><span class="sxs-lookup"><span data-stu-id="ce436-118">Add them?"</span></span>
- <span data-ttu-id="ce436-119">Selezionare **Ripristina** per il messaggio di **informazione** "Dipendenze non risolte".</span><span class="sxs-lookup"><span data-stu-id="ce436-119">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="ce436-120">Avviare l'app</span><span class="sxs-lookup"><span data-stu-id="ce436-120">Launch the app</span></span>

<span data-ttu-id="ce436-121">Scegliere **Avvia senza eseguire debug** dal menu **Debug**.</span><span class="sxs-lookup"><span data-stu-id="ce436-121">From the **Debug** menu, select **Start Without Debugging**.</span></span> <span data-ttu-id="ce436-122">In alternativa, è possibile premere il tasto di scelta rapida visualizzato accanto all'opzione di menu.</span><span class="sxs-lookup"><span data-stu-id="ce436-122">Alternatively, you can press the keyboard shortcut displayed next to the menu option.</span></span> <span data-ttu-id="ce436-123">Il tasto di scelta rapida varia a seconda del sistema operativo.</span><span class="sxs-lookup"><span data-stu-id="ce436-123">This shortcut varies depending on your operating system.</span></span>

<span data-ttu-id="ce436-124">Nella prossima esercitazione viene aggiunto un modello al progetto.</span><span class="sxs-lookup"><span data-stu-id="ce436-124">In the next tutorial, we add a model to the project.</span></span> 

> [!div class="step-by-step"]
> [<span data-ttu-id="ce436-125">Avanti: Aggiunta di un modello</span><span class="sxs-lookup"><span data-stu-id="ce436-125">Next: Adding a model</span></span>](xref:tutorials/razor-pages-vsc/model)  
