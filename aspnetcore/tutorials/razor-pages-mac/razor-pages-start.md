---
title: Introduzione a pagine Razor in ASP.NET Core con macOS con Visual Studio per Mac
author: rick-anderson
description: Informazioni introduttive su pagine Razor in ASP.NET Core con Visual Studio per Mac.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: tutorials/razor-pages-mac/razor-pages-start
ms.openlocfilehash: 8a4cc6c52684558ce9176f98205e439096e88cbf
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/25/2018
ms.locfileid: "50089651"
---
# <a name="get-started-with-razor-pages-in-aspnet-core-on-macos-with-visual-studio-for-mac"></a><span data-ttu-id="7a59e-103">Introduzione a pagine Razor in ASP.NET Core con macOS con Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="7a59e-103">Get started with Razor Pages in ASP.NET Core on macOS with Visual Studio for Mac</span></span>

<span data-ttu-id="7a59e-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="7a59e-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="7a59e-105">Questa esercitazione illustra le nozioni di base della creazione di un'app Web pagine Razor di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7a59e-105">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="7a59e-106">Si consiglia di leggere [Introduzione all'uso di pagine Razor](xref:razor-pages/index) prima di iniziare questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="7a59e-106">We recommend you review [Introduction to Razor Pages](xref:razor-pages/index) before starting this tutorial.</span></span> <span data-ttu-id="7a59e-107">Pagine Razor è il modo consigliato per creare l'interfaccia utente per applicazioni Web in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7a59e-107">Razor Pages is the recommended way to build UI for web applications in ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7a59e-108">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="7a59e-108">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs-macos.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="7a59e-109">Creare un'app Web Razor</span><span class="sxs-lookup"><span data-stu-id="7a59e-109">Create a Razor web app</span></span>

<span data-ttu-id="7a59e-110">Da un terminale eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="7a59e-110">From a terminal, run the following commands:</span></span>

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

<span data-ttu-id="7a59e-111">I comandi precedenti usano [.NET Core CLI](/dotnet/core/tools/dotnet) per creare ed eseguire un progetto di pagine Razor.</span><span class="sxs-lookup"><span data-stu-id="7a59e-111">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create and run a Razor Pages project.</span></span> <span data-ttu-id="7a59e-112">Aprire un browser all'indirizzo http://localhost:5000 per visualizzare l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="7a59e-112">Open a browser to http://localhost:5000 to view the application.</span></span>

![Pagina Home o di indice](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE [razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a><span data-ttu-id="7a59e-114">Aprire il progetto</span><span class="sxs-lookup"><span data-stu-id="7a59e-114">Open the project</span></span>

<span data-ttu-id="7a59e-115">Premere Ctrl + C per arrestare l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="7a59e-115">Press Ctrl+C to shut down the application.</span></span>

<span data-ttu-id="7a59e-116">Da Visual Studio, selezionare **File > Apri**, quindi selezionare il file *RazorPagesMovie.csproj*.</span><span class="sxs-lookup"><span data-stu-id="7a59e-116">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="7a59e-117">Avviare l'app</span><span class="sxs-lookup"><span data-stu-id="7a59e-117">Launch the app</span></span>

<span data-ttu-id="7a59e-118">In Visual Studio selezionare **Esegui > Avvia senza eseguire debug** per avviare l'app.</span><span class="sxs-lookup"><span data-stu-id="7a59e-118">In Visual Studio, select **Run > Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="7a59e-119">Visual Studio avvia [Kestrel](xref:fundamentals/servers/kestrel), avvia un browser e passa a `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="7a59e-119">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5000`.</span></span>

<span data-ttu-id="7a59e-120">Nella prossima esercitazione viene aggiunto un modello al progetto.</span><span class="sxs-lookup"><span data-stu-id="7a59e-120">In the next tutorial, we add a model to the project.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="7a59e-121">Avanti: Aggiunta di un modello</span><span class="sxs-lookup"><span data-stu-id="7a59e-121">Next: Adding a model</span></span>](xref:tutorials/razor-pages-mac/model)
