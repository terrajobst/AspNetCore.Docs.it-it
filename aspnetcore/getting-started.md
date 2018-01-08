---
title: Introduzione ad ASP.NET Core 2.0
author: rick-anderson
description: Un'esercitazione rapida per creare ed eseguire una semplice app Hello World usando ASP.NET Core.
keywords: ASP.NET Core, esercitazione, introduzione
ms.author: riande
manager: wpickett
ms.date: 10/18/2017
ms.topic: get-started-article
ms.assetid: 73543e9d-d9d5-47d6-9664-17a9beea6cd3
ms.technology: aspnet
ms.prod: asp.net-core
uid: getting-started
ms.openlocfilehash: 5b8c9f770e749c13bc562f157b4ebfd25a88a4e0
ms.sourcegitcommit: 019e5a0342fd49a94056d14fc7a1a1d0f81d2a39
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/22/2017
---
# <a name="get-started-with-aspnet-core"></a><span data-ttu-id="2b746-104">Introduzione ad ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2b746-104">Get Started with ASP.NET Core</span></span>

> [!NOTE]
> <span data-ttu-id="2b746-105">Queste istruzioni sono relative alla versione più recente di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2b746-105">These instructions are for the latest version of ASP.NET Core.</span></span> <span data-ttu-id="2b746-106">Per iniziare con una versione precedente,</span><span class="sxs-lookup"><span data-stu-id="2b746-106">Looking to get started with an earlier version?</span></span> <span data-ttu-id="2b746-107">vedere la [versione 1.1 di questa esercitazione](xref:getting-started-1.1).</span><span class="sxs-lookup"><span data-stu-id="2b746-107">See [the 1.1 version of this tutorial](xref:getting-started-1.1).</span></span>

1. <span data-ttu-id="2b746-108">Installare [.NET Core](https://www.microsoft.com/net/core/).</span><span class="sxs-lookup"><span data-stu-id="2b746-108">Install [.NET Core](https://www.microsoft.com/net/core/).</span></span>

2. <span data-ttu-id="2b746-109">Creare un nuovo progetto .NET Core.</span><span class="sxs-lookup"><span data-stu-id="2b746-109">Create a new .NET Core project.</span></span>

   <span data-ttu-id="2b746-110">In macOS e Linux aprire una finestra del terminale.</span><span class="sxs-lookup"><span data-stu-id="2b746-110">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="2b746-111">In Windows aprire un prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="2b746-111">On Windows, open a command prompt.</span></span> <span data-ttu-id="2b746-112">Immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="2b746-112">Enter the following command:</span></span>

    ```terminal
    dotnet new razor -o aspnetcoreapp
    ```
    
4. <span data-ttu-id="2b746-113">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="2b746-113">Run the app.</span></span>

    <span data-ttu-id="2b746-114">Usare i comandi seguenti per eseguire l'app:</span><span class="sxs-lookup"><span data-stu-id="2b746-114">Use the following commands to run the app:</span></span>

    ```terminal
    cd aspnetcoreapp
    dotnet run
    ```

5. <span data-ttu-id="2b746-115">Passare a [http://localhost:5000](http://localhost:5000)</span><span class="sxs-lookup"><span data-stu-id="2b746-115">Browse to [http://localhost:5000](http://localhost:5000)</span></span>

6. <span data-ttu-id="2b746-116">Aprire *Pages/About.cshtml* e modificare la pagina in modo che visualizzi il messaggio "Hello, world!</span><span class="sxs-lookup"><span data-stu-id="2b746-116">Open *Pages/About.cshtml* and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="2b746-117">The time on the server is @DateTime.Now " (Buongiorno mondo! L'ora nel server è):</span><span class="sxs-lookup"><span data-stu-id="2b746-117">The time on the server is @DateTime.Now ":</span></span>

    [!code-html[Main](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]

7. <span data-ttu-id="2b746-118">Passare a [http://localhost:5000/About](http://localhost:5000/About) e verificare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="2b746-118">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

### <a name="next-steps"></a><span data-ttu-id="2b746-119">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="2b746-119">Next steps</span></span>

<span data-ttu-id="2b746-120">Per le esercitazioni, introduttive, vedere [Esercitazioni di ASP.NET Core](tutorials/index.md)</span><span class="sxs-lookup"><span data-stu-id="2b746-120">For getting-started tutorials, see [ASP.NET Core Tutorials](tutorials/index.md)</span></span>

<span data-ttu-id="2b746-121">Per un'introduzione ai concetti e all'architettura di ASP.NET Core, vedere [Introduzione ad ASP.NET Core](index.md) e [Nozioni fondamentali di ASP.NET Core](fundamentals/index.md).</span><span class="sxs-lookup"><span data-stu-id="2b746-121">For an introduction to ASP.NET Core concepts and architecture, see [ASP.NET Core Introduction](index.md) and [ASP.NET Core Fundamentals](fundamentals/index.md).</span></span>

<span data-ttu-id="2b746-122">Un'app ASP.NET Core può usare la libreria di classi base e il runtime di .NET Framework o .NET Core.</span><span class="sxs-lookup"><span data-stu-id="2b746-122">An ASP.NET Core app can use the .NET Core or .NET Framework Base Class Library and runtime.</span></span> <span data-ttu-id="2b746-123">Per altre informazioni, vedere [Scelta di .NET Core o .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="2b746-123">For more information, see [Choosing between .NET Core and .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>
