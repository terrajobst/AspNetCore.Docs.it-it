---
title: Creare un'API Web con ASP.NET Core e Visual Studio per Windows
author: rick-anderson
description: Compilare un'API Web con ASP.NET Core MVC e Visual Studio per Windows
ms.author: riande
ms.custom: mvc
ms.date: 05/17/2018
uid: tutorials/first-web-api
ms.openlocfilehash: 3da22cbbbe0db7771656997a13587521e182fb2a
ms.sourcegitcommit: 79b756ea03eae77a716f500ef88253ee9b1464d2
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/25/2018
ms.locfileid: "36277400"
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-windows"></a><span data-ttu-id="ba756-103">Creare un'API Web con ASP.NET Core e Visual Studio per Windows</span><span class="sxs-lookup"><span data-stu-id="ba756-103">Create a Web API with ASP.NET Core and Visual Studio for Windows</span></span>

<span data-ttu-id="ba756-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT) e [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="ba756-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="ba756-105">Questa esercitazione consente di creare un'API Web per la gestione di un elenco di elementi di tipo "attività"</span><span class="sxs-lookup"><span data-stu-id="ba756-105">This tutorial builds a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="ba756-106">e non un'interfaccia utente (UI).</span><span class="sxs-lookup"><span data-stu-id="ba756-106">A user interface (UI) isn't created.</span></span>

<span data-ttu-id="ba756-107">Sono disponibili tre versioni di questa esercitazione:</span><span class="sxs-lookup"><span data-stu-id="ba756-107">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="ba756-108">Windows: API Web con Visual Studio per Windows (questa esercitazione)</span><span class="sxs-lookup"><span data-stu-id="ba756-108">Windows: Web API with Visual Studio for Windows (This tutorial)</span></span>
* <span data-ttu-id="ba756-109">macOS: [API Web con Visual Studio per Mac](xref:tutorials/first-web-api-mac)</span><span class="sxs-lookup"><span data-stu-id="ba756-109">macOS: [Web API with Visual Studio for Mac](xref:tutorials/first-web-api-mac)</span></span>
* <span data-ttu-id="ba756-110">macOS, Linux, Windows: [API Web con Visual Studio Code](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="ba756-110">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[intro to web API](../includes/webApi/intro.md)]

## <a name="prerequisites"></a><span data-ttu-id="ba756-111">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="ba756-111">Prerequisites</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-windows.md)]

## <a name="create-the-project"></a><span data-ttu-id="ba756-112">Creare il progetto</span><span class="sxs-lookup"><span data-stu-id="ba756-112">Create the project</span></span>

<span data-ttu-id="ba756-113">Seguire questa procedura in Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="ba756-113">Follow these steps in Visual Studio:</span></span>

* <span data-ttu-id="ba756-114">Scegliere **Nuovo** > **Progetto** dal menu **File**.</span><span class="sxs-lookup"><span data-stu-id="ba756-114">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="ba756-115">Selezionare il modello **Applicazione Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="ba756-115">Select the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="ba756-116">Assegnare al progetto il nome *TodoApi* e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="ba756-116">Name the project *TodoApi* and click **OK**.</span></span>
* <span data-ttu-id="ba756-117">Nella finestra di dialogo **Nuova Applicazione Web ASP.NET Core - TodoApi** scegliere la versione per ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ba756-117">In the **New ASP.NET Core Web Application - TodoApi** dialog, choose the ASP.NET Core version.</span></span> <span data-ttu-id="ba756-118">Selezionare il modello **API**, quindi scegliere **OK**.</span><span class="sxs-lookup"><span data-stu-id="ba756-118">Select the **API** template and click **OK**.</span></span> <span data-ttu-id="ba756-119">**Non** selezionare **Abilita supporto Docker**.</span><span class="sxs-lookup"><span data-stu-id="ba756-119">Do **not** select **Enable Docker Support**.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="ba756-120">Avviare l'app</span><span class="sxs-lookup"><span data-stu-id="ba756-120">Launch the app</span></span>

