---
title: Creare un'API Web con ASP.NET Core e Visual Studio
author: rick-anderson
description: Compilare un'API Web con ASP.NET Core MVC e Visual Studio in Windows
ms.author: riande
ms.custom: mvc
ms.date: 05/17/2018
uid: tutorials/first-web-api
ms.openlocfilehash: eb23d5376742e04712f714263815582fddc5d18e
ms.sourcegitcommit: e9b99854b0a8021dafabee0db5e1338067f250a9
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2018
ms.locfileid: "52450697"
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio"></a><span data-ttu-id="794b8-103">Creare un'API Web con ASP.NET Core e Visual Studio</span><span class="sxs-lookup"><span data-stu-id="794b8-103">Create a Web API with ASP.NET Core and Visual Studio</span></span>

<span data-ttu-id="794b8-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT) e [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="794b8-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="794b8-105">Questa esercitazione consente di creare un'API Web per la gestione di un elenco di elementi di tipo "attività"</span><span class="sxs-lookup"><span data-stu-id="794b8-105">This tutorial builds a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="794b8-106">e non un'interfaccia utente (UI).</span><span class="sxs-lookup"><span data-stu-id="794b8-106">A user interface (UI) isn't created.</span></span>

<span data-ttu-id="794b8-107">Sono disponibili tre versioni di questa esercitazione:</span><span class="sxs-lookup"><span data-stu-id="794b8-107">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="794b8-108">Windows: API Web con Visual Studio in Windows (questa esercitazione, vedere questa [versione video](https://www.youtube.com/watch?v=TTkhEyGBfAk))</span><span class="sxs-lookup"><span data-stu-id="794b8-108">Windows: Web API with Visual Studio on Windows (This tutorial, see the [video version](https://www.youtube.com/watch?v=TTkhEyGBfAk))</span></span>
* <span data-ttu-id="794b8-109">macOS: [API Web con Visual Studio per Mac](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="794b8-109">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="794b8-110">macOS, Linux, Windows: [API Web con Visual Studio Code](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="794b8-110">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

> [!NOTE]
> <span data-ttu-id="794b8-111">È in corso un test di usabilità di una nuova proposta di struttura del sommario di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="794b8-111">We’re testing the usability of a proposed new structure for the ASP.NET Core table of contents.</span></span>  <span data-ttu-id="794b8-112">Se si dispone di alcuni minuti per provare a cercare 7 argomenti diversi nel sommario corrente o proposto, [fare clic qui per partecipare al test](https://dpk4xbh5.optimalworkshop.com/treejack/rps16hd5).</span><span class="sxs-lookup"><span data-stu-id="794b8-112">If you have a few minutes to try an exercise of finding 7 different topics in the current or proposed table of contents, please [click here to participate in the study](https://dpk4xbh5.optimalworkshop.com/treejack/rps16hd5).</span></span>

[!INCLUDE[intro to web API](../includes/webApi/intro.md)]

## <a name="prerequisites"></a><span data-ttu-id="794b8-113">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="794b8-113">Prerequisites</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-windows.md)]

## <a name="create-the-project"></a><span data-ttu-id="794b8-114">Creare il progetto</span><span class="sxs-lookup"><span data-stu-id="794b8-114">Create the project</span></span>

<span data-ttu-id="794b8-115">Seguire questa procedura in Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="794b8-115">Follow these steps in Visual Studio:</span></span>

* <span data-ttu-id="794b8-116">Scegliere **Nuovo** > **Progetto** dal menu **File**.</span><span class="sxs-lookup"><span data-stu-id="794b8-116">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="794b8-117">Selezionare il modello **Applicazione Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="794b8-117">Select the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="794b8-118">Assegnare al progetto il nome *TodoApi* e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="794b8-118">Name the project *TodoApi* and click **OK**.</span></span>
* <span data-ttu-id="794b8-119">Nella finestra di dialogo **Nuova Applicazione Web ASP.NET Core - TodoApi** scegliere la versione per ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="794b8-119">In the **New ASP.NET Core Web Application - TodoApi** dialog, choose the ASP.NET Core version.</span></span> <span data-ttu-id="794b8-120">Selezionare il modello **API**, quindi scegliere **OK**.</span><span class="sxs-lookup"><span data-stu-id="794b8-120">Select the **API** template and click **OK**.</span></span> <span data-ttu-id="794b8-121">**Non** selezionare **Abilita supporto Docker**.</span><span class="sxs-lookup"><span data-stu-id="794b8-121">Do **not** select **Enable Docker Support**.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="794b8-122">Avviare l'app</span><span class="sxs-lookup"><span data-stu-id="794b8-122">Launch the app</span></span>

<span data-ttu-id="794b8-123">In Visual Studio premere CTRL+F5 per avviare l'app.</span><span class="sxs-lookup"><span data-stu-id="794b8-123">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="794b8-124">Visual Studio apre un browser e naviga all'indirizzo `http://localhost:<port>/api/values`, dove `<port>` è un numero di porta selezionato a caso.</span><span class="sxs-lookup"><span data-stu-id="794b8-124">Visual Studio launches a browser and navigates to `http://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="794b8-125">In Chrome, Microsoft Edge e Firefox viene visualizzato l'output seguente:</span><span class="sxs-lookup"><span data-stu-id="794b8-125">Chrome, Microsoft Edge, and Firefox display the following output:</span></span>

```json
["value1","value2"]
```

<span data-ttu-id="794b8-126">Se si usa Internet Explorer, viene richiesto di salvare un file *values.json*.</span><span class="sxs-lookup"><span data-stu-id="794b8-126">If using Internet Explorer, you'll be prompted to save a *values.json* file.</span></span>

### <a name="add-a-model-class"></a><span data-ttu-id="794b8-127">Aggiungere una classe del modello</span><span class="sxs-lookup"><span data-stu-id="794b8-127">Add a model class</span></span>

<span data-ttu-id="794b8-128">Un modello è un oggetto che rappresenta i dati nell'app.</span><span class="sxs-lookup"><span data-stu-id="794b8-128">A model is an object representing the data in the app.</span></span> <span data-ttu-id="794b8-129">In questo caso l'unico modello è un elemento attività.</span><span class="sxs-lookup"><span data-stu-id="794b8-129">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="794b8-130">In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto.</span><span class="sxs-lookup"><span data-stu-id="794b8-130">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="794b8-131">Selezionare **Aggiungi** > **Nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="794b8-131">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="794b8-132">Assegnare il nome *Modelli* alla cartella.</span><span class="sxs-lookup"><span data-stu-id="794b8-132">Name the folder *Models*.</span></span>

> [!NOTE]
> <span data-ttu-id="794b8-133">È possibile inserire le classi del modello in qualsiasi punto del progetto.</span><span class="sxs-lookup"><span data-stu-id="794b8-133">The model classes can go anywhere in the project.</span></span> <span data-ttu-id="794b8-134">ma per convenzione viene usata la cartella *Models*.</span><span class="sxs-lookup"><span data-stu-id="794b8-134">The *Models* folder is used by convention for model classes.</span></span>

<span data-ttu-id="794b8-135">In Esplora soluzioni fare clic con il pulsante destro del mouse sulla cartella *Models* e scegliere**Aggiungi** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="794b8-135">In Solution Explorer, right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="794b8-136">Assegnare alla classe il nome *TodoItem* e fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="794b8-136">Name the class *TodoItem* and click **Add**.</span></span>

<span data-ttu-id="794b8-137">Aggiornare la classe `TodoItem` con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="794b8-137">Update the `TodoItem` class with the following code:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="794b8-138">Il database genera `Id` quando viene creato un `TodoItem`.</span><span class="sxs-lookup"><span data-stu-id="794b8-138">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="794b8-139">Creare il contesto di database</span><span class="sxs-lookup"><span data-stu-id="794b8-139">Create the database context</span></span>

<span data-ttu-id="794b8-140">Il *contesto di database* è la classe principale che coordina le funzionalità di Entity Framework per un determinato modello di dati.</span><span class="sxs-lookup"><span data-stu-id="794b8-140">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="794b8-141">Questa classe viene creata mediante derivazione dalla classe `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="794b8-141">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="794b8-142">In Esplora soluzioni fare clic con il pulsante destro del mouse sulla cartella *Models* e scegliere**Aggiungi** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="794b8-142">In Solution Explorer, right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="794b8-143">Assegnare alla classe il nome *TodoContext* e fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="794b8-143">Name the class *TodoContext* and click **Add**.</span></span>

<span data-ttu-id="794b8-144">Sostituire la classe con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="794b8-144">Replace the class with the following code:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

### <a name="add-a-controller"></a><span data-ttu-id="794b8-145">Aggiungere un controller</span><span class="sxs-lookup"><span data-stu-id="794b8-145">Add a controller</span></span>

<span data-ttu-id="794b8-146">In Esplora soluzioni fare clic con il pulsante destro del mouse sulla cartella *Controllers*.</span><span class="sxs-lookup"><span data-stu-id="794b8-146">In Solution Explorer, right-click the *Controllers* folder.</span></span> <span data-ttu-id="794b8-147">Selezionare **Aggiungi** > **Nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="794b8-147">Select **Add** > **New Item**.</span></span> <span data-ttu-id="794b8-148">Nella finestra di dialogo **Aggiungi nuovo elemento** selezionare il modello **API Controller Class** (Classe controller API).</span><span class="sxs-lookup"><span data-stu-id="794b8-148">In the **Add New Item** dialog, select the **API Controller Class** template.</span></span> <span data-ttu-id="794b8-149">Assegnare alla classe il nome *TodoController* e fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="794b8-149">Name the class *TodoController*, and click **Add**.</span></span>

![Finestra di dialogo Aggiungi nuovo elemento con controller nella casella di ricerca e controller API Web selezionato](first-web-api/_static/new_controller.png)

<span data-ttu-id="794b8-151">Sostituire la classe con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="794b8-151">Replace the class with the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="794b8-152">Avviare l'app</span><span class="sxs-lookup"><span data-stu-id="794b8-152">Launch the app</span></span>

<span data-ttu-id="794b8-153">In Visual Studio premere CTRL+F5 per avviare l'app.</span><span class="sxs-lookup"><span data-stu-id="794b8-153">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="794b8-154">Visual Studio apre un browser e naviga all'indirizzo `http://localhost:<port>/api/values`, dove `<port>` è un numero di porta selezionato a caso.</span><span class="sxs-lookup"><span data-stu-id="794b8-154">Visual Studio launches a browser and navigates to `http://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="794b8-155">Passare al controller `Todo` all'indirizzo `http://localhost:<port>/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="794b8-155">Navigate to the `Todo` controller at `http://localhost:<port>/api/todo`.</span></span>

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]
