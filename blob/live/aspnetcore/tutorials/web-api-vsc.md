---
title: Creare un'API Web con ASP.NET Core e Visual Studio Code
description: Compilare un'API Web in macOS, Linux o Windows con ASP.NET Core MVC e Visual Studio Code
author: rick-anderson
ms.author: riande
ms.date: 09/22/2017
ms.topic: get-started-article
ms.prod: asp.net-core
ms.technology: aspnet
keywords: ASP.NET Core, WebAPI, Web API, REST, Mac, Linux, HTTP, Service, HTTP Service,VS Code
manager: wpickett
ms.assetid: 830b4bf5-dd14-423e-9f59-764a6f13a8f6
uid: tutorials/web-api-vsc
ms.openlocfilehash: 40f9259101e5d006378562a27e97948641e29450
ms.sourcegitcommit: 281f0c614543a6c3db565ea4655b70fe49b61d84
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/03/2018
---
# <a name="create-a-web-api-with-aspnet-core-mvc-and-visual-studio-code-on-linux-macos-and-windows"></a><span data-ttu-id="7b381-104">Creare un'API Web con ASP.NET Core MVC e Visual Studio Code in Linux, macOS e Windows</span><span class="sxs-lookup"><span data-stu-id="7b381-104">Create a Web API with ASP.NET Core MVC and Visual Studio Code on Linux, macOS, and Windows</span></span>

