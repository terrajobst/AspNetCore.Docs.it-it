---
title: Creare un'API Web con ASP.NET Core e Visual Studio Code
author: rick-anderson
description: Compilare un'API Web in macOS, Linux o Windows con ASP.NET Core MVC e Visual Studio Code
ms.author: riande
ms.custom: mvc
ms.date: 05/08/2018
uid: tutorials/web-api-vsc
ms.openlocfilehash: 4c41c949a9b5ca8db8928a0a53aff928fd7c8a4e
ms.sourcegitcommit: 79b756ea03eae77a716f500ef88253ee9b1464d2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/25/2018
ms.locfileid: "36297250"
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-code"></a><span data-ttu-id="4348c-103">Creare un'API Web con ASP.NET Core e Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4348c-103">Create a Web API with ASP.NET Core and Visual Studio Code</span></span>

<span data-ttu-id="4348c-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT) e [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="4348c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="4348c-105">Questa esercitazione consente di creare un'API Web per la gestione di un elenco di elementi di tipo "attività".</span><span class="sxs-lookup"><span data-stu-id="4348c-105">In this tutorial, build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="4348c-106">Non è stata costruita un'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="4348c-106">A UI isn't constructed.</span></span>

<span data-ttu-id="4348c-107">Sono disponibili tre versioni di questa esercitazione:</span><span class="sxs-lookup"><span data-stu-id="4348c-107">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="4348c-108">macOS, Linux, Windows: API Web con Visual Studio Code (questa esercitazione)</span><span class="sxs-lookup"><span data-stu-id="4348c-108">macOS, Linux, Windows: Web API with Visual Studio Code (This tutorial)</span></span>
* <span data-ttu-id="4348c-109">macOS: [API Web con Visual Studio per Mac](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="4348c-109">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="4348c-110">Windows: [API Web con Visual Studio per Windows](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="4348c-110">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

## <a name="prerequisites"></a><span data-ttu-id="4348c-111">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="4348c-111">Prerequisites</span></span>

[!INCLUDE[prerequisites](~/includes/net-core-prereqs-vscode.md)]

## <a name="create-the-project"></a><span data-ttu-id="4348c-112">Creare il progetto</span><span class="sxs-lookup"><span data-stu-id="4348c-112">Create the project</span></span>

<span data-ttu-id="4348c-113">Da una console eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="4348c-113">From a console, run the following commands:</span></span>

```console
dotnet new webapi -o TodoApi
code TodoApi
```

<span data-ttu-id="4348c-114">In Visual Studio Code (VS Code) viene aperta la cartella *TodoApi*.</span><span class="sxs-lookup"><span data-stu-id="4348c-114">The *TodoApi* folder opens in Visual Studio Code (VS Code).</span></span> <span data-ttu-id="4348c-115">Selezionare il file *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="4348c-115">Select the *Startup.cs* file.</span></span>

* <span data-ttu-id="4348c-116">Selezionare **Yes** (Sì) nel messaggio di **avviso** indicante che le risorse necessarie per la compilazione e il debug mancano nella directory "TodoApi"</span><span class="sxs-lookup"><span data-stu-id="4348c-116">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'TodoApi'.</span></span> <span data-ttu-id="4348c-117">e se si vuole aggiungerle.</span><span class="sxs-lookup"><span data-stu-id="4348c-117">Add them?"</span></span>
* <span data-ttu-id="4348c-118">Selezionare **Ripristina** per il messaggio di **informazione** "Dipendenze non risolte".</span><span class="sxs-lookup"><span data-stu-id="4348c-118">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

<!-- uid: tutorials/first-mvc-app-xplat/start-mvc uses the pic below. If you change it, make sure it's consistent -->

![VS Code con avviso indicante che le risorse necessarie per la compilazione e il debug mancano nella directory "TodoApi"](web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="4348c-122">Premere **Debug** (F5) per compilare ed eseguire il programma.</span><span class="sxs-lookup"><span data-stu-id="4348c-122">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="4348c-123">In un browser passare a http://localhost:5000/api/values.</span><span class="sxs-lookup"><span data-stu-id="4348c-123">In a browser, navigate to http://localhost:5000/api/values.</span></span> <span data-ttu-id="4348c-124">Verrà visualizzato l'output seguente:</span><span class="sxs-lookup"><span data-stu-id="4348c-124">The following output is displayed:</span></span>

```json
["value1","value2"]
```

<span data-ttu-id="4348c-125">Vedere la [Guida di Visual Studio Code](#visual-studio-code-help) per suggerimenti sull'uso di Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="4348c-125">See [Visual Studio Code help](#visual-studio-code-help) for tips on using VS Code.</span></span>

## <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="4348c-126">Aggiungere il supporto per Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="4348c-126">Add support for Entity Framework Core</span></span>

:::moniker range="<= aspnetcore-2.0"
<span data-ttu-id="4348c-127">La creazione di un nuovo progetto in ASP.NET Core 2.0 aggiunge il riferimento al pacchetto [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) nel file *TodoApi.csproj*:</span><span class="sxs-lookup"><span data-stu-id="4348c-127">Creating a new project in ASP.NET Core 2.0 adds the [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) package reference to the *TodoApi.csproj* file:</span></span>

[!code-xml[](first-web-api/samples/2.0/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]
:::moniker-end
:::moniker range=">= aspnetcore-2.1"
<span data-ttu-id="4348c-128">La creazione di un nuovo progetto in ASP.NET Core 2.1 o versioni successive aggiunge il riferimento al pacchetto [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App) nel file *TodoApi.csproj*:</span><span class="sxs-lookup"><span data-stu-id="4348c-128">Creating a new project in ASP.NET Core 2.1 or later adds the [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App) package reference to the *TodoApi.csproj* file:</span></span>

[!code-xml[](first-web-api/samples/2.1/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]
:::moniker-end

<span data-ttu-id="4348c-129">Non occorre installare il provider di database [Entity Framework Core InMemory](/ef/core/providers/in-memory/) separatamente.</span><span class="sxs-lookup"><span data-stu-id="4348c-129">There's no need to install the [Entity Framework Core InMemory](/ef/core/providers/in-memory/) database provider separately.</span></span> <span data-ttu-id="4348c-130">Questo provider di database consente l'uso di Entity Framework Core con un database in memoria.</span><span class="sxs-lookup"><span data-stu-id="4348c-130">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

## <a name="add-a-model-class"></a><span data-ttu-id="4348c-131">Aggiungere una classe del modello</span><span class="sxs-lookup"><span data-stu-id="4348c-131">Add a model class</span></span>

<span data-ttu-id="4348c-132">Un modello è un oggetto che rappresenta i dati nell'app.</span><span class="sxs-lookup"><span data-stu-id="4348c-132">A model is an object representing the data in your app.</span></span> <span data-ttu-id="4348c-133">In questo caso l'unico modello è un elemento attività.</span><span class="sxs-lookup"><span data-stu-id="4348c-133">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="4348c-134">Aggiungere una cartella denominata *Modelli*.</span><span class="sxs-lookup"><span data-stu-id="4348c-134">Add a folder named *Models*.</span></span> <span data-ttu-id="4348c-135">È possibile inserire le classi del modello in qualsiasi punto del progetto, ma per convenzione viene usata la cartella *Models*.</span><span class="sxs-lookup"><span data-stu-id="4348c-135">You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="4348c-136">Aggiungere una classe `TodoItem` con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="4348c-136">Add a `TodoItem` class with the following code:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="4348c-137">Il database genera `Id` quando viene creato un `TodoItem`.</span><span class="sxs-lookup"><span data-stu-id="4348c-137">The database generates the `Id` when a `TodoItem` is created.</span></span>

## <a name="create-the-database-context"></a><span data-ttu-id="4348c-138">Creare il contesto di database</span><span class="sxs-lookup"><span data-stu-id="4348c-138">Create the database context</span></span>

<span data-ttu-id="4348c-139">Il *contesto di database* è la classe principale che coordina le funzionalità di Entity Framework per un determinato modello di dati.</span><span class="sxs-lookup"><span data-stu-id="4348c-139">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="4348c-140">Questa classe viene creata mediante derivazione dalla classe `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="4348c-140">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="4348c-141">Aggiungere una classe `TodoContext` alla cartella *Models*:</span><span class="sxs-lookup"><span data-stu-id="4348c-141">Add a `TodoContext` class in the *Models* folder:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="4348c-142">Aggiungere un controller</span><span class="sxs-lookup"><span data-stu-id="4348c-142">Add a controller</span></span>

<span data-ttu-id="4348c-143">Nella cartella *Controllers* creare una classe denominata `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="4348c-143">In the *Controllers* folder, create a class named `TodoController`.</span></span> <span data-ttu-id="4348c-144">Sostituire il contenuto con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="4348c-144">Replace its contents with the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="4348c-145">Avviare l'app</span><span class="sxs-lookup"><span data-stu-id="4348c-145">Launch the app</span></span>

<span data-ttu-id="4348c-146">In Visual Studio Code premere F5 per avviare l'app.</span><span class="sxs-lookup"><span data-stu-id="4348c-146">In VS Code, press F5 to launch the app.</span></span> <span data-ttu-id="4348c-147">Passare a http://localhost:5000/api/todo (il controller `Todo` appena creato).</span><span class="sxs-lookup"><span data-stu-id="4348c-147">Navigate to http://localhost:5000/api/todo (the `Todo` controller we created).</span></span>

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

## <a name="visual-studio-code-help"></a><span data-ttu-id="4348c-148">Guida di Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="4348c-148">Visual Studio Code help</span></span>

* [<span data-ttu-id="4348c-149">Introduzione</span><span class="sxs-lookup"><span data-stu-id="4348c-149">Getting started</span></span>](https://code.visualstudio.com/docs)
* [<span data-ttu-id="4348c-150">Debug</span><span class="sxs-lookup"><span data-stu-id="4348c-150">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
* [<span data-ttu-id="4348c-151">Terminale integrato</span><span class="sxs-lookup"><span data-stu-id="4348c-151">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
* [<span data-ttu-id="4348c-152">Tasti di scelta rapida</span><span class="sxs-lookup"><span data-stu-id="4348c-152">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  * [<span data-ttu-id="4348c-153">Tasti di scelta rapida di macOS</span><span class="sxs-lookup"><span data-stu-id="4348c-153">macOS keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  * [<span data-ttu-id="4348c-154">Tasti di scelta rapida di Linux</span><span class="sxs-lookup"><span data-stu-id="4348c-154">Linux keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  * [<span data-ttu-id="4348c-155">Tasti di scelta rapida di Windows</span><span class="sxs-lookup"><span data-stu-id="4348c-155">Windows keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

[!INCLUDE[next steps](../includes/webApi/next.md)]
