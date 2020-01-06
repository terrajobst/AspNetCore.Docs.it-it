---
title: "Esercitazione: creare un'API Web con ASP.NET Core"
author: rick-anderson
description: Informazioni su come creare un'API Web con ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 12/05/2019
uid: tutorials/first-web-api
ms.openlocfilehash: 3bf930d19684e84365f0ff0255fccd2939fb3f39
ms.sourcegitcommit: 2cb857f0de774df421e35289662ba92cfe56ffd1
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/25/2019
ms.locfileid: "75354914"
---
# <a name="tutorial-create-a-web-api-with-aspnet-core"></a><span data-ttu-id="f8173-103">Esercitazione: creare un'API Web con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f8173-103">Tutorial: Create a web API with ASP.NET Core</span></span>

<span data-ttu-id="f8173-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT) e [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="f8173-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="f8173-105">Questa esercitazione illustra le nozioni di base della creazione di un'API Web con ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f8173-105">This tutorial teaches the basics of building a web API with ASP.NET Core.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="f8173-106">In questa esercitazione si imparerà a:</span><span class="sxs-lookup"><span data-stu-id="f8173-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="f8173-107">Creare un progetto di API Web.</span><span class="sxs-lookup"><span data-stu-id="f8173-107">Create a web API project.</span></span>
> * <span data-ttu-id="f8173-108">Aggiungere una classe modello e un contesto di database.</span><span class="sxs-lookup"><span data-stu-id="f8173-108">Add a model class and a database context.</span></span>
> * <span data-ttu-id="f8173-109">Eseguire lo scaffolding di un controller con i metodi CRUD.</span><span class="sxs-lookup"><span data-stu-id="f8173-109">Scaffold a controller with CRUD methods.</span></span>
> * <span data-ttu-id="f8173-110">Configurare il routing, i percorsi URL e i valori restituiti.</span><span class="sxs-lookup"><span data-stu-id="f8173-110">Configure routing, URL paths, and return values.</span></span>
> * <span data-ttu-id="f8173-111">Chiamare l'API Web con Postman.</span><span class="sxs-lookup"><span data-stu-id="f8173-111">Call the web API with Postman.</span></span>

<span data-ttu-id="f8173-112">Al termine si avrà un'API Web che può gestire gli elementi di tipo "attività" archiviati in un database.</span><span class="sxs-lookup"><span data-stu-id="f8173-112">At the end, you have a web API that can manage "to-do" items stored in a database.</span></span>

## <a name="overview"></a><span data-ttu-id="f8173-113">Panoramica di</span><span class="sxs-lookup"><span data-stu-id="f8173-113">Overview</span></span>

<span data-ttu-id="f8173-114">Questa esercitazione consente di creare l'API seguente:</span><span class="sxs-lookup"><span data-stu-id="f8173-114">This tutorial creates the following API:</span></span>

|<span data-ttu-id="f8173-115">API</span><span class="sxs-lookup"><span data-stu-id="f8173-115">API</span></span> | <span data-ttu-id="f8173-116">Descrizione</span><span class="sxs-lookup"><span data-stu-id="f8173-116">Description</span></span> | <span data-ttu-id="f8173-117">Testo della richiesta</span><span class="sxs-lookup"><span data-stu-id="f8173-117">Request body</span></span> | <span data-ttu-id="f8173-118">Corpo della risposta</span><span class="sxs-lookup"><span data-stu-id="f8173-118">Response body</span></span> |
|--- | ---- | ---- | ---- |
|<span data-ttu-id="f8173-119">GET /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="f8173-119">GET /api/TodoItems</span></span> | <span data-ttu-id="f8173-120">Ottiene tutti gli elementi attività</span><span class="sxs-lookup"><span data-stu-id="f8173-120">Get all to-do items</span></span> | <span data-ttu-id="f8173-121">nessuna</span><span class="sxs-lookup"><span data-stu-id="f8173-121">None</span></span> | <span data-ttu-id="f8173-122">Matrice di elementi attività</span><span class="sxs-lookup"><span data-stu-id="f8173-122">Array of to-do items</span></span>|
|<span data-ttu-id="f8173-123">GET /api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="f8173-123">GET /api/TodoItems/{id}</span></span> | <span data-ttu-id="f8173-124">Ottiene un elemento in base all'ID</span><span class="sxs-lookup"><span data-stu-id="f8173-124">Get an item by ID</span></span> | <span data-ttu-id="f8173-125">nessuna</span><span class="sxs-lookup"><span data-stu-id="f8173-125">None</span></span> | <span data-ttu-id="f8173-126">Elemento attività</span><span class="sxs-lookup"><span data-stu-id="f8173-126">To-do item</span></span>|
|<span data-ttu-id="f8173-127">POST /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="f8173-127">POST /api/TodoItems</span></span> | <span data-ttu-id="f8173-128">Aggiunge un nuovo elemento</span><span class="sxs-lookup"><span data-stu-id="f8173-128">Add a new item</span></span> | <span data-ttu-id="f8173-129">Elemento attività</span><span class="sxs-lookup"><span data-stu-id="f8173-129">To-do item</span></span> | <span data-ttu-id="f8173-130">Elemento attività</span><span class="sxs-lookup"><span data-stu-id="f8173-130">To-do item</span></span> |
|<span data-ttu-id="f8173-131">PUT /api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="f8173-131">PUT /api/TodoItems/{id}</span></span> | <span data-ttu-id="f8173-132">Aggiorna un elemento esistente &nbsp;</span><span class="sxs-lookup"><span data-stu-id="f8173-132">Update an existing item &nbsp;</span></span> | <span data-ttu-id="f8173-133">Elemento attività</span><span class="sxs-lookup"><span data-stu-id="f8173-133">To-do item</span></span> | <span data-ttu-id="f8173-134">nessuna</span><span class="sxs-lookup"><span data-stu-id="f8173-134">None</span></span> |
|<span data-ttu-id="f8173-135">Elimina &nbsp;/api/TodoItems/{id} &nbsp;</span><span class="sxs-lookup"><span data-stu-id="f8173-135">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span></span> | <span data-ttu-id="f8173-136">Eliminare un elemento &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="f8173-136">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="f8173-137">nessuna</span><span class="sxs-lookup"><span data-stu-id="f8173-137">None</span></span> | <span data-ttu-id="f8173-138">nessuna</span><span class="sxs-lookup"><span data-stu-id="f8173-138">None</span></span>|

<span data-ttu-id="f8173-139">Il diagramma seguente visualizza la struttura dell'app.</span><span class="sxs-lookup"><span data-stu-id="f8173-139">The following diagram shows the design of the app.</span></span>