<span data-ttu-id="7b381-105">Di [Rick Anderson](https://twitter.com/RickAndMSFT) e [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="7b381-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="7b381-106">In questa esercitazione si creerà un'API Web per la gestione di un elenco di elementi di tipo "attività"</span><span class="sxs-lookup"><span data-stu-id="7b381-106">In this tutorial, you’ll build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="7b381-107">e non un'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="7b381-107">You won’t build a UI.</span></span>

<span data-ttu-id="7b381-108">Sono disponibili 3 versioni dell'esercitazione:</span><span class="sxs-lookup"><span data-stu-id="7b381-108">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="7b381-109">macOS, Linux, Windows: API Web con Visual Studio Code (questa esercitazione)</span><span class="sxs-lookup"><span data-stu-id="7b381-109">macOS, Linux, Windows: Web API with Visual Studio Code (This tutorial)</span></span>
* <span data-ttu-id="7b381-110">macOS: [API Web con Visual Studio per Mac](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="7b381-110">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="7b381-111">Windows: [API Web con Visual Studio per Windows](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="7b381-111">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

## <a name="set-up-your-development-environment"></a><span data-ttu-id="7b381-112">Configurare l'ambiente di sviluppo</span><span class="sxs-lookup"><span data-stu-id="7b381-112">Set up your development environment</span></span>

<span data-ttu-id="7b381-113">Scaricare e installare:</span><span class="sxs-lookup"><span data-stu-id="7b381-113">Download and install:</span></span>
- <span data-ttu-id="7b381-114">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) o versione successiva.</span><span class="sxs-lookup"><span data-stu-id="7b381-114">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later.</span></span>
- [<span data-ttu-id="7b381-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="7b381-115">Visual Studio Code</span></span>](https://code.visualstudio.com)
- <span data-ttu-id="7b381-116">Visual Studio Code [estensione C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span><span class="sxs-lookup"><span data-stu-id="7b381-116">Visual Studio Code [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)</span></span>

## <a name="create-the-project"></a><span data-ttu-id="7b381-117">Creare il progetto</span><span class="sxs-lookup"><span data-stu-id="7b381-117">Create the project</span></span>

<span data-ttu-id="7b381-118">Da una console eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="7b381-118">From a console, run the following commands:</span></span>

```console
mkdir TodoApi
cd TodoApi
dotnet new webapi
```

<span data-ttu-id="7b381-119">Aprire la cartella *TodoApi* in Visual Studio Code (VS Code) e selezionare il file *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="7b381-119">Open the *TodoApi* folder in Visual Studio Code (VS Code) and select the *Startup.cs* file.</span></span>

- <span data-ttu-id="7b381-120">Selezionare **Yes** (Sì) nel messaggio di **avviso** indicante che le risorse necessarie per la compilazione e il debug mancano nella directory "TodoApi"</span><span class="sxs-lookup"><span data-stu-id="7b381-120">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'TodoApi'.</span></span> <span data-ttu-id="7b381-121">e se si vuole aggiungerle.</span><span class="sxs-lookup"><span data-stu-id="7b381-121">Add them?"</span></span>
- <span data-ttu-id="7b381-122">Selezionare **Ripristina** per il messaggio di **informazione** "Dipendenze non risolte".</span><span class="sxs-lookup"><span data-stu-id="7b381-122">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

<!-- uid: tutorials/first-mvc-app-xplat/start-mvc uses the pic below. If you change it, make sure it's consistent -->

![VS Code con avviso indicante che le risorse necessarie per la compilazione e il debug mancano nella directory "TodoApi"](web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="7b381-126">Premere **Debug** (F5) per compilare ed eseguire il programma.</span><span class="sxs-lookup"><span data-stu-id="7b381-126">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="7b381-127">In un browser passare a http://localhost:5000/api/values.</span><span class="sxs-lookup"><span data-stu-id="7b381-127">In a browser navigate to http://localhost:5000/api/values .</span></span> <span data-ttu-id="7b381-128">Verrà visualizzato quanto segue:</span><span class="sxs-lookup"><span data-stu-id="7b381-128">The following is displayed:</span></span>

`["value1","value2"]`

<span data-ttu-id="7b381-129">Vedere la [Guida di Visual Studio Code](#visual-studio-code-help) per suggerimenti sull'uso di Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="7b381-129">See [Visual Studio Code help](#visual-studio-code-help) for tips on using VS Code.</span></span>

## <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="7b381-130">Aggiungere il supporto per Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="7b381-130">Add support for Entity Framework Core</span></span>

<span data-ttu-id="7b381-131">La creazione di un nuovo progetto in .NET Core 2.0 aggiunge il provider 'Microsoft.AspNetCore.All' nel file *TodoApi.csproj*.</span><span class="sxs-lookup"><span data-stu-id="7b381-131">Creating a new project in .NET Core 2.0 adds the 'Microsoft.AspNetCore.All' provider in the *TodoApi.csproj* file.</span></span> <span data-ttu-id="7b381-132">Non occorre installare il provider di database [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) separatamente.</span><span class="sxs-lookup"><span data-stu-id="7b381-132">There is no need to install the [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) database provider separately.</span></span> <span data-ttu-id="7b381-133">Questo provider di database consente l'uso di Entity Framework Core con un database in memoria.</span><span class="sxs-lookup"><span data-stu-id="7b381-133">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

[!code-xml[Main](web-api-vsc/sample/TodoApi/TodoApi.csproj?highlight=12)]

## <a name="add-a-model-class"></a><span data-ttu-id="7b381-134">Aggiungere una classe del modello</span><span class="sxs-lookup"><span data-stu-id="7b381-134">Add a model class</span></span>

<span data-ttu-id="7b381-135">Un modello è un oggetto che rappresenta i dati nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="7b381-135">A model is an object that represents the data in your application.</span></span> <span data-ttu-id="7b381-136">In questo caso l'unico modello è un elemento attività.</span><span class="sxs-lookup"><span data-stu-id="7b381-136">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="7b381-137">Aggiungere una cartella denominata *Modelli*.</span><span class="sxs-lookup"><span data-stu-id="7b381-137">Add a folder named *Models*.</span></span> <span data-ttu-id="7b381-138">È possibile inserire le classi del modello in qualsiasi punto del progetto, ma per convenzione viene usata la cartella *Models*.</span><span class="sxs-lookup"><span data-stu-id="7b381-138">You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="7b381-139">Aggiungere una classe `TodoItem` con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="7b381-139">Add a `TodoItem` class with the following code:</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="7b381-140">Il database genera `Id` quando viene creato un `TodoItem`.</span><span class="sxs-lookup"><span data-stu-id="7b381-140">The database generates the `Id` when a `TodoItem` is created.</span></span>

## <a name="create-the-database-context"></a><span data-ttu-id="7b381-141">Creare il contesto di database</span><span class="sxs-lookup"><span data-stu-id="7b381-141">Create the database context</span></span>

<span data-ttu-id="7b381-142">Il *contesto di database* è la classe principale che coordina le funzionalità di Entity Framework per un determinato modello di dati.</span><span class="sxs-lookup"><span data-stu-id="7b381-142">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="7b381-143">Questa classe viene creata mediante derivazione dalla classe `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="7b381-143">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="7b381-144">Aggiungere una classe `TodoContext` alla cartella *Models*:</span><span class="sxs-lookup"><span data-stu-id="7b381-144">Add a `TodoContext` class in the *Models* folder:</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="7b381-145">Aggiungere un controller</span><span class="sxs-lookup"><span data-stu-id="7b381-145">Add a controller</span></span>

<span data-ttu-id="7b381-146">Nella cartella *Controllers* creare una classe denominata `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="7b381-146">In the *Controllers* folder, create a class named `TodoController`.</span></span> <span data-ttu-id="7b381-147">Aggiungere il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="7b381-147">Add the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="7b381-148">Avviare l'app</span><span class="sxs-lookup"><span data-stu-id="7b381-148">Launch the app</span></span>

<span data-ttu-id="7b381-149">In Visual Studio Code premere F5 per avviare l'app.</span><span class="sxs-lookup"><span data-stu-id="7b381-149">In VS Code, press F5 to launch the app.</span></span> <span data-ttu-id="7b381-150">Passare a http://localhost:5000/api/todo (il controller `Todo` appena creato).</span><span class="sxs-lookup"><span data-stu-id="7b381-150">Navigate to  http://localhost:5000/api/todo   (The `Todo` controller we just created).</span></span>

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

## <a name="visual-studio-code-help"></a><span data-ttu-id="7b381-151">Guida di Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="7b381-151">Visual Studio Code help</span></span>

- [<span data-ttu-id="7b381-152">Introduzione</span><span class="sxs-lookup"><span data-stu-id="7b381-152">Getting started</span></span>](https://code.visualstudio.com/docs)
- [<span data-ttu-id="7b381-153">Debug</span><span class="sxs-lookup"><span data-stu-id="7b381-153">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
- [<span data-ttu-id="7b381-154">Terminale integrato</span><span class="sxs-lookup"><span data-stu-id="7b381-154">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [<span data-ttu-id="7b381-155">Tasti di scelta rapida</span><span class="sxs-lookup"><span data-stu-id="7b381-155">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [<span data-ttu-id="7b381-156">Tasti di scelta rapida di Mac</span><span class="sxs-lookup"><span data-stu-id="7b381-156">Mac keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [<span data-ttu-id="7b381-157">Tasti di scelta rapida di Linux</span><span class="sxs-lookup"><span data-stu-id="7b381-157">Linux keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [<span data-ttu-id="7b381-158">Tasti di scelta rapida di Windows</span><span class="sxs-lookup"><span data-stu-id="7b381-158">Windows keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

[!INCLUDE[next steps](../includes/webApi/next.md)]


