---
title: Creare un'API Web con ASP.NET Core e Visual Studio per Windows
author: rick-anderson
description: Compilare un'API Web con ASP.NET Core MVC e Visual Studio per Windows
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/17/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-web-api
ms.openlocfilehash: 1680d5e0be0f4844c904d923af30634c53bd1b81
ms.sourcegitcommit: 3a893ae05f010656d99d6ddf55e82f1b5b6933bc
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/18/2018
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-windows"></a><span data-ttu-id="8399b-103">Creare un'API Web con ASP.NET Core e Visual Studio per Windows</span><span class="sxs-lookup"><span data-stu-id="8399b-103">Create a Web API with ASP.NET Core and Visual Studio for Windows</span></span>

<span data-ttu-id="8399b-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT) e [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="8399b-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="8399b-105">Questa esercitazione consente di creare un'API Web per la gestione di un elenco di elementi di tipo "attività"</span><span class="sxs-lookup"><span data-stu-id="8399b-105">This tutorial builds a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="8399b-106">e non un'interfaccia utente (UI).</span><span class="sxs-lookup"><span data-stu-id="8399b-106">A user interface (UI) isn't created.</span></span>

<span data-ttu-id="8399b-107">Sono disponibili tre versioni di questa esercitazione:</span><span class="sxs-lookup"><span data-stu-id="8399b-107">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="8399b-108">Windows: API Web con Visual Studio per Windows (questa esercitazione)</span><span class="sxs-lookup"><span data-stu-id="8399b-108">Windows: Web API with Visual Studio for Windows (This tutorial)</span></span>
* <span data-ttu-id="8399b-109">macOS: [API Web con Visual Studio per Mac](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="8399b-109">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="8399b-110">macOS, Linux, Windows: [API Web con Visual Studio Code](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="8399b-110">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[intro to web API](../includes/webApi/intro.md)]

## <a name="prerequisites"></a><span data-ttu-id="8399b-111">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="8399b-111">Prerequisites</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-windows.md)]

## <a name="create-the-project"></a><span data-ttu-id="8399b-112">Creare il progetto</span><span class="sxs-lookup"><span data-stu-id="8399b-112">Create the project</span></span>

<span data-ttu-id="8399b-113">Seguire questa procedura in Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="8399b-113">Follow these steps in Visual Studio:</span></span>

* <span data-ttu-id="8399b-114">Scegliere **Nuovo** > **Progetto** dal menu **File**.</span><span class="sxs-lookup"><span data-stu-id="8399b-114">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="8399b-115">Selezionare il modello **Applicazione Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="8399b-115">Select the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="8399b-116">Assegnare al progetto il nome *TodoApi* e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="8399b-116">Name the project *TodoApi* and click **OK**.</span></span>
* <span data-ttu-id="8399b-117">Nella finestra di dialogo **Nuova Applicazione Web ASP.NET Core - TodoApi** scegliere la versione per ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8399b-117">In the **New ASP.NET Core Web Application - TodoApi** dialog, choose the ASP.NET Core version.</span></span> <span data-ttu-id="8399b-118">Selezionare il modello **API**, quindi scegliere **OK**.</span><span class="sxs-lookup"><span data-stu-id="8399b-118">Select the **API** template and click **OK**.</span></span> <span data-ttu-id="8399b-119">**Non** selezionare **Abilita supporto Docker**.</span><span class="sxs-lookup"><span data-stu-id="8399b-119">Do **not** select **Enable Docker Support**.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="8399b-120">Avviare l'app</span><span class="sxs-lookup"><span data-stu-id="8399b-120">Launch the app</span></span>

<span data-ttu-id="8399b-121">In Visual Studio premere CTRL+F5 per avviare l'app.</span><span class="sxs-lookup"><span data-stu-id="8399b-121">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="8399b-122">Visual Studio apre un browser e naviga all'indirizzo `http://localhost:<port>/api/values`, dove `<port>` è un numero di porta selezionato a caso.</span><span class="sxs-lookup"><span data-stu-id="8399b-122">Visual Studio launches a browser and navigates to `http://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="8399b-123">In Chrome, Microsoft Edge e Firefox viene visualizzato l'output seguente:</span><span class="sxs-lookup"><span data-stu-id="8399b-123">Chrome, Microsoft Edge, and Firefox display the following output:</span></span>

```json
["value1","value2"]
```

<span data-ttu-id="8399b-124">Se si usa Internet Explorer, viene richiesto di salvare un file *values.json*.</span><span class="sxs-lookup"><span data-stu-id="8399b-124">If using Internet Explorer, you'll be prompted to save a *values.json* file.</span></span>

### <a name="add-a-model-class"></a><span data-ttu-id="8399b-125">Aggiungere una classe del modello</span><span class="sxs-lookup"><span data-stu-id="8399b-125">Add a model class</span></span>

<span data-ttu-id="8399b-126">Un modello è un oggetto che rappresenta i dati nell'app.</span><span class="sxs-lookup"><span data-stu-id="8399b-126">A model is an object representing the data in the app.</span></span> <span data-ttu-id="8399b-127">In questo caso l'unico modello è un elemento attività.</span><span class="sxs-lookup"><span data-stu-id="8399b-127">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="8399b-128">In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto.</span><span class="sxs-lookup"><span data-stu-id="8399b-128">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="8399b-129">Selezionare **Aggiungi** > **Nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="8399b-129">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="8399b-130">Assegnare il nome *Modelli* alla cartella.</span><span class="sxs-lookup"><span data-stu-id="8399b-130">Name the folder *Models*.</span></span>

> [!NOTE]
> <span data-ttu-id="8399b-131">È possibile inserire le classi del modello in qualsiasi punto del progetto.</span><span class="sxs-lookup"><span data-stu-id="8399b-131">The model classes can go anywhere in the project.</span></span> <span data-ttu-id="8399b-132">ma per convenzione viene usata la cartella *Models*.</span><span class="sxs-lookup"><span data-stu-id="8399b-132">The *Models* folder is used by convention for model classes.</span></span>

<span data-ttu-id="8399b-133">In Esplora soluzioni fare clic con il pulsante destro del mouse sulla cartella *Models* e scegliere**Aggiungi** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="8399b-133">In Solution Explorer, right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="8399b-134">Assegnare alla classe il nome *TodoItem* e fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="8399b-134">Name the class *TodoItem* and click **Add**.</span></span>

<span data-ttu-id="8399b-135">Aggiornare la classe `TodoItem` con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="8399b-135">Update the `TodoItem` class with the following code:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="8399b-136">Il database genera `Id` quando viene creato un `TodoItem`.</span><span class="sxs-lookup"><span data-stu-id="8399b-136">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="8399b-137">Creare il contesto di database</span><span class="sxs-lookup"><span data-stu-id="8399b-137">Create the database context</span></span>

<span data-ttu-id="8399b-138">Il *contesto di database* è la classe principale che coordina le funzionalità di Entity Framework per un determinato modello di dati.</span><span class="sxs-lookup"><span data-stu-id="8399b-138">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="8399b-139">Questa classe viene creata mediante derivazione dalla classe `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="8399b-139">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="8399b-140">In Esplora soluzioni fare clic con il pulsante destro del mouse sulla cartella *Models* e scegliere**Aggiungi** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="8399b-140">In Solution Explorer, right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="8399b-141">Assegnare alla classe il nome *TodoContext* e fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="8399b-141">Name the class *TodoContext* and click **Add**.</span></span>

<span data-ttu-id="8399b-142">Sostituire la classe con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="8399b-142">Replace the class with the following code:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

### <a name="add-a-controller"></a><span data-ttu-id="8399b-143">Aggiungere un controller</span><span class="sxs-lookup"><span data-stu-id="8399b-143">Add a controller</span></span>

<span data-ttu-id="8399b-144">In Esplora soluzioni fare clic con il pulsante destro del mouse sulla cartella *Controllers*.</span><span class="sxs-lookup"><span data-stu-id="8399b-144">In Solution Explorer, right-click the *Controllers* folder.</span></span> <span data-ttu-id="8399b-145">Selezionare **Aggiungi** > **Nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="8399b-145">Select **Add** > **New Item**.</span></span> <span data-ttu-id="8399b-146">Nella finestra di dialogo **Aggiungi nuovo elemento** selezionare il modello **API Controller Class** (Classe controller API).</span><span class="sxs-lookup"><span data-stu-id="8399b-146">In the **Add New Item** dialog, select the **API Controller Class** template.</span></span> <span data-ttu-id="8399b-147">Assegnare alla classe il nome *TodoController* e fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="8399b-147">Name the class *TodoController*, and click **Add**.</span></span>

![Finestra di dialogo Aggiungi nuovo elemento con controller nella casella di ricerca e controller API Web selezionato](first-web-api/_static/new_controller.png)

<span data-ttu-id="8399b-149">Sostituire la classe con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="8399b-149">Replace the class with the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="8399b-150">Avviare l'app</span><span class="sxs-lookup"><span data-stu-id="8399b-150">Launch the app</span></span>

<span data-ttu-id="8399b-151">In Visual Studio premere CTRL+F5 per avviare l'app.</span><span class="sxs-lookup"><span data-stu-id="8399b-151">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="8399b-152">Visual Studio apre un browser e naviga all'indirizzo `http://localhost:<port>/api/values`, dove `<port>` è un numero di porta selezionato a caso.</span><span class="sxs-lookup"><span data-stu-id="8399b-152">Visual Studio launches a browser and navigates to `http://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="8399b-153">Passare al controller `Todo` all'indirizzo `http://localhost:<port>/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="8399b-153">Navigate to the `Todo` controller at `http://localhost:<port>/api/todo`.</span></span>

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]
