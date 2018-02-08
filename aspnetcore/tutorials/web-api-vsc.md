---
title: Creare un'API Web con ASP.NET Core e Visual Studio Code
author: rick-anderson
description: Compilare un'API Web in macOS, Linux o Windows con ASP.NET Core MVC e Visual Studio Code
manager: wpickett
ms.author: riande
ms.date: 09/22/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/web-api-vsc
ms.openlocfilehash: 44566c4014400aa2ca3d512eeaa226637b5f0b97
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2018
---
# <a name="create-a-web-api-with-aspnet-core-mvc-and-visual-studio-code-on-linux-macos-and-windows"></a><span data-ttu-id="78fd5-103">Creare un'API Web con ASP.NET Core MVC e Visual Studio Code in Linux, macOS e Windows</span><span class="sxs-lookup"><span data-stu-id="78fd5-103">Create a Web API with ASP.NET Core MVC and Visual Studio Code on Linux, macOS, and Windows</span></span>

<span data-ttu-id="78fd5-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT) e [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="78fd5-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="78fd5-105">Questa esercitazione consente di creare un'API Web per la gestione di un elenco di elementi di tipo "attività".</span><span class="sxs-lookup"><span data-stu-id="78fd5-105">In this tutorial, build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="78fd5-106">Non è stata costruita un'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="78fd5-106">A UI isn't constructed.</span></span>