<span data-ttu-id="ba756-121">In Visual Studio premere CTRL+F5 per avviare l'app.</span><span class="sxs-lookup"><span data-stu-id="ba756-121">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="ba756-122">Visual Studio apre un browser e naviga all'indirizzo `http://localhost:<port>/api/values`, dove `<port>` è un numero di porta selezionato a caso.</span><span class="sxs-lookup"><span data-stu-id="ba756-122">Visual Studio launches a browser and navigates to `http://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="ba756-123">In Chrome, Microsoft Edge e Firefox viene visualizzato l'output seguente:</span><span class="sxs-lookup"><span data-stu-id="ba756-123">Chrome, Microsoft Edge, and Firefox display the following output:</span></span>

```json
["value1","value2"]
```

<span data-ttu-id="ba756-124">Se si usa Internet Explorer, viene richiesto di salvare un file *values.json*.</span><span class="sxs-lookup"><span data-stu-id="ba756-124">If using Internet Explorer, you'll be prompted to save a *values.json* file.</span></span>

### <a name="add-a-model-class"></a><span data-ttu-id="ba756-125">Aggiungere una classe del modello</span><span class="sxs-lookup"><span data-stu-id="ba756-125">Add a model class</span></span>

<span data-ttu-id="ba756-126">Un modello è un oggetto che rappresenta i dati nell'app.</span><span class="sxs-lookup"><span data-stu-id="ba756-126">A model is an object representing the data in the app.</span></span> <span data-ttu-id="ba756-127">In questo caso l'unico modello è un elemento attività.</span><span class="sxs-lookup"><span data-stu-id="ba756-127">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="ba756-128">In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto.</span><span class="sxs-lookup"><span data-stu-id="ba756-128">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="ba756-129">Selezionare **Aggiungi** > **Nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="ba756-129">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="ba756-130">Assegnare il nome *Modelli* alla cartella.</span><span class="sxs-lookup"><span data-stu-id="ba756-130">Name the folder *Models*.</span></span>

> [!NOTE]
> <span data-ttu-id="ba756-131">È possibile inserire le classi del modello in qualsiasi punto del progetto.</span><span class="sxs-lookup"><span data-stu-id="ba756-131">The model classes can go anywhere in the project.</span></span> <span data-ttu-id="ba756-132">ma per convenzione viene usata la cartella *Models*.</span><span class="sxs-lookup"><span data-stu-id="ba756-132">The *Models* folder is used by convention for model classes.</span></span>

<span data-ttu-id="ba756-133">In Esplora soluzioni fare clic con il pulsante destro del mouse sulla cartella *Models* e scegliere**Aggiungi** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="ba756-133">In Solution Explorer, right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="ba756-134">Assegnare alla classe il nome *TodoItem* e fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="ba756-134">Name the class *TodoItem* and click **Add**.</span></span>

<span data-ttu-id="ba756-135">Aggiornare la classe `TodoItem` con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="ba756-135">Update the `TodoItem` class with the following code:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="ba756-136">Il database genera `Id` quando viene creato un `TodoItem`.</span><span class="sxs-lookup"><span data-stu-id="ba756-136">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="ba756-137">Creare il contesto di database</span><span class="sxs-lookup"><span data-stu-id="ba756-137">Create the database context</span></span>

<span data-ttu-id="ba756-138">Il *contesto di database* è la classe principale che coordina le funzionalità di Entity Framework per un determinato modello di dati.</span><span class="sxs-lookup"><span data-stu-id="ba756-138">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="ba756-139">Questa classe viene creata mediante derivazione dalla classe `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="ba756-139">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="ba756-140">In Esplora soluzioni fare clic con il pulsante destro del mouse sulla cartella *Models* e scegliere**Aggiungi** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="ba756-140">In Solution Explorer, right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="ba756-141">Assegnare alla classe il nome *TodoContext* e fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="ba756-141">Name the class *TodoContext* and click **Add**.</span></span>

<span data-ttu-id="ba756-142">Sostituire la classe con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="ba756-142">Replace the class with the following code:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

### <a name="add-a-controller"></a><span data-ttu-id="ba756-143">Aggiungere un controller</span><span class="sxs-lookup"><span data-stu-id="ba756-143">Add a controller</span></span>

<span data-ttu-id="ba756-144">In Esplora soluzioni fare clic con il pulsante destro del mouse sulla cartella *Controllers*.</span><span class="sxs-lookup"><span data-stu-id="ba756-144">In Solution Explorer, right-click the *Controllers* folder.</span></span> <span data-ttu-id="ba756-145">Selezionare **Aggiungi** > **Nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="ba756-145">Select **Add** > **New Item**.</span></span> <span data-ttu-id="ba756-146">Nella finestra di dialogo **Aggiungi nuovo elemento** selezionare il modello **API Controller Class** (Classe controller API).</span><span class="sxs-lookup"><span data-stu-id="ba756-146">In the **Add New Item** dialog, select the **API Controller Class** template.</span></span> <span data-ttu-id="ba756-147">Assegnare alla classe il nome *TodoController* e fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="ba756-147">Name the class *TodoController*, and click **Add**.</span></span>

![Finestra di dialogo Aggiungi nuovo elemento con controller nella casella di ricerca e controller API Web selezionato](first-web-api/_static/new_controller.png)

<span data-ttu-id="ba756-149">Sostituire la classe con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="ba756-149">Replace the class with the following code:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="ba756-150">Avviare l'app</span><span class="sxs-lookup"><span data-stu-id="ba756-150">Launch the app</span></span>

<span data-ttu-id="ba756-151">In Visual Studio premere CTRL+F5 per avviare l'app.</span><span class="sxs-lookup"><span data-stu-id="ba756-151">In Visual Studio, press CTRL+F5 to launch the app.</span></span> <span data-ttu-id="ba756-152">Visual Studio apre un browser e naviga all'indirizzo `http://localhost:<port>/api/values`, dove `<port>` è un numero di porta selezionato a caso.</span><span class="sxs-lookup"><span data-stu-id="ba756-152">Visual Studio launches a browser and navigates to `http://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="ba756-153">Passare al controller `Todo` all'indirizzo `http://localhost:<port>/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="ba756-153">Navigate to the `Todo` controller at `http://localhost:<port>/api/todo`.</span></span>

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]
