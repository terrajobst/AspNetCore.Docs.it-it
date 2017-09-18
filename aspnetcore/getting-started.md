---
title: Introduzione ad ASP.NET Core 2.0
author: rick-anderson
description: Un'esercitazione rapida per creare ed eseguire una semplice app Hello World usando ASP.NET Core.
keywords: ASP.NET Core, esercitazione, introduzione
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: get-started-article
ms.assetid: 73543e9d-d9d5-47d6-9664-17a9beea6cd3
ms.technology: aspnet
ms.prod: asp.net-core
uid: getting-started
ms.openlocfilehash: 3399df3958093da9117b013736b1cb370fd6beb2
ms.sourcegitcommit: 297ee5d2f3b3b24eb8a2c4a25195c9e2973cb91b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/14/2017
---
# <a name="getting-started-with-aspnet-core"></a><span data-ttu-id="b523f-104">Introduzione ad ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b523f-104">Getting Started with ASP.NET Core</span></span>

> [!NOTE]
> <span data-ttu-id="b523f-105">Queste istruzioni sono relative alla versione più recente di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b523f-105">These instructions are for the latest version of ASP.NET Core.</span></span> <span data-ttu-id="b523f-106">Per iniziare con una versione precedente,</span><span class="sxs-lookup"><span data-stu-id="b523f-106">Looking to get started with an earlier version?</span></span> <span data-ttu-id="b523f-107">vedere la [versione 1.1 di questa esercitazione](xref:getting-started-1.1).</span><span class="sxs-lookup"><span data-stu-id="b523f-107">See [the 1.1 version of this tutorial](xref:getting-started-1.1).</span></span>

1. <span data-ttu-id="b523f-108">Installare [.NET Core](https://microsoft.com/net/core/).</span><span class="sxs-lookup"><span data-stu-id="b523f-108">Install [.NET Core](https://microsoft.com/net/core/).</span></span>

2. <span data-ttu-id="b523f-109">Creare un nuovo progetto .NET Core.</span><span class="sxs-lookup"><span data-stu-id="b523f-109">Create a new .NET Core project.</span></span>

   <span data-ttu-id="b523f-110">In macOS e Linux aprire una finestra del terminale.</span><span class="sxs-lookup"><span data-stu-id="b523f-110">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="b523f-111">In Windows aprire un prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="b523f-111">On Windows, open a command prompt.</span></span>

   ```terminal
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   dotnet new web
   ```
    
4. <span data-ttu-id="b523f-112">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="b523f-112">Run the app.</span></span>

   <span data-ttu-id="b523f-113">Il comando `dotnet run` compila prima l'app se necessario.</span><span class="sxs-lookup"><span data-stu-id="b523f-113">The `dotnet run` command builds the app first if needed.</span></span>

   ```terminal
   dotnet run
   ```

5. <span data-ttu-id="b523f-114">Passare a `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="b523f-114">Browse to `http://localhost:5000`</span></span>

### <a name="next-steps"></a><span data-ttu-id="b523f-115">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="b523f-115">Next steps</span></span>

<span data-ttu-id="b523f-116">Per le esercitazioni, introduttive, vedere [Esercitazioni di ASP.NET Core](tutorials/index.md)</span><span class="sxs-lookup"><span data-stu-id="b523f-116">For getting-started tutorials, see [ASP.NET Core Tutorials](tutorials/index.md)</span></span>

<span data-ttu-id="b523f-117">Per un'introduzione ai concetti e all'architettura di ASP.NET Core, vedere [Introduzione ad ASP.NET Core](index.md) e [Nozioni fondamentali di ASP.NET Core](fundamentals/index.md).</span><span class="sxs-lookup"><span data-stu-id="b523f-117">For an introduction to ASP.NET Core concepts and architecture, see [ASP.NET Core Introduction](index.md) and [ASP.NET Core Fundamentals](fundamentals/index.md).</span></span>

<span data-ttu-id="b523f-118">Un'app ASP.NET Core può usare la libreria di classi base e il runtime di .NET Framework o .NET Core.</span><span class="sxs-lookup"><span data-stu-id="b523f-118">An ASP.NET Core app can use the .NET Core or .NET Framework Base Class Library and runtime.</span></span> <span data-ttu-id="b523f-119">Per altre informazioni, vedere [Scelta di .NET Core o .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="b523f-119">For more information, see [Choosing between .NET Core and .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>
