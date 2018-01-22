---
title: Introduzione ad ASP.NET Core 2.0
author: rick-anderson
description: Un'esercitazione rapida per creare ed eseguire una semplice app Hello World usando ASP.NET Core.
ms.author: riande
manager: wpickett
ms.date: 10/18/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: getting-started
ms.openlocfilehash: b5f1fb0de2776177374b8b4d5ea6041b0fc653a9
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/19/2018
---
# <a name="get-started-with-aspnet-core"></a><span data-ttu-id="387b3-103">Introduzione ad ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="387b3-103">Get Started with ASP.NET Core</span></span>

> [!NOTE]
> <span data-ttu-id="387b3-104">Queste istruzioni sono relative alla versione più recente di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="387b3-104">These instructions are for the latest version of ASP.NET Core.</span></span> <span data-ttu-id="387b3-105">Per iniziare con una versione precedente,</span><span class="sxs-lookup"><span data-stu-id="387b3-105">Looking to get started with an earlier version?</span></span> <span data-ttu-id="387b3-106">vedere la [versione 1.1 di questa esercitazione](xref:getting-started-1.1).</span><span class="sxs-lookup"><span data-stu-id="387b3-106">See [the 1.1 version of this tutorial](xref:getting-started-1.1).</span></span>

1. <span data-ttu-id="387b3-107">Installare [.NET Core](https://www.microsoft.com/net/core/).</span><span class="sxs-lookup"><span data-stu-id="387b3-107">Install [.NET Core](https://www.microsoft.com/net/core/).</span></span>

2. <span data-ttu-id="387b3-108">Creare un nuovo progetto .NET Core.</span><span class="sxs-lookup"><span data-stu-id="387b3-108">Create a new .NET Core project.</span></span>

   <span data-ttu-id="387b3-109">In macOS e Linux aprire una finestra del terminale.</span><span class="sxs-lookup"><span data-stu-id="387b3-109">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="387b3-110">In Windows aprire un prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="387b3-110">On Windows, open a command prompt.</span></span> <span data-ttu-id="387b3-111">Immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="387b3-111">Enter the following command:</span></span>

    ```terminal
    dotnet new razor -o aspnetcoreapp
    ```
    
4. <span data-ttu-id="387b3-112">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="387b3-112">Run the app.</span></span>

    <span data-ttu-id="387b3-113">Usare i comandi seguenti per eseguire l'app:</span><span class="sxs-lookup"><span data-stu-id="387b3-113">Use the following commands to run the app:</span></span>

    ```terminal
    cd aspnetcoreapp
    dotnet run
    ```

5. <span data-ttu-id="387b3-114">Passare a [http://localhost:5000](http://localhost:5000)</span><span class="sxs-lookup"><span data-stu-id="387b3-114">Browse to [http://localhost:5000](http://localhost:5000)</span></span>

6. <span data-ttu-id="387b3-115">Aprire *Pages/About.cshtml* e modificare la pagina in modo che visualizzi il messaggio "Hello, world!</span><span class="sxs-lookup"><span data-stu-id="387b3-115">Open *Pages/About.cshtml* and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="387b3-116">The time on the server is @DateTime.Now " (Buongiorno mondo! L'ora nel server è):</span><span class="sxs-lookup"><span data-stu-id="387b3-116">The time on the server is @DateTime.Now ":</span></span>

    [!code-html[Main](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]

7. <span data-ttu-id="387b3-117">Passare a [http://localhost:5000/About](http://localhost:5000/About) e verificare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="387b3-117">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

### <a name="next-steps"></a><span data-ttu-id="387b3-118">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="387b3-118">Next steps</span></span>

<span data-ttu-id="387b3-119">Per le esercitazioni, introduttive, vedere [Esercitazioni di ASP.NET Core](tutorials/index.md)</span><span class="sxs-lookup"><span data-stu-id="387b3-119">For getting-started tutorials, see [ASP.NET Core Tutorials](tutorials/index.md)</span></span>

<span data-ttu-id="387b3-120">Per un'introduzione ai concetti e all'architettura di ASP.NET Core, vedere [Introduzione ad ASP.NET Core](index.md) e [Nozioni fondamentali di ASP.NET Core](fundamentals/index.md).</span><span class="sxs-lookup"><span data-stu-id="387b3-120">For an introduction to ASP.NET Core concepts and architecture, see [ASP.NET Core Introduction](index.md) and [ASP.NET Core Fundamentals](fundamentals/index.md).</span></span>

<span data-ttu-id="387b3-121">Un'app ASP.NET Core può usare la libreria di classi base e il runtime di .NET Framework o .NET Core.</span><span class="sxs-lookup"><span data-stu-id="387b3-121">An ASP.NET Core app can use the .NET Core or .NET Framework Base Class Library and runtime.</span></span> <span data-ttu-id="387b3-122">Per altre informazioni, vedere [Scelta di .NET Core o .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="387b3-122">For more information, see [Choosing between .NET Core and .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>