<span data-ttu-id="78fd5-107">Sono disponibili 3 versioni dell'esercitazione:</span><span class="sxs-lookup"><span data-stu-id="78fd5-107">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="78fd5-108">macOS, Linux, Windows: API Web con Visual Studio Code (questa esercitazione)</span><span class="sxs-lookup"><span data-stu-id="78fd5-108">macOS, Linux, Windows: Web API with Visual Studio Code (This tutorial)</span></span>
* <span data-ttu-id="78fd5-109">macOS: [API Web con Visual Studio per Mac](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="78fd5-109">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="78fd5-110">Windows: [API Web con Visual Studio per Windows](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="78fd5-110">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

## <a name="set-up-your-development-environment"></a><span data-ttu-id="78fd5-111">Configurare l'ambiente di sviluppo</span><span class="sxs-lookup"><span data-stu-id="78fd5-111">Set up your development environment</span></span>

<span data-ttu-id="78fd5-112">Scaricare e installare:</span><span class="sxs-lookup"><span data-stu-id="78fd5-112">Download and install:</span></span>
- <span data-ttu-id="78fd5-113">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="78fd5-113">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span>
- [<span data-ttu-id="78fd5-114">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="78fd5-114">Visual Studio Code</span></span>](https://code.visualstudio.com)
- <span data-ttu-id="78fd5-115">Visual Studio Code [estensione C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span><span class="sxs-lookup"><span data-stu-id="78fd5-115">Visual Studio Code [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span></span>

## <a name="create-the-project"></a><span data-ttu-id="78fd5-116">Creare il progetto</span><span class="sxs-lookup"><span data-stu-id="78fd5-116">Create the project</span></span>

<span data-ttu-id="78fd5-117">Da una console eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="78fd5-117">From a console, run the following commands:</span></span>

```console
mkdir TodoApi
cd TodoApi
dotnet new webapi
```

<span data-ttu-id="78fd5-118">Aprire la cartella *TodoApi* in Visual Studio Code (VS Code) e selezionare il file *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="78fd5-118">Open the *TodoApi* folder in Visual Studio Code (VS Code) and select the *Startup.cs* file.</span></span>

- <span data-ttu-id="78fd5-119">Selezionare **Yes** (Sì) nel messaggio di **avviso** indicante che le risorse necessarie per la compilazione e il debug mancano nella directory "TodoApi"</span><span class="sxs-lookup"><span data-stu-id="78fd5-119">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'TodoApi'.</span></span> <span data-ttu-id="78fd5-120">e se si vuole aggiungerle.</span><span class="sxs-lookup"><span data-stu-id="78fd5-120">Add them?"</span></span>
- <span data-ttu-id="78fd5-121">Selezionare **Ripristina** per il messaggio di **informazione** "Dipendenze non risolte".</span><span class="sxs-lookup"><span data-stu-id="78fd5-121">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

<!-- uid: tutorials/first-mvc-app-xplat/start-mvc uses the pic below. If you change it, make sure it's consistent -->

![VS Code con avviso indicante che le risorse necessarie per la compilazione e il debug mancano nella directory "TodoApi"](web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="78fd5-125">Premere **Debug** (F5) per compilare ed eseguire il programma.</span><span class="sxs-lookup"><span data-stu-id="78fd5-125">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="78fd5-126">In un browser passare a http://localhost:5000/api/values.</span><span class="sxs-lookup"><span data-stu-id="78fd5-126">In a browser navigate to http://localhost:5000/api/values .</span></span> <span data-ttu-id="78fd5-127">Verrà visualizzato quanto segue:</span><span class="sxs-lookup"><span data-stu-id="78fd5-127">The following is displayed:</span></span>

`["value1","value2"]`

<span data-ttu-id="78fd5-128">Vedere la [Guida di Visual Studio Code](#visual-studio-code-help) per suggerimenti sull'uso di Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="78fd5-128">See [Visual Studio Code help](#visual-studio-code-help) for tips on using VS Code.</span></span>

## <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="78fd5-129">Aggiungere il supporto per Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="78fd5-129">Add support for Entity Framework Core</span></span>

<span data-ttu-id="78fd5-130">La creazione di un nuovo progetto in .NET Core 2.0 aggiunge il provider 'Microsoft.AspNetCore.All' nel file *TodoApi.csproj*.</span><span class="sxs-lookup"><span data-stu-id="78fd5-130">Creating a new project in .NET Core 2.0 adds the 'Microsoft.AspNetCore.All' provider in the *TodoApi.csproj* file.</span></span> <span data-ttu-id="78fd5-131">Non occorre installare il provider di database [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) separatamente.</span><span class="sxs-lookup"><span data-stu-id="78fd5-131">There's no need to install the [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) database provider separately.</span></span> <span data-ttu-id="78fd5-132">Questo provider di database consente l'uso di Entity Framework Core con un database in memoria.</span><span class="sxs-lookup"><span data-stu-id="78fd5-132">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

[!code-xml[Main](web-api-vsc/sample/TodoApi/TodoApi.csproj?highlight=12)]

## <a name="add-a-model-class"></a><span data-ttu-id="78fd5-133">Aggiungere una classe del modello</span><span class="sxs-lookup"><span data-stu-id="78fd5-133">Add a model class</span></span>

<span data-ttu-id="78fd5-134">Un modello è un oggetto che rappresenta i dati nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="78fd5-134">A model is an object that represents the data in your application.</span></span> <span data-ttu-id="78fd5-135">In questo caso l'unico modello è un elemento attività.</span><span class="sxs-lookup"><span data-stu-id="78fd5-135">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="78fd5-136">Aggiungere una cartella denominata *Modelli*.</span><span class="sxs-lookup"><span data-stu-id="78fd5-136">Add a folder named *Models*.</span></span> <span data-ttu-id="78fd5-137">È possibile inserire le classi del modello in qualsiasi punto del progetto, ma per convenzione viene usata la cartella *Models*.</span><span class="sxs-lookup"><span data-stu-id="78fd5-137">You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="78fd5-138">Aggiungere una classe `TodoItem` con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="78fd5-138">Add a `TodoItem` class with the following code:</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="78fd5-139">Il database genera `Id` quando viene creato un `TodoItem`.</span><span class="sxs-lookup"><span data-stu-id="78fd5-139">The database generates the `Id` when a `TodoItem` is created.</span></span>

## <a name="create-the-database-context"></a><span data-ttu-id="78fd5-140">Creare il contesto di database</span><span class="sxs-lookup"><span data-stu-id="78fd5-140">Create the database context</span></span>

<span data-ttu-id="78fd5-141">Il *contesto di database* è la classe principale che coordina le funzionalità di Entity Framework per un determinato modello di dati.</span><span class="sxs-lookup"><span data-stu-id="78fd5-141">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="78fd5-142">Questa classe viene creata mediante derivazione dalla classe `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="78fd5-142">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="78fd5-143">Aggiungere una classe `TodoContext` alla cartella *Models*:</span><span class="sxs-lookup"><span data-stu-id="78fd5-143">Add a `TodoContext` class in the *Models* folder:</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="78fd5-144">Aggiungere un controller</span><span class="sxs-lookup"><span data-stu-id="78fd5-144">Add a controller</span></span>

<span data-ttu-id="78fd5-145">Nella cartella *Controllers* creare una classe denominata `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="78fd5-145">In the *Controllers* folder, create a class named `TodoController`.</span></span> <span data-ttu-id="78fd5-146">Aggiungere il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="78fd5-146">Add the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="78fd5-147">Avviare l'app</span><span class="sxs-lookup"><span data-stu-id="78fd5-147">Launch the app</span></span>

<span data-ttu-id="78fd5-148">In Visual Studio Code premere F5 per avviare l'app.</span><span class="sxs-lookup"><span data-stu-id="78fd5-148">In VS Code, press F5 to launch the app.</span></span> <span data-ttu-id="78fd5-149">Passare a http://localhost:5000/api/todo (il controller `Todo` appena creato).</span><span class="sxs-lookup"><span data-stu-id="78fd5-149">Navigate to  http://localhost:5000/api/todo   (The `Todo` controller we just created).</span></span>

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

## <a name="visual-studio-code-help"></a><span data-ttu-id="78fd5-150">Guida di Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="78fd5-150">Visual Studio Code help</span></span>

- [<span data-ttu-id="78fd5-151">Introduzione</span><span class="sxs-lookup"><span data-stu-id="78fd5-151">Getting started</span></span>](https://code.visualstudio.com/docs)
- [<span data-ttu-id="78fd5-152">Debug</span><span class="sxs-lookup"><span data-stu-id="78fd5-152">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
- [<span data-ttu-id="78fd5-153">Terminale integrato</span><span class="sxs-lookup"><span data-stu-id="78fd5-153">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [<span data-ttu-id="78fd5-154">Tasti di scelta rapida</span><span class="sxs-lookup"><span data-stu-id="78fd5-154">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [<span data-ttu-id="78fd5-155">Tasti di scelta rapida di Mac</span><span class="sxs-lookup"><span data-stu-id="78fd5-155">Mac keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [<span data-ttu-id="78fd5-156">Tasti di scelta rapida di Linux</span><span class="sxs-lookup"><span data-stu-id="78fd5-156">Linux keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [<span data-ttu-id="78fd5-157">Tasti di scelta rapida di Windows</span><span class="sxs-lookup"><span data-stu-id="78fd5-157">Windows keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

[!INCLUDE[next steps](../includes/webApi/next.md)]


