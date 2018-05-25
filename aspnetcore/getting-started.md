---
title: Introduzione ad ASP.NET Core
author: rick-anderson
description: Un'esercitazione rapida per creare ed eseguire una semplice app Hello World usando ASP.NET Core.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/10/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: getting-started
ms.openlocfilehash: e814277663ff5a964171a71ebb6e0f094e0ddc60
ms.sourcegitcommit: 3d071fabaf90e32906df97b08a8d00e602db25c0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/10/2018
---
# <a name="get-started-with-aspnet-core"></a><span data-ttu-id="1fcc9-103">Introduzione ad ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1fcc9-103">Get started with ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-2.0"

1. <span data-ttu-id="1fcc9-104">Installare [!INCLUDE[](~/includes/net-core-sdk-download-link.md)].</span><span class="sxs-lookup"><span data-stu-id="1fcc9-104">Install the [!INCLUDE[](~/includes/net-core-sdk-download-link.md)].</span></span>

2. <span data-ttu-id="1fcc9-105">Creare un nuovo progetto .NET Core.</span><span class="sxs-lookup"><span data-stu-id="1fcc9-105">Create a new .NET Core project.</span></span>

   <span data-ttu-id="1fcc9-106">In macOS e Linux aprire una finestra del terminale.</span><span class="sxs-lookup"><span data-stu-id="1fcc9-106">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="1fcc9-107">In Windows aprire un prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="1fcc9-107">On Windows, open a command prompt.</span></span> <span data-ttu-id="1fcc9-108">Immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="1fcc9-108">Enter the following command:</span></span>

    ```terminal
    dotnet new razor -o aspnetcoreapp
    ```

3. <span data-ttu-id="1fcc9-109">Eseguire l'app con i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="1fcc9-109">Run the app with the following commands:</span></span>

    ```terminal
    cd aspnetcoreapp
    dotnet run
    ```

4. <span data-ttu-id="1fcc9-110">Passare a [http://localhost:5000](http://localhost:5000).</span><span class="sxs-lookup"><span data-stu-id="1fcc9-110">Browse to [http://localhost:5000](http://localhost:5000).</span></span>

5. <span data-ttu-id="1fcc9-111">Aprire *Pages/About.cshtml* e modificare la pagina in modo che visualizzi il messaggio "Hello, world!</span><span class="sxs-lookup"><span data-stu-id="1fcc9-111">Open *Pages/About.cshtml* and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="1fcc9-112">The time on the server is@DateTime.Now" (Buongiorno mondo! L'ora nel server è):</span><span class="sxs-lookup"><span data-stu-id="1fcc9-112">The time on the server is @DateTime.Now":</span></span>

    <span data-ttu-id="1fcc9-113">[!code-cshtml[](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]</span><span class="sxs-lookup"><span data-stu-id="1fcc9-113">[!code-cshtml[](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]</span></span>

6. <span data-ttu-id="1fcc9-114">Passare a [http://localhost:5000/About](http://localhost:5000/About) e verificare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="1fcc9-114">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]
::: moniker-end

::: moniker range="<= aspnetcore-1.1"

1. <span data-ttu-id="1fcc9-115">Installare il **programma di installazione di SDK** per SDK 1.0.4 dalla [pagina di tutti i download di .NET Core](https://www.microsoft.com/net/download/all).</span><span class="sxs-lookup"><span data-stu-id="1fcc9-115">Install the .NET Core **SDK Installer** for SDK 1.0.4 from the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>

2. <span data-ttu-id="1fcc9-116">Creare una cartella per un nuovo progetto .NET Core.</span><span class="sxs-lookup"><span data-stu-id="1fcc9-116">Create a folder for a new .NET Core project.</span></span>

   <span data-ttu-id="1fcc9-117">In macOS e Linux aprire una finestra del terminale.</span><span class="sxs-lookup"><span data-stu-id="1fcc9-117">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="1fcc9-118">In Windows aprire un prompt dei comandi.</span><span class="sxs-lookup"><span data-stu-id="1fcc9-118">On Windows, open a command prompt.</span></span>

   ```terminal
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

3. <span data-ttu-id="1fcc9-119">Se è installata una versione SDK successiva nel computer in uso, creare un file *global.json* per selezionare l'SDK 1.0.4.</span><span class="sxs-lookup"><span data-stu-id="1fcc9-119">If you have installed a later SDK version on your machine, create a *global.json* file to select the 1.0.4 SDK.</span></span>

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

4. <span data-ttu-id="1fcc9-120">Creare un nuovo progetto .NET Core.</span><span class="sxs-lookup"><span data-stu-id="1fcc9-120">Create a new .NET Core project.</span></span>

   ```terminal
   dotnet new web
   ```

5. <span data-ttu-id="1fcc9-121">Ripristinare i pacchetti.</span><span class="sxs-lookup"><span data-stu-id="1fcc9-121">Restore the packages.</span></span>

    ```terminal
    dotnet restore
    ```

6. <span data-ttu-id="1fcc9-122">Eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="1fcc9-122">Run the app.</span></span>

   ```terminal
   dotnet run
   ```

   <span data-ttu-id="1fcc9-123">Il comando [dotnet run](/dotnet/core/tools/dotnet-run) compila prima l'app se necessario.</span><span class="sxs-lookup"><span data-stu-id="1fcc9-123">The [dotnet run](/dotnet/core/tools/dotnet-run) command builds the app first, if needed.</span></span>

7. <span data-ttu-id="1fcc9-124">Passare a `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="1fcc9-124">Browse to `http://localhost:5000`.</span></span>

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]
::: moniker-end