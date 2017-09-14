---
title: Creare un'API Web con ASP.NET Core e Visual Studio per Windows
author: rick-anderson
description: Compilare un'API Web con ASP.NET Core MVC e Visual Studio per Windows
keywords: ASP.NET Core,WebAPI,API Web,REST,HTTP,Service,HTTP Service
ms.author: riande
manager: wpickett
ms.date: 8/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-web-api
ms.openlocfilehash: c57c73c6f9c60874ef88749b838ed1cc1d353ead
ms.sourcegitcommit: 7fef13045e98d716c589a2982613dad261694a65
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/31/2017
---
#<a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-windows"></a><span data-ttu-id="cd3d4-104">Creare un'API Web con ASP.NET Core e Visual Studio per Windows</span><span class="sxs-lookup"><span data-stu-id="cd3d4-104">Create a web API with ASP.NET Core and Visual Studio for Windows</span></span>

<span data-ttu-id="cd3d4-105">Di [Rick Anderson](https://twitter.com/RickAndMSFT) e [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="cd3d4-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="cd3d4-106">In questa esercitazione si crea un'API Web per la gestione di un elenco di elementi di tipo "attività".</span><span class="sxs-lookup"><span data-stu-id="cd3d4-106">In this tutorial, you’ll build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="cd3d4-107">Non si crea un'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="cd3d4-107">You won’t build a UI.</span></span>

<span data-ttu-id="cd3d4-108">Sono disponibili 3 versioni dell'esercitazione:</span><span class="sxs-lookup"><span data-stu-id="cd3d4-108">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="cd3d4-109">Windows: API Web con Visual Studio per Windows (questa esercitazione)</span><span class="sxs-lookup"><span data-stu-id="cd3d4-109">Windows: Web API with Visual Studio for Windows (This tutorial)</span></span>
* <span data-ttu-id="cd3d4-110">macOS: [API Web con Visual Studio per Mac](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="cd3d4-110">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="cd3d4-111">macOS, Linux, Windows: [API Web con Visual Studio Code](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="cd3d4-111">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[intro to web API](../includes/webApi/intro.md)]

## <a name="prerequisites"></a><span data-ttu-id="cd3d4-112">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="cd3d4-112">Prerequisites</span></span>

[!INCLUDE[install 2.0](../includes/install2.0.md)]

<span data-ttu-id="cd3d4-113">Vedere [questo PDF](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/_static/_webAPI.pdf) per la versione ASP.NET Core 1.1.</span><span class="sxs-lookup"><span data-stu-id="cd3d4-113">See [this PDF](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/_static/_webAPI.pdf) for the ASP.NET Core 1.1 version.</span></span>

## <a name="create-the-project"></a><span data-ttu-id="cd3d4-114">Creare il progetto</span><span class="sxs-lookup"><span data-stu-id="cd3d4-114">Create the project</span></span>

<span data-ttu-id="cd3d4-115">In Visual Studio selezionare il menu **File** > **Nuovo** > **Progetto**.</span><span class="sxs-lookup"><span data-stu-id="cd3d4-115">From Visual Studio, select **File** menu, > **New** > **Project**.</span></span>

<span data-ttu-id="cd3d4-116">Selezionare il modello di progetto **Applicazione Web ASP.NET Core (.NET Core)**.</span><span class="sxs-lookup"><span data-stu-id="cd3d4-116">Select the **ASP.NET Core Web Application (.NET Core)** project template.</span></span> <span data-ttu-id="cd3d4-117">Assegnare il nome `TodoApi` al progetto e scegliere **OK**.</span><span class="sxs-lookup"><span data-stu-id="cd3d4-117">Name the project `TodoApi` and select **OK**.</span></span>

![Finestra di dialogo Nuovo progetto](first-web-api/_static/new-project.png)

<span data-ttu-id="cd3d4-119">Nella finestra di dialogo **Nuova Applicazione Web ASP.NET Core - TodoApi**, selezionare il modello **API Web**.</span><span class="sxs-lookup"><span data-stu-id="cd3d4-119">In the **New ASP.NET Core Web Application - TodoApi** dialog, select the **Web API** template.</span></span> <span data-ttu-id="cd3d4-120">Scegliere **OK**.</span><span class="sxs-lookup"><span data-stu-id="cd3d4-120">Select **OK**.</span></span> <span data-ttu-id="cd3d4-121">**Non** selezionare **Abilita supporto Docker**.</span><span class="sxs-lookup"><span data-stu-id="cd3d4-121">Do **not** select **Enable Docker Support**.</span></span>

![Finestra di dialogo Nuova Applicazione Web ASP.NET Core con il modello di progetto API Web selezionato tra i modelli ASP.NET Core](first-web-api/_static/web-api-project.png)

### <a name="launch-the-app"></a><span data-ttu-id="cd3d4-123">Avviare l'app</span><span class="sxs-lookup"><span data-stu-id="cd3d4-123">Launch the app</span></span>

<span data-ttu-id="cd3d4-124">In Visual Studio premere CTRL+F5 per avviare l'app.</span><span class="sxs-lookup"><span data-stu-id="cd3d4-124">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="cd3d4-125">Visual Studio apre un browser e naviga all'indirizzo `http://localhost:port/api/values`, dove *port* è un numero di porta selezionato a caso.</span><span class="sxs-lookup"><span data-stu-id="cd3d4-125">Visual Studio launches a browser and navigates to `http://localhost:port/api/values`, where *port* is a randomly-chosen port number.</span></span> <span data-ttu-id="cd3d4-126">In Chrome, Edge e Firefox viene visualizzato quanto segue:</span><span class="sxs-lookup"><span data-stu-id="cd3d4-126">Chrome, Edge, and Firefox display the following:</span></span>

```
["value1","value2"]
``` 

### <a name="add-a-model-class"></a><span data-ttu-id="cd3d4-127">Aggiungere una classe modello</span><span class="sxs-lookup"><span data-stu-id="cd3d4-127">Add a model class</span></span>

<span data-ttu-id="cd3d4-128">Un modello è un oggetto che rappresenta i dati nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="cd3d4-128">A model is an object that represents the data in your application.</span></span> <span data-ttu-id="cd3d4-129">In questo caso l'unico modello è un elemento attività da eseguire.</span><span class="sxs-lookup"><span data-stu-id="cd3d4-129">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="cd3d4-130">Aggiungere una cartella denominata "Models".</span><span class="sxs-lookup"><span data-stu-id="cd3d4-130">Add a folder named "Models".</span></span> <span data-ttu-id="cd3d4-131">In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto.</span><span class="sxs-lookup"><span data-stu-id="cd3d4-131">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="cd3d4-132">Selezionare **Aggiungi** > **Nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="cd3d4-132">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="cd3d4-133">Assegnare il nome *Models* alla cartella.</span><span class="sxs-lookup"><span data-stu-id="cd3d4-133">Name the folder *Models*.</span></span>

<span data-ttu-id="cd3d4-134">Nota: è possibile inserire le classi del modello in qualsiasi punto del progetto, ma per convenzione viene usata la cartella *Models*.</span><span class="sxs-lookup"><span data-stu-id="cd3d4-134">Note: The model classes go anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="cd3d4-135">Aggiungere una classe `TodoItem`.</span><span class="sxs-lookup"><span data-stu-id="cd3d4-135">Add a `TodoItem` class.</span></span> <span data-ttu-id="cd3d4-136">Fare clic con il pulsante destro del mouse sulla cartella *Models* e scegliere **Aggiungi** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="cd3d4-136">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="cd3d4-137">Assegnare il nome `TodoItem` alla classe, quindi selezionare **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="cd3d4-137">Name the class `TodoItem` and select **Add**.</span></span>

<span data-ttu-id="cd3d4-138">Sostituire il codice generato con il seguente:</span><span class="sxs-lookup"><span data-stu-id="cd3d4-138">Replace the generated code with the following:</span></span>

<span data-ttu-id="cd3d4-139">[!code-csharp[Principale](first-web-api/sample/TodoApi/Models/TodoItem.cs)]</span><span class="sxs-lookup"><span data-stu-id="cd3d4-139">[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]</span></span>

<span data-ttu-id="cd3d4-140">Il database genera il `Id` quando viene creato un `TodoItem`.</span><span class="sxs-lookup"><span data-stu-id="cd3d4-140">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="cd3d4-141">Creare il contesto di database</span><span class="sxs-lookup"><span data-stu-id="cd3d4-141">Create the database context</span></span>

<span data-ttu-id="cd3d4-142">Il *contesto di database* è la classe principale che coordina le funzionalità di Entity Framework per un determinato modello di dati.</span><span class="sxs-lookup"><span data-stu-id="cd3d4-142">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="cd3d4-143">Questa classe viene creata mediante derivazione dalla classe `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="cd3d4-143">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="cd3d4-144">Aggiungere una classe `TodoContext`.</span><span class="sxs-lookup"><span data-stu-id="cd3d4-144">Add a `TodoContext` class.</span></span> <span data-ttu-id="cd3d4-145">Fare clic con il pulsante destro del mouse sulla cartella *Models* e scegliere **Aggiungi** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="cd3d4-145">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="cd3d4-146">Assegnare il nome `TodoContext` alla classe, quindi selezionare **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="cd3d4-146">Name the class `TodoContext` and select **Add**.</span></span>

<span data-ttu-id="cd3d4-147">Sostituire il codice generato con il seguente:</span><span class="sxs-lookup"><span data-stu-id="cd3d4-147">Replace the generated code with the following:</span></span>

<span data-ttu-id="cd3d4-148">[!code-csharp[Principale](first-web-api/sample/TodoApi/Models/TodoContext.cs)]</span><span class="sxs-lookup"><span data-stu-id="cd3d4-148">[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]</span></span>

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

### <a name="add-a-controller"></a><span data-ttu-id="cd3d4-149">Aggiungere un controller</span><span class="sxs-lookup"><span data-stu-id="cd3d4-149">Add a controller</span></span>

<span data-ttu-id="cd3d4-150">In Esplora soluzioni fare clic con il pulsante destro del mouse sulla cartella *Controllers*.</span><span class="sxs-lookup"><span data-stu-id="cd3d4-150">In Solution Explorer, right-click the *Controllers* folder.</span></span> <span data-ttu-id="cd3d4-151">Selezionare **Aggiungi** > **Nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="cd3d4-151">Select **Add** > **New Item**.</span></span> <span data-ttu-id="cd3d4-152">Nella finestra di dialogo **Aggiungi nuovo elemento** selezionare il modello **Classe controller API Web**.</span><span class="sxs-lookup"><span data-stu-id="cd3d4-152">In the **Add New Item** dialog, select the **Web  API Controller Class** template.</span></span> <span data-ttu-id="cd3d4-153">Assegnare alla classe il nome `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="cd3d4-153">Name the class `TodoController`.</span></span>

![Finestra di dialogo Aggiungi nuovo elemento con controller nella casella di ricerca e controller API Web selezionato](first-web-api/_static/new_controller.png)

<span data-ttu-id="cd3d4-155">Sostituire il codice generato con il seguente:</span><span class="sxs-lookup"><span data-stu-id="cd3d4-155">Replace the generated code with the following:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]
  
### <a name="launch-the-app"></a><span data-ttu-id="cd3d4-156">Avviare l'app</span><span class="sxs-lookup"><span data-stu-id="cd3d4-156">Launch the app</span></span>

<span data-ttu-id="cd3d4-157">In Visual Studio premere CTRL+F5 per avviare l'app.</span><span class="sxs-lookup"><span data-stu-id="cd3d4-157">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="cd3d4-158">Visual Studio apre un browser e naviga all'indirizzo `http://localhost:port/api/values`, dove *port* è un numero di porta selezionato a caso.</span><span class="sxs-lookup"><span data-stu-id="cd3d4-158">Visual Studio launches a browser and navigates to `http://localhost:port/api/values`, where *port* is a randomly chosen port number.</span></span> <span data-ttu-id="cd3d4-159">Se si usa Firefox, Chrome o Edge i dati vengono visualizzati.</span><span class="sxs-lookup"><span data-stu-id="cd3d4-159">If you're using Chrome, Edge or Firefox, the data will be displayed.</span></span> <span data-ttu-id="cd3d4-160">Se si usa Internet Explorer viene richiesto di aprire o salvare il file *values.json*.</span><span class="sxs-lookup"><span data-stu-id="cd3d4-160">If you're using IE, IE will prompt to you open or save the *values.json* file.</span></span> <span data-ttu-id="cd3d4-161">Passare al controller `Todo` appena creato: `http://localhost:port/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="cd3d4-161">Navigate to the `Todo` controller we just created `http://localhost:port/api/todo`.</span></span>

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]

