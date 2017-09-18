---
title: Introduzione all'uso di pagine Razor in ASP.NET Core su Mac
author: rick-anderson
description: Introduzione all'uso di pagine Razor in ASP.NET Core con Visual Studio per Mac
keywords: ASP.NET Core, pagine Razor, scaffolding, Entity Framework Core, EF, EF Core, database, mac, macOS, Visual Studio per Mac
ms.author: riande
manager: wpickett
ms.date: 7/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages-mac/razor-pages-start
ms.openlocfilehash: 56ff18589d189b0d2760c761c58b5b030d02940b
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/12/2017
---
# <a name="getting-started-with-razor-pages-in-aspnet-core-with-visual-studio-for-mac"></a><span data-ttu-id="8cc93-104">Introduzione all'uso di pagine Razor in ASP.NET Core con Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="8cc93-104">Getting started with Razor Pages in ASP.NET Core with Visual Studio for Mac</span></span>

<span data-ttu-id="8cc93-105">Di [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="8cc93-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="8cc93-106">Questa esercitazione illustra le nozioni di base della creazione di un'app Web pagine Razor di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8cc93-106">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="8cc93-107">Si consiglia di leggere [Introduzione all'uso di pagine Razor](xref:mvc/razor-pages/index) prima di iniziare questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="8cc93-107">We recommend you complete [Introduction to Razor Pages](xref:mvc/razor-pages/index) before starting this tutorial.</span></span> <span data-ttu-id="8cc93-108">Pagine Razor è il modo consigliato per creare l'interfaccia utente per applicazioni Web in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8cc93-108">Razor Pages is the recommended way to build UI for web applications in ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8cc93-109">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="8cc93-109">Prerequisites</span></span>

<span data-ttu-id="8cc93-110">Installare gli elementi seguenti:</span><span class="sxs-lookup"><span data-stu-id="8cc93-110">Install the following:</span></span>

* <span data-ttu-id="8cc93-111">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) o versione successiva</span><span class="sxs-lookup"><span data-stu-id="8cc93-111">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later</span></span>
* [<span data-ttu-id="8cc93-112">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="8cc93-112">Visual Studio for Mac</span></span>](https://www.visualstudio.com/vs/visual-studio-mac/)

## <a name="create-a-razor-web-app"></a><span data-ttu-id="8cc93-113">Creare un'app Web Razor</span><span class="sxs-lookup"><span data-stu-id="8cc93-113">Create a Razor web app</span></span>

<span data-ttu-id="8cc93-114">Da un terminale eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="8cc93-114">From a terminal, run the following commands:</span></span>

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

<span data-ttu-id="8cc93-115">I comandi precedenti usano [.NET Core CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet) per creare ed eseguire un progetto di pagine Razor.</span><span class="sxs-lookup"><span data-stu-id="8cc93-115">The preceding commands use the [.NET Core CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet) to create and run a Razor Pages project.</span></span> <span data-ttu-id="8cc93-116">Aprire un browser all'indirizzo http://localhost:5000/ per visualizzare l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="8cc93-116">Open a browser to http://localhost:5000 to view the application.</span></span>

![Pagina Home o di indice](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE[razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a><span data-ttu-id="8cc93-118">Aprire il progetto</span><span class="sxs-lookup"><span data-stu-id="8cc93-118">Open the project</span></span>

<span data-ttu-id="8cc93-119">Premere Ctrl + C per arrestare l'applicazione.</span><span class="sxs-lookup"><span data-stu-id="8cc93-119">Press Ctrl+C to shut down the application.</span></span>

<span data-ttu-id="8cc93-120">Da Visual Studio, selezionare **File > Apri**, quindi selezionare il file *RazorPagesMovie.csproj*.</span><span class="sxs-lookup"><span data-stu-id="8cc93-120">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="8cc93-121">Avviare l'app</span><span class="sxs-lookup"><span data-stu-id="8cc93-121">Launch the app</span></span>

<span data-ttu-id="8cc93-122">In Visual Studio selezionare **Esegui > Avvia senza eseguire debug** per avviare l'app.</span><span class="sxs-lookup"><span data-stu-id="8cc93-122">In Visual Studio, select **Run > Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="8cc93-123">Visual Studio avvia [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview), avvia un browser e passa a `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="8cc93-123">Visual Studio starts [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview), launches a browser, and navigates to `http://localhost:5000`.</span></span>

<span data-ttu-id="8cc93-124">Nella prossima esercitazione viene aggiunto un modello al progetto.</span><span class="sxs-lookup"><span data-stu-id="8cc93-124">In the next tutorial, we add a model to the project.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="8cc93-125">Avanti: Aggiunta di un modello</span><span class="sxs-lookup"><span data-stu-id="8cc93-125">Next: Adding a model</span></span>](xref:tutorials/razor-pages-mac/model)
