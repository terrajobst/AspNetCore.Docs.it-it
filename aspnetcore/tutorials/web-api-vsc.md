---
title: Creare un'API Web con ASP.NET Core e Visual Studio Code
author: rick-anderson
description: Compilare un'API Web in macOS, Linux o Windows con ASP.NET Core MVC e Visual Studio Code
keywords: ASP.NET Core, WebAPI, Web API, REST, Mac, Linux, HTTP, servizio, servizio HTTP, VS Code
ms.author: riande
manager: wpickett
ms.date: 5/24/2017
ms.topic: get-started-article
ms.assetid: 830b4bf5-dd14-423e-9f59-764a6f13a8f6
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/web-api-vsc
ms.openlocfilehash: 17687e38aae066bdab4663268a2af54f20a6ad75
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/12/2017
---
# <a name="create-a-web-api-with-aspnet-core-mvc-and-visual-studio-code-on-linux-macos-and-windows"></a><span data-ttu-id="c5bc1-104">Creare un'API Web con ASP.NET Core MVC e Visual Studio Code in Linux, macOS e Windows</span><span class="sxs-lookup"><span data-stu-id="c5bc1-104">Create a Web API with ASP.NET Core MVC and Visual Studio Code on Linux, macOS, and Windows</span></span>

<span data-ttu-id="c5bc1-105">Di [Rick Anderson](https://twitter.com/RickAndMSFT) e [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="c5bc1-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="c5bc1-106">In questa esercitazione si creerà un'API Web per la gestione di un elenco di elementi di tipo "attività"</span><span class="sxs-lookup"><span data-stu-id="c5bc1-106">In this tutorial, you’ll build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="c5bc1-107">e non un'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="c5bc1-107">You won’t build a UI.</span></span>

<span data-ttu-id="c5bc1-108">Sono disponibili 3 versioni dell'esercitazione:</span><span class="sxs-lookup"><span data-stu-id="c5bc1-108">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="c5bc1-109">macOS, Linux, Windows: API Web con Visual Studio Code (questa esercitazione)</span><span class="sxs-lookup"><span data-stu-id="c5bc1-109">macOS, Linux, Windows: Web API with Visual Studio Code (This tutorial)</span></span>
* <span data-ttu-id="c5bc1-110">macOS: [API Web con Visual Studio per Mac](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="c5bc1-110">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="c5bc1-111">Windows: [API Web con Visual Studio per Windows](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="c5bc1-111">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

## <a name="set-up-your-development-environment"></a><span data-ttu-id="c5bc1-112">Configurare l'ambiente di sviluppo</span><span class="sxs-lookup"><span data-stu-id="c5bc1-112">Set up your development environment</span></span>

<span data-ttu-id="c5bc1-113">Scaricare e installare:</span><span class="sxs-lookup"><span data-stu-id="c5bc1-113">Download and install:</span></span>
- [<span data-ttu-id="c5bc1-114">.NET Core</span><span class="sxs-lookup"><span data-stu-id="c5bc1-114">.NET Core</span></span>](https://www.microsoft.com/net/core)
- [<span data-ttu-id="c5bc1-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c5bc1-115">Visual Studio Code</span></span>](https://code.visualstudio.com)
- <span data-ttu-id="c5bc1-116">Visual Studio Code [estensione C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span><span class="sxs-lookup"><span data-stu-id="c5bc1-116">Visual Studio Code [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span></span>

## <a name="create-the-project"></a><span data-ttu-id="c5bc1-117">Creare il progetto</span><span class="sxs-lookup"><span data-stu-id="c5bc1-117">Create the project</span></span>

<span data-ttu-id="c5bc1-118">Da una console eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="c5bc1-118">From a console, run the following commands:</span></span>

```console
mkdir TodoApi
cd TodoApi
dotnet new webapi
```

<span data-ttu-id="c5bc1-119">Aprire la cartella *TodoApi* in Visual Studio Code (VS Code) e selezionare il file *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="c5bc1-119">Open the *TodoApi* folder in Visual Studio Code (VS Code) and select the *Startup.cs* file.</span></span>

- <span data-ttu-id="c5bc1-120">Selezionare **Yes** (Sì) nel messaggio di **avviso** indicante che le risorse necessarie per la compilazione e il debug mancano nella directory "TodoApi"</span><span class="sxs-lookup"><span data-stu-id="c5bc1-120">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'TodoApi'.</span></span> <span data-ttu-id="c5bc1-121">e se si vuole aggiungerle.</span><span class="sxs-lookup"><span data-stu-id="c5bc1-121">Add them?"</span></span>
- <span data-ttu-id="c5bc1-122">Selezionare **Ripristina** per il messaggio di **informazione** "Dipendenze non risolte".</span><span class="sxs-lookup"><span data-stu-id="c5bc1-122">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

<!-- uid: tutorials/first-mvc-app-xplat/start-mvc uses the pic below. If you change it, make sure it's consistent -->

![VS Code con avviso indicante che le risorse necessarie per la compilazione e il debug mancano nella directory "TodoApi"](web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="c5bc1-126">Premere **Debug** (F5) per compilare ed eseguire il programma.</span><span class="sxs-lookup"><span data-stu-id="c5bc1-126">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="c5bc1-127">In un browser passare a http://localhost:5000/api/values.</span><span class="sxs-lookup"><span data-stu-id="c5bc1-127">In a browser navigate to http://localhost:5000/api/values .</span></span> <span data-ttu-id="c5bc1-128">Verrà visualizzato quanto segue:</span><span class="sxs-lookup"><span data-stu-id="c5bc1-128">The following is displayed:</span></span>

`["value1","value2"]`

<span data-ttu-id="c5bc1-129">Vedere la [Guida di Visual Studio Code](#visual-studio-code-help) per suggerimenti sull'uso di Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="c5bc1-129">See [Visual Studio Code help](#visual-studio-code-help) for tips on using VS Code.</span></span>

## <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="c5bc1-130">Aggiungere il supporto per Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="c5bc1-130">Add support for Entity Framework Core</span></span>

<span data-ttu-id="c5bc1-131">Modificare il file *TodoApi.csproj* per installare il provider di database [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/).</span><span class="sxs-lookup"><span data-stu-id="c5bc1-131">Edit the *TodoApi.csproj* file to install the [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) database provider.</span></span> <span data-ttu-id="c5bc1-132">Questo provider di database consente l'uso di Entity Framework Core con un database in memoria.</span><span class="sxs-lookup"><span data-stu-id="c5bc1-132">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

<span data-ttu-id="c5bc1-133">[!code-xml[Principale](web-api-vsc/sample/TodoApi/TodoApi.csproj?highlight=12)]</span><span class="sxs-lookup"><span data-stu-id="c5bc1-133">[!code-xml[Main](web-api-vsc/sample/TodoApi/TodoApi.csproj?highlight=12)]</span></span>

<span data-ttu-id="c5bc1-134">Eseguire `dotnet restore` per scaricare e installare il provider di database in memoria di Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="c5bc1-134">Run `dotnet restore` to download and install the EF Core InMemory DB provider.</span></span> <span data-ttu-id="c5bc1-135">È possibile eseguire `dotnet restore` dal terminale o immettere `⌘⇧P` (macOS) o `Ctrl+Shift+P` (Linux) in Visual Studio Code e quindi digitare **.NET**.</span><span class="sxs-lookup"><span data-stu-id="c5bc1-135">You can run `dotnet restore` from the terminal or enter `⌘⇧P` (macOS) or `Ctrl+Shift+P` (Linux) in VS Code and then type **.NET**.</span></span> <span data-ttu-id="c5bc1-136">Selezionare **.NET: Ripristina pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="c5bc1-136">Select **.NET: Restore Packages**.</span></span>

## <a name="add-a-model-class"></a><span data-ttu-id="c5bc1-137">Aggiungere una classe del modello</span><span class="sxs-lookup"><span data-stu-id="c5bc1-137">Add a model class</span></span>

<span data-ttu-id="c5bc1-138">Un modello è un oggetto che rappresenta i dati nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="c5bc1-138">A model is an object that represents the data in your application.</span></span> <span data-ttu-id="c5bc1-139">In questo caso l'unico modello è un elemento attività.</span><span class="sxs-lookup"><span data-stu-id="c5bc1-139">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="c5bc1-140">Aggiungere una cartella denominata *Models*.</span><span class="sxs-lookup"><span data-stu-id="c5bc1-140">Add a folder named *Models*.</span></span> <span data-ttu-id="c5bc1-141">È possibile inserire le classi del modello in qualsiasi punto del progetto, ma per convenzione viene usata la cartella *Models*.</span><span class="sxs-lookup"><span data-stu-id="c5bc1-141">You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="c5bc1-142">Aggiungere una classe `TodoItem` con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="c5bc1-142">Add a `TodoItem` class with the following code:</span></span>

<span data-ttu-id="c5bc1-143">[!code-csharp[Principale](first-web-api/sample/TodoApi/Models/TodoItem.cs)]</span><span class="sxs-lookup"><span data-stu-id="c5bc1-143">[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]</span></span>

<span data-ttu-id="c5bc1-144">Il database genera `Id` quando viene creato un `TodoItem`.</span><span class="sxs-lookup"><span data-stu-id="c5bc1-144">The database generates the `Id` when a `TodoItem` is created.</span></span>

## <a name="create-the-database-context"></a><span data-ttu-id="c5bc1-145">Creare il contesto di database</span><span class="sxs-lookup"><span data-stu-id="c5bc1-145">Create the database context</span></span>

<span data-ttu-id="c5bc1-146">Il *contesto di database* è la classe principale che coordina le funzionalità di Entity Framework per un determinato modello di dati.</span><span class="sxs-lookup"><span data-stu-id="c5bc1-146">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="c5bc1-147">Questa classe viene creata mediante derivazione dalla classe `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="c5bc1-147">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="c5bc1-148">Aggiungere una classe `TodoContext` alla cartella *Models*:</span><span class="sxs-lookup"><span data-stu-id="c5bc1-148">Add a `TodoContext` class in the *Models* folder:</span></span>

<span data-ttu-id="c5bc1-149">[!code-csharp[Principale](first-web-api/sample/TodoApi/Models/TodoContext.cs)]</span><span class="sxs-lookup"><span data-stu-id="c5bc1-149">[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]</span></span>

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="c5bc1-150">Aggiungere un controller</span><span class="sxs-lookup"><span data-stu-id="c5bc1-150">Add a controller</span></span>

<span data-ttu-id="c5bc1-151">Nella cartella *Controllers* creare una classe denominata `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="c5bc1-151">In the *Controllers* folder, create a class named `TodoController`.</span></span> <span data-ttu-id="c5bc1-152">Aggiungere il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="c5bc1-152">Add the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="c5bc1-153">Avviare l'app</span><span class="sxs-lookup"><span data-stu-id="c5bc1-153">Launch the app</span></span>

<span data-ttu-id="c5bc1-154">In Visual Studio Code premere F5 per avviare l'app.</span><span class="sxs-lookup"><span data-stu-id="c5bc1-154">In VS Code, press F5 to launch the app.</span></span> <span data-ttu-id="c5bc1-155">Passare a http://localhost:5000/api/todo (il controller `Todo` appena creato).</span><span class="sxs-lookup"><span data-stu-id="c5bc1-155">Navigate to  http://localhost:5000/api/todo   (The `Todo` controller we just created).</span></span>

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

## <a name="visual-studio-code-help"></a><span data-ttu-id="c5bc1-156">Guida di Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="c5bc1-156">Visual Studio Code help</span></span>

- [<span data-ttu-id="c5bc1-157">Introduzione</span><span class="sxs-lookup"><span data-stu-id="c5bc1-157">Getting started</span></span>](https://code.visualstudio.com/docs)
- [<span data-ttu-id="c5bc1-158">Debug</span><span class="sxs-lookup"><span data-stu-id="c5bc1-158">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
- [<span data-ttu-id="c5bc1-159">Terminale integrato</span><span class="sxs-lookup"><span data-stu-id="c5bc1-159">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [<span data-ttu-id="c5bc1-160">Tasti di scelta rapida</span><span class="sxs-lookup"><span data-stu-id="c5bc1-160">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [<span data-ttu-id="c5bc1-161">Tasti di scelta rapida di Mac</span><span class="sxs-lookup"><span data-stu-id="c5bc1-161">Mac keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [<span data-ttu-id="c5bc1-162">Tasti di scelta rapida di Linux</span><span class="sxs-lookup"><span data-stu-id="c5bc1-162">Linux keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [<span data-ttu-id="c5bc1-163">Tasti di scelta rapida di Windows</span><span class="sxs-lookup"><span data-stu-id="c5bc1-163">Windows keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

[!INCLUDE[next steps](../includes/webApi/next.md)]


