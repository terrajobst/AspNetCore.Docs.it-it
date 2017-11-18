---
title: Introduzione ad ASP.NET Core 1.1
author: rick-anderson
description: Un'esercitazione rapida per creare ed eseguire una semplice app Hello World usando ASP.NET Core 1.1.
keywords: ASP.NET Core, esercitazione, introduzione
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: get-started-article
ms.assetid: 73543e9d-d9d5-47d6-9664-17a9beea6cd3
ms.technology: aspnet
ms.prod: asp.net-core
uid: getting-started-1.1
ms.openlocfilehash: e8fd9ef60ebc1cff6ca0e03000ea50eebff0a9f9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
# <a name="getting-started-with-aspnet-core-11"></a><span data-ttu-id="bdb44-104">Introduzione ad ASP.NET Core 1.1</span><span class="sxs-lookup"><span data-stu-id="bdb44-104">Getting Started with ASP.NET Core 1.1</span></span>

> [!NOTE]
> <span data-ttu-id="bdb44-105">Queste istruzioni sono relative ad ASP.NET Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="bdb44-105">These instructions are for ASP.NET Core 1.1.</span></span> <span data-ttu-id="bdb44-106">Per cercare la versione più recente,</span><span class="sxs-lookup"><span data-stu-id="bdb44-106">Looking for the latest version?</span></span> <span data-ttu-id="bdb44-107">vedere la [versione corrente di questa esercitazione](xref:getting-started).</span><span class="sxs-lookup"><span data-stu-id="bdb44-107">See [the current version of this tutorial](xref:getting-started).</span></span>

1. <span data-ttu-id="bdb44-108">Installare il **programma di installazione SDK** di .NET Core per SDK 1.0.4 dalla [pagina dei download di .NET Core 1.0.5 e 1.1.2 con SDK 1.0.4](https://github.com/dotnet/core/blob/master/release-notes/download-archives/1.0.5-download.md).</span><span class="sxs-lookup"><span data-stu-id="bdb44-108">Install the .NET Core **SDK Installer** for SDK 1.0.4 from the [.NET Core 1.0.5 & 1.1.2 SDK 1.0.4 downloads page](https://github.com/dotnet/core/blob/master/release-notes/download-archives/1.0.5-download.md).</span></span>

2. <span data-ttu-id="bdb44-109">Creare una cartella per un nuovo progetto .NET Core.</span><span class="sxs-lookup"><span data-stu-id="bdb44-109">Create a folder for a new .NET Core project.</span></span>

   <span data-ttu-id="bdb44-110">In macOS e Linux aprire una finestra del terminale.</span><span class="sxs-lookup"><span data-stu-id="bdb44-110">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="bdb44-111">In Windows aprire un prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="bdb44-111">On Windows, open a command prompt.</span></span>

   ```terminal
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

2. <span data-ttu-id="bdb44-112">Se è installata una versione SDK successiva nel computer in uso, creare un file *global.json* per selezionare l'SDK 1.0.4.</span><span class="sxs-lookup"><span data-stu-id="bdb44-112">If you have installed a later SDK version on your machine, create a *global.json* file to select the 1.0.4 SDK.</span></span>

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

2. <span data-ttu-id="bdb44-113">Creare un nuovo progetto .NET Core.</span><span class="sxs-lookup"><span data-stu-id="bdb44-113">Create a new .NET Core project.</span></span>

   ```terminal
   dotnet new web
   ```
   
3.  <span data-ttu-id="bdb44-114">Ripristinare i pacchetti.</span><span class="sxs-lookup"><span data-stu-id="bdb44-114">Restore the packages.</span></span>

    ```terminal
    dotnet restore
    ```

4. <span data-ttu-id="bdb44-115">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="bdb44-115">Run the app.</span></span>

   <span data-ttu-id="bdb44-116">Il comando `dotnet run` compila prima l'app se necessario.</span><span class="sxs-lookup"><span data-stu-id="bdb44-116">The `dotnet run` command builds the app first if needed.</span></span>

   ```terminal
   dotnet run
   ```

5. <span data-ttu-id="bdb44-117">Passare a `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="bdb44-117">Browse to `http://localhost:5000`</span></span>

<!-- H3 to avoid a single-entry internal TOC -->
### <a name="next-steps"></a><span data-ttu-id="bdb44-118">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="bdb44-118">Next steps</span></span>

<span data-ttu-id="bdb44-119">Per le esercitazioni, introduttive, vedere [Esercitazioni di ASP.NET Core](tutorials/index.md)</span><span class="sxs-lookup"><span data-stu-id="bdb44-119">For getting-started tutorials, see [ASP.NET Core Tutorials](tutorials/index.md)</span></span>

<span data-ttu-id="bdb44-120">Per un'introduzione ai concetti e all'architettura di ASP.NET Core, vedere [Introduzione ad ASP.NET Core](index.md) e [Nozioni fondamentali di ASP.NET Core](fundamentals/index.md).</span><span class="sxs-lookup"><span data-stu-id="bdb44-120">For an introduction to ASP.NET Core concepts and architecture, see [ASP.NET Core Introduction](index.md) and [ASP.NET Core Fundamentals](fundamentals/index.md).</span></span>

<span data-ttu-id="bdb44-121">Un'app ASP.NET Core può usare la libreria di classi base e il runtime di .NET Framework o .NET Core.</span><span class="sxs-lookup"><span data-stu-id="bdb44-121">An ASP.NET Core app can use the .NET Core or .NET Framework Base Class Library and runtime.</span></span> <span data-ttu-id="bdb44-122">Per altre informazioni, vedere [Scelta di .NET Core o .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="bdb44-122">For more information, see [Choosing between .NET Core and .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>
