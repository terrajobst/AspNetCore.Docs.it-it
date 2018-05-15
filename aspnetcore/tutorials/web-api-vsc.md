---
title: Creare un'API Web con ASP.NET Core e Visual Studio Code
author: rick-anderson
description: Compilare un'API Web in macOS, Linux o Windows con ASP.NET Core MVC e Visual Studio Code
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 04/27/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/web-api-vsc
ms.openlocfilehash: f991aeadbaa3f7696d6fd6b8791d26248e7560a6
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/03/2018
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-code"></a><span data-ttu-id="e38db-103">Creare un'API Web con ASP.NET Core e Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e38db-103">Create a Web API with ASP.NET Core and Visual Studio Code</span></span>

<span data-ttu-id="e38db-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT) e [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="e38db-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

::: moniker range="= aspnetcore-2.1"
[!INCLUDE[](~/includes/2.1.md)]
::: moniker-end

<span data-ttu-id="e38db-106">Questa esercitazione consente di creare un'API Web per la gestione di un elenco di elementi di tipo "attività".</span><span class="sxs-lookup"><span data-stu-id="e38db-106">In this tutorial, build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="e38db-107">Non è stata costruita un'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="e38db-107">A UI isn't constructed.</span></span>

<span data-ttu-id="e38db-108">Sono disponibili tre versioni di questa esercitazione:</span><span class="sxs-lookup"><span data-stu-id="e38db-108">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="e38db-109">macOS, Linux, Windows: API Web con Visual Studio Code (questa esercitazione)</span><span class="sxs-lookup"><span data-stu-id="e38db-109">macOS, Linux, Windows: Web API with Visual Studio Code (This tutorial)</span></span>
* <span data-ttu-id="e38db-110">macOS: [API Web con Visual Studio per Mac](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="e38db-110">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="e38db-111">Windows: [API Web con Visual Studio per Windows](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="e38db-111">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

## <a name="prerequisites"></a><span data-ttu-id="e38db-112">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="e38db-112">Prerequisites</span></span>

[!INCLUDE[prerequisites](~/includes/net-core-prereqs-vscode.md)]

## <a name="create-the-project"></a><span data-ttu-id="e38db-113">Creare il progetto</span><span class="sxs-lookup"><span data-stu-id="e38db-113">Create the project</span></span>

<span data-ttu-id="e38db-114">Da una console eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="e38db-114">From a console, run the following commands:</span></span>

```console
dotnet new webapi -o TodoApi
code TodoApi
```

<span data-ttu-id="e38db-115">In Visual Studio Code (VS Code) viene aperta la cartella *TodoApi*.</span><span class="sxs-lookup"><span data-stu-id="e38db-115">The *TodoApi* folder opens in Visual Studio Code (VS Code).</span></span> <span data-ttu-id="e38db-116">Selezionare il file *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="e38db-116">Select the *Startup.cs* file.</span></span>

* <span data-ttu-id="e38db-117">Selezionare **Yes** (Sì) nel messaggio di **avviso** indicante che le risorse necessarie per la compilazione e il debug mancano nella directory "TodoApi"</span><span class="sxs-lookup"><span data-stu-id="e38db-117">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'TodoApi'.</span></span> <span data-ttu-id="e38db-118">e se si vuole aggiungerle.</span><span class="sxs-lookup"><span data-stu-id="e38db-118">Add them?"</span></span>
* <span data-ttu-id="e38db-119">Selezionare **Ripristina** per il messaggio di **informazione** "Dipendenze non risolte".</span><span class="sxs-lookup"><span data-stu-id="e38db-119">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

<!-- uid: tutorials/first-mvc-app-xplat/start-mvc uses the pic below. If you change it, make sure it's consistent -->

![VS Code con avviso indicante che le risorse necessarie per la compilazione e il debug mancano nella directory "TodoApi"](web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="e38db-123">Premere **Debug** (F5) per compilare ed eseguire il programma.</span><span class="sxs-lookup"><span data-stu-id="e38db-123">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="e38db-124">In un browser passare a http://localhost:5000/api/values.</span><span class="sxs-lookup"><span data-stu-id="e38db-124">In a browser, navigate to http://localhost:5000/api/values.</span></span> <span data-ttu-id="e38db-125">Verrà visualizzato l'output seguente:</span><span class="sxs-lookup"><span data-stu-id="e38db-125">The following output is displayed:</span></span>

```json
["value1","value2"]
```

<span data-ttu-id="e38db-126">Vedere la [Guida di Visual Studio Code](#visual-studio-code-help) per suggerimenti sull'uso di Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="e38db-126">See [Visual Studio Code help](#visual-studio-code-help) for tips on using VS Code.</span></span>

## <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="e38db-127">Aggiungere il supporto per Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="e38db-127">Add support for Entity Framework Core</span></span>

:::moniker range="<= aspnetcore-2.0"
<span data-ttu-id="e38db-128">La creazione di un nuovo progetto in ASP.NET Core 2.0 aggiunge il riferimento al pacchetto [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) nel file *TodoApi.csproj*:</span><span class="sxs-lookup"><span data-stu-id="e38db-128">Creating a new project in ASP.NET Core 2.0 adds the [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) package reference to the *TodoApi.csproj* file:</span></span>

<span data-ttu-id="e38db-129">[!code-xml[](first-web-api/samples/2.0/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]</span><span class="sxs-lookup"><span data-stu-id="e38db-129">[!code-xml[](first-web-api/samples/2.0/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]</span></span>
:::moniker-end
:::moniker range=">= aspnetcore-2.1"
<span data-ttu-id="e38db-130">La creazione di un nuovo progetto in ASP.NET Core 2.1 o versioni successive aggiunge il riferimento al pacchetto [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App) nel file *TodoApi.csproj*:</span><span class="sxs-lookup"><span data-stu-id="e38db-130">Creating a new project in ASP.NET Core 2.1 or later adds the [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App) package reference to the *TodoApi.csproj* file:</span></span>

<span data-ttu-id="e38db-131">[!code-xml[](first-web-api/samples/2.1/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]</span><span class="sxs-lookup"><span data-stu-id="e38db-131">[!code-xml[](first-web-api/samples/2.1/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]</span></span>
:::moniker-end

<span data-ttu-id="e38db-132">Non occorre installare il provider di database [Entity Framework Core InMemory](/ef/core/providers/in-memory/) separatamente.</span><span class="sxs-lookup"><span data-stu-id="e38db-132">There's no need to install the [Entity Framework Core InMemory](/ef/core/providers/in-memory/) database provider separately.</span></span> <span data-ttu-id="e38db-133">Questo provider di database consente l'uso di Entity Framework Core con un database in memoria.</span><span class="sxs-lookup"><span data-stu-id="e38db-133">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

## <a name="add-a-model-class"></a><span data-ttu-id="e38db-134">Aggiungere una classe del modello</span><span class="sxs-lookup"><span data-stu-id="e38db-134">Add a model class</span></span>

<span data-ttu-id="e38db-135">Un modello è un oggetto che rappresenta i dati nell'app.</span><span class="sxs-lookup"><span data-stu-id="e38db-135">A model is an object representing the data in your app.</span></span> <span data-ttu-id="e38db-136">In questo caso l'unico modello è un elemento attività.</span><span class="sxs-lookup"><span data-stu-id="e38db-136">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="e38db-137">Aggiungere una cartella denominata *Modelli*.</span><span class="sxs-lookup"><span data-stu-id="e38db-137">Add a folder named *Models*.</span></span> <span data-ttu-id="e38db-138">È possibile inserire le classi del modello in qualsiasi punto del progetto, ma per convenzione viene usata la cartella *Models*.</span><span class="sxs-lookup"><span data-stu-id="e38db-138">You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="e38db-139">Aggiungere una classe `TodoItem` con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="e38db-139">Add a `TodoItem` class with the following code:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="e38db-140">Il database genera `Id` quando viene creato un `TodoItem`.</span><span class="sxs-lookup"><span data-stu-id="e38db-140">The database generates the `Id` when a `TodoItem` is created.</span></span>

## <a name="create-the-database-context"></a><span data-ttu-id="e38db-141">Creare il contesto di database</span><span class="sxs-lookup"><span data-stu-id="e38db-141">Create the database context</span></span>

<span data-ttu-id="e38db-142">Il *contesto di database* è la classe principale che coordina le funzionalità di Entity Framework per un determinato modello di dati.</span><span class="sxs-lookup"><span data-stu-id="e38db-142">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="e38db-143">Questa classe viene creata mediante derivazione dalla classe `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="e38db-143">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="e38db-144">Aggiungere una classe `TodoContext` alla cartella *Models*:</span><span class="sxs-lookup"><span data-stu-id="e38db-144">Add a `TodoContext` class in the *Models* folder:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="e38db-145">Aggiungere un controller</span><span class="sxs-lookup"><span data-stu-id="e38db-145">Add a controller</span></span>

<span data-ttu-id="e38db-146">Nella cartella *Controllers* creare una classe denominata `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="e38db-146">In the *Controllers* folder, create a class named `TodoController`.</span></span> <span data-ttu-id="e38db-147">Sostituire il contenuto con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="e38db-147">Replace its contents with the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="e38db-148">Avviare l'app</span><span class="sxs-lookup"><span data-stu-id="e38db-148">Launch the app</span></span>

<span data-ttu-id="e38db-149">In Visual Studio Code premere F5 per avviare l'app.</span><span class="sxs-lookup"><span data-stu-id="e38db-149">In VS Code, press F5 to launch the app.</span></span> <span data-ttu-id="e38db-150">Passare a http://localhost:5000/api/todo (il controller `Todo` appena creato).</span><span class="sxs-lookup"><span data-stu-id="e38db-150">Navigate to http://localhost:5000/api/todo (the `Todo` controller we created).</span></span>

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

## <a name="visual-studio-code-help"></a><span data-ttu-id="e38db-151">Guida di Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e38db-151">Visual Studio Code help</span></span>

* [<span data-ttu-id="e38db-152">Introduzione</span><span class="sxs-lookup"><span data-stu-id="e38db-152">Getting started</span></span>](https://code.visualstudio.com/docs)
* [<span data-ttu-id="e38db-153">Debug</span><span class="sxs-lookup"><span data-stu-id="e38db-153">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
* [<span data-ttu-id="e38db-154">Terminale integrato</span><span class="sxs-lookup"><span data-stu-id="e38db-154">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
* [<span data-ttu-id="e38db-155">Tasti di scelta rapida</span><span class="sxs-lookup"><span data-stu-id="e38db-155">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  * [<span data-ttu-id="e38db-156">Tasti di scelta rapida di macOS</span><span class="sxs-lookup"><span data-stu-id="e38db-156">macOS keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  * [<span data-ttu-id="e38db-157">Tasti di scelta rapida di Linux</span><span class="sxs-lookup"><span data-stu-id="e38db-157">Linux keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  * [<span data-ttu-id="e38db-158">Tasti di scelta rapida di Windows</span><span class="sxs-lookup"><span data-stu-id="e38db-158">Windows keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

[!INCLUDE[next steps](../includes/webApi/next.md)]