![Il client è rappresentato da una casella a sinistra.](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="f8173-145">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="f8173-145">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f8173-146">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f8173-146">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.1.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f8173-147">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f8173-147">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.1.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f8173-148">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="f8173-148">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.1.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="f8173-149">Creare un progetto Web</span><span class="sxs-lookup"><span data-stu-id="f8173-149">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f8173-150">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f8173-150">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f8173-151">Scegliere **nuovo** > **progetto**dal menu **file** .</span><span class="sxs-lookup"><span data-stu-id="f8173-151">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="f8173-152">Selezionare il modello **Applicazione Web ASP.NET Core** e fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="f8173-152">Select the **ASP.NET Core Web Application** template and click **Next**.</span></span>
* <span data-ttu-id="f8173-153">Assegnare al progetto il nome *TodoApi* e fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="f8173-153">Name the project *TodoApi* and click **Create**.</span></span>
* <span data-ttu-id="f8173-154">Nella finestra di dialogo **Crea una nuova applicazione Web ASP.NET Core** verificare che siano selezionati **.net Core** e **ASP.NET Core 3,1** .</span><span class="sxs-lookup"><span data-stu-id="f8173-154">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 3.1** are selected.</span></span> <span data-ttu-id="f8173-155">Selezionare il modello **API** e fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="f8173-155">Select the **API** template and click **Create**.</span></span>

![Finestra di dialogo Nuovo progetto di Visual Studio](first-web-api/_static/vs3.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f8173-157">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f8173-157">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="f8173-158">Aprire il [terminale integrato](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="f8173-158">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="f8173-159">Cambiare directory (`cd`) e passare alla cartella che conterrà la cartella del progetto.</span><span class="sxs-lookup"><span data-stu-id="f8173-159">Change directories (`cd`) to the folder that will contain the project folder.</span></span>
* <span data-ttu-id="f8173-160">Eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="f8173-160">Run the following commands:</span></span>

   ```dotnetcli
   dotnet new webapi -o TodoApi
   cd TodoApi
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.EntityFrameworkCore.InMemory
   code -r ../TodoApi
   ```

* <span data-ttu-id="f8173-161">Quando una finestra di dialogo chiede se si vuole aggiungere gli asset necessari al progetto, selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="f8173-161">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

  <span data-ttu-id="f8173-162">I comandi precedenti:</span><span class="sxs-lookup"><span data-stu-id="f8173-162">The preceding commands:</span></span>

  * <span data-ttu-id="f8173-163">Crea un nuovo progetto API Web e lo apre in Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="f8173-163">Creates a new web API project and opens it in Visual Studio Code.</span></span>
  * <span data-ttu-id="f8173-164">Aggiunge i pacchetti NuGet necessari nella sezione successiva.</span><span class="sxs-lookup"><span data-stu-id="f8173-164">Adds the NuGet packages which are required in the next section.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f8173-165">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="f8173-165">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="f8173-166">Selezionare **File** > **nuova soluzione**.</span><span class="sxs-lookup"><span data-stu-id="f8173-166">Select **File** > **New Solution**.</span></span>

  ![Nuova soluzione macOS](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="f8173-168">Selezionare **.NET Core** > **App** > **API** > **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="f8173-168">Select **.NET Core** > **App** > **API** > **Next**.</span></span>

  ![Finestra di dialogo Nuovo progetto di macOS](first-web-api-mac/_static/1.png)
  
* <span data-ttu-id="f8173-170">Nella finestra di dialogo **Configura nuova ASP.NET Core API Web** selezionare **Framework di destinazione** di \* *.NET Core 3,1*.</span><span class="sxs-lookup"><span data-stu-id="f8173-170">In the **Configure your new ASP.NET Core Web API** dialog, select **Target Framework** of \**.NET Core 3.1*.</span></span>

* <span data-ttu-id="f8173-171">Immettere *TodoApi* in **Nome progetto**, quindi selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="f8173-171">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![Finestra di dialogo di configurazione](first-web-api-mac/_static/2.png)

[!INCLUDE[](~/includes/mac-terminal-access.md)]

<span data-ttu-id="f8173-173">Aprire un terminale di comando nella cartella di progetto ed eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="f8173-173">Open a command terminal in the project folder and run the following commands:</span></span>

   ```dotnetcli
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.EntityFrameworkCore.InMemory
   ```

---

### <a name="test-the-api"></a><span data-ttu-id="f8173-174">Testare l'API</span><span class="sxs-lookup"><span data-stu-id="f8173-174">Test the API</span></span>

<span data-ttu-id="f8173-175">Il modello di progetto crea un'API `WeatherForecast`.</span><span class="sxs-lookup"><span data-stu-id="f8173-175">The project template creates a `WeatherForecast` API.</span></span> <span data-ttu-id="f8173-176">Chiamare il metodo `Get` da un browser per eseguire il test dell'app.</span><span class="sxs-lookup"><span data-stu-id="f8173-176">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f8173-177">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f8173-177">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="f8173-178">Premere CTRL+F5 per eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="f8173-178">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="f8173-179">Visual Studio apre un browser e naviga all'indirizzo `https://localhost:<port>/WeatherForecast`, dove `<port>` è un numero di porta selezionato a caso.</span><span class="sxs-lookup"><span data-stu-id="f8173-179">Visual Studio launches a browser and navigates to `https://localhost:<port>/WeatherForecast`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="f8173-180">Se in una finestra di dialogo viene chiesto se considerare attendibile il certificato IIS Express, selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="f8173-180">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="f8173-181">Nella finestra di dialogo **Avviso di sicurezza** successiva selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="f8173-181">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f8173-182">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f8173-182">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="f8173-183">Premere CTRL+F5 per eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="f8173-183">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="f8173-184">In un browser passare all'URL seguente: [https://localhost:5001/WeatherForecast](https://localhost:5001/WeatherForecast).</span><span class="sxs-lookup"><span data-stu-id="f8173-184">In a browser, go to following URL: [https://localhost:5001/WeatherForecast](https://localhost:5001/WeatherForecast).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f8173-185">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="f8173-185">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="f8173-186">Selezionare **Esegui** > **Avvia debug** per avviare l'app.</span><span class="sxs-lookup"><span data-stu-id="f8173-186">Select **Run** > **Start Debugging** to launch the app.</span></span> <span data-ttu-id="f8173-187">Visual Studio per Mac apre un browser e naviga all'indirizzo `https://localhost:<port>`, dove `<port>` è un numero di porta selezionato a caso.</span><span class="sxs-lookup"><span data-stu-id="f8173-187">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="f8173-188">Viene restituito un errore HTTP 404 (Non trovato).</span><span class="sxs-lookup"><span data-stu-id="f8173-188">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="f8173-189">Accodare `/WeatherForecast` all'URL (impostare l'URL su `https://localhost:<port>/WeatherForecast`).</span><span class="sxs-lookup"><span data-stu-id="f8173-189">Append `/WeatherForecast` to the URL (change the URL to `https://localhost:<port>/WeatherForecast`).</span></span>

---

<span data-ttu-id="f8173-190">Viene restituito un codice JSON simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="f8173-190">JSON similar to the following is returned:</span></span>

```json
[
    {
        "date": "2019-07-16T19:04:05.7257911-06:00",
        "temperatureC": 52,
        "temperatureF": 125,
        "summary": "Mild"
    },
    {
        "date": "2019-07-17T19:04:05.7258461-06:00",
        "temperatureC": 36,
        "temperatureF": 96,
        "summary": "Warm"
    },
    {
        "date": "2019-07-18T19:04:05.7258467-06:00",
        "temperatureC": 39,
        "temperatureF": 102,
        "summary": "Cool"
    },
    {
        "date": "2019-07-19T19:04:05.7258471-06:00",
        "temperatureC": 10,
        "temperatureF": 49,
        "summary": "Bracing"
    },
    {
        "date": "2019-07-20T19:04:05.7258474-06:00",
        "temperatureC": -1,
        "temperatureF": 31,
        "summary": "Chilly"
    }
]
```

## <a name="add-a-model-class"></a><span data-ttu-id="f8173-191">Aggiungere una classe del modello</span><span class="sxs-lookup"><span data-stu-id="f8173-191">Add a model class</span></span>

<span data-ttu-id="f8173-192">Un *modello* è un set di classi che rappresentano i dati gestiti dall'app.</span><span class="sxs-lookup"><span data-stu-id="f8173-192">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="f8173-193">Il modello per questa app è una classe `TodoItem` singola.</span><span class="sxs-lookup"><span data-stu-id="f8173-193">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f8173-194">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f8173-194">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f8173-195">In **Esplora soluzioni** fare clic con il pulsante destro del mouse sul progetto.</span><span class="sxs-lookup"><span data-stu-id="f8173-195">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="f8173-196">Selezionare **Aggiungi** > **Nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="f8173-196">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="f8173-197">Assegnare il nome *Modelli* alla cartella.</span><span class="sxs-lookup"><span data-stu-id="f8173-197">Name the folder *Models*.</span></span>

* <span data-ttu-id="f8173-198">Fare clic con il pulsante destro del mouse sulla cartella *Models* e scegliere **Aggiungi** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="f8173-198">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="f8173-199">Assegnare alla classe il nome *TodoItem* e selezionare **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="f8173-199">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="f8173-200">Sostituire il codice del modello con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="f8173-200">Replace the template code with the following code:</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f8173-201">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f8173-201">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="f8173-202">Aggiungere una cartella denominata *Modelli*.</span><span class="sxs-lookup"><span data-stu-id="f8173-202">Add a folder named *Models*.</span></span>

* <span data-ttu-id="f8173-203">Aggiungere una classe `TodoItem` alla cartella *Models* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="f8173-203">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f8173-204">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="f8173-204">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="f8173-205">Fare clic con il pulsante destro del mouse sul progetto.</span><span class="sxs-lookup"><span data-stu-id="f8173-205">Right-click the project.</span></span> <span data-ttu-id="f8173-206">Selezionare **Aggiungi** > **Nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="f8173-206">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="f8173-207">Assegnare il nome *Modelli* alla cartella.</span><span class="sxs-lookup"><span data-stu-id="f8173-207">Name the folder *Models*.</span></span>

  ![Nuova cartella](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="f8173-209">Fare clic con il pulsante destro del mouse sulla cartella *Models* e scegliere **Aggiungi** > **nuovo file** > **generale** > **classe vuota**.</span><span class="sxs-lookup"><span data-stu-id="f8173-209">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="f8173-210">Assegnare alla classe il nome *TodoItem* e fare clic su **Nuovo**.</span><span class="sxs-lookup"><span data-stu-id="f8173-210">Name the class *TodoItem*, and then click **New**.</span></span>

* <span data-ttu-id="f8173-211">Sostituire il codice del modello con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="f8173-211">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="f8173-212">La proprietà `Id` funziona come chiave univoca in un database relazionale.</span><span class="sxs-lookup"><span data-stu-id="f8173-212">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="f8173-213">È possibile inserire le classi del modello in qualsiasi punto del progetto, ma per convenzione viene usata la cartella *Models*.</span><span class="sxs-lookup"><span data-stu-id="f8173-213">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="f8173-214">Aggiungere un contesto di database</span><span class="sxs-lookup"><span data-stu-id="f8173-214">Add a database context</span></span>

<span data-ttu-id="f8173-215">Il *contesto di database* è la classe principale che coordina le funzionalità di Entity Framework per un modello di dati.</span><span class="sxs-lookup"><span data-stu-id="f8173-215">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="f8173-216">Questa classe viene creata mediante derivazione dalla classe `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="f8173-216">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f8173-217">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f8173-217">Visual Studio</span></span>](#tab/visual-studio)

### <a name="add-microsoftentityframeworkcoresqlserver"></a><span data-ttu-id="f8173-218">Aggiungere Microsoft.EntityFrameworkCore.SqlServer</span><span class="sxs-lookup"><span data-stu-id="f8173-218">Add Microsoft.EntityFrameworkCore.SqlServer</span></span>

* <span data-ttu-id="f8173-219">Dal menu **Strumenti** selezionare **Gestione pacchetti NuGet > Gestisci pacchetti NuGet per la soluzione**.</span><span class="sxs-lookup"><span data-stu-id="f8173-219">From the **Tools** menu, select **NuGet Package Manager > Manage NuGet Packages for Solution**.</span></span>
* <span data-ttu-id="f8173-220">Selezionare la scheda **Esplora** e quindi immettere **Microsoft.EntityFrameworkCore.SqlServer** nella casella di ricerca.</span><span class="sxs-lookup"><span data-stu-id="f8173-220">Select the **Browse** tab, and then enter **Microsoft.EntityFrameworkCore.SqlServer** in the search box.</span></span>
* <span data-ttu-id="f8173-221">Nel riquadro sinistro selezionare **Microsoft. EntityFrameworkCore. SqlServer** .</span><span class="sxs-lookup"><span data-stu-id="f8173-221">Select **Microsoft.EntityFrameworkCore.SqlServer** in the left pane.</span></span>
* <span data-ttu-id="f8173-222">Selezionare la casella di controllo **Progetto** nel riquadro di destra e quindi selezionare **Installa**.</span><span class="sxs-lookup"><span data-stu-id="f8173-222">Select the **Project** check box in the right pane and then select **Install**.</span></span>
* <span data-ttu-id="f8173-223">Usare le istruzioni precedenti per aggiungere il pacchetto NuGet `Microsoft.EntityFrameworkCore.InMemory`.</span><span class="sxs-lookup"><span data-stu-id="f8173-223">Use the preceding instructions to add the `Microsoft.EntityFrameworkCore.InMemory` NuGet package.</span></span>

![Gestione pacchetti NuGet](first-web-api/_static/vs3NuGet.png)

## <a name="add-the-todocontext-database-context"></a><span data-ttu-id="f8173-225">Aggiungere il contesto del database TodoContext</span><span class="sxs-lookup"><span data-stu-id="f8173-225">Add the TodoContext database context</span></span>

* <span data-ttu-id="f8173-226">Fare clic con il pulsante destro del mouse sulla cartella *Models* e scegliere **Aggiungi** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="f8173-226">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="f8173-227">Assegnare alla classe il nome *TodoContext* e fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="f8173-227">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="f8173-228">Visual Studio Code/Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="f8173-228">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="f8173-229">Aggiungere una classe `TodoContext` alla cartella *Models*.</span><span class="sxs-lookup"><span data-stu-id="f8173-229">Add a `TodoContext` class to the *Models* folder.</span></span>

---

* <span data-ttu-id="f8173-230">Immettere il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="f8173-230">Enter the following code:</span></span>

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="f8173-231">Registrare il contesto del database</span><span class="sxs-lookup"><span data-stu-id="f8173-231">Register the database context</span></span>

<span data-ttu-id="f8173-232">In ASP.NET Core i servizi come il contesto del database devono essere registrati con il [contenitore di inserimento delle dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="f8173-232">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="f8173-233">Il contenitore rende disponibile il servizio ai controller.</span><span class="sxs-lookup"><span data-stu-id="f8173-233">The container provides the service to controllers.</span></span>

<span data-ttu-id="f8173-234">Aggiornare *Startup.cs* con il codice evidenziato seguente:</span><span class="sxs-lookup"><span data-stu-id="f8173-234">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Startup.cs?highlight=7-8,23-24&name=snippet_all)]

<span data-ttu-id="f8173-235">Il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="f8173-235">The preceding code:</span></span>

* <span data-ttu-id="f8173-236">Rimuove le dichiarazioni `using` inutilizzate.</span><span class="sxs-lookup"><span data-stu-id="f8173-236">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="f8173-237">Aggiunge il contesto del database al contenitore di inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="f8173-237">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="f8173-238">Specifica che il contesto del database userà un database in memoria.</span><span class="sxs-lookup"><span data-stu-id="f8173-238">Specifies that the database context will use an in-memory database.</span></span>

## <a name="scaffold-a-controller"></a><span data-ttu-id="f8173-239">Eseguire lo scaffolding di un controller</span><span class="sxs-lookup"><span data-stu-id="f8173-239">Scaffold a controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f8173-240">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f8173-240">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f8173-241">Fare clic con il pulsante destro del mouse sulla cartella *Controllers*.</span><span class="sxs-lookup"><span data-stu-id="f8173-241">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="f8173-242">Selezionare **aggiungi** > **nuovo elemento con impalcatura**.</span><span class="sxs-lookup"><span data-stu-id="f8173-242">Select **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="f8173-243">Selezionare **Controller API con azioni, che usa Entity Framework** e quindi selezionare **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="f8173-243">Select **API Controller with actions, using Entity Framework**, and then select **Add**.</span></span>
* <span data-ttu-id="f8173-244">Nella finestra di dialogo **Add API Controller with actions, using Entity Framework** (Aggiungi controller API con azioni, che usa Entity Framework):</span><span class="sxs-lookup"><span data-stu-id="f8173-244">In the **Add API Controller with actions, using Entity Framework** dialog:</span></span>

  * <span data-ttu-id="f8173-245">Selezionare **TodoItem (TodoApi. Models)** nella **classe del modello**.</span><span class="sxs-lookup"><span data-stu-id="f8173-245">Select **TodoItem (TodoApi.Models)** in the **Model class**.</span></span>
  * <span data-ttu-id="f8173-246">Selezionare **TodoContext (TodoApi. Models)** nella **classe del contesto dati**.</span><span class="sxs-lookup"><span data-stu-id="f8173-246">Select **TodoContext (TodoApi.Models)** in the **Data context class**.</span></span>
  * <span data-ttu-id="f8173-247">Selezionare **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="f8173-247">Select **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="f8173-248">Visual Studio Code/Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="f8173-248">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="f8173-249">Eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="f8173-249">Run the following commands:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet tool install --global dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator controller -name TodoItemsController -async -api -m TodoItem -dc TodoContext -outDir Controllers
```

<span data-ttu-id="f8173-250">I comandi precedenti:</span><span class="sxs-lookup"><span data-stu-id="f8173-250">The preceding commands:</span></span>

* <span data-ttu-id="f8173-251">Aggiungere i pacchetti NuGet necessari per lo scaffolding.</span><span class="sxs-lookup"><span data-stu-id="f8173-251">Add NuGet packages required for scaffolding.</span></span>
* <span data-ttu-id="f8173-252">Installa il motore di scaffolding (`dotnet-aspnet-codegenerator`).</span><span class="sxs-lookup"><span data-stu-id="f8173-252">Installs the scaffolding engine (`dotnet-aspnet-codegenerator`).</span></span>
* <span data-ttu-id="f8173-253">Esegue lo scaffolding di `TodoItemsController`.</span><span class="sxs-lookup"><span data-stu-id="f8173-253">Scaffolds the `TodoItemsController`.</span></span>

---

<span data-ttu-id="f8173-254">Il codice generato:</span><span class="sxs-lookup"><span data-stu-id="f8173-254">The generated code:</span></span>

* <span data-ttu-id="f8173-255">Definisce una classe controller API senza metodi.</span><span class="sxs-lookup"><span data-stu-id="f8173-255">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="f8173-256">Contrassegna la classe con l'attributo [`[ApiController]`](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) .</span><span class="sxs-lookup"><span data-stu-id="f8173-256">Marks the class with the [`[ApiController]`](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute.</span></span> <span data-ttu-id="f8173-257">L'attributo indica che il controller risponde alle richieste di API Web.</span><span class="sxs-lookup"><span data-stu-id="f8173-257">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="f8173-258">Per informazioni sui comportamenti specifici consentiti dall'attributo, vedere <xref:web-api/index>.</span><span class="sxs-lookup"><span data-stu-id="f8173-258">For information about specific behaviors that the attribute enables, see <xref:web-api/index>.</span></span>
* <span data-ttu-id="f8173-259">Usa l'inserimento delle dipendenze per inserire il contesto del database (`TodoContext`) nel controller.</span><span class="sxs-lookup"><span data-stu-id="f8173-259">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="f8173-260">Il contesto di database viene usato in ognuno dei metodi [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) nel controller.</span><span class="sxs-lookup"><span data-stu-id="f8173-260">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

## <a name="examine-the-posttodoitem-create-method"></a><span data-ttu-id="f8173-261">Esaminare il metodo di creazione PostTodoItem</span><span class="sxs-lookup"><span data-stu-id="f8173-261">Examine the PostTodoItem create method</span></span>

<span data-ttu-id="f8173-262">Sostituire l'istruzione return in `PostTodoItem` per usare l'operatore [nameof](/dotnet/csharp/language-reference/operators/nameof):</span><span class="sxs-lookup"><span data-stu-id="f8173-262">Replace the return statement in the `PostTodoItem` to use the [nameof](/dotnet/csharp/language-reference/operators/nameof) operator:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Create)]

<span data-ttu-id="f8173-263">Il codice precedente è un metodo HTTP POST, come indicato dall'attributo [`[HttpPost]`](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) .</span><span class="sxs-lookup"><span data-stu-id="f8173-263">The preceding code is an HTTP POST method, as indicated by the [`[HttpPost]`](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="f8173-264">Il metodo ottiene il valore dell'elemento attività dal corpo della richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="f8173-264">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="f8173-265">Il metodo <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*>:</span><span class="sxs-lookup"><span data-stu-id="f8173-265">The <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> method:</span></span>

* <span data-ttu-id="f8173-266">Restituisce un codice di stato HTTP 201 in caso di esito positivo.</span><span class="sxs-lookup"><span data-stu-id="f8173-266">Returns an HTTP 201 status code if successful.</span></span> <span data-ttu-id="f8173-267">HTTP 201 è la risposta standard per un metodo HTTP POST che crea una nuova risorsa nel server.</span><span class="sxs-lookup"><span data-stu-id="f8173-267">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="f8173-268">Aggiunge un'intestazione [Location](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) alla risposta.</span><span class="sxs-lookup"><span data-stu-id="f8173-268">Adds a [Location](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) header to the response.</span></span> <span data-ttu-id="f8173-269">L'intestazione `Location` specifica l'[URI](https://developer.mozilla.org/docs/Glossary/URI) dell'elemento attività appena creato.</span><span class="sxs-lookup"><span data-stu-id="f8173-269">The `Location` header specifies the [URI](https://developer.mozilla.org/docs/Glossary/URI) of the newly created to-do item.</span></span> <span data-ttu-id="f8173-270">Per altre informazioni, vedere [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="f8173-270">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="f8173-271">Fa riferimento all'azione `GetTodoItem` per creare l'URI dell'intestazione `Location`.</span><span class="sxs-lookup"><span data-stu-id="f8173-271">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="f8173-272">La parola chiave `nameof` C# viene usata per evitare di impostare il nome dell'azione come hardcoded nella chiamata a `CreatedAtAction`.</span><span class="sxs-lookup"><span data-stu-id="f8173-272">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

### <a name="install-postman"></a><span data-ttu-id="f8173-273">Installare Postman</span><span class="sxs-lookup"><span data-stu-id="f8173-273">Install Postman</span></span>

<span data-ttu-id="f8173-274">Questa esercitazione usa Postman per testare l'API Web.</span><span class="sxs-lookup"><span data-stu-id="f8173-274">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="f8173-275">Installare [Postman](https://www.getpostman.com/downloads/)</span><span class="sxs-lookup"><span data-stu-id="f8173-275">Install [Postman](https://www.getpostman.com/downloads/)</span></span>
* <span data-ttu-id="f8173-276">Avviare l'app Web.</span><span class="sxs-lookup"><span data-stu-id="f8173-276">Start the web app.</span></span>
* <span data-ttu-id="f8173-277">Avviare Postman.</span><span class="sxs-lookup"><span data-stu-id="f8173-277">Start Postman.</span></span>
* <span data-ttu-id="f8173-278">Disattivare **SSL certificate verification** (Verifica certificato SSL)</span><span class="sxs-lookup"><span data-stu-id="f8173-278">Disable **SSL certificate verification**</span></span>
* <span data-ttu-id="f8173-279">In **File** > **Impostazioni** (scheda**generale** ) disabilitare la **Verifica del certificato SSL**.</span><span class="sxs-lookup"><span data-stu-id="f8173-279">From **File** > **Settings** (**General** tab), disable **SSL certificate verification**.</span></span>
    > [!WARNING]
    > <span data-ttu-id="f8173-280">Riattivare la verifica dei certificati SSL al termine del test del controller.</span><span class="sxs-lookup"><span data-stu-id="f8173-280">Re-enable SSL certificate verification after testing the controller.</span></span>

<a name="post"></a>

### <a name="test-posttodoitem-with-postman"></a><span data-ttu-id="f8173-281">Testare PostTodoItem con Postman</span><span class="sxs-lookup"><span data-stu-id="f8173-281">Test PostTodoItem with Postman</span></span>

* <span data-ttu-id="f8173-282">Creare una nuova richiesta.</span><span class="sxs-lookup"><span data-stu-id="f8173-282">Create a new request.</span></span>
* <span data-ttu-id="f8173-283">Impostare il metodo HTTP su `POST`.</span><span class="sxs-lookup"><span data-stu-id="f8173-283">Set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="f8173-284">Fare clic sulla scheda **Body** (Corpo).</span><span class="sxs-lookup"><span data-stu-id="f8173-284">Select the **Body** tab.</span></span>
* <span data-ttu-id="f8173-285">Selezionare il pulsante di opzione **raw** (non elaborato).</span><span class="sxs-lookup"><span data-stu-id="f8173-285">Select the **raw** radio button.</span></span>
* <span data-ttu-id="f8173-286">Impostare il tipo su **JSON (application/json)** .</span><span class="sxs-lookup"><span data-stu-id="f8173-286">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="f8173-287">Nel corpo della richiesta immettere codice JSON per un elemento attività:</span><span class="sxs-lookup"><span data-stu-id="f8173-287">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="f8173-288">Selezionare **Send** (Invia).</span><span class="sxs-lookup"><span data-stu-id="f8173-288">Select **Send**.</span></span>

  ![Postman con richiesta di creazione](first-web-api/_static/3/create.png)

### <a name="test-the-location-header-uri"></a><span data-ttu-id="f8173-290">Testare l'URI dell'intestazione della posizione</span><span class="sxs-lookup"><span data-stu-id="f8173-290">Test the location header URI</span></span>

* <span data-ttu-id="f8173-291">Selezionare la scheda **Headers** (Intestazioni) nel riquadro **Response** (Risposta).</span><span class="sxs-lookup"><span data-stu-id="f8173-291">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="f8173-292">Copiare il valore dell'intestazione **Location** (Posizione):</span><span class="sxs-lookup"><span data-stu-id="f8173-292">Copy the **Location** header value:</span></span>

  ![Scheda Headers (Intestazioni) della console Postman](first-web-api/_static/3/create.png)

* <span data-ttu-id="f8173-294">Impostare il metodo su GET.</span><span class="sxs-lookup"><span data-stu-id="f8173-294">Set the method to GET.</span></span>
* <span data-ttu-id="f8173-295">Incollare l'URI, ad esempio `https://localhost:5001/api/TodoItems/1`.</span><span class="sxs-lookup"><span data-stu-id="f8173-295">Paste the URI (for example, `https://localhost:5001/api/TodoItems/1`).</span></span>
* <span data-ttu-id="f8173-296">Selezionare **Send** (Invia).</span><span class="sxs-lookup"><span data-stu-id="f8173-296">Select **Send**.</span></span>

## <a name="examine-the-get-methods"></a><span data-ttu-id="f8173-297">Esaminare i metodi GET</span><span class="sxs-lookup"><span data-stu-id="f8173-297">Examine the GET methods</span></span>

<span data-ttu-id="f8173-298">Questi metodi implementano due metodi GET:</span><span class="sxs-lookup"><span data-stu-id="f8173-298">These methods implement two GET endpoints:</span></span>

* `GET /api/TodoItems`
* `GET /api/TodoItems/{id}`

<span data-ttu-id="f8173-299">Testare l'app chiamando i due endpoint da un browser o da Postman.</span><span class="sxs-lookup"><span data-stu-id="f8173-299">Test the app by calling the two endpoints from a browser or Postman.</span></span> <span data-ttu-id="f8173-300">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="f8173-300">For example:</span></span>

* [https://localhost:5001/api/TodoItems](https://localhost:5001/api/TodoItems)
* [https://localhost:5001/api/TodoItems/1](https://localhost:5001/api/TodoItems/1)

<span data-ttu-id="f8173-301">Una risposta simile alla seguente viene generata dalla chiamata a `GetTodoItems`:</span><span class="sxs-lookup"><span data-stu-id="f8173-301">A response similar to the following is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

### <a name="test-get-with-postman"></a><span data-ttu-id="f8173-302">Testare Get con Postman</span><span class="sxs-lookup"><span data-stu-id="f8173-302">Test Get with Postman</span></span>

* <span data-ttu-id="f8173-303">Creare una nuova richiesta.</span><span class="sxs-lookup"><span data-stu-id="f8173-303">Create a new request.</span></span>
* <span data-ttu-id="f8173-304">Impostare il metodo HTTP su **GET**.</span><span class="sxs-lookup"><span data-stu-id="f8173-304">Set the HTTP method to **GET**.</span></span>
* <span data-ttu-id="f8173-305">Impostare l'URL della richiesta su `https://localhost:<port>/api/TodoItems`.</span><span class="sxs-lookup"><span data-stu-id="f8173-305">Set the request URL to `https://localhost:<port>/api/TodoItems`.</span></span> <span data-ttu-id="f8173-306">Ad esempio `https://localhost:5001/api/TodoItems`.</span><span class="sxs-lookup"><span data-stu-id="f8173-306">For example, `https://localhost:5001/api/TodoItems`.</span></span>
* <span data-ttu-id="f8173-307">Impostare **Two pane view** (Visualizzazione in due riquadri) in Postman.</span><span class="sxs-lookup"><span data-stu-id="f8173-307">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="f8173-308">Selezionare **Send** (Invia).</span><span class="sxs-lookup"><span data-stu-id="f8173-308">Select **Send**.</span></span>

<span data-ttu-id="f8173-309">Questa app usa un database in memoria.</span><span class="sxs-lookup"><span data-stu-id="f8173-309">This app uses an in-memory database.</span></span> <span data-ttu-id="f8173-310">Se l'app viene arrestata e avviata, la richiesta GET precedente non restituirà alcun dato.</span><span class="sxs-lookup"><span data-stu-id="f8173-310">If the app is stopped and started, the preceding GET request will not return any data.</span></span> <span data-ttu-id="f8173-311">Se non vengono restituiti dati, eseguire [POST](#post) per pubblicare i dati nell'app.</span><span class="sxs-lookup"><span data-stu-id="f8173-311">If no data is returned, [POST](#post) data to the app.</span></span>

## <a name="routing-and-url-paths"></a><span data-ttu-id="f8173-312">Routing e percorsi di URL</span><span class="sxs-lookup"><span data-stu-id="f8173-312">Routing and URL paths</span></span>

<span data-ttu-id="f8173-313">L'attributo [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) indica un metodo che risponde a una richiesta HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="f8173-313">The [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="f8173-314">Il percorso dell'URL per ogni metodo viene costruito nel modo seguente:</span><span class="sxs-lookup"><span data-stu-id="f8173-314">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="f8173-315">Iniziare con la stringa di modello nell'attributo `Route` del controller:</span><span class="sxs-lookup"><span data-stu-id="f8173-315">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=TodoController&highlight=1)]

* <span data-ttu-id="f8173-316">Sostituire `[controller]` con il nome del controller, ovvero, per convenzione, il nome della classe controller meno il suffisso "Controller".</span><span class="sxs-lookup"><span data-stu-id="f8173-316">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="f8173-317">In questo esempio il nome della classe controller è **TodoItems**Controller, quindi il nome del controller è "TodoItems".</span><span class="sxs-lookup"><span data-stu-id="f8173-317">For this sample, the controller class name is **TodoItems**Controller, so the controller name is "TodoItems".</span></span> <span data-ttu-id="f8173-318">Il [routing](xref:mvc/controllers/routing) ASP.NET Core non fa distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="f8173-318">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="f8173-319">Se l'attributo `[HttpGet]` ha un modello di route, ad esempio `[HttpGet("products")]`, aggiungerlo al percorso.</span><span class="sxs-lookup"><span data-stu-id="f8173-319">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="f8173-320">In questo esempio non si usa un modello.</span><span class="sxs-lookup"><span data-stu-id="f8173-320">This sample doesn't use a template.</span></span> <span data-ttu-id="f8173-321">Per altre informazioni, vedere [Routing con attributi Http[verb]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="f8173-321">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="f8173-322">Nel metodo `GetTodoItem` seguente, `"{id}"` è una variabile segnaposto per l'identificatore univoco dell'elemento attività.</span><span class="sxs-lookup"><span data-stu-id="f8173-322">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="f8173-323">Quando viene richiamato `GetTodoItem`, il valore di `"{id}"` nell'URL viene fornito al metodo nel parametro `id`.</span><span class="sxs-lookup"><span data-stu-id="f8173-323">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its `id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a><span data-ttu-id="f8173-324">Valori restituiti</span><span class="sxs-lookup"><span data-stu-id="f8173-324">Return values</span></span>

<span data-ttu-id="f8173-325">Il tipo restituito dei metodi `GetTodoItems` e `GetTodoItem` è [ActionResult\<T>](xref:web-api/action-return-types#actionresultt-type).</span><span class="sxs-lookup"><span data-stu-id="f8173-325">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="f8173-326">ASP.NET Core serializza automaticamente l'oggetto su [JSON](https://www.json.org/) e scrive il codice JSON nel corpo del messaggio di risposta.</span><span class="sxs-lookup"><span data-stu-id="f8173-326">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="f8173-327">Il codice di risposta per questo tipo restituito è 200, presupponendo che non esistano eccezioni non gestite.</span><span class="sxs-lookup"><span data-stu-id="f8173-327">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="f8173-328">Le eccezioni non gestite vengono convertite in errori 5xx.</span><span class="sxs-lookup"><span data-stu-id="f8173-328">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="f8173-329">I tipi restituiti `ActionResult` possono rappresentare un ampio intervallo di codici di stato HTTP.</span><span class="sxs-lookup"><span data-stu-id="f8173-329">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="f8173-330">Ad esempio, `GetTodoItem` può restituire due valori di stato diversi:</span><span class="sxs-lookup"><span data-stu-id="f8173-330">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="f8173-331">Se nessun elemento corrisponde all'ID richiesto, il metodo restituisce un codice di errore 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound).</span><span class="sxs-lookup"><span data-stu-id="f8173-331">If no item matches the requested ID, the method returns a 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) error code.</span></span>
* <span data-ttu-id="f8173-332">In caso contrario, il metodo restituisce il codice 200 con un corpo della risposta JSON.</span><span class="sxs-lookup"><span data-stu-id="f8173-332">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="f8173-333">La restituzione di `item` risulta in una risposta HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="f8173-333">Returning `item` results in an HTTP 200 response.</span></span>

## <a name="the-puttodoitem-method"></a><span data-ttu-id="f8173-334">Metodo PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="f8173-334">The PutTodoItem method</span></span>

<span data-ttu-id="f8173-335">Esaminare il metodo `PutTodoItem`:</span><span class="sxs-lookup"><span data-stu-id="f8173-335">Examine the `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Update)]

<span data-ttu-id="f8173-336">`PutTodoItem` è simile a `PostTodoItem` ma usa la richiesta HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="f8173-336">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="f8173-337">La risposta è [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="f8173-337">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="f8173-338">In base alla specifica HTTP, una richiesta PUT richiede che il client invii l'intera entità aggiornata e non solo le modifiche.</span><span class="sxs-lookup"><span data-stu-id="f8173-338">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="f8173-339">Per supportare gli aggiornamenti parziali, usare [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span><span class="sxs-lookup"><span data-stu-id="f8173-339">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="f8173-340">Se si riceve un errore chiamando `PutTodoItem`, chiamare `GET` per verificare che il database includa un elemento.</span><span class="sxs-lookup"><span data-stu-id="f8173-340">If you get an error calling `PutTodoItem`, call `GET` to ensure there's an item in the database.</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="f8173-341">Testare il metodo PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="f8173-341">Test the PutTodoItem method</span></span>

<span data-ttu-id="f8173-342">Questo esempio usa un database in memoria che deve essere inizializzato ogni volta che l'app viene avviata.</span><span class="sxs-lookup"><span data-stu-id="f8173-342">This sample uses an in-memory database that must be initialized each time the app is started.</span></span> <span data-ttu-id="f8173-343">Deve esistere un elemento nel database prima di eseguire una chiamata PUT.</span><span class="sxs-lookup"><span data-stu-id="f8173-343">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="f8173-344">Chiamare GET per assicurarsi che sia presente un elemento nel database prima di eseguire una chiamata PUT.</span><span class="sxs-lookup"><span data-stu-id="f8173-344">Call GET to insure there's an item in the database before making a PUT call.</span></span>

<span data-ttu-id="f8173-345">Aggiornare l'elemento attività con ID = 1 e impostarne il nome su "feed fish":</span><span class="sxs-lookup"><span data-stu-id="f8173-345">Update the to-do item that has ID = 1 and set its name to "feed fish":</span></span>

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="f8173-346">L'immagine seguente visualizza l'aggiornamento di Postman:</span><span class="sxs-lookup"><span data-stu-id="f8173-346">The following image shows the Postman update:</span></span>

![Console Postman con risposta 204 (No Content)](first-web-api/_static/3/pmcput.png)

## <a name="the-deletetodoitem-method"></a><span data-ttu-id="f8173-348">Metodo DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="f8173-348">The DeleteTodoItem method</span></span>

<span data-ttu-id="f8173-349">Esaminare il metodo `DeleteTodoItem`:</span><span class="sxs-lookup"><span data-stu-id="f8173-349">Examine the `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Delete)]

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="f8173-350">Testare il metodo DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="f8173-350">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="f8173-351">Usare Postman per eliminare un elemento attività:</span><span class="sxs-lookup"><span data-stu-id="f8173-351">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="f8173-352">Impostare il metodo su `DELETE`.</span><span class="sxs-lookup"><span data-stu-id="f8173-352">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="f8173-353">Impostare l'URI dell'oggetto da eliminare, ad esempio `https://localhost:5001/api/TodoItems/1`.</span><span class="sxs-lookup"><span data-stu-id="f8173-353">Set the URI of the object to delete (for example `https://localhost:5001/api/TodoItems/1`).</span></span>
* <span data-ttu-id="f8173-354">Selezionare **Send** (Invia).</span><span class="sxs-lookup"><span data-stu-id="f8173-354">Select **Send**.</span></span>

## <a name="call-the-web-api-with-javascript"></a><span data-ttu-id="f8173-355">Chiamare l'API Web con JavaScript</span><span class="sxs-lookup"><span data-stu-id="f8173-355">Call the web API with JavaScript</span></span>

<span data-ttu-id="f8173-356">Vedere [esercitazione: chiamare un'API web ASP.NET Core con JavaScript](xref:tutorials/web-api-javascript).</span><span class="sxs-lookup"><span data-stu-id="f8173-356">See [Tutorial: Call an ASP.NET Core web API with JavaScript](xref:tutorials/web-api-javascript).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="f8173-357">In questa esercitazione si imparerà a:</span><span class="sxs-lookup"><span data-stu-id="f8173-357">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="f8173-358">Creare un progetto di API Web.</span><span class="sxs-lookup"><span data-stu-id="f8173-358">Create a web API project.</span></span>
> * <span data-ttu-id="f8173-359">Aggiungere una classe modello e un contesto di database.</span><span class="sxs-lookup"><span data-stu-id="f8173-359">Add a model class and a database context.</span></span>
> * <span data-ttu-id="f8173-360">Aggiungere un controller.</span><span class="sxs-lookup"><span data-stu-id="f8173-360">Add a controller.</span></span>
> * <span data-ttu-id="f8173-361">Aggiungere metodi CRUD.</span><span class="sxs-lookup"><span data-stu-id="f8173-361">Add CRUD methods.</span></span>
> * <span data-ttu-id="f8173-362">Configurare routing e percorsi URL.</span><span class="sxs-lookup"><span data-stu-id="f8173-362">Configure routing and URL paths.</span></span>
> * <span data-ttu-id="f8173-363">Specificare valori restituiti.</span><span class="sxs-lookup"><span data-stu-id="f8173-363">Specify return values.</span></span>
> * <span data-ttu-id="f8173-364">Chiamare l'API Web con Postman.</span><span class="sxs-lookup"><span data-stu-id="f8173-364">Call the web API with Postman.</span></span>
> * <span data-ttu-id="f8173-365">Chiamare l'API Web con JavaScript.</span><span class="sxs-lookup"><span data-stu-id="f8173-365">Call the web API with JavaScript.</span></span>

<span data-ttu-id="f8173-366">Al termine si dispone di un'API web che può gestire gli elementi di tipo "attività" archiviati in un database relazionale.</span><span class="sxs-lookup"><span data-stu-id="f8173-366">At the end, you have a web API that can manage "to-do" items stored in a relational database.</span></span>

## <a name="overview"></a><span data-ttu-id="f8173-367">Panoramica di</span><span class="sxs-lookup"><span data-stu-id="f8173-367">Overview</span></span>

<span data-ttu-id="f8173-368">Questa esercitazione consente di creare l'API seguente:</span><span class="sxs-lookup"><span data-stu-id="f8173-368">This tutorial creates the following API:</span></span>

|<span data-ttu-id="f8173-369">API</span><span class="sxs-lookup"><span data-stu-id="f8173-369">API</span></span> | <span data-ttu-id="f8173-370">Descrizione</span><span class="sxs-lookup"><span data-stu-id="f8173-370">Description</span></span> | <span data-ttu-id="f8173-371">Testo della richiesta</span><span class="sxs-lookup"><span data-stu-id="f8173-371">Request body</span></span> | <span data-ttu-id="f8173-372">Corpo della risposta</span><span class="sxs-lookup"><span data-stu-id="f8173-372">Response body</span></span> |
|--- | ---- | ---- | ---- |
|<span data-ttu-id="f8173-373">GET /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="f8173-373">GET /api/TodoItems</span></span> | <span data-ttu-id="f8173-374">Ottiene tutti gli elementi attività</span><span class="sxs-lookup"><span data-stu-id="f8173-374">Get all to-do items</span></span> | <span data-ttu-id="f8173-375">nessuna</span><span class="sxs-lookup"><span data-stu-id="f8173-375">None</span></span> | <span data-ttu-id="f8173-376">Matrice di elementi attività</span><span class="sxs-lookup"><span data-stu-id="f8173-376">Array of to-do items</span></span>|
|<span data-ttu-id="f8173-377">GET /api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="f8173-377">GET /api/TodoItems/{id}</span></span> | <span data-ttu-id="f8173-378">Ottiene un elemento in base all'ID</span><span class="sxs-lookup"><span data-stu-id="f8173-378">Get an item by ID</span></span> | <span data-ttu-id="f8173-379">nessuna</span><span class="sxs-lookup"><span data-stu-id="f8173-379">None</span></span> | <span data-ttu-id="f8173-380">Elemento attività</span><span class="sxs-lookup"><span data-stu-id="f8173-380">To-do item</span></span>|
|<span data-ttu-id="f8173-381">POST /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="f8173-381">POST /api/TodoItems</span></span> | <span data-ttu-id="f8173-382">Aggiunge un nuovo elemento</span><span class="sxs-lookup"><span data-stu-id="f8173-382">Add a new item</span></span> | <span data-ttu-id="f8173-383">Elemento attività</span><span class="sxs-lookup"><span data-stu-id="f8173-383">To-do item</span></span> | <span data-ttu-id="f8173-384">Elemento attività</span><span class="sxs-lookup"><span data-stu-id="f8173-384">To-do item</span></span> |
|<span data-ttu-id="f8173-385">PUT /api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="f8173-385">PUT /api/TodoItems/{id}</span></span> | <span data-ttu-id="f8173-386">Aggiorna un elemento esistente &nbsp;</span><span class="sxs-lookup"><span data-stu-id="f8173-386">Update an existing item &nbsp;</span></span> | <span data-ttu-id="f8173-387">Elemento attività</span><span class="sxs-lookup"><span data-stu-id="f8173-387">To-do item</span></span> | <span data-ttu-id="f8173-388">nessuna</span><span class="sxs-lookup"><span data-stu-id="f8173-388">None</span></span> |
|<span data-ttu-id="f8173-389">Elimina &nbsp;/api/TodoItems/{id} &nbsp;</span><span class="sxs-lookup"><span data-stu-id="f8173-389">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span></span> | <span data-ttu-id="f8173-390">Eliminare un elemento &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="f8173-390">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="f8173-391">nessuna</span><span class="sxs-lookup"><span data-stu-id="f8173-391">None</span></span> | <span data-ttu-id="f8173-392">nessuna</span><span class="sxs-lookup"><span data-stu-id="f8173-392">None</span></span>|

<span data-ttu-id="f8173-393">Il diagramma seguente visualizza la struttura dell'app.</span><span class="sxs-lookup"><span data-stu-id="f8173-393">The following diagram shows the design of the app.</span></span>

![Il client è rappresentato da una casella a sinistra.](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="f8173-399">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="f8173-399">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f8173-400">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f8173-400">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f8173-401">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f8173-401">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f8173-402">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="f8173-402">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="f8173-403">Creare un progetto Web</span><span class="sxs-lookup"><span data-stu-id="f8173-403">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f8173-404">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f8173-404">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f8173-405">Scegliere **nuovo** > **progetto**dal menu **file** .</span><span class="sxs-lookup"><span data-stu-id="f8173-405">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="f8173-406">Selezionare il modello **Applicazione Web ASP.NET Core** e fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="f8173-406">Select the **ASP.NET Core Web Application** template and click **Next**.</span></span>
* <span data-ttu-id="f8173-407">Assegnare al progetto il nome *TodoApi* e fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="f8173-407">Name the project *TodoApi* and click **Create**.</span></span>
* <span data-ttu-id="f8173-408">Nella finestra di dialogo **Crea una nuova applicazione Web ASP.NET Core** verificare che siano selezionati **.NET Core** e **ASP.NET Core 2.2**.</span><span class="sxs-lookup"><span data-stu-id="f8173-408">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 2.2** are selected.</span></span> <span data-ttu-id="f8173-409">Selezionare il modello **API** e fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="f8173-409">Select the **API** template and click **Create**.</span></span> <span data-ttu-id="f8173-410">**Non** selezionare **Abilita supporto Docker**.</span><span class="sxs-lookup"><span data-stu-id="f8173-410">**Don't** select **Enable Docker Support**.</span></span>

![Finestra di dialogo Nuovo progetto di Visual Studio](first-web-api/_static/vs.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f8173-412">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f8173-412">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="f8173-413">Aprire il [terminale integrato](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="f8173-413">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="f8173-414">Cambiare directory (`cd`) e passare alla cartella che conterrà la cartella del progetto.</span><span class="sxs-lookup"><span data-stu-id="f8173-414">Change directories (`cd`) to the folder that will contain the project folder.</span></span>
* <span data-ttu-id="f8173-415">Eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="f8173-415">Run the following commands:</span></span>

   ```dotnetcli
   dotnet new webapi -o TodoApi
   code -r TodoApi
   ```

  <span data-ttu-id="f8173-416">Questi comandi creano un nuovo progetto API Web e aprono una nuova istanza di Visual Studio Code nella nuova cartella di progetto.</span><span class="sxs-lookup"><span data-stu-id="f8173-416">These commands create a new web API project and open a new instance of Visual Studio Code in the new project folder.</span></span>

* <span data-ttu-id="f8173-417">Quando una finestra di dialogo chiede se si vuole aggiungere gli asset necessari al progetto, selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="f8173-417">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f8173-418">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="f8173-418">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="f8173-419">Selezionare **File** > **nuova soluzione**.</span><span class="sxs-lookup"><span data-stu-id="f8173-419">Select **File** > **New Solution**.</span></span>

  ![Nuova soluzione macOS](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="f8173-421">Selezionare **.NET Core** > **App** > **API** > **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="f8173-421">Select **.NET Core** > **App** > **API** > **Next**.</span></span>

  ![Finestra di dialogo Nuovo progetto di macOS](first-web-api-mac/_static/1.png)
  
* <span data-ttu-id="f8173-423">Nella finestra di dialogo **Configure your new ASP.NET Core Web API** (Configura la nuova API Web ASP.NET Core), accettare l'impostazione predefinita di **Framework di destinazione**, ovvero \* *.NET Core 2.2*.</span><span class="sxs-lookup"><span data-stu-id="f8173-423">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of \**.NET Core 2.2*.</span></span>

* <span data-ttu-id="f8173-424">Immettere *TodoApi* in **Nome progetto**, quindi selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="f8173-424">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![Finestra di dialogo di configurazione](first-web-api-mac/_static/2.png)

---

### <a name="test-the-api"></a><span data-ttu-id="f8173-426">Testare l'API</span><span class="sxs-lookup"><span data-stu-id="f8173-426">Test the API</span></span>

<span data-ttu-id="f8173-427">Il modello di progetto crea un'API `values`.</span><span class="sxs-lookup"><span data-stu-id="f8173-427">The project template creates a `values` API.</span></span> <span data-ttu-id="f8173-428">Chiamare il metodo `Get` da un browser per eseguire il test dell'app.</span><span class="sxs-lookup"><span data-stu-id="f8173-428">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f8173-429">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f8173-429">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="f8173-430">Premere CTRL+F5 per eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="f8173-430">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="f8173-431">Visual Studio apre un browser e naviga all'indirizzo `https://localhost:<port>/api/values`, dove `<port>` è un numero di porta selezionato a caso.</span><span class="sxs-lookup"><span data-stu-id="f8173-431">Visual Studio launches a browser and navigates to `https://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="f8173-432">Se in una finestra di dialogo viene chiesto se considerare attendibile il certificato IIS Express, selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="f8173-432">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="f8173-433">Nella finestra di dialogo **Avviso di sicurezza** successiva selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="f8173-433">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f8173-434">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f8173-434">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="f8173-435">Premere CTRL+F5 per eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="f8173-435">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="f8173-436">In un browser passare all'URL seguente: [https://localhost:5001/api/values](https://localhost:5001/api/values).</span><span class="sxs-lookup"><span data-stu-id="f8173-436">In a browser, go to following URL: [https://localhost:5001/api/values](https://localhost:5001/api/values).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f8173-437">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="f8173-437">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="f8173-438">Selezionare **Esegui** > **Avvia debug** per avviare l'app.</span><span class="sxs-lookup"><span data-stu-id="f8173-438">Select **Run** > **Start Debugging** to launch the app.</span></span> <span data-ttu-id="f8173-439">Visual Studio per Mac apre un browser e naviga all'indirizzo `https://localhost:<port>`, dove `<port>` è un numero di porta selezionato a caso.</span><span class="sxs-lookup"><span data-stu-id="f8173-439">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="f8173-440">Viene restituito un errore HTTP 404 (Non trovato).</span><span class="sxs-lookup"><span data-stu-id="f8173-440">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="f8173-441">Accodare `/api/values` all'URL (impostare l'URL su `https://localhost:<port>/api/values`).</span><span class="sxs-lookup"><span data-stu-id="f8173-441">Append `/api/values` to the URL (change the URL to `https://localhost:<port>/api/values`).</span></span>

---

<span data-ttu-id="f8173-442">Viene restituito il codice JSON seguente:</span><span class="sxs-lookup"><span data-stu-id="f8173-442">The following JSON is returned:</span></span>

```json
["value1","value2"]
```

## <a name="add-a-model-class"></a><span data-ttu-id="f8173-443">Aggiungere una classe del modello</span><span class="sxs-lookup"><span data-stu-id="f8173-443">Add a model class</span></span>

<span data-ttu-id="f8173-444">Un *modello* è un set di classi che rappresentano i dati gestiti dall'app.</span><span class="sxs-lookup"><span data-stu-id="f8173-444">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="f8173-445">Il modello per questa app è una classe `TodoItem` singola.</span><span class="sxs-lookup"><span data-stu-id="f8173-445">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f8173-446">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f8173-446">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f8173-447">In **Esplora soluzioni** fare clic con il pulsante destro del mouse sul progetto.</span><span class="sxs-lookup"><span data-stu-id="f8173-447">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="f8173-448">Selezionare **Aggiungi** > **Nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="f8173-448">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="f8173-449">Assegnare il nome *Modelli* alla cartella.</span><span class="sxs-lookup"><span data-stu-id="f8173-449">Name the folder *Models*.</span></span>

* <span data-ttu-id="f8173-450">Fare clic con il pulsante destro del mouse sulla cartella *Models* e scegliere **Aggiungi** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="f8173-450">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="f8173-451">Assegnare alla classe il nome *TodoItem* e selezionare **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="f8173-451">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="f8173-452">Sostituire il codice del modello con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="f8173-452">Replace the template code with the following code:</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f8173-453">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f8173-453">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="f8173-454">Aggiungere una cartella denominata *Modelli*.</span><span class="sxs-lookup"><span data-stu-id="f8173-454">Add a folder named *Models*.</span></span>

* <span data-ttu-id="f8173-455">Aggiungere una classe `TodoItem` alla cartella *Models* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="f8173-455">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f8173-456">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="f8173-456">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="f8173-457">Fare clic con il pulsante destro del mouse sul progetto.</span><span class="sxs-lookup"><span data-stu-id="f8173-457">Right-click the project.</span></span> <span data-ttu-id="f8173-458">Selezionare **Aggiungi** > **Nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="f8173-458">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="f8173-459">Assegnare il nome *Modelli* alla cartella.</span><span class="sxs-lookup"><span data-stu-id="f8173-459">Name the folder *Models*.</span></span>

  ![Nuova cartella](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="f8173-461">Fare clic con il pulsante destro del mouse sulla cartella *Models* e scegliere **Aggiungi** > **nuovo file** > **generale** > **classe vuota**.</span><span class="sxs-lookup"><span data-stu-id="f8173-461">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="f8173-462">Assegnare alla classe il nome *TodoItem* e fare clic su **Nuovo**.</span><span class="sxs-lookup"><span data-stu-id="f8173-462">Name the class *TodoItem*, and then click **New**.</span></span>

* <span data-ttu-id="f8173-463">Sostituire il codice del modello con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="f8173-463">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="f8173-464">La proprietà `Id` funziona come chiave univoca in un database relazionale.</span><span class="sxs-lookup"><span data-stu-id="f8173-464">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="f8173-465">È possibile inserire le classi del modello in qualsiasi punto del progetto, ma per convenzione viene usata la cartella *Models*.</span><span class="sxs-lookup"><span data-stu-id="f8173-465">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="f8173-466">Aggiungere un contesto di database</span><span class="sxs-lookup"><span data-stu-id="f8173-466">Add a database context</span></span>

<span data-ttu-id="f8173-467">Il *contesto di database* è la classe principale che coordina le funzionalità di Entity Framework per un modello di dati.</span><span class="sxs-lookup"><span data-stu-id="f8173-467">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="f8173-468">Questa classe viene creata mediante derivazione dalla classe `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="f8173-468">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f8173-469">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f8173-469">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f8173-470">Fare clic con il pulsante destro del mouse sulla cartella *Models* e scegliere **Aggiungi** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="f8173-470">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="f8173-471">Assegnare alla classe il nome *TodoContext* e fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="f8173-471">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="f8173-472">Visual Studio Code/Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="f8173-472">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="f8173-473">Aggiungere una classe `TodoContext` alla cartella *Models*.</span><span class="sxs-lookup"><span data-stu-id="f8173-473">Add a `TodoContext` class to the *Models* folder.</span></span>

---

* <span data-ttu-id="f8173-474">Sostituire il codice del modello con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="f8173-474">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="f8173-475">Registrare il contesto del database</span><span class="sxs-lookup"><span data-stu-id="f8173-475">Register the database context</span></span>

<span data-ttu-id="f8173-476">In ASP.NET Core i servizi come il contesto del database devono essere registrati con il [contenitore di inserimento delle dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="f8173-476">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="f8173-477">Il contenitore rende disponibile il servizio ai controller.</span><span class="sxs-lookup"><span data-stu-id="f8173-477">The container provides the service to controllers.</span></span>

<span data-ttu-id="f8173-478">Aggiornare *Startup.cs* con il codice evidenziato seguente:</span><span class="sxs-lookup"><span data-stu-id="f8173-478">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup1.cs?highlight=5,8,25-26&name=snippet_all)]

<span data-ttu-id="f8173-479">Il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="f8173-479">The preceding code:</span></span>

* <span data-ttu-id="f8173-480">Rimuove le dichiarazioni `using` inutilizzate.</span><span class="sxs-lookup"><span data-stu-id="f8173-480">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="f8173-481">Aggiunge il contesto del database al contenitore di inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="f8173-481">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="f8173-482">Specifica che il contesto del database userà un database in memoria.</span><span class="sxs-lookup"><span data-stu-id="f8173-482">Specifies that the database context will use an in-memory database.</span></span>

## <a name="add-a-controller"></a><span data-ttu-id="f8173-483">Aggiungere un controller</span><span class="sxs-lookup"><span data-stu-id="f8173-483">Add a controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f8173-484">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f8173-484">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f8173-485">Fare clic con il pulsante destro del mouse sulla cartella *Controllers*.</span><span class="sxs-lookup"><span data-stu-id="f8173-485">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="f8173-486">Selezionare **aggiungi** > **nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="f8173-486">Select **Add** > **New Item**.</span></span>
* <span data-ttu-id="f8173-487">Nella finestra di dialogo **Aggiungi nuovo elemento** selezionare il modello **API Controller Class** (Classe controller API).</span><span class="sxs-lookup"><span data-stu-id="f8173-487">In the **Add New Item** dialog, select the **API Controller Class** template.</span></span>
* <span data-ttu-id="f8173-488">Assegnare alla classe il nome *TodoController* e selezionare **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="f8173-488">Name the class *TodoController*, and select **Add**.</span></span>

  ![Finestra di dialogo Aggiungi nuovo elemento con controller nella casella di ricerca e controller API Web selezionato](first-web-api/_static/new_controller.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="f8173-490">Visual Studio Code/Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="f8173-490">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="f8173-491">Nella cartella *Controllers* creare una classe denominata `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="f8173-491">In the *Controllers* folder, create a class named `TodoController`.</span></span>

---

* <span data-ttu-id="f8173-492">Sostituire il codice del modello con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="f8173-492">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="f8173-493">Il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="f8173-493">The preceding code:</span></span>

* <span data-ttu-id="f8173-494">Definisce una classe controller API senza metodi.</span><span class="sxs-lookup"><span data-stu-id="f8173-494">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="f8173-495">Contrassegna la classe con l'attributo [`[ApiController]`](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) .</span><span class="sxs-lookup"><span data-stu-id="f8173-495">Marks the class with the [`[ApiController]`](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute.</span></span> <span data-ttu-id="f8173-496">L'attributo indica che il controller risponde alle richieste di API Web.</span><span class="sxs-lookup"><span data-stu-id="f8173-496">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="f8173-497">Per informazioni sui comportamenti specifici consentiti dall'attributo, vedere <xref:web-api/index>.</span><span class="sxs-lookup"><span data-stu-id="f8173-497">For information about specific behaviors that the attribute enables, see <xref:web-api/index>.</span></span>
* <span data-ttu-id="f8173-498">Usa l'inserimento delle dipendenze per inserire il contesto del database (`TodoContext`) nel controller.</span><span class="sxs-lookup"><span data-stu-id="f8173-498">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="f8173-499">Il contesto di database viene usato in ognuno dei metodi [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) nel controller.</span><span class="sxs-lookup"><span data-stu-id="f8173-499">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>
* <span data-ttu-id="f8173-500">Aggiunge un elemento denominato `Item1` al database se il database è vuoto.</span><span class="sxs-lookup"><span data-stu-id="f8173-500">Adds an item named `Item1` to the database if the database is empty.</span></span> <span data-ttu-id="f8173-501">Questo codice si trova nel costruttore, quindi viene eseguito ogni volta che è presente una nuova richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="f8173-501">This code is in the constructor, so it runs every time there's a new HTTP request.</span></span> <span data-ttu-id="f8173-502">Se si eliminano tutti gli elementi, il costruttore crea di nuovo `Item1` quando viene nuovamente chiamato un metodo API.</span><span class="sxs-lookup"><span data-stu-id="f8173-502">If you delete all items, the constructor creates `Item1` again the next time an API method is called.</span></span> <span data-ttu-id="f8173-503">Potrebbe pertanto sembrare che l'eliminazione non abbia funzionato, mentre di fatto ha funzionato.</span><span class="sxs-lookup"><span data-stu-id="f8173-503">So it may look like the deletion didn't work when it actually did work.</span></span>

## <a name="add-get-methods"></a><span data-ttu-id="f8173-504">Aggiungere metodi Get</span><span class="sxs-lookup"><span data-stu-id="f8173-504">Add Get methods</span></span>

<span data-ttu-id="f8173-505">Per specificare un'API che recupera elementi attività, aggiungere i metodi seguenti alla classe `TodoController`:</span><span class="sxs-lookup"><span data-stu-id="f8173-505">To provide an API that retrieves to-do items, add the following methods to the `TodoController` class:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

<span data-ttu-id="f8173-506">Questi metodi implementano due metodi GET:</span><span class="sxs-lookup"><span data-stu-id="f8173-506">These methods implement two GET endpoints:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="f8173-507">Arrestare l'app se è ancora in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="f8173-507">Stop the app if it's still running.</span></span> <span data-ttu-id="f8173-508">Quindi eseguirla di nuovo per includere le modifiche più recenti.</span><span class="sxs-lookup"><span data-stu-id="f8173-508">Then run it again to include the latest changes.</span></span>

<span data-ttu-id="f8173-509">Testare l'app chiamando i due endpoint da un browser.</span><span class="sxs-lookup"><span data-stu-id="f8173-509">Test the app by calling the two endpoints from a browser.</span></span> <span data-ttu-id="f8173-510">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="f8173-510">For example:</span></span>

* `https://localhost:<port>/api/todo`
* `https://localhost:<port>/api/todo/1`

<span data-ttu-id="f8173-511">La chiamata a `GetTodoItems` genera la risposta HTTP seguente:</span><span class="sxs-lookup"><span data-stu-id="f8173-511">The following HTTP response is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

## <a name="routing-and-url-paths"></a><span data-ttu-id="f8173-512">Routing e percorsi di URL</span><span class="sxs-lookup"><span data-stu-id="f8173-512">Routing and URL paths</span></span>

<span data-ttu-id="f8173-513">L'attributo [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) indica un metodo che risponde a una richiesta HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="f8173-513">The [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="f8173-514">Il percorso dell'URL per ogni metodo viene costruito nel modo seguente:</span><span class="sxs-lookup"><span data-stu-id="f8173-514">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="f8173-515">Iniziare con la stringa di modello nell'attributo `Route` del controller:</span><span class="sxs-lookup"><span data-stu-id="f8173-515">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

* <span data-ttu-id="f8173-516">Sostituire `[controller]` con il nome del controller, ovvero, per convenzione, il nome della classe controller meno il suffisso "Controller".</span><span class="sxs-lookup"><span data-stu-id="f8173-516">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="f8173-517">In questo esempio il nome della classe controller è **Todo**Controller, pertanto il nome del controller è "todo".</span><span class="sxs-lookup"><span data-stu-id="f8173-517">For this sample, the controller class name is **Todo**Controller, so the controller name is "todo".</span></span> <span data-ttu-id="f8173-518">Il [routing](xref:mvc/controllers/routing) ASP.NET Core non fa distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="f8173-518">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="f8173-519">Se l'attributo `[HttpGet]` ha un modello di route, ad esempio `[HttpGet("products")]`, aggiungerlo al percorso.</span><span class="sxs-lookup"><span data-stu-id="f8173-519">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="f8173-520">In questo esempio non si usa un modello.</span><span class="sxs-lookup"><span data-stu-id="f8173-520">This sample doesn't use a template.</span></span> <span data-ttu-id="f8173-521">Per altre informazioni, vedere [Routing con attributi Http[verb]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="f8173-521">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="f8173-522">Nel metodo `GetTodoItem` seguente, `"{id}"` è una variabile segnaposto per l'identificatore univoco dell'elemento attività.</span><span class="sxs-lookup"><span data-stu-id="f8173-522">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="f8173-523">Quando viene chiamato `GetTodoItem`, il valore di `"{id}"` viene specificato per il metodo nel rispettivo parametro `id`.</span><span class="sxs-lookup"><span data-stu-id="f8173-523">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its`id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a><span data-ttu-id="f8173-524">Valori restituiti</span><span class="sxs-lookup"><span data-stu-id="f8173-524">Return values</span></span>

<span data-ttu-id="f8173-525">Il tipo restituito dei metodi `GetTodoItems` e `GetTodoItem` è [ActionResult\<T>](xref:web-api/action-return-types#actionresultt-type).</span><span class="sxs-lookup"><span data-stu-id="f8173-525">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="f8173-526">ASP.NET Core serializza automaticamente l'oggetto su [JSON](https://www.json.org/) e scrive il codice JSON nel corpo del messaggio di risposta.</span><span class="sxs-lookup"><span data-stu-id="f8173-526">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="f8173-527">Il codice di risposta per questo tipo restituito è 200, presupponendo che non esistano eccezioni non gestite.</span><span class="sxs-lookup"><span data-stu-id="f8173-527">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="f8173-528">Le eccezioni non gestite vengono convertite in errori 5xx.</span><span class="sxs-lookup"><span data-stu-id="f8173-528">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="f8173-529">I tipi restituiti `ActionResult` possono rappresentare un ampio intervallo di codici di stato HTTP.</span><span class="sxs-lookup"><span data-stu-id="f8173-529">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="f8173-530">Ad esempio, `GetTodoItem` può restituire due valori di stato diversi:</span><span class="sxs-lookup"><span data-stu-id="f8173-530">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="f8173-531">Se nessun elemento corrisponde all'ID richiesto, il metodo restituisce un codice di errore 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound).</span><span class="sxs-lookup"><span data-stu-id="f8173-531">If no item matches the requested ID, the method returns a 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) error code.</span></span>
* <span data-ttu-id="f8173-532">In caso contrario, il metodo restituisce il codice 200 con un corpo della risposta JSON.</span><span class="sxs-lookup"><span data-stu-id="f8173-532">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="f8173-533">La restituzione di `item` risulta in una risposta HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="f8173-533">Returning `item` results in an HTTP 200 response.</span></span>

## <a name="test-the-gettodoitems-method"></a><span data-ttu-id="f8173-534">Testare il metodo GetTodoItems</span><span class="sxs-lookup"><span data-stu-id="f8173-534">Test the GetTodoItems method</span></span>

<span data-ttu-id="f8173-535">Questa esercitazione usa Postman per testare l'API Web.</span><span class="sxs-lookup"><span data-stu-id="f8173-535">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="f8173-536">Installare il [post](https://www.getpostman.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="f8173-536">Install [Postman](https://www.getpostman.com/downloads/).</span></span>
* <span data-ttu-id="f8173-537">Avviare l'app Web.</span><span class="sxs-lookup"><span data-stu-id="f8173-537">Start the web app.</span></span>
* <span data-ttu-id="f8173-538">Avviare Postman.</span><span class="sxs-lookup"><span data-stu-id="f8173-538">Start Postman.</span></span>
* <span data-ttu-id="f8173-539">Disabilitare la **Verifica del certificato SSL**.</span><span class="sxs-lookup"><span data-stu-id="f8173-539">Disable **SSL certificate verification**.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f8173-540">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f8173-540">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f8173-541">In **File** > **Impostazioni** (scheda**generale** ) disabilitare la **Verifica del certificato SSL**.</span><span class="sxs-lookup"><span data-stu-id="f8173-541">From **File** > **Settings** (**General** tab), disable **SSL certificate verification**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="f8173-542">Visual Studio Code/Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="f8173-542">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="f8173-543">Da **Postman** > **Preferences** (Preferenze) (scheda **General** (Generale)) disabilitare **SSL certificate verification** (Verifica certificato SSL).</span><span class="sxs-lookup"><span data-stu-id="f8173-543">From **Postman** > **Preferences** (**General** tab), disable **SSL certificate verification**.</span></span> <span data-ttu-id="f8173-544">In alternativa, selezionare la chiave inglese e selezionare **Settings** (Impostazioni), quindi disabilitare la verifica del certificato SSL.</span><span class="sxs-lookup"><span data-stu-id="f8173-544">Alternatively, select the wrench and select **Settings**, then disable the SSL certificate verification.</span></span>

---
  
> [!WARNING]
> <span data-ttu-id="f8173-545">Riattivare la verifica dei certificati SSL al termine del test del controller.</span><span class="sxs-lookup"><span data-stu-id="f8173-545">Re-enable SSL certificate verification after testing the controller.</span></span>

* <span data-ttu-id="f8173-546">Creare una nuova richiesta.</span><span class="sxs-lookup"><span data-stu-id="f8173-546">Create a new request.</span></span>
  * <span data-ttu-id="f8173-547">Impostare il metodo HTTP su **GET**.</span><span class="sxs-lookup"><span data-stu-id="f8173-547">Set the HTTP method to **GET**.</span></span>
  * <span data-ttu-id="f8173-548">Impostare l'URL della richiesta su `https://localhost:<port>/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="f8173-548">Set the request URL to `https://localhost:<port>/api/todo`.</span></span> <span data-ttu-id="f8173-549">Ad esempio `https://localhost:5001/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="f8173-549">For example, `https://localhost:5001/api/todo`.</span></span>
* <span data-ttu-id="f8173-550">Impostare **Two pane view** (Visualizzazione in due riquadri) in Postman.</span><span class="sxs-lookup"><span data-stu-id="f8173-550">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="f8173-551">Selezionare **Send** (Invia).</span><span class="sxs-lookup"><span data-stu-id="f8173-551">Select **Send**.</span></span>

![Postman con richiesta GET](first-web-api/_static/2pv.png)

## <a name="add-a-create-method"></a><span data-ttu-id="f8173-553">Aggiungere un metodo Create</span><span class="sxs-lookup"><span data-stu-id="f8173-553">Add a Create method</span></span>

<span data-ttu-id="f8173-554">Aggiungere il metodo `PostTodoItem` seguente in *Controllers/TodoController.cs*:</span><span class="sxs-lookup"><span data-stu-id="f8173-554">Add the following `PostTodoItem` method inside of *Controllers/TodoController.cs*:</span></span> 

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="f8173-555">Il codice precedente è un metodo HTTP POST, come indicato dall'attributo [`[HttpPost]`](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) .</span><span class="sxs-lookup"><span data-stu-id="f8173-555">The preceding code is an HTTP POST method, as indicated by the [`[HttpPost]`](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="f8173-556">Il metodo ottiene il valore dell'elemento attività dal corpo della richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="f8173-556">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="f8173-557">Il metodo `CreatedAtAction`:</span><span class="sxs-lookup"><span data-stu-id="f8173-557">The `CreatedAtAction` method:</span></span>

* <span data-ttu-id="f8173-558">Restituisce un codice di stato HTTP 201 in caso di esito positivo.</span><span class="sxs-lookup"><span data-stu-id="f8173-558">Returns an HTTP 201 status code, if successful.</span></span> <span data-ttu-id="f8173-559">HTTP 201 è la risposta standard per un metodo HTTP POST che crea una nuova risorsa nel server.</span><span class="sxs-lookup"><span data-stu-id="f8173-559">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="f8173-560">Aggiunge un'intestazione `Location` alla risposta.</span><span class="sxs-lookup"><span data-stu-id="f8173-560">Adds a `Location` header to the response.</span></span> <span data-ttu-id="f8173-561">L'intestazione `Location` specifica l'URI dell'elemento attività appena creato.</span><span class="sxs-lookup"><span data-stu-id="f8173-561">The `Location` header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="f8173-562">Per altre informazioni, vedere [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="f8173-562">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="f8173-563">Fa riferimento all'azione `GetTodoItem` per creare l'URI dell'intestazione `Location`.</span><span class="sxs-lookup"><span data-stu-id="f8173-563">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="f8173-564">La parola chiave `nameof` C# viene usata per evitare di impostare il nome dell'azione come hardcoded nella chiamata a `CreatedAtAction`.</span><span class="sxs-lookup"><span data-stu-id="f8173-564">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

### <a name="test-the-posttodoitem-method"></a><span data-ttu-id="f8173-565">Testare il metodo PostTodoItem</span><span class="sxs-lookup"><span data-stu-id="f8173-565">Test the PostTodoItem method</span></span>

* <span data-ttu-id="f8173-566">Compilazione del progetto.</span><span class="sxs-lookup"><span data-stu-id="f8173-566">Build the project.</span></span>
* <span data-ttu-id="f8173-567">In Postman impostare il metodo HTTP su `POST`.</span><span class="sxs-lookup"><span data-stu-id="f8173-567">In Postman, set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="f8173-568">Fare clic sulla scheda **Body** (Corpo).</span><span class="sxs-lookup"><span data-stu-id="f8173-568">Select the **Body** tab.</span></span>
* <span data-ttu-id="f8173-569">Selezionare il pulsante di opzione **raw** (non elaborato).</span><span class="sxs-lookup"><span data-stu-id="f8173-569">Select the **raw** radio button.</span></span>
* <span data-ttu-id="f8173-570">Impostare il tipo su **JSON (application/json)** .</span><span class="sxs-lookup"><span data-stu-id="f8173-570">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="f8173-571">Nel corpo della richiesta immettere codice JSON per un elemento attività:</span><span class="sxs-lookup"><span data-stu-id="f8173-571">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="f8173-572">Selezionare **Send** (Invia).</span><span class="sxs-lookup"><span data-stu-id="f8173-572">Select **Send**.</span></span>

  ![Postman con richiesta di creazione](first-web-api/_static/create.png)

  <span data-ttu-id="f8173-574">Se viene visualizzato un errore HTTP 405 - Metodo non consentito, è probabile che il progetto non sia stato compilato dopo l'aggiunta del metodo `PostTodoItem`.</span><span class="sxs-lookup"><span data-stu-id="f8173-574">If you get a 405 Method Not Allowed error, it's probably the result of not compiling the project after adding the `PostTodoItem` method.</span></span>

### <a name="test-the-location-header-uri"></a><span data-ttu-id="f8173-575">Testare l'URI dell'intestazione della posizione</span><span class="sxs-lookup"><span data-stu-id="f8173-575">Test the location header URI</span></span>

* <span data-ttu-id="f8173-576">Selezionare la scheda **Headers** (Intestazioni) nel riquadro **Response** (Risposta).</span><span class="sxs-lookup"><span data-stu-id="f8173-576">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="f8173-577">Copiare il valore dell'intestazione **Location** (Posizione):</span><span class="sxs-lookup"><span data-stu-id="f8173-577">Copy the **Location** header value:</span></span>

  ![Scheda Headers (Intestazioni) della console Postman](first-web-api/_static/pmc2.png)

* <span data-ttu-id="f8173-579">Impostare il metodo su GET.</span><span class="sxs-lookup"><span data-stu-id="f8173-579">Set the method to GET.</span></span>
* <span data-ttu-id="f8173-580">Incollare l'URI, ad esempio `https://localhost:5001/api/Todo/2`.</span><span class="sxs-lookup"><span data-stu-id="f8173-580">Paste the URI (for example, `https://localhost:5001/api/Todo/2`).</span></span>
* <span data-ttu-id="f8173-581">Selezionare **Send** (Invia).</span><span class="sxs-lookup"><span data-stu-id="f8173-581">Select **Send**.</span></span>

## <a name="add-a-puttodoitem-method"></a><span data-ttu-id="f8173-582">Aggiungere un metodo PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="f8173-582">Add a PutTodoItem method</span></span>

<span data-ttu-id="f8173-583">Aggiungere il metodo `PutTodoItem` seguente:</span><span class="sxs-lookup"><span data-stu-id="f8173-583">Add the following `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

<span data-ttu-id="f8173-584">`PutTodoItem` è simile a `PostTodoItem` ma usa la richiesta HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="f8173-584">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="f8173-585">La risposta è [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="f8173-585">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="f8173-586">In base alla specifica HTTP, una richiesta PUT richiede che il client invii l'intera entità aggiornata e non solo le modifiche.</span><span class="sxs-lookup"><span data-stu-id="f8173-586">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="f8173-587">Per supportare gli aggiornamenti parziali, usare [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span><span class="sxs-lookup"><span data-stu-id="f8173-587">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="f8173-588">Se si riceve un errore chiamando `PutTodoItem`, chiamare `GET` per verificare che il database includa un elemento.</span><span class="sxs-lookup"><span data-stu-id="f8173-588">If you get an error calling `PutTodoItem`, call `GET` to ensure there's an item in the database.</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="f8173-589">Testare il metodo PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="f8173-589">Test the PutTodoItem method</span></span>

<span data-ttu-id="f8173-590">Questo esempio usa un database in memoria che deve essere inizializzato ogni volta che l'app viene avviata.</span><span class="sxs-lookup"><span data-stu-id="f8173-590">This sample uses an in-memory database that must be initialized each time the app is started.</span></span> <span data-ttu-id="f8173-591">Deve esistere un elemento nel database prima di eseguire una chiamata PUT.</span><span class="sxs-lookup"><span data-stu-id="f8173-591">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="f8173-592">Chiamare GET per assicurarsi che sia presente un elemento nel database prima di eseguire una chiamata PUT.</span><span class="sxs-lookup"><span data-stu-id="f8173-592">Call GET to insure there's an item in the database before making a PUT call.</span></span>

<span data-ttu-id="f8173-593">Aggiornare l'elemento attività con id = 1 e impostarne il nome su "feed fish":</span><span class="sxs-lookup"><span data-stu-id="f8173-593">Update the to-do item that has id = 1 and set its name to "feed fish":</span></span>

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="f8173-594">L'immagine seguente visualizza l'aggiornamento di Postman:</span><span class="sxs-lookup"><span data-stu-id="f8173-594">The following image shows the Postman update:</span></span>

![Console Postman con risposta 204 (No Content)](first-web-api/_static/pmcput.png)

## <a name="add-a-deletetodoitem-method"></a><span data-ttu-id="f8173-596">Aggiungere un metodo DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="f8173-596">Add a DeleteTodoItem method</span></span>

<span data-ttu-id="f8173-597">Aggiungere il metodo `DeleteTodoItem` seguente:</span><span class="sxs-lookup"><span data-stu-id="f8173-597">Add the following `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="f8173-598">La risposta `DeleteTodoItem` è [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="f8173-598">The `DeleteTodoItem` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="f8173-599">Testare il metodo DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="f8173-599">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="f8173-600">Usare Postman per eliminare un elemento attività:</span><span class="sxs-lookup"><span data-stu-id="f8173-600">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="f8173-601">Impostare il metodo su `DELETE`.</span><span class="sxs-lookup"><span data-stu-id="f8173-601">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="f8173-602">Impostare l'URI dell'oggetto da eliminare, ad esempio `https://localhost:5001/api/todo/1`.</span><span class="sxs-lookup"><span data-stu-id="f8173-602">Set the URI of the object to delete (for example `https://localhost:5001/api/todo/1`).</span></span>
* <span data-ttu-id="f8173-603">Selezionare **Send** (Invia).</span><span class="sxs-lookup"><span data-stu-id="f8173-603">Select **Send**.</span></span>

<span data-ttu-id="f8173-604">L'app di esempio consente di eliminare tutti gli elementi.</span><span class="sxs-lookup"><span data-stu-id="f8173-604">The sample app allows you to delete all the items.</span></span> <span data-ttu-id="f8173-605">Quando viene eliminato l'ultimo elemento, tuttavia, ne viene creato uno nuovo dal costruttore della classe modello alla successiva chiamata dell'API.</span><span class="sxs-lookup"><span data-stu-id="f8173-605">However, when the last item is deleted, a new one is created by the model class constructor the next time the API is called.</span></span>

## <a name="call-the-web-api-with-javascript"></a><span data-ttu-id="f8173-606">Chiamare l'API Web con JavaScript</span><span class="sxs-lookup"><span data-stu-id="f8173-606">Call the web API with JavaScript</span></span>

<span data-ttu-id="f8173-607">In questa sezione viene aggiunta una pagina HTML che usa JavaScript per chiamare l'API Web.</span><span class="sxs-lookup"><span data-stu-id="f8173-607">In this section, an HTML page is added that uses JavaScript to call the web API.</span></span> <span data-ttu-id="f8173-608">jQuery avvia la richiesta.</span><span class="sxs-lookup"><span data-stu-id="f8173-608">jQuery initiates the request.</span></span> <span data-ttu-id="f8173-609">JavaScript aggiorna la pagina con i dettagli della risposta dell'API Web.</span><span class="sxs-lookup"><span data-stu-id="f8173-609">JavaScript updates the page with the details from the web API's response.</span></span>

<span data-ttu-id="f8173-610">Configurare l'app per [servire file statici](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) e [abilitare il mapping del file predefinito](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) aggiornando *Startup.cs* con il codice evidenziato seguente:</span><span class="sxs-lookup"><span data-stu-id="f8173-610">Configure the app to [serve static files](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [enable default file mapping](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) by updating *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup.cs?highlight=14-15&name=snippet_configure)]

<span data-ttu-id="f8173-611">Creare una cartella *wwwroot* nella directory del progetto.</span><span class="sxs-lookup"><span data-stu-id="f8173-611">Create a *wwwroot* folder in the project directory.</span></span>

<span data-ttu-id="f8173-612">Aggiungere un file HTML denominato *index.html* alla directory *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="f8173-612">Add an HTML file named *index.html* to the *wwwroot* directory.</span></span> <span data-ttu-id="f8173-613">Sostituire il contenuto con il markup seguente:</span><span class="sxs-lookup"><span data-stu-id="f8173-613">Replace its contents with the following markup:</span></span>

[!code-html[](first-web-api/samples/2.2/TodoApi/wwwroot/index.html)]

<span data-ttu-id="f8173-614">Aggiungere un file JavaScript con nome *site.js* alla directory *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="f8173-614">Add a JavaScript file named *site.js* to the *wwwroot* directory.</span></span> <span data-ttu-id="f8173-615">Sostituire il contenuto con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="f8173-615">Replace its contents with the following code:</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_SiteJs)]

<span data-ttu-id="f8173-616">Può essere necessario modificare le impostazioni di avvio del progetto ASP.NET Core per il test della pagina HTML in locale:</span><span class="sxs-lookup"><span data-stu-id="f8173-616">A change to the ASP.NET Core project's launch settings may be required to test the HTML page locally:</span></span>

* <span data-ttu-id="f8173-617">Aprire *Properties\launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="f8173-617">Open *Properties\launchSettings.json*.</span></span>
* <span data-ttu-id="f8173-618">Rimuovere la proprietà `launchUrl` per forzare l'app ad aprire *index.html*, il file predefinito del progetto.</span><span class="sxs-lookup"><span data-stu-id="f8173-618">Remove the `launchUrl` property to force the app to open at *index.html*&mdash;the project's default file.</span></span>

<span data-ttu-id="f8173-619">In questo esempio vengono chiamati tutti i metodi CRUD dell'API Web.</span><span class="sxs-lookup"><span data-stu-id="f8173-619">This sample calls all of the CRUD methods of the web API.</span></span> <span data-ttu-id="f8173-620">Di seguito sono incluse le spiegazioni delle chiamate all'API.</span><span class="sxs-lookup"><span data-stu-id="f8173-620">Following are explanations of the calls to the API.</span></span>

### <a name="get-a-list-of-to-do-items"></a><span data-ttu-id="f8173-621">Ottenere un elenco di elementi attività</span><span class="sxs-lookup"><span data-stu-id="f8173-621">Get a list of to-do items</span></span>

<span data-ttu-id="f8173-622">jQuery invia una richiesta HTTP GET all'API Web, che restituisce JSON che rappresenta una matrice di elementi attività.</span><span class="sxs-lookup"><span data-stu-id="f8173-622">jQuery sends an HTTP GET request to the web API, which returns JSON representing an array of to-do items.</span></span> <span data-ttu-id="f8173-623">La funzione di callback `success` viene chiamata se la richiesta ha esito positivo.</span><span class="sxs-lookup"><span data-stu-id="f8173-623">The `success` callback function is invoked if the request succeeds.</span></span> <span data-ttu-id="f8173-624">Nel callback il modello DOM viene aggiornato con le informazioni dell'elemento attività.</span><span class="sxs-lookup"><span data-stu-id="f8173-624">In the callback, the DOM is updated with the to-do information.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_GetData)]

### <a name="add-a-to-do-item"></a><span data-ttu-id="f8173-625">Aggiungere un elemento attività</span><span class="sxs-lookup"><span data-stu-id="f8173-625">Add a to-do item</span></span>

<span data-ttu-id="f8173-626">jQuery invia una richiesta HTTP POST con l'elemento attività nel corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="f8173-626">jQuery sends an HTTP POST request with the to-do item in the request body.</span></span> <span data-ttu-id="f8173-627">Le opzioni `accepts` e `contentType` sono impostate su `application/json` per specificare il tipo di supporto ricevuto e inviato.</span><span class="sxs-lookup"><span data-stu-id="f8173-627">The `accepts` and `contentType` options are set to `application/json` to specify the media type being received and sent.</span></span> <span data-ttu-id="f8173-628">L'elemento attività viene convertito in JSON usando [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span><span class="sxs-lookup"><span data-stu-id="f8173-628">The to-do item is converted to JSON by using [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span></span> <span data-ttu-id="f8173-629">Quando l'API restituisce un codice di stato di esecuzione riuscita, viene chiamata la funzione `getData` per aggiornare la tabella HTML.</span><span class="sxs-lookup"><span data-stu-id="f8173-629">When the API returns a successful status code, the `getData` function is invoked to update the HTML table.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AddItem)]

### <a name="update-a-to-do-item"></a><span data-ttu-id="f8173-630">Aggiornare un elemento attività</span><span class="sxs-lookup"><span data-stu-id="f8173-630">Update a to-do item</span></span>

<span data-ttu-id="f8173-631">L'aggiornamento di un elemento attività è simile all'aggiunta di un elemento di questo tipo.</span><span class="sxs-lookup"><span data-stu-id="f8173-631">Updating a to-do item is similar to adding one.</span></span> <span data-ttu-id="f8173-632">L'`url` cambia per includere l'identificatore univoco dell'elemento e `type` è `PUT`.</span><span class="sxs-lookup"><span data-stu-id="f8173-632">The `url` changes to add the unique identifier of the item, and the `type` is `PUT`.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxPut)]

### <a name="delete-a-to-do-item"></a><span data-ttu-id="f8173-633">Eliminare un elemento attività</span><span class="sxs-lookup"><span data-stu-id="f8173-633">Delete a to-do item</span></span>

<span data-ttu-id="f8173-634">È possibile eliminare un elemento attività impostando `type` su `DELETE` nella chiamata AJAX e specificando l'identificatore univoco dell'elemento nell'URL.</span><span class="sxs-lookup"><span data-stu-id="f8173-634">Deleting a to-do item is accomplished by setting the `type` on the AJAX call to `DELETE` and specifying the item's unique identifier in the URL.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxDelete)]

::: moniker-end

<a name="auth"></a>

## <a name="add-authentication-support-to-a-web-api"></a><span data-ttu-id="f8173-635">Aggiungere il supporto per l'autenticazione a un'API Web</span><span class="sxs-lookup"><span data-stu-id="f8173-635">Add authentication support to a web API</span></span>

[!INCLUDE[](~/includes/IdentityServer4.md)]

## <a name="additional-resources"></a><span data-ttu-id="f8173-636">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="f8173-636">Additional resources</span></span>

<span data-ttu-id="f8173-637">[Visualizzare o scaricare il codice di esempio per questa esercitazione](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span><span class="sxs-lookup"><span data-stu-id="f8173-637">[View or download sample code for this tutorial](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span></span> <span data-ttu-id="f8173-638">Vedere [come scaricare un esempio](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="f8173-638">See [how to download](xref:index#how-to-download-a-sample).</span></span>

<span data-ttu-id="f8173-639">Per ulteriori informazioni, vedere le seguenti risorse:</span><span class="sxs-lookup"><span data-stu-id="f8173-639">For more information, see the following resources:</span></span>

* <xref:web-api/index>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:data/ef-rp/intro>
* <xref:mvc/controllers/routing>
* <xref:web-api/action-return-types>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/index>
* [<span data-ttu-id="f8173-640">Versione YouTube dell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="f8173-640">YouTube version of this tutorial</span></span>](https://www.youtube.com/watch?v=TTkhEyGBfAk)
