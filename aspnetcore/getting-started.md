---
title: Introduzione ad ASP.NET Core
author: rick-anderson
description: Un'esercitazione rapida per creare ed eseguire una semplice app Hello World usando ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 10/18/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: getting-started
ms.openlocfilehash: c2f18c69901a5a6503314d508a776e6985872681
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
# <a name="get-started-with-aspnet-core"></a><span data-ttu-id="0b73d-103">Introduzione ad ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0b73d-103">Get Started with ASP.NET Core</span></span>

> [!NOTE]
> <span data-ttu-id="0b73d-104">Queste istruzioni sono relative alla versione più recente di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0b73d-104">These instructions are for the latest version of ASP.NET Core.</span></span> <span data-ttu-id="0b73d-105">Vedere [Introduzione ad ASP.NET Core 1.1](xref:getting-started-1.1) per la versione 1.1 di questo documento.</span><span class="sxs-lookup"><span data-stu-id="0b73d-105">See [Getting Started with ASP.NET Core 1.1](xref:getting-started-1.1) for the 1.1 version of this document.</span></span>

1. <span data-ttu-id="0b73d-106">Installare [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span><span class="sxs-lookup"><span data-stu-id="0b73d-106">Install the [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span></span>

2. <span data-ttu-id="0b73d-107">Creare un nuovo progetto .NET Core.</span><span class="sxs-lookup"><span data-stu-id="0b73d-107">Create a new .NET Core project.</span></span>

   <span data-ttu-id="0b73d-108">In macOS e Linux aprire una finestra del terminale.</span><span class="sxs-lookup"><span data-stu-id="0b73d-108">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="0b73d-109">In Windows aprire un prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="0b73d-109">On Windows, open a command prompt.</span></span> <span data-ttu-id="0b73d-110">Immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="0b73d-110">Enter the following command:</span></span>

    ```terminal
    dotnet new razor -o aspnetcoreapp
    ```
    
3. <span data-ttu-id="0b73d-111">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="0b73d-111">Run the app.</span></span>

    <span data-ttu-id="0b73d-112">Usare i comandi seguenti per eseguire l'app:</span><span class="sxs-lookup"><span data-stu-id="0b73d-112">Use the following commands to run the app:</span></span>

    ```terminal
    cd aspnetcoreapp
    dotnet run
    ```

4. <span data-ttu-id="0b73d-113">Passare a [http://localhost:5000](http://localhost:5000)</span><span class="sxs-lookup"><span data-stu-id="0b73d-113">Browse to [http://localhost:5000](http://localhost:5000)</span></span>

5. <span data-ttu-id="0b73d-114">Aprire <em>Pages/About.cshtml</em> e modificare la pagina in modo che visualizzi il messaggio "Hello, world!</span><span class="sxs-lookup"><span data-stu-id="0b73d-114">Open <em>Pages/About.cshtml</em> and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="0b73d-115">The time on the server is @DateTime.Now " (Buongiorno mondo! L'ora nel server è):</span><span class="sxs-lookup"><span data-stu-id="0b73d-115">The time on the server is @DateTime.Now ":</span></span>

    [!code-html[](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]

6. <span data-ttu-id="0b73d-116">Passare a [http://localhost:5000/About](http://localhost:5000/About) e verificare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="0b73d-116">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

### <a name="next-steps"></a><span data-ttu-id="0b73d-117">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="0b73d-117">Next steps</span></span>

<span data-ttu-id="0b73d-118">Per le esercitazioni, introduttive, vedere [Esercitazioni di ASP.NET Core](tutorials/index.md)</span><span class="sxs-lookup"><span data-stu-id="0b73d-118">For getting-started tutorials, see [ASP.NET Core Tutorials](tutorials/index.md)</span></span>

<span data-ttu-id="0b73d-119">Per un'introduzione ai concetti e all'architettura di ASP.NET Core, vedere [Introduzione ad ASP.NET Core](index.md) e [Nozioni fondamentali di ASP.NET Core](fundamentals/index.md).</span><span class="sxs-lookup"><span data-stu-id="0b73d-119">For an introduction to ASP.NET Core concepts and architecture, see [ASP.NET Core Introduction](index.md) and [ASP.NET Core Fundamentals](fundamentals/index.md).</span></span>

<span data-ttu-id="0b73d-120">Un'app ASP.NET Core può usare la libreria di classi base e il runtime di .NET Framework o .NET Core.</span><span class="sxs-lookup"><span data-stu-id="0b73d-120">An ASP.NET Core app can use the .NET Core or .NET Framework Base Class Library and runtime.</span></span> <span data-ttu-id="0b73d-121">Per altre informazioni, vedere [Scelta di .NET Core o .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="0b73d-121">For more information, see [Choosing between .NET Core and .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>
