---
title: Creare un'API Web con ASP.NET Core e Visual Studio per Windows
author: rick-anderson
description: Compilare un'API Web con ASP.NET Core MVC e Visual Studio per Windows
ms.author: riande
manager: wpickett
ms.date: 08/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-web-api
ms.openlocfilehash: 234dbf73f9751ad4f995d6e7471d94f060fb15bf
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/19/2018
---
#<a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-windows"></a><span data-ttu-id="86432-103">Creare un'API Web con ASP.NET Core e Visual Studio per Windows</span><span class="sxs-lookup"><span data-stu-id="86432-103">Create a web API with ASP.NET Core and Visual Studio for Windows</span></span>

<span data-ttu-id="86432-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT) e [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="86432-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="86432-105">Questa esercitazione consente di creare un'API Web per la gestione di un elenco di elementi di tipo "attività"</span><span class="sxs-lookup"><span data-stu-id="86432-105">This tutorial builds a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="86432-106">e non un'interfaccia utente (UI).</span><span class="sxs-lookup"><span data-stu-id="86432-106">A user interface (UI) is not created.</span></span>

<span data-ttu-id="86432-107">Sono disponibili 3 versioni dell'esercitazione:</span><span class="sxs-lookup"><span data-stu-id="86432-107">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="86432-108">Windows: API Web con Visual Studio per Windows (questa esercitazione)</span><span class="sxs-lookup"><span data-stu-id="86432-108">Windows: Web API with Visual Studio for Windows (This tutorial)</span></span>
* <span data-ttu-id="86432-109">macOS: [API Web con Visual Studio per Mac](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="86432-109">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="86432-110">macOS, Linux, Windows: [API Web con Visual Studio Code](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="86432-110">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[intro to web API](../includes/webApi/intro.md)]

## <a name="prerequisites"></a><span data-ttu-id="86432-111">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="86432-111">Prerequisites</span></span>

[!INCLUDE[install 2.0](../includes/install2.0.md)]

<span data-ttu-id="86432-112">Vedere [questo PDF](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-web-api/_static/_webAPI.pdf) per la versione ASP.NET Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="86432-112">See [this PDF](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-web-api/_static/_webAPI.pdf) for the ASP.NET Core 1.1 version.</span></span>

## <a name="create-the-project"></a><span data-ttu-id="86432-113">Creare il progetto</span><span class="sxs-lookup"><span data-stu-id="86432-113">Create the project</span></span>

<span data-ttu-id="86432-114">In Visual Studio selezionare il menu **File** > **Nuovo** > **Progetto**.</span><span class="sxs-lookup"><span data-stu-id="86432-114">From Visual Studio, select **File** menu, > **New** > **Project**.</span></span>

<span data-ttu-id="86432-115">Selezionare il modello di progetto **Applicazione Web ASP.NET Core (.NET Core)**.</span><span class="sxs-lookup"><span data-stu-id="86432-115">Select the **ASP.NET Core Web Application (.NET Core)** project template.</span></span> <span data-ttu-id="86432-116">Assegnare il nome `TodoApi` al progetto e scegliere **OK**.</span><span class="sxs-lookup"><span data-stu-id="86432-116">Name the project `TodoApi` and select **OK**.</span></span>

![Finestra di dialogo Nuovo progetto](first-web-api/_static/new-project.png)

<span data-ttu-id="86432-118">Nella finestra di dialogo **Nuova Applicazione Web ASP.NET Core - TodoApi**, selezionare il modello **API Web**.</span><span class="sxs-lookup"><span data-stu-id="86432-118">In the **New ASP.NET Core Web Application - TodoApi** dialog, select the **Web API** template.</span></span> <span data-ttu-id="86432-119">Scegliere **OK**.</span><span class="sxs-lookup"><span data-stu-id="86432-119">Select **OK**.</span></span> <span data-ttu-id="86432-120">**Non** selezionare **Abilita supporto Docker**.</span><span class="sxs-lookup"><span data-stu-id="86432-120">Do **not** select **Enable Docker Support**.</span></span>

![Finestra di dialogo Nuova Applicazione Web ASP.NET Core con il modello di progetto API Web selezionato tra i modelli ASP.NET Core](first-web-api/_static/web-api-project.png)

### <a name="launch-the-app"></a><span data-ttu-id="86432-122">Avviare l'app</span><span class="sxs-lookup"><span data-stu-id="86432-122">Launch the app</span></span>

<span data-ttu-id="86432-123">In Visual Studio premere CTRL+F5 per avviare l'app.</span><span class="sxs-lookup"><span data-stu-id="86432-123">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="86432-124">Visual Studio apre un browser e naviga all'indirizzo `http://localhost:port/api/values`, dove *port* è un numero di porta selezionato a caso.</span><span class="sxs-lookup"><span data-stu-id="86432-124">Visual Studio launches a browser and navigates to `http://localhost:port/api/values`, where *port* is a randomly chosen port number.</span></span> <span data-ttu-id="86432-125">In Chrome, Microsoft Edge e Firefox viene visualizzato l'output seguente:</span><span class="sxs-lookup"><span data-stu-id="86432-125">Chrome, Microsoft Edge, and Firefox display the following output:</span></span>

```
["value1","value2"]
```

### <a name="add-a-model-class"></a><span data-ttu-id="86432-126">Aggiungere una classe del modello</span><span class="sxs-lookup"><span data-stu-id="86432-126">Add a model class</span></span>

<span data-ttu-id="86432-127">Un modello è un oggetto che rappresenta i dati nell'app.</span><span class="sxs-lookup"><span data-stu-id="86432-127">A model is an object that represents the data in the app.</span></span> <span data-ttu-id="86432-128">In questo caso l'unico modello è un elemento attività.</span><span class="sxs-lookup"><span data-stu-id="86432-128">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="86432-129">Aggiungere una cartella denominata "Models".</span><span class="sxs-lookup"><span data-stu-id="86432-129">Add a folder named "Models".</span></span> <span data-ttu-id="86432-130">In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto.</span><span class="sxs-lookup"><span data-stu-id="86432-130">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="86432-131">Selezionare **Aggiungi** > **Nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="86432-131">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="86432-132">Assegnare il nome *Modelli* alla cartella.</span><span class="sxs-lookup"><span data-stu-id="86432-132">Name the folder *Models*.</span></span>

<span data-ttu-id="86432-133">Nota: è possibile inserire le classi del modello in qualsiasi punto del progetto.</span><span class="sxs-lookup"><span data-stu-id="86432-133">Note: The model classes go anywhere in the project.</span></span> <span data-ttu-id="86432-134">ma per convenzione viene usata la cartella *Models*.</span><span class="sxs-lookup"><span data-stu-id="86432-134">The *Models* folder is used by convention for model classes.</span></span>

<span data-ttu-id="86432-135">Aggiungere una classe `TodoItem`.</span><span class="sxs-lookup"><span data-stu-id="86432-135">Add a `TodoItem` class.</span></span> <span data-ttu-id="86432-136">Fare clic con il pulsante destro del mouse sulla cartella *Models* e scegliere **Aggiungi** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="86432-136">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="86432-137">Assegnare il nome `TodoItem` alla classe, quindi selezionare **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="86432-137">Name the class `TodoItem` and select **Add**.</span></span>

<span data-ttu-id="86432-138">Aggiornare la classe `TodoItem` con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="86432-138">Update the `TodoItem` class with the following code:</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="86432-139">Il database genera `Id` quando viene creato un `TodoItem`.</span><span class="sxs-lookup"><span data-stu-id="86432-139">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="86432-140">Creare il contesto di database</span><span class="sxs-lookup"><span data-stu-id="86432-140">Create the database context</span></span>

<span data-ttu-id="86432-141">Il *contesto di database* è la classe principale che coordina le funzionalità di Entity Framework per un determinato modello di dati.</span><span class="sxs-lookup"><span data-stu-id="86432-141">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="86432-142">Questa classe viene creata mediante derivazione dalla classe `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="86432-142">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="86432-143">Aggiungere una classe `TodoContext`.</span><span class="sxs-lookup"><span data-stu-id="86432-143">Add a `TodoContext` class.</span></span> <span data-ttu-id="86432-144">Fare clic con il pulsante destro del mouse sulla cartella *Models* e scegliere **Aggiungi** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="86432-144">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="86432-145">Assegnare il nome `TodoContext` alla classe, quindi selezionare **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="86432-145">Name the class `TodoContext` and select **Add**.</span></span>

<span data-ttu-id="86432-146">Sostituire la classe con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="86432-146">Replace the class with the following code:</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

### <a name="add-a-controller"></a><span data-ttu-id="86432-147">Aggiungere un controller</span><span class="sxs-lookup"><span data-stu-id="86432-147">Add a controller</span></span>

<span data-ttu-id="86432-148">In Esplora soluzioni fare clic con il pulsante destro del mouse sulla cartella *Controllers*.</span><span class="sxs-lookup"><span data-stu-id="86432-148">In Solution Explorer, right-click the *Controllers* folder.</span></span> <span data-ttu-id="86432-149">Selezionare **Aggiungi** > **Nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="86432-149">Select **Add** > **New Item**.</span></span> <span data-ttu-id="86432-150">Nella finestra di dialogo **Aggiungi nuovo elemento** selezionare il modello **Classe controller API Web**.</span><span class="sxs-lookup"><span data-stu-id="86432-150">In the **Add New Item** dialog, select the **Web API Controller Class** template.</span></span> <span data-ttu-id="86432-151">Assegnare alla classe il nome `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="86432-151">Name the class `TodoController`.</span></span>

![Finestra di dialogo Aggiungi nuovo elemento con controller nella casella di ricerca e controller API Web selezionato](first-web-api/_static/new_controller.png)

<span data-ttu-id="86432-153">Sostituire la classe con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="86432-153">Replace the class with the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="86432-154">Avviare l'app</span><span class="sxs-lookup"><span data-stu-id="86432-154">Launch the app</span></span>

<span data-ttu-id="86432-155">In Visual Studio premere CTRL+F5 per avviare l'app.</span><span class="sxs-lookup"><span data-stu-id="86432-155">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="86432-156">Visual Studio apre un browser e naviga all'indirizzo `http://localhost:port/api/values`, dove *port* è un numero di porta selezionato a caso.</span><span class="sxs-lookup"><span data-stu-id="86432-156">Visual Studio launches a browser and navigates to `http://localhost:port/api/values`, where *port* is a randomly chosen port number.</span></span> <span data-ttu-id="86432-157">Passare al controller `Todo` all'indirizzo `http://localhost:port/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="86432-157">Navigate to the `Todo` controller at `http://localhost:port/api/todo`.</span></span>

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]

