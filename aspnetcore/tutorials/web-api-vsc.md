---
title: Creare un'API Web con ASP.NET Core e Visual Studio Code
author: rick-anderson
description: Compilare un'API Web in macOS, Linux o Windows con ASP.NET Core MVC e Visual Studio Code
ms.author: riande
ms.custom: mvc
ms.date: 07/30/2018
uid: tutorials/web-api-vsc
ms.openlocfilehash: 4ce808ec4241ab2fc3c2fb81c3fdb15dd853cd90
ms.sourcegitcommit: 927e510d68f269d8335b5a7c8592621219a90965
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/30/2018
ms.locfileid: "39342276"
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-code"></a><span data-ttu-id="8d8d0-103">Creare un'API Web con ASP.NET Core e Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8d8d0-103">Create a Web API with ASP.NET Core and Visual Studio Code</span></span>

<span data-ttu-id="8d8d0-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT) e [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="8d8d0-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="8d8d0-105">Questa esercitazione consente di creare un'API Web per la gestione di un elenco di elementi di tipo "attività".</span><span class="sxs-lookup"><span data-stu-id="8d8d0-105">In this tutorial, build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="8d8d0-106">Non è stata costruita un'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="8d8d0-106">A UI isn't constructed.</span></span>

<span data-ttu-id="8d8d0-107">Sono disponibili tre versioni di questa esercitazione:</span><span class="sxs-lookup"><span data-stu-id="8d8d0-107">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="8d8d0-108">macOS, Linux, Windows: API Web con Visual Studio Code (questa esercitazione)</span><span class="sxs-lookup"><span data-stu-id="8d8d0-108">macOS, Linux, Windows: Web API with Visual Studio Code (This tutorial)</span></span>
* <span data-ttu-id="8d8d0-109">macOS: [API Web con Visual Studio per Mac](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="8d8d0-109">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="8d8d0-110">Windows: [API Web con Visual Studio per Windows](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="8d8d0-110">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

## <a name="prerequisites"></a><span data-ttu-id="8d8d0-111">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="8d8d0-111">Prerequisites</span></span>

[!INCLUDE[prerequisites](~/includes/net-core-prereqs-vscode.md)]

## <a name="create-the-project"></a><span data-ttu-id="8d8d0-112">Creare il progetto</span><span class="sxs-lookup"><span data-stu-id="8d8d0-112">Create the project</span></span>

<span data-ttu-id="8d8d0-113">Da una console eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="8d8d0-113">From a console, run the following commands:</span></span>

```console
dotnet new webapi -o TodoApi
code TodoApi
```

<span data-ttu-id="8d8d0-114">In Visual Studio Code (VS Code) viene aperta la cartella *TodoApi*.</span><span class="sxs-lookup"><span data-stu-id="8d8d0-114">The *TodoApi* folder opens in Visual Studio Code (VS Code).</span></span> <span data-ttu-id="8d8d0-115">Selezionare il file *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="8d8d0-115">Select the *Startup.cs* file.</span></span>

* <span data-ttu-id="8d8d0-116">Selezionare **Yes** (Sì) nel messaggio di **avviso** indicante che le risorse necessarie per la compilazione e il debug mancano nella directory "TodoApi"</span><span class="sxs-lookup"><span data-stu-id="8d8d0-116">Select **Yes** to the **Warn** message "Required assets to build and debug are missing from 'TodoApi'.</span></span> <span data-ttu-id="8d8d0-117">e se si vuole aggiungerle.</span><span class="sxs-lookup"><span data-stu-id="8d8d0-117">Add them?"</span></span>
* <span data-ttu-id="8d8d0-118">Selezionare **Ripristina** per il messaggio di **informazione** "Dipendenze non risolte".</span><span class="sxs-lookup"><span data-stu-id="8d8d0-118">Select **Restore** to the **Info** message "There are unresolved dependencies".</span></span>

<!-- uid: tutorials/first-mvc-app-xplat/start-mvc uses the pic below. If you change it, make sure it's consistent -->

![VS Code con avviso indicante che le risorse necessarie per la compilazione e il debug mancano nella directory "TodoApi"](web-api-vsc/_static/vsc_restore.png)

<span data-ttu-id="8d8d0-122">Premere **Debug** (F5) per compilare ed eseguire il programma.</span><span class="sxs-lookup"><span data-stu-id="8d8d0-122">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="8d8d0-123">In un browser passare a http://localhost:5000/api/values.</span><span class="sxs-lookup"><span data-stu-id="8d8d0-123">In a browser, navigate to http://localhost:5000/api/values.</span></span> <span data-ttu-id="8d8d0-124">Verrà visualizzato l'output seguente:</span><span class="sxs-lookup"><span data-stu-id="8d8d0-124">The following output is displayed:</span></span>

```json
["value1","value2"]
```

<span data-ttu-id="8d8d0-125">Vedere la [Guida di Visual Studio Code](#visual-studio-code-help) per suggerimenti sull'uso di Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="8d8d0-125">See [Visual Studio Code help](#visual-studio-code-help) for tips on using VS Code.</span></span>

## <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="8d8d0-126">Aggiungere il supporto per Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="8d8d0-126">Add support for Entity Framework Core</span></span>

:::moniker range=">= aspnetcore-2.1"

<span data-ttu-id="8d8d0-127">La creazione di un nuovo progetto in ASP.NET Core 2.1 o versioni successive aggiunge il riferimento al pacchetto [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App) nel file *TodoApi.csproj*.</span><span class="sxs-lookup"><span data-stu-id="8d8d0-127">Creating a new project in ASP.NET Core 2.1 or later adds the [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App) package reference to the *TodoApi.csproj* file.</span></span> <span data-ttu-id="8d8d0-128">Aggiungere l'attributo `Version`, se non è già specificato.</span><span class="sxs-lookup"><span data-stu-id="8d8d0-128">Add the `Version` attribute, if not already specified.</span></span>

[!code-xml[](first-web-api/samples/2.1/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]

:::moniker-end

:::moniker range="<= aspnetcore-2.0"

<span data-ttu-id="8d8d0-129">La creazione di un nuovo progetto in ASP.NET Core 2.0 aggiunge il riferimento al pacchetto [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) nel file *TodoApi.csproj*:</span><span class="sxs-lookup"><span data-stu-id="8d8d0-129">Creating a new project in ASP.NET Core 2.0 adds the [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) package reference to the *TodoApi.csproj* file:</span></span>

[!code-xml[](first-web-api/samples/2.0/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]

:::moniker-end

<span data-ttu-id="8d8d0-130">Non occorre installare il provider di database [Entity Framework Core InMemory](/ef/core/providers/in-memory/) separatamente.</span><span class="sxs-lookup"><span data-stu-id="8d8d0-130">There's no need to install the [Entity Framework Core InMemory](/ef/core/providers/in-memory/) database provider separately.</span></span> <span data-ttu-id="8d8d0-131">Questo provider di database consente l'uso di Entity Framework Core con un database in memoria.</span><span class="sxs-lookup"><span data-stu-id="8d8d0-131">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

## <a name="add-a-model-class"></a><span data-ttu-id="8d8d0-132">Aggiungere una classe del modello</span><span class="sxs-lookup"><span data-stu-id="8d8d0-132">Add a model class</span></span>

<span data-ttu-id="8d8d0-133">Un modello è un oggetto che rappresenta i dati nell'app.</span><span class="sxs-lookup"><span data-stu-id="8d8d0-133">A model is an object representing the data in your app.</span></span> <span data-ttu-id="8d8d0-134">In questo caso l'unico modello è un elemento attività.</span><span class="sxs-lookup"><span data-stu-id="8d8d0-134">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="8d8d0-135">Aggiungere una cartella denominata *Modelli*.</span><span class="sxs-lookup"><span data-stu-id="8d8d0-135">Add a folder named *Models*.</span></span> <span data-ttu-id="8d8d0-136">È possibile inserire le classi del modello in qualsiasi punto del progetto, ma per convenzione viene usata la cartella *Models*.</span><span class="sxs-lookup"><span data-stu-id="8d8d0-136">You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="8d8d0-137">Aggiungere una classe `TodoItem` con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="8d8d0-137">Add a `TodoItem` class with the following code:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="8d8d0-138">Il database genera `Id` quando viene creato un `TodoItem`.</span><span class="sxs-lookup"><span data-stu-id="8d8d0-138">The database generates the `Id` when a `TodoItem` is created.</span></span>

## <a name="create-the-database-context"></a><span data-ttu-id="8d8d0-139">Creare il contesto di database</span><span class="sxs-lookup"><span data-stu-id="8d8d0-139">Create the database context</span></span>

<span data-ttu-id="8d8d0-140">Il *contesto di database* è la classe principale che coordina le funzionalità di Entity Framework per un determinato modello di dati.</span><span class="sxs-lookup"><span data-stu-id="8d8d0-140">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="8d8d0-141">Questa classe viene creata mediante derivazione dalla classe `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="8d8d0-141">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="8d8d0-142">Aggiungere una classe `TodoContext` alla cartella *Models*:</span><span class="sxs-lookup"><span data-stu-id="8d8d0-142">Add a `TodoContext` class in the *Models* folder:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="8d8d0-143">Aggiungere un controller</span><span class="sxs-lookup"><span data-stu-id="8d8d0-143">Add a controller</span></span>

<span data-ttu-id="8d8d0-144">Nella cartella *Controllers* creare una classe denominata `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="8d8d0-144">In the *Controllers* folder, create a class named `TodoController`.</span></span> <span data-ttu-id="8d8d0-145">Sostituire il contenuto con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="8d8d0-145">Replace its contents with the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="8d8d0-146">Avviare l'app</span><span class="sxs-lookup"><span data-stu-id="8d8d0-146">Launch the app</span></span>

<span data-ttu-id="8d8d0-147">In Visual Studio Code premere F5 per avviare l'app.</span><span class="sxs-lookup"><span data-stu-id="8d8d0-147">In VS Code, press F5 to launch the app.</span></span> <span data-ttu-id="8d8d0-148">Passare a http://localhost:5000/api/todo (il controller `Todo` appena creato).</span><span class="sxs-lookup"><span data-stu-id="8d8d0-148">Navigate to http://localhost:5000/api/todo (the `Todo` controller we created).</span></span>

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

## <a name="visual-studio-code-help"></a><span data-ttu-id="8d8d0-149">Guida di Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8d8d0-149">Visual Studio Code help</span></span>

* [<span data-ttu-id="8d8d0-150">Introduzione</span><span class="sxs-lookup"><span data-stu-id="8d8d0-150">Getting started</span></span>](https://code.visualstudio.com/docs)
* [<span data-ttu-id="8d8d0-151">Debug</span><span class="sxs-lookup"><span data-stu-id="8d8d0-151">Debugging</span></span>](https://code.visualstudio.com/docs/editor/debugging)
* [<span data-ttu-id="8d8d0-152">Terminale integrato</span><span class="sxs-lookup"><span data-stu-id="8d8d0-152">Integrated terminal</span></span>](https://code.visualstudio.com/docs/editor/integrated-terminal)
* [<span data-ttu-id="8d8d0-153">Tasti di scelta rapida</span><span class="sxs-lookup"><span data-stu-id="8d8d0-153">Keyboard shortcuts</span></span>](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  * [<span data-ttu-id="8d8d0-154">Tasti di scelta rapida di macOS</span><span class="sxs-lookup"><span data-stu-id="8d8d0-154">macOS keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  * [<span data-ttu-id="8d8d0-155">Tasti di scelta rapida di Linux</span><span class="sxs-lookup"><span data-stu-id="8d8d0-155">Linux keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  * [<span data-ttu-id="8d8d0-156">Tasti di scelta rapida di Windows</span><span class="sxs-lookup"><span data-stu-id="8d8d0-156">Windows keyboard shortcuts</span></span>](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

[!INCLUDE[next steps](../includes/webApi/next.md)]
