---
title: "Esercitazione: Creare un'API Web con ASP.NET Core"
author: rick-anderson
description: Informazioni su come creare un'API Web con ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 09/29/2019
uid: tutorials/first-web-api
ms.openlocfilehash: 7bb98fe5befa8eea80885d246da31ad87d5cfc2d
ms.sourcegitcommit: fe88748b762525cb490f7e39089a4760f6a73a24
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/30/2019
ms.locfileid: "71691207"
---
# <a name="tutorial-create-a-web-api-with-aspnet-core"></a><span data-ttu-id="319e0-103">Esercitazione: Creare un'API Web con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="319e0-103">Tutorial: Create a web API with ASP.NET Core</span></span>

<span data-ttu-id="319e0-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT) e [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="319e0-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="319e0-105">Questa esercitazione illustra le nozioni di base della creazione di un'API Web con ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="319e0-105">This tutorial teaches the basics of building a web API with ASP.NET Core.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="319e0-106">In questa esercitazione si imparerà a:</span><span class="sxs-lookup"><span data-stu-id="319e0-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="319e0-107">Creare un progetto di API Web.</span><span class="sxs-lookup"><span data-stu-id="319e0-107">Create a web API project.</span></span>
> * <span data-ttu-id="319e0-108">Aggiungere una classe modello e un contesto di database.</span><span class="sxs-lookup"><span data-stu-id="319e0-108">Add a model class and a database context.</span></span>
> * <span data-ttu-id="319e0-109">Eseguire lo scaffolding di un controller con i metodi CRUD.</span><span class="sxs-lookup"><span data-stu-id="319e0-109">Scaffold a controller with CRUD methods.</span></span>
> * <span data-ttu-id="319e0-110">Configurare il routing, i percorsi URL e i valori restituiti.</span><span class="sxs-lookup"><span data-stu-id="319e0-110">Configure routing, URL paths, and return values.</span></span>
> * <span data-ttu-id="319e0-111">Chiamare l'API Web con Postman.</span><span class="sxs-lookup"><span data-stu-id="319e0-111">Call the web API with Postman.</span></span>

<span data-ttu-id="319e0-112">Al termine si avrà un'API Web che può gestire gli elementi di tipo "attività" archiviati in un database.</span><span class="sxs-lookup"><span data-stu-id="319e0-112">At the end, you have a web API that can manage "to-do" items stored in a database.</span></span>

## <a name="overview"></a><span data-ttu-id="319e0-113">Panoramica</span><span class="sxs-lookup"><span data-stu-id="319e0-113">Overview</span></span>

<span data-ttu-id="319e0-114">Questa esercitazione consente di creare l'API seguente:</span><span class="sxs-lookup"><span data-stu-id="319e0-114">This tutorial creates the following API:</span></span>

|<span data-ttu-id="319e0-115">API</span><span class="sxs-lookup"><span data-stu-id="319e0-115">API</span></span> | <span data-ttu-id="319e0-116">Descrizione</span><span class="sxs-lookup"><span data-stu-id="319e0-116">Description</span></span> | <span data-ttu-id="319e0-117">Corpo della richiesta</span><span class="sxs-lookup"><span data-stu-id="319e0-117">Request body</span></span> | <span data-ttu-id="319e0-118">Corpo della risposta</span><span class="sxs-lookup"><span data-stu-id="319e0-118">Response body</span></span> |
|--- | ---- | ---- | ---- |
|<span data-ttu-id="319e0-119">GET /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="319e0-119">GET /api/TodoItems</span></span> | <span data-ttu-id="319e0-120">Ottiene tutti gli elementi attività</span><span class="sxs-lookup"><span data-stu-id="319e0-120">Get all to-do items</span></span> | <span data-ttu-id="319e0-121">nessuno</span><span class="sxs-lookup"><span data-stu-id="319e0-121">None</span></span> | <span data-ttu-id="319e0-122">Matrice di elementi attività</span><span class="sxs-lookup"><span data-stu-id="319e0-122">Array of to-do items</span></span>|
|<span data-ttu-id="319e0-123">GET /api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="319e0-123">GET /api/TodoItems/{id}</span></span> | <span data-ttu-id="319e0-124">Ottiene un elemento in base all'ID</span><span class="sxs-lookup"><span data-stu-id="319e0-124">Get an item by ID</span></span> | <span data-ttu-id="319e0-125">nessuno</span><span class="sxs-lookup"><span data-stu-id="319e0-125">None</span></span> | <span data-ttu-id="319e0-126">Elemento attività</span><span class="sxs-lookup"><span data-stu-id="319e0-126">To-do item</span></span>|
|<span data-ttu-id="319e0-127">POST /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="319e0-127">POST /api/TodoItems</span></span> | <span data-ttu-id="319e0-128">Aggiunge un nuovo elemento</span><span class="sxs-lookup"><span data-stu-id="319e0-128">Add a new item</span></span> | <span data-ttu-id="319e0-129">Elemento attività</span><span class="sxs-lookup"><span data-stu-id="319e0-129">To-do item</span></span> | <span data-ttu-id="319e0-130">Elemento attività</span><span class="sxs-lookup"><span data-stu-id="319e0-130">To-do item</span></span> |
|<span data-ttu-id="319e0-131">PUT /api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="319e0-131">PUT /api/TodoItems/{id}</span></span> | <span data-ttu-id="319e0-132">Aggiorna un elemento esistente &nbsp;</span><span class="sxs-lookup"><span data-stu-id="319e0-132">Update an existing item &nbsp;</span></span> | <span data-ttu-id="319e0-133">Elemento attività</span><span class="sxs-lookup"><span data-stu-id="319e0-133">To-do item</span></span> | <span data-ttu-id="319e0-134">nessuno</span><span class="sxs-lookup"><span data-stu-id="319e0-134">None</span></span> |
|<span data-ttu-id="319e0-135">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="319e0-135">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span></span> | <span data-ttu-id="319e0-136">Elimina un elemento &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="319e0-136">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="319e0-137">nessuno</span><span class="sxs-lookup"><span data-stu-id="319e0-137">None</span></span> | <span data-ttu-id="319e0-138">nessuno</span><span class="sxs-lookup"><span data-stu-id="319e0-138">None</span></span>|

<span data-ttu-id="319e0-139">Il diagramma seguente visualizza la struttura dell'app.</span><span class="sxs-lookup"><span data-stu-id="319e0-139">The following diagram shows the design of the app.</span></span>

![Il client è rappresentato da una casella a sinistra.](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="319e0-145">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="319e0-145">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="319e0-146">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="319e0-146">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="319e0-147">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="319e0-147">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="319e0-148">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="319e0-148">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="319e0-149">Creare un progetto Web</span><span class="sxs-lookup"><span data-stu-id="319e0-149">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="319e0-150">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="319e0-150">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="319e0-151">Scegliere **Nuovo** > **Progetto** dal menu **File**.</span><span class="sxs-lookup"><span data-stu-id="319e0-151">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="319e0-152">Selezionare il modello **Applicazione Web ASP.NET Core** e fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="319e0-152">Select the **ASP.NET Core Web Application** template and click **Next**.</span></span>
* <span data-ttu-id="319e0-153">Assegnare al progetto il nome *TodoApi* e fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="319e0-153">Name the project *TodoApi* and click **Create**.</span></span>
* <span data-ttu-id="319e0-154">Nella finestra di dialogo **Crea una nuova applicazione Web ASP.NET Core** verificare che siano selezionati **.NET Core** e **ASP.NET Core 3.0**.</span><span class="sxs-lookup"><span data-stu-id="319e0-154">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 3.0** are selected.</span></span> <span data-ttu-id="319e0-155">Selezionare il modello **API** e fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="319e0-155">Select the **API** template and click **Create**.</span></span>

![Finestra di dialogo Nuovo progetto di Visual Studio](first-web-api/_static/vs3.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="319e0-157">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="319e0-157">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="319e0-158">Aprire il [terminale integrato](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="319e0-158">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="319e0-159">Cambiare directory (`cd`) e passare alla cartella che conterrà la cartella del progetto.</span><span class="sxs-lookup"><span data-stu-id="319e0-159">Change directories (`cd`) to the folder that will contain the project folder.</span></span>
* <span data-ttu-id="319e0-160">Eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="319e0-160">Run the following commands:</span></span>

   ```dotnetcli
   dotnet new webapi -o TodoApi
   cd TodoApi
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.EntityFrameworkCore.InMemory
   code -r ../TodoApi
   ```

* <span data-ttu-id="319e0-161">Quando una finestra di dialogo chiede se si vuole aggiungere gli asset necessari al progetto, selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="319e0-161">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

  <span data-ttu-id="319e0-162">I comandi precedenti:</span><span class="sxs-lookup"><span data-stu-id="319e0-162">The preceding commands:</span></span>

  * <span data-ttu-id="319e0-163">Crea un nuovo progetto API Web e lo apre in Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="319e0-163">Creates a new web API project and opens it in Visual Studio Code.</span></span>
  * <span data-ttu-id="319e0-164">Aggiunge i pacchetti NuGet necessari nella sezione successiva.</span><span class="sxs-lookup"><span data-stu-id="319e0-164">Adds the NuGet packages which are required in the next section.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="319e0-165">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="319e0-165">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="319e0-166">Selezionare **File** > **Nuova soluzione**.</span><span class="sxs-lookup"><span data-stu-id="319e0-166">Select **File** > **New Solution**.</span></span>

  ![Nuova soluzione macOS](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="319e0-168">Selezionare **.NET Core** > **App** > **API** > **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="319e0-168">Select **.NET Core** > **App** > **API** > **Next**.</span></span>

  ![Finestra di dialogo Nuovo progetto di macOS](first-web-api-mac/_static/1.png)
  
* <span data-ttu-id="319e0-170">Nella finestra di dialogo **Configura la nuova API Web ASP.NET Core** selezionare il **framework di destinazione** \* *.NET Core 3.0*.</span><span class="sxs-lookup"><span data-stu-id="319e0-170">In the **Configure your new ASP.NET Core Web API** dialog, select **Target Framework** of \**.NET Core 3.0*.</span></span>

* <span data-ttu-id="319e0-171">Immettere *TodoApi* in **Nome progetto**, quindi selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="319e0-171">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![Finestra di dialogo di configurazione](first-web-api-mac/_static/2.png)

[!INCLUDE[](~/includes/mac-terminal-access.md)]

<span data-ttu-id="319e0-173">Aprire un terminale di comando nella cartella di progetto ed eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="319e0-173">Open a command terminal in the project folder and run the following commands:</span></span>

   ```dotnetcli
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.EntityFrameworkCore.InMemory
   ```

---

### <a name="test-the-api"></a><span data-ttu-id="319e0-174">Testare l'API</span><span class="sxs-lookup"><span data-stu-id="319e0-174">Test the API</span></span>

<span data-ttu-id="319e0-175">Il modello di progetto crea un'API `WeatherForecast`.</span><span class="sxs-lookup"><span data-stu-id="319e0-175">The project template creates a `WeatherForecast` API.</span></span> <span data-ttu-id="319e0-176">Chiamare il metodo `Get` da un browser per eseguire il test dell'app.</span><span class="sxs-lookup"><span data-stu-id="319e0-176">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="319e0-177">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="319e0-177">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="319e0-178">Premere CTRL+F5 per eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="319e0-178">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="319e0-179">Visual Studio apre un browser e naviga all'indirizzo `https://localhost:<port>/WeatherForecast`, dove `<port>` è un numero di porta selezionato a caso.</span><span class="sxs-lookup"><span data-stu-id="319e0-179">Visual Studio launches a browser and navigates to `https://localhost:<port>/WeatherForecast`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="319e0-180">Se in una finestra di dialogo viene chiesto se considerare attendibile il certificato IIS Express, selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="319e0-180">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="319e0-181">Nella finestra di dialogo **Avviso di sicurezza** successiva selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="319e0-181">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="319e0-182">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="319e0-182">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="319e0-183">Premere CTRL+F5 per eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="319e0-183">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="319e0-184">In un browser passare all'URL seguente: [https://localhost:5001/WeatherForecast](https://localhost:5001/WeatherForecast).</span><span class="sxs-lookup"><span data-stu-id="319e0-184">In a browser, go to following URL: [https://localhost:5001/WeatherForecast](https://localhost:5001/WeatherForecast).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="319e0-185">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="319e0-185">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="319e0-186">Selezionare **Esegui** > **Avvia debug** per avviare l'app.</span><span class="sxs-lookup"><span data-stu-id="319e0-186">Select **Run** > **Start Debugging** to launch the app.</span></span> <span data-ttu-id="319e0-187">Visual Studio per Mac apre un browser e naviga all'indirizzo `https://localhost:<port>`, dove `<port>` è un numero di porta selezionato a caso.</span><span class="sxs-lookup"><span data-stu-id="319e0-187">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="319e0-188">Viene restituito un errore HTTP 404 (Non trovato).</span><span class="sxs-lookup"><span data-stu-id="319e0-188">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="319e0-189">Accodare `/WeatherForecast` all'URL (impostare l'URL su `https://localhost:<port>/WeatherForecast`).</span><span class="sxs-lookup"><span data-stu-id="319e0-189">Append `/WeatherForecast` to the URL (change the URL to `https://localhost:<port>/WeatherForecast`).</span></span>

---

<span data-ttu-id="319e0-190">Viene restituito un codice JSON simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="319e0-190">JSON similar to the following is returned:</span></span>

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

## <a name="add-a-model-class"></a><span data-ttu-id="319e0-191">Aggiungere una classe del modello</span><span class="sxs-lookup"><span data-stu-id="319e0-191">Add a model class</span></span>

<span data-ttu-id="319e0-192">Un *modello* è un set di classi che rappresentano i dati gestiti dall'app.</span><span class="sxs-lookup"><span data-stu-id="319e0-192">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="319e0-193">Il modello per questa app è una classe `TodoItem` singola.</span><span class="sxs-lookup"><span data-stu-id="319e0-193">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="319e0-194">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="319e0-194">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="319e0-195">In **Esplora soluzioni** fare clic con il pulsante destro del mouse sul progetto.</span><span class="sxs-lookup"><span data-stu-id="319e0-195">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="319e0-196">Selezionare **Aggiungi** > **Nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="319e0-196">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="319e0-197">Assegnare il nome *Models* alla cartella.</span><span class="sxs-lookup"><span data-stu-id="319e0-197">Name the folder *Models*.</span></span>

* <span data-ttu-id="319e0-198">Fare clic con il pulsante destro del mouse sulla cartella *Models* e scegliere **Aggiungi** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="319e0-198">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="319e0-199">Assegnare alla classe il nome *TodoItem* e selezionare **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="319e0-199">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="319e0-200">Sostituire il codice del modello con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="319e0-200">Replace the template code with the following code:</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="319e0-201">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="319e0-201">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="319e0-202">Aggiungere una cartella denominata *Models*.</span><span class="sxs-lookup"><span data-stu-id="319e0-202">Add a folder named *Models*.</span></span>

* <span data-ttu-id="319e0-203">Aggiungere una classe `TodoItem` alla cartella *Models* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="319e0-203">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="319e0-204">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="319e0-204">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="319e0-205">Fare clic con il pulsante destro del mouse sul progetto.</span><span class="sxs-lookup"><span data-stu-id="319e0-205">Right-click the project.</span></span> <span data-ttu-id="319e0-206">Selezionare **Aggiungi** > **Nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="319e0-206">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="319e0-207">Assegnare il nome *Models* alla cartella.</span><span class="sxs-lookup"><span data-stu-id="319e0-207">Name the folder *Models*.</span></span>

  ![Nuova cartella](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="319e0-209">Fare clic con il pulsante destro del mouse sulla cartella *Models* e scegliere **Aggiungi** > **Nuovo file** > **Generale** > **Classe vuota**.</span><span class="sxs-lookup"><span data-stu-id="319e0-209">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="319e0-210">Assegnare alla classe il nome *TodoItem* e fare clic su **Nuovo**.</span><span class="sxs-lookup"><span data-stu-id="319e0-210">Name the class *TodoItem*, and then click **New**.</span></span>

* <span data-ttu-id="319e0-211">Sostituire il codice del modello con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="319e0-211">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="319e0-212">La proprietà `Id` funziona come chiave univoca in un database relazionale.</span><span class="sxs-lookup"><span data-stu-id="319e0-212">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="319e0-213">È possibile inserire le classi del modello in qualsiasi punto del progetto, ma per convenzione viene usata la cartella *Models*.</span><span class="sxs-lookup"><span data-stu-id="319e0-213">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="319e0-214">Aggiungere un contesto di database</span><span class="sxs-lookup"><span data-stu-id="319e0-214">Add a database context</span></span>

<span data-ttu-id="319e0-215">Il *contesto di database* è la classe principale che coordina le funzionalità di Entity Framework per un modello di dati.</span><span class="sxs-lookup"><span data-stu-id="319e0-215">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="319e0-216">Questa classe viene creata mediante derivazione dalla classe `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="319e0-216">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="319e0-217">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="319e0-217">Visual Studio</span></span>](#tab/visual-studio)

### <a name="add-microsoftentityframeworkcoresqlserver"></a><span data-ttu-id="319e0-218">Aggiungere Microsoft.EntityFrameworkCore.SqlServer</span><span class="sxs-lookup"><span data-stu-id="319e0-218">Add Microsoft.EntityFrameworkCore.SqlServer</span></span>

* <span data-ttu-id="319e0-219">Dal menu **Strumenti** selezionare **Gestione pacchetti NuGet > Gestisci pacchetti NuGet per la soluzione**.</span><span class="sxs-lookup"><span data-stu-id="319e0-219">From the **Tools** menu, select **NuGet Package Manager > Manage NuGet Packages for Solution**.</span></span>
* <span data-ttu-id="319e0-220">Selezionare la scheda **Esplora** e quindi immettere **Microsoft.EntityFrameworkCore.SqlServer** nella casella di ricerca.</span><span class="sxs-lookup"><span data-stu-id="319e0-220">Select the **Browse** tab, and then enter **Microsoft.EntityFrameworkCore.SqlServer** in the search box.</span></span>
* <span data-ttu-id="319e0-221">Nel riquadro sinistro selezionare **Microsoft. EntityFrameworkCore. SqlServer** .</span><span class="sxs-lookup"><span data-stu-id="319e0-221">Select **Microsoft.EntityFrameworkCore.SqlServer** in the left pane.</span></span>
* <span data-ttu-id="319e0-222">Selezionare la casella di controllo **Progetto** nel riquadro di destra e quindi selezionare **Installa**.</span><span class="sxs-lookup"><span data-stu-id="319e0-222">Select the **Project** check box in the right pane and then select **Install**.</span></span>
* <span data-ttu-id="319e0-223">Usare le istruzioni precedenti per aggiungere il pacchetto NuGet `Microsoft.EntityFrameworkCore.InMemory`.</span><span class="sxs-lookup"><span data-stu-id="319e0-223">Use the preceding instructions to add the `Microsoft.EntityFrameworkCore.InMemory` NuGet package.</span></span>

![Gestione pacchetti NuGet](first-web-api/_static/vs3NuGet.png)

## <a name="add-the-todocontext-database-context"></a><span data-ttu-id="319e0-225">Aggiungere il contesto del database TodoContext</span><span class="sxs-lookup"><span data-stu-id="319e0-225">Add the TodoContext database context</span></span>

* <span data-ttu-id="319e0-226">Fare clic con il pulsante destro del mouse sulla cartella *Models* e scegliere **Aggiungi** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="319e0-226">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="319e0-227">Assegnare alla classe il nome *TodoContext* e fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="319e0-227">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="319e0-228">Visual Studio Code/Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="319e0-228">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="319e0-229">Aggiungere una classe `TodoContext` alla cartella *Models*.</span><span class="sxs-lookup"><span data-stu-id="319e0-229">Add a `TodoContext` class to the *Models* folder.</span></span>

---

* <span data-ttu-id="319e0-230">Immettere il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="319e0-230">Enter the following code:</span></span>

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="319e0-231">Registrare il contesto del database</span><span class="sxs-lookup"><span data-stu-id="319e0-231">Register the database context</span></span>

<span data-ttu-id="319e0-232">In ASP.NET Core i servizi come il contesto del database devono essere registrati con il [contenitore di inserimento delle dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="319e0-232">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="319e0-233">Il contenitore rende disponibile il servizio ai controller.</span><span class="sxs-lookup"><span data-stu-id="319e0-233">The container provides the service to controllers.</span></span>

<span data-ttu-id="319e0-234">Aggiornare *Startup.cs* con il codice evidenziato seguente:</span><span class="sxs-lookup"><span data-stu-id="319e0-234">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Startup.cs?highlight=7-8,23-24&name=snippet_all)]

<span data-ttu-id="319e0-235">Il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="319e0-235">The preceding code:</span></span>

* <span data-ttu-id="319e0-236">Rimuove le dichiarazioni `using` inutilizzate.</span><span class="sxs-lookup"><span data-stu-id="319e0-236">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="319e0-237">Aggiunge il contesto del database al contenitore di inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="319e0-237">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="319e0-238">Specifica che il contesto del database userà un database in memoria.</span><span class="sxs-lookup"><span data-stu-id="319e0-238">Specifies that the database context will use an in-memory database.</span></span>

## <a name="scaffold-a-controller"></a><span data-ttu-id="319e0-239">Eseguire lo scaffolding di un controller</span><span class="sxs-lookup"><span data-stu-id="319e0-239">Scaffold a controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="319e0-240">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="319e0-240">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="319e0-241">Fare clic con il pulsante destro del mouse sulla cartella *Controllers*.</span><span class="sxs-lookup"><span data-stu-id="319e0-241">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="319e0-242">Selezionare **Aggiungi** > **Nuovo elemento di scaffolding**.</span><span class="sxs-lookup"><span data-stu-id="319e0-242">Select **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="319e0-243">Selezionare **Controller API con azioni, che usa Entity Framework** e quindi selezionare **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="319e0-243">Select **API Controller with actions, using Entity Framework**, and then select **Add**.</span></span>
* <span data-ttu-id="319e0-244">Nella finestra di dialogo **Add API Controller with actions, using Entity Framework** (Aggiungi controller API con azioni, che usa Entity Framework):</span><span class="sxs-lookup"><span data-stu-id="319e0-244">In the **Add API Controller with actions, using Entity Framework** dialog:</span></span>

  * <span data-ttu-id="319e0-245">Selezionare **TodoItem (TodoApi. Models)** nella **classe del modello**.</span><span class="sxs-lookup"><span data-stu-id="319e0-245">Select **TodoItem (TodoApi.Models)** in the **Model class**.</span></span>
  * <span data-ttu-id="319e0-246">Selezionare **TodoContext (TodoApi. Models)** nella **classe del contesto dati**.</span><span class="sxs-lookup"><span data-stu-id="319e0-246">Select **TodoContext (TodoApi.Models)** in the **Data context class**.</span></span>
  * <span data-ttu-id="319e0-247">Selezionare **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="319e0-247">Select **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="319e0-248">Visual Studio Code/Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="319e0-248">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="319e0-249">Eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="319e0-249">Run the following commands:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet tool install --global dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator controller -name TodoItemsController -async -api -m TodoItem -dc TodoContext -outDir Controllers
```

<span data-ttu-id="319e0-250">I comandi precedenti:</span><span class="sxs-lookup"><span data-stu-id="319e0-250">The preceding commands:</span></span>

* <span data-ttu-id="319e0-251">Aggiungere i pacchetti NuGet necessari per lo scaffolding.</span><span class="sxs-lookup"><span data-stu-id="319e0-251">Add NuGet packages required for scaffolding.</span></span>
* <span data-ttu-id="319e0-252">Installa il motore di scaffolding (`dotnet-aspnet-codegenerator`).</span><span class="sxs-lookup"><span data-stu-id="319e0-252">Installs the scaffolding engine (`dotnet-aspnet-codegenerator`).</span></span>
* <span data-ttu-id="319e0-253">Esegue lo scaffolding di `TodoItemsController`.</span><span class="sxs-lookup"><span data-stu-id="319e0-253">Scaffolds the `TodoItemsController`.</span></span>

---

<span data-ttu-id="319e0-254">Il codice generato:</span><span class="sxs-lookup"><span data-stu-id="319e0-254">The generated code:</span></span>

* <span data-ttu-id="319e0-255">Definisce una classe controller API senza metodi.</span><span class="sxs-lookup"><span data-stu-id="319e0-255">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="319e0-256">Contrassegna la classe con l'attributo [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute).</span><span class="sxs-lookup"><span data-stu-id="319e0-256">Decorates the class with the [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute.</span></span> <span data-ttu-id="319e0-257">L'attributo indica che il controller risponde alle richieste di API Web.</span><span class="sxs-lookup"><span data-stu-id="319e0-257">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="319e0-258">Per informazioni sui comportamenti specifici consentiti dall'attributo, vedere <xref:web-api/index>.</span><span class="sxs-lookup"><span data-stu-id="319e0-258">For information about specific behaviors that the attribute enables, see <xref:web-api/index>.</span></span>
* <span data-ttu-id="319e0-259">Usa l'inserimento delle dipendenze per inserire il contesto del database (`TodoContext`) nel controller.</span><span class="sxs-lookup"><span data-stu-id="319e0-259">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="319e0-260">Il contesto di database viene usato in ognuno dei metodi [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) nel controller.</span><span class="sxs-lookup"><span data-stu-id="319e0-260">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

## <a name="examine-the-posttodoitem-create-method"></a><span data-ttu-id="319e0-261">Esaminare il metodo di creazione PostTodoItem</span><span class="sxs-lookup"><span data-stu-id="319e0-261">Examine the PostTodoItem create method</span></span>

<span data-ttu-id="319e0-262">Sostituire l'istruzione return in `PostTodoItem` per usare l'operatore [nameof](/dotnet/csharp/language-reference/operators/nameof):</span><span class="sxs-lookup"><span data-stu-id="319e0-262">Replace the return statement in the `PostTodoItem` to use the [nameof](/dotnet/csharp/language-reference/operators/nameof) operator:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Create)]

<span data-ttu-id="319e0-263">Il codice precedente è un metodo HTTP POST, come indicato dall'attributo [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute).</span><span class="sxs-lookup"><span data-stu-id="319e0-263">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="319e0-264">Il metodo ottiene il valore dell'elemento attività dal corpo della richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="319e0-264">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="319e0-265">Il metodo <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*>:</span><span class="sxs-lookup"><span data-stu-id="319e0-265">The <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> method:</span></span>

* <span data-ttu-id="319e0-266">Restituisce un codice di stato HTTP 201 in caso di esito positivo.</span><span class="sxs-lookup"><span data-stu-id="319e0-266">Returns an HTTP 201 status code if successful.</span></span> <span data-ttu-id="319e0-267">HTTP 201 è la risposta standard per un metodo HTTP POST che crea una nuova risorsa nel server.</span><span class="sxs-lookup"><span data-stu-id="319e0-267">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="319e0-268">Aggiunge un'intestazione [Location](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) alla risposta.</span><span class="sxs-lookup"><span data-stu-id="319e0-268">Adds a [Location](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) header to the response.</span></span> <span data-ttu-id="319e0-269">L'intestazione `Location` specifica l'[URI](https://developer.mozilla.org/docs/Glossary/URI) dell'elemento attività appena creato.</span><span class="sxs-lookup"><span data-stu-id="319e0-269">The `Location` header specifies the [URI](https://developer.mozilla.org/docs/Glossary/URI) of the newly created to-do item.</span></span> <span data-ttu-id="319e0-270">Per altre informazioni, vedere [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="319e0-270">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="319e0-271">Fa riferimento all'azione `GetTodoItem` per creare l'URI dell'intestazione `Location`.</span><span class="sxs-lookup"><span data-stu-id="319e0-271">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="319e0-272">La parola chiave `nameof` C# viene usata per evitare di impostare il nome dell'azione come hardcoded nella chiamata a `CreatedAtAction`.</span><span class="sxs-lookup"><span data-stu-id="319e0-272">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

### <a name="install-postman"></a><span data-ttu-id="319e0-273">Installare Postman</span><span class="sxs-lookup"><span data-stu-id="319e0-273">Install Postman</span></span>

<span data-ttu-id="319e0-274">Questa esercitazione usa Postman per testare l'API Web.</span><span class="sxs-lookup"><span data-stu-id="319e0-274">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="319e0-275">Installare [Postman](https://www.getpostman.com/downloads/)</span><span class="sxs-lookup"><span data-stu-id="319e0-275">Install [Postman](https://www.getpostman.com/downloads/)</span></span>
* <span data-ttu-id="319e0-276">Avviare l'app Web.</span><span class="sxs-lookup"><span data-stu-id="319e0-276">Start the web app.</span></span>
* <span data-ttu-id="319e0-277">Avviare Postman.</span><span class="sxs-lookup"><span data-stu-id="319e0-277">Start Postman.</span></span>
* <span data-ttu-id="319e0-278">Disattivare **SSL certificate verification** (Verifica certificato SSL)</span><span class="sxs-lookup"><span data-stu-id="319e0-278">Disable **SSL certificate verification**</span></span>
* <span data-ttu-id="319e0-279">Da **File** > **Settings** (Impostazioni) (scheda **General** (Generale)) disabilitare **SSL certificate verification** (Verifica certificato SSL).</span><span class="sxs-lookup"><span data-stu-id="319e0-279">From **File** > **Settings** (**General** tab), disable **SSL certificate verification**.</span></span>
    > [!WARNING]
    > <span data-ttu-id="319e0-280">Riattivare la verifica dei certificati SSL al termine del test del controller.</span><span class="sxs-lookup"><span data-stu-id="319e0-280">Re-enable SSL certificate verification after testing the controller.</span></span>

<a name="post"></a>

### <a name="test-posttodoitem-with-postman"></a><span data-ttu-id="319e0-281">Testare PostTodoItem con Postman</span><span class="sxs-lookup"><span data-stu-id="319e0-281">Test PostTodoItem with Postman</span></span>

* <span data-ttu-id="319e0-282">Creare una nuova richiesta.</span><span class="sxs-lookup"><span data-stu-id="319e0-282">Create a new request.</span></span>
* <span data-ttu-id="319e0-283">Impostare il metodo HTTP su `POST`.</span><span class="sxs-lookup"><span data-stu-id="319e0-283">Set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="319e0-284">Fare clic sulla scheda **Body** (Corpo).</span><span class="sxs-lookup"><span data-stu-id="319e0-284">Select the **Body** tab.</span></span>
* <span data-ttu-id="319e0-285">Selezionare il pulsante di opzione **raw** (non elaborato).</span><span class="sxs-lookup"><span data-stu-id="319e0-285">Select the **raw** radio button.</span></span>
* <span data-ttu-id="319e0-286">Impostare il tipo su **JSON (application/json)** .</span><span class="sxs-lookup"><span data-stu-id="319e0-286">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="319e0-287">Nel corpo della richiesta immettere codice JSON per un elemento attività:</span><span class="sxs-lookup"><span data-stu-id="319e0-287">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="319e0-288">Selezionare **Send** (Invia).</span><span class="sxs-lookup"><span data-stu-id="319e0-288">Select **Send**.</span></span>

  ![Postman con richiesta di creazione](first-web-api/_static/3/create.png)

### <a name="test-the-location-header-uri"></a><span data-ttu-id="319e0-290">Testare l'URI dell'intestazione della posizione</span><span class="sxs-lookup"><span data-stu-id="319e0-290">Test the location header URI</span></span>

* <span data-ttu-id="319e0-291">Selezionare la scheda **Headers** (Intestazioni) nel riquadro **Response** (Risposta).</span><span class="sxs-lookup"><span data-stu-id="319e0-291">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="319e0-292">Copiare il valore dell'intestazione **Location** (Posizione):</span><span class="sxs-lookup"><span data-stu-id="319e0-292">Copy the **Location** header value:</span></span>

  ![Scheda Headers (Intestazioni) della console Postman](first-web-api/_static/3/create.png)

* <span data-ttu-id="319e0-294">Impostare il metodo su GET.</span><span class="sxs-lookup"><span data-stu-id="319e0-294">Set the method to GET.</span></span>
* <span data-ttu-id="319e0-295">Incollare l'URI (ad esempio, `https://localhost:5001/api/TodoItems/1`).</span><span class="sxs-lookup"><span data-stu-id="319e0-295">Paste the URI (for example, `https://localhost:5001/api/TodoItems/1`).</span></span>
* <span data-ttu-id="319e0-296">Selezionare **Send** (Invia).</span><span class="sxs-lookup"><span data-stu-id="319e0-296">Select **Send**.</span></span>

## <a name="examine-the-get-methods"></a><span data-ttu-id="319e0-297">Esaminare i metodi GET</span><span class="sxs-lookup"><span data-stu-id="319e0-297">Examine the GET methods</span></span>

<span data-ttu-id="319e0-298">Questi metodi implementano due metodi GET:</span><span class="sxs-lookup"><span data-stu-id="319e0-298">These methods implement two GET endpoints:</span></span>

* `GET /api/TodoItems`
* `GET /api/TodoItems/{id}`

<span data-ttu-id="319e0-299">Testare l'app chiamando i due endpoint da un browser o da Postman.</span><span class="sxs-lookup"><span data-stu-id="319e0-299">Test the app by calling the two endpoints from a browser or Postman.</span></span> <span data-ttu-id="319e0-300">Esempio:</span><span class="sxs-lookup"><span data-stu-id="319e0-300">For example:</span></span>

* [https://localhost:5001/api/TodoItems](https://localhost:5001/api/TodoItems)
* [https://localhost:5001/api/TodoItems/1](https://localhost:5001/api/TodoItems/1)

<span data-ttu-id="319e0-301">Una risposta simile alla seguente viene generata dalla chiamata a `GetTodoItems`:</span><span class="sxs-lookup"><span data-stu-id="319e0-301">A response similar to the following is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

### <a name="test-get-with-postman"></a><span data-ttu-id="319e0-302">Testare Get con Postman</span><span class="sxs-lookup"><span data-stu-id="319e0-302">Test Get with Postman</span></span>

* <span data-ttu-id="319e0-303">Creare una nuova richiesta.</span><span class="sxs-lookup"><span data-stu-id="319e0-303">Create a new request.</span></span>
* <span data-ttu-id="319e0-304">Impostare il metodo HTTP su **GET**.</span><span class="sxs-lookup"><span data-stu-id="319e0-304">Set the HTTP method to **GET**.</span></span>
* <span data-ttu-id="319e0-305">Impostare l'URL della richiesta su `https://localhost:<port>/api/TodoItems`.</span><span class="sxs-lookup"><span data-stu-id="319e0-305">Set the request URL to `https://localhost:<port>/api/TodoItems`.</span></span> <span data-ttu-id="319e0-306">Ad esempio `https://localhost:5001/api/TodoItems`.</span><span class="sxs-lookup"><span data-stu-id="319e0-306">For example, `https://localhost:5001/api/TodoItems`.</span></span>
* <span data-ttu-id="319e0-307">Impostare **Two pane view** (Visualizzazione in due riquadri) in Postman.</span><span class="sxs-lookup"><span data-stu-id="319e0-307">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="319e0-308">Selezionare **Send** (Invia).</span><span class="sxs-lookup"><span data-stu-id="319e0-308">Select **Send**.</span></span>

<span data-ttu-id="319e0-309">Questa app usa un database in memoria.</span><span class="sxs-lookup"><span data-stu-id="319e0-309">This app uses an in-memory database.</span></span> <span data-ttu-id="319e0-310">Se l'app viene arrestata e avviata, la richiesta GET precedente non restituirà alcun dato.</span><span class="sxs-lookup"><span data-stu-id="319e0-310">If the app is stopped and started, the preceding GET request will not return any data.</span></span> <span data-ttu-id="319e0-311">Se non vengono restituiti dati, eseguire [POST](#post) per pubblicare i dati nell'app.</span><span class="sxs-lookup"><span data-stu-id="319e0-311">If no data is returned, [POST](#post) data to the app.</span></span>

## <a name="routing-and-url-paths"></a><span data-ttu-id="319e0-312">Routing e percorsi di URL</span><span class="sxs-lookup"><span data-stu-id="319e0-312">Routing and URL paths</span></span>

<span data-ttu-id="319e0-313">L'attributo [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) indica un metodo che risponde a una richiesta HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="319e0-313">The [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="319e0-314">Il percorso dell'URL per ogni metodo viene costruito nel modo seguente:</span><span class="sxs-lookup"><span data-stu-id="319e0-314">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="319e0-315">Iniziare con la stringa di modello nell'attributo `Route` del controller:</span><span class="sxs-lookup"><span data-stu-id="319e0-315">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=TodoController&highlight=1)]

* <span data-ttu-id="319e0-316">Sostituire `[controller]` con il nome del controller, ovvero, per convenzione, il nome della classe controller meno il suffisso "Controller".</span><span class="sxs-lookup"><span data-stu-id="319e0-316">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="319e0-317">In questo esempio il nome della classe controller è **TodoItems**Controller, quindi il nome del controller è "TodoItems".</span><span class="sxs-lookup"><span data-stu-id="319e0-317">For this sample, the controller class name is **TodoItems**Controller, so the controller name is "TodoItems".</span></span> <span data-ttu-id="319e0-318">Il [routing](xref:mvc/controllers/routing) ASP.NET Core non fa distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="319e0-318">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="319e0-319">Se l'attributo `[HttpGet]` ha un modello di route, ad esempio `[HttpGet("products")]`, aggiungerlo al percorso.</span><span class="sxs-lookup"><span data-stu-id="319e0-319">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="319e0-320">In questo esempio non si usa un modello.</span><span class="sxs-lookup"><span data-stu-id="319e0-320">This sample doesn't use a template.</span></span> <span data-ttu-id="319e0-321">Per altre informazioni, vedere [Routing con attributi Http[verb]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="319e0-321">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="319e0-322">Nel metodo `GetTodoItem` seguente, `"{id}"` è una variabile segnaposto per l'identificatore univoco dell'elemento attività.</span><span class="sxs-lookup"><span data-stu-id="319e0-322">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="319e0-323">Quando viene richiamato `GetTodoItem`, il valore di `"{id}"` nell'URL viene fornito al metodo nel parametro `id`.</span><span class="sxs-lookup"><span data-stu-id="319e0-323">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its `id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a><span data-ttu-id="319e0-324">Valori restituiti</span><span class="sxs-lookup"><span data-stu-id="319e0-324">Return values</span></span>

<span data-ttu-id="319e0-325">Il tipo restituito dei metodi `GetTodoItems` e `GetTodoItem` è [ActionResult\<T>](xref:web-api/action-return-types#actionresultt-type).</span><span class="sxs-lookup"><span data-stu-id="319e0-325">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="319e0-326">ASP.NET Core serializza automaticamente l'oggetto su [JSON](https://www.json.org/) e scrive il codice JSON nel corpo del messaggio di risposta.</span><span class="sxs-lookup"><span data-stu-id="319e0-326">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="319e0-327">Il codice di risposta per questo tipo restituito è 200, presupponendo che non esistano eccezioni non gestite.</span><span class="sxs-lookup"><span data-stu-id="319e0-327">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="319e0-328">Le eccezioni non gestite vengono convertite in errori 5xx.</span><span class="sxs-lookup"><span data-stu-id="319e0-328">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="319e0-329">I tipi restituiti `ActionResult` possono rappresentare un ampio intervallo di codici di stato HTTP.</span><span class="sxs-lookup"><span data-stu-id="319e0-329">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="319e0-330">Ad esempio, `GetTodoItem` può restituire due valori di stato diversi:</span><span class="sxs-lookup"><span data-stu-id="319e0-330">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="319e0-331">Se nessun elemento corrisponde all'ID richiesto, il metodo restituisce un codice di errore 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound).</span><span class="sxs-lookup"><span data-stu-id="319e0-331">If no item matches the requested ID, the method returns a 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) error code.</span></span>
* <span data-ttu-id="319e0-332">In caso contrario, il metodo restituisce il codice 200 con un corpo della risposta JSON.</span><span class="sxs-lookup"><span data-stu-id="319e0-332">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="319e0-333">La restituzione di `item` risulta in una risposta HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="319e0-333">Returning `item` results in an HTTP 200 response.</span></span>

## <a name="the-puttodoitem-method"></a><span data-ttu-id="319e0-334">Metodo PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="319e0-334">The PutTodoItem method</span></span>

<span data-ttu-id="319e0-335">Esaminare il metodo `PutTodoItem`:</span><span class="sxs-lookup"><span data-stu-id="319e0-335">Examine the `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Update)]

<span data-ttu-id="319e0-336">`PutTodoItem` è simile a `PostTodoItem` ma usa la richiesta HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="319e0-336">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="319e0-337">La risposta è [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="319e0-337">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="319e0-338">In base alla specifica HTTP, una richiesta PUT richiede che il client invii l'intera entità aggiornata e non solo le modifiche.</span><span class="sxs-lookup"><span data-stu-id="319e0-338">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="319e0-339">Per supportare gli aggiornamenti parziali, usare [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span><span class="sxs-lookup"><span data-stu-id="319e0-339">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="319e0-340">Se si riceve un errore chiamando `PutTodoItem`, chiamare `GET` per verificare che il database includa un elemento.</span><span class="sxs-lookup"><span data-stu-id="319e0-340">If you get an error calling `PutTodoItem`, call `GET` to ensure there's an item in the database.</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="319e0-341">Testare il metodo PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="319e0-341">Test the PutTodoItem method</span></span>

<span data-ttu-id="319e0-342">Questo esempio usa un database in memoria che deve essere inizializzato ogni volta che l'app viene avviata.</span><span class="sxs-lookup"><span data-stu-id="319e0-342">This sample uses an in-memory database that must be initialed each time the app is started.</span></span> <span data-ttu-id="319e0-343">Deve esistere un elemento nel database prima di eseguire una chiamata PUT.</span><span class="sxs-lookup"><span data-stu-id="319e0-343">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="319e0-344">Chiamare GET per assicurarsi che sia presente un elemento nel database prima di eseguire una chiamata PUT.</span><span class="sxs-lookup"><span data-stu-id="319e0-344">Call GET to insure there's an item in the database before making a PUT call.</span></span>

<span data-ttu-id="319e0-345">Aggiornare l'elemento attività con ID = 1 e impostarne il nome su "feed fish":</span><span class="sxs-lookup"><span data-stu-id="319e0-345">Update the to-do item that has ID = 1 and set its name to "feed fish":</span></span>

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="319e0-346">L'immagine seguente visualizza l'aggiornamento di Postman:</span><span class="sxs-lookup"><span data-stu-id="319e0-346">The following image shows the Postman update:</span></span>

![Console Postman con risposta 204 (No Content)](first-web-api/_static/3/pmcput.png)

## <a name="the-deletetodoitem-method"></a><span data-ttu-id="319e0-348">Metodo DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="319e0-348">The DeleteTodoItem method</span></span>

<span data-ttu-id="319e0-349">Esaminare il metodo `DeleteTodoItem`:</span><span class="sxs-lookup"><span data-stu-id="319e0-349">Examine the `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Delete)]

<span data-ttu-id="319e0-350">La risposta `DeleteTodoItem` è [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="319e0-350">The `DeleteTodoItem` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="319e0-351">Testare il metodo DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="319e0-351">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="319e0-352">Usare Postman per eliminare un elemento attività:</span><span class="sxs-lookup"><span data-stu-id="319e0-352">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="319e0-353">Impostare il metodo su `DELETE`.</span><span class="sxs-lookup"><span data-stu-id="319e0-353">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="319e0-354">Impostare l'URI dell'oggetto da eliminare, ad esempio `https://localhost:5001/api/TodoItems/1`.</span><span class="sxs-lookup"><span data-stu-id="319e0-354">Set the URI of the object to delete (for example `https://localhost:5001/api/TodoItems/1`).</span></span>
* <span data-ttu-id="319e0-355">Selezionare **Send** (Invia).</span><span class="sxs-lookup"><span data-stu-id="319e0-355">Select **Send**.</span></span>

## <a name="call-the-web-api-with-javascript"></a><span data-ttu-id="319e0-356">Chiamare l'API Web con JavaScript</span><span class="sxs-lookup"><span data-stu-id="319e0-356">Call the web API with JavaScript</span></span>

<span data-ttu-id="319e0-357">Vedere [l'esercitazione: Chiamare un'API Web ASP.NET Core con JavaScript](xref:tutorials/web-api-javascript).</span><span class="sxs-lookup"><span data-stu-id="319e0-357">See [Tutorial: Call an ASP.NET Core web API with JavaScript](xref:tutorials/web-api-javascript).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="319e0-358">In questa esercitazione si imparerà a:</span><span class="sxs-lookup"><span data-stu-id="319e0-358">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="319e0-359">Creare un progetto di API Web.</span><span class="sxs-lookup"><span data-stu-id="319e0-359">Create a web API project.</span></span>
> * <span data-ttu-id="319e0-360">Aggiungere una classe modello e un contesto di database.</span><span class="sxs-lookup"><span data-stu-id="319e0-360">Add a model class and a database context.</span></span>
> * <span data-ttu-id="319e0-361">Aggiungere un controller.</span><span class="sxs-lookup"><span data-stu-id="319e0-361">Add a controller.</span></span>
> * <span data-ttu-id="319e0-362">Aggiungere metodi CRUD.</span><span class="sxs-lookup"><span data-stu-id="319e0-362">Add CRUD methods.</span></span>
> * <span data-ttu-id="319e0-363">Configurare routing e percorsi URL.</span><span class="sxs-lookup"><span data-stu-id="319e0-363">Configure routing and URL paths.</span></span>
> * <span data-ttu-id="319e0-364">Specificare valori restituiti.</span><span class="sxs-lookup"><span data-stu-id="319e0-364">Specify return values.</span></span>
> * <span data-ttu-id="319e0-365">Chiamare l'API Web con Postman.</span><span class="sxs-lookup"><span data-stu-id="319e0-365">Call the web API with Postman.</span></span>
> * <span data-ttu-id="319e0-366">Chiamare l'API Web con JavaScript.</span><span class="sxs-lookup"><span data-stu-id="319e0-366">Call the web API with JavaScript.</span></span>

<span data-ttu-id="319e0-367">Al termine si dispone di un'API web che può gestire gli elementi di tipo "attività" archiviati in un database relazionale.</span><span class="sxs-lookup"><span data-stu-id="319e0-367">At the end, you have a web API that can manage "to-do" items stored in a relational database.</span></span>

## <a name="overview"></a><span data-ttu-id="319e0-368">Panoramica</span><span class="sxs-lookup"><span data-stu-id="319e0-368">Overview</span></span>

<span data-ttu-id="319e0-369">Questa esercitazione consente di creare l'API seguente:</span><span class="sxs-lookup"><span data-stu-id="319e0-369">This tutorial creates the following API:</span></span>

|<span data-ttu-id="319e0-370">API</span><span class="sxs-lookup"><span data-stu-id="319e0-370">API</span></span> | <span data-ttu-id="319e0-371">Descrizione</span><span class="sxs-lookup"><span data-stu-id="319e0-371">Description</span></span> | <span data-ttu-id="319e0-372">Corpo della richiesta</span><span class="sxs-lookup"><span data-stu-id="319e0-372">Request body</span></span> | <span data-ttu-id="319e0-373">Corpo della risposta</span><span class="sxs-lookup"><span data-stu-id="319e0-373">Response body</span></span> |
|--- | ---- | ---- | ---- |
|<span data-ttu-id="319e0-374">GET /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="319e0-374">GET /api/TodoItems</span></span> | <span data-ttu-id="319e0-375">Ottiene tutti gli elementi attività</span><span class="sxs-lookup"><span data-stu-id="319e0-375">Get all to-do items</span></span> | <span data-ttu-id="319e0-376">nessuno</span><span class="sxs-lookup"><span data-stu-id="319e0-376">None</span></span> | <span data-ttu-id="319e0-377">Matrice di elementi attività</span><span class="sxs-lookup"><span data-stu-id="319e0-377">Array of to-do items</span></span>|
|<span data-ttu-id="319e0-378">GET /api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="319e0-378">GET /api/TodoItems/{id}</span></span> | <span data-ttu-id="319e0-379">Ottiene un elemento in base all'ID</span><span class="sxs-lookup"><span data-stu-id="319e0-379">Get an item by ID</span></span> | <span data-ttu-id="319e0-380">nessuno</span><span class="sxs-lookup"><span data-stu-id="319e0-380">None</span></span> | <span data-ttu-id="319e0-381">Elemento attività</span><span class="sxs-lookup"><span data-stu-id="319e0-381">To-do item</span></span>|
|<span data-ttu-id="319e0-382">POST /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="319e0-382">POST /api/TodoItems</span></span> | <span data-ttu-id="319e0-383">Aggiunge un nuovo elemento</span><span class="sxs-lookup"><span data-stu-id="319e0-383">Add a new item</span></span> | <span data-ttu-id="319e0-384">Elemento attività</span><span class="sxs-lookup"><span data-stu-id="319e0-384">To-do item</span></span> | <span data-ttu-id="319e0-385">Elemento attività</span><span class="sxs-lookup"><span data-stu-id="319e0-385">To-do item</span></span> |
|<span data-ttu-id="319e0-386">PUT /api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="319e0-386">PUT /api/TodoItems/{id}</span></span> | <span data-ttu-id="319e0-387">Aggiorna un elemento esistente &nbsp;</span><span class="sxs-lookup"><span data-stu-id="319e0-387">Update an existing item &nbsp;</span></span> | <span data-ttu-id="319e0-388">Elemento attività</span><span class="sxs-lookup"><span data-stu-id="319e0-388">To-do item</span></span> | <span data-ttu-id="319e0-389">nessuno</span><span class="sxs-lookup"><span data-stu-id="319e0-389">None</span></span> |
|<span data-ttu-id="319e0-390">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="319e0-390">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span></span> | <span data-ttu-id="319e0-391">Elimina un elemento &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="319e0-391">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="319e0-392">nessuno</span><span class="sxs-lookup"><span data-stu-id="319e0-392">None</span></span> | <span data-ttu-id="319e0-393">nessuno</span><span class="sxs-lookup"><span data-stu-id="319e0-393">None</span></span>|

<span data-ttu-id="319e0-394">Il diagramma seguente visualizza la struttura dell'app.</span><span class="sxs-lookup"><span data-stu-id="319e0-394">The following diagram shows the design of the app.</span></span>

![Il client è rappresentato da una casella a sinistra.](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="319e0-400">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="319e0-400">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="319e0-401">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="319e0-401">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="319e0-402">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="319e0-402">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="319e0-403">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="319e0-403">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="319e0-404">Creare un progetto Web</span><span class="sxs-lookup"><span data-stu-id="319e0-404">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="319e0-405">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="319e0-405">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="319e0-406">Scegliere **Nuovo** > **Progetto** dal menu **File**.</span><span class="sxs-lookup"><span data-stu-id="319e0-406">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="319e0-407">Selezionare il modello **Applicazione Web ASP.NET Core** e fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="319e0-407">Select the **ASP.NET Core Web Application** template and click **Next**.</span></span>
* <span data-ttu-id="319e0-408">Assegnare al progetto il nome *TodoApi* e fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="319e0-408">Name the project *TodoApi* and click **Create**.</span></span>
* <span data-ttu-id="319e0-409">Nella finestra di dialogo **Crea una nuova applicazione Web ASP.NET Core** verificare che siano selezionati **.NET Core** e **ASP.NET Core 2.2**.</span><span class="sxs-lookup"><span data-stu-id="319e0-409">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 2.2** are selected.</span></span> <span data-ttu-id="319e0-410">Selezionare il modello **API** e fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="319e0-410">Select the **API** template and click **Create**.</span></span> <span data-ttu-id="319e0-411">**Non** selezionare **Abilita supporto Docker**.</span><span class="sxs-lookup"><span data-stu-id="319e0-411">**Don't** select **Enable Docker Support**.</span></span>

![Finestra di dialogo Nuovo progetto di Visual Studio](first-web-api/_static/vs.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="319e0-413">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="319e0-413">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="319e0-414">Aprire il [terminale integrato](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="319e0-414">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="319e0-415">Cambiare directory (`cd`) e passare alla cartella che conterrà la cartella del progetto.</span><span class="sxs-lookup"><span data-stu-id="319e0-415">Change directories (`cd`) to the folder that will contain the project folder.</span></span>
* <span data-ttu-id="319e0-416">Eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="319e0-416">Run the following commands:</span></span>

   ```dotnetcli
   dotnet new webapi -o TodoApi
   code -r TodoApi
   ```

  <span data-ttu-id="319e0-417">Questi comandi creano un nuovo progetto API Web e aprono una nuova istanza di Visual Studio Code nella nuova cartella di progetto.</span><span class="sxs-lookup"><span data-stu-id="319e0-417">These commands create a new web API project and open a new instance of Visual Studio Code in the new project folder.</span></span>

* <span data-ttu-id="319e0-418">Quando una finestra di dialogo chiede se si vuole aggiungere gli asset necessari al progetto, selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="319e0-418">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="319e0-419">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="319e0-419">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="319e0-420">Selezionare **File** > **Nuova soluzione**.</span><span class="sxs-lookup"><span data-stu-id="319e0-420">Select **File** > **New Solution**.</span></span>

  ![Nuova soluzione macOS](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="319e0-422">Selezionare **.NET Core** > **App** > **API** > **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="319e0-422">Select **.NET Core** > **App** > **API** > **Next**.</span></span>

  ![Finestra di dialogo Nuovo progetto di macOS](first-web-api-mac/_static/1.png)
  
* <span data-ttu-id="319e0-424">Nella finestra di dialogo **Configure your new ASP.NET Core Web API** (Configura la nuova API Web ASP.NET Core), accettare l'impostazione predefinita di **Framework di destinazione**, ovvero \* *.NET Core 2.2*.</span><span class="sxs-lookup"><span data-stu-id="319e0-424">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of \**.NET Core 2.2*.</span></span>

* <span data-ttu-id="319e0-425">Immettere *TodoApi* in **Nome progetto**, quindi selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="319e0-425">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![Finestra di dialogo di configurazione](first-web-api-mac/_static/2.png)

---

### <a name="test-the-api"></a><span data-ttu-id="319e0-427">Testare l'API</span><span class="sxs-lookup"><span data-stu-id="319e0-427">Test the API</span></span>

<span data-ttu-id="319e0-428">Il modello di progetto crea un'API `values`.</span><span class="sxs-lookup"><span data-stu-id="319e0-428">The project template creates a `values` API.</span></span> <span data-ttu-id="319e0-429">Chiamare il metodo `Get` da un browser per eseguire il test dell'app.</span><span class="sxs-lookup"><span data-stu-id="319e0-429">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="319e0-430">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="319e0-430">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="319e0-431">Premere CTRL+F5 per eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="319e0-431">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="319e0-432">Visual Studio apre un browser e naviga all'indirizzo `https://localhost:<port>/api/values`, dove `<port>` è un numero di porta selezionato a caso.</span><span class="sxs-lookup"><span data-stu-id="319e0-432">Visual Studio launches a browser and navigates to `https://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="319e0-433">Se in una finestra di dialogo viene chiesto se considerare attendibile il certificato IIS Express, selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="319e0-433">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="319e0-434">Nella finestra di dialogo **Avviso di sicurezza** successiva selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="319e0-434">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="319e0-435">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="319e0-435">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="319e0-436">Premere CTRL+F5 per eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="319e0-436">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="319e0-437">In un browser passare all'URL seguente: [https://localhost:5001/api/values](https://localhost:5001/api/values).</span><span class="sxs-lookup"><span data-stu-id="319e0-437">In a browser, go to following URL: [https://localhost:5001/api/values](https://localhost:5001/api/values).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="319e0-438">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="319e0-438">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="319e0-439">Selezionare **Esegui** > **Avvia debug** per avviare l'app.</span><span class="sxs-lookup"><span data-stu-id="319e0-439">Select **Run** > **Start Debugging** to launch the app.</span></span> <span data-ttu-id="319e0-440">Visual Studio per Mac apre un browser e naviga all'indirizzo `https://localhost:<port>`, dove `<port>` è un numero di porta selezionato a caso.</span><span class="sxs-lookup"><span data-stu-id="319e0-440">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="319e0-441">Viene restituito un errore HTTP 404 (Non trovato).</span><span class="sxs-lookup"><span data-stu-id="319e0-441">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="319e0-442">Accodare `/api/values` all'URL (impostare l'URL su `https://localhost:<port>/api/values`).</span><span class="sxs-lookup"><span data-stu-id="319e0-442">Append `/api/values` to the URL (change the URL to `https://localhost:<port>/api/values`).</span></span>

---

<span data-ttu-id="319e0-443">Viene restituito il codice JSON seguente:</span><span class="sxs-lookup"><span data-stu-id="319e0-443">The following JSON is returned:</span></span>

```json
["value1","value2"]
```

## <a name="add-a-model-class"></a><span data-ttu-id="319e0-444">Aggiungere una classe del modello</span><span class="sxs-lookup"><span data-stu-id="319e0-444">Add a model class</span></span>

<span data-ttu-id="319e0-445">Un *modello* è un set di classi che rappresentano i dati gestiti dall'app.</span><span class="sxs-lookup"><span data-stu-id="319e0-445">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="319e0-446">Il modello per questa app è una classe `TodoItem` singola.</span><span class="sxs-lookup"><span data-stu-id="319e0-446">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="319e0-447">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="319e0-447">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="319e0-448">In **Esplora soluzioni** fare clic con il pulsante destro del mouse sul progetto.</span><span class="sxs-lookup"><span data-stu-id="319e0-448">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="319e0-449">Selezionare **Aggiungi** > **Nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="319e0-449">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="319e0-450">Assegnare il nome *Models* alla cartella.</span><span class="sxs-lookup"><span data-stu-id="319e0-450">Name the folder *Models*.</span></span>

* <span data-ttu-id="319e0-451">Fare clic con il pulsante destro del mouse sulla cartella *Models* e scegliere **Aggiungi** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="319e0-451">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="319e0-452">Assegnare alla classe il nome *TodoItem* e selezionare **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="319e0-452">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="319e0-453">Sostituire il codice del modello con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="319e0-453">Replace the template code with the following code:</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="319e0-454">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="319e0-454">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="319e0-455">Aggiungere una cartella denominata *Models*.</span><span class="sxs-lookup"><span data-stu-id="319e0-455">Add a folder named *Models*.</span></span>

* <span data-ttu-id="319e0-456">Aggiungere una classe `TodoItem` alla cartella *Models* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="319e0-456">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="319e0-457">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="319e0-457">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="319e0-458">Fare clic con il pulsante destro del mouse sul progetto.</span><span class="sxs-lookup"><span data-stu-id="319e0-458">Right-click the project.</span></span> <span data-ttu-id="319e0-459">Selezionare **Aggiungi** > **Nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="319e0-459">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="319e0-460">Assegnare il nome *Models* alla cartella.</span><span class="sxs-lookup"><span data-stu-id="319e0-460">Name the folder *Models*.</span></span>

  ![Nuova cartella](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="319e0-462">Fare clic con il pulsante destro del mouse sulla cartella *Models* e scegliere **Aggiungi** > **Nuovo file** > **Generale** > **Classe vuota**.</span><span class="sxs-lookup"><span data-stu-id="319e0-462">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="319e0-463">Assegnare alla classe il nome *TodoItem* e fare clic su **Nuovo**.</span><span class="sxs-lookup"><span data-stu-id="319e0-463">Name the class *TodoItem*, and then click **New**.</span></span>

* <span data-ttu-id="319e0-464">Sostituire il codice del modello con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="319e0-464">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="319e0-465">La proprietà `Id` funziona come chiave univoca in un database relazionale.</span><span class="sxs-lookup"><span data-stu-id="319e0-465">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="319e0-466">È possibile inserire le classi del modello in qualsiasi punto del progetto, ma per convenzione viene usata la cartella *Models*.</span><span class="sxs-lookup"><span data-stu-id="319e0-466">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="319e0-467">Aggiungere un contesto di database</span><span class="sxs-lookup"><span data-stu-id="319e0-467">Add a database context</span></span>

<span data-ttu-id="319e0-468">Il *contesto di database* è la classe principale che coordina le funzionalità di Entity Framework per un modello di dati.</span><span class="sxs-lookup"><span data-stu-id="319e0-468">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="319e0-469">Questa classe viene creata mediante derivazione dalla classe `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="319e0-469">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="319e0-470">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="319e0-470">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="319e0-471">Fare clic con il pulsante destro del mouse sulla cartella *Models* e scegliere **Aggiungi** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="319e0-471">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="319e0-472">Assegnare alla classe il nome *TodoContext* e fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="319e0-472">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="319e0-473">Visual Studio Code/Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="319e0-473">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="319e0-474">Aggiungere una classe `TodoContext` alla cartella *Models*.</span><span class="sxs-lookup"><span data-stu-id="319e0-474">Add a `TodoContext` class to the *Models* folder.</span></span>

---

* <span data-ttu-id="319e0-475">Sostituire il codice del modello con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="319e0-475">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="319e0-476">Registrare il contesto del database</span><span class="sxs-lookup"><span data-stu-id="319e0-476">Register the database context</span></span>

<span data-ttu-id="319e0-477">In ASP.NET Core i servizi come il contesto del database devono essere registrati con il [contenitore di inserimento delle dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="319e0-477">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="319e0-478">Il contenitore rende disponibile il servizio ai controller.</span><span class="sxs-lookup"><span data-stu-id="319e0-478">The container provides the service to controllers.</span></span>

<span data-ttu-id="319e0-479">Aggiornare *Startup.cs* con il codice evidenziato seguente:</span><span class="sxs-lookup"><span data-stu-id="319e0-479">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup1.cs?highlight=5,8,25-26&name=snippet_all)]

<span data-ttu-id="319e0-480">Il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="319e0-480">The preceding code:</span></span>

* <span data-ttu-id="319e0-481">Rimuove le dichiarazioni `using` inutilizzate.</span><span class="sxs-lookup"><span data-stu-id="319e0-481">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="319e0-482">Aggiunge il contesto del database al contenitore di inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="319e0-482">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="319e0-483">Specifica che il contesto del database userà un database in memoria.</span><span class="sxs-lookup"><span data-stu-id="319e0-483">Specifies that the database context will use an in-memory database.</span></span>

## <a name="add-a-controller"></a><span data-ttu-id="319e0-484">Aggiungere un controller</span><span class="sxs-lookup"><span data-stu-id="319e0-484">Add a controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="319e0-485">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="319e0-485">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="319e0-486">Fare clic con il pulsante destro del mouse sulla cartella *Controllers*.</span><span class="sxs-lookup"><span data-stu-id="319e0-486">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="319e0-487">Selezionare **Aggiungi** > **Nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="319e0-487">Select **Add** > **New Item**.</span></span>
* <span data-ttu-id="319e0-488">Nella finestra di dialogo **Aggiungi nuovo elemento** selezionare il modello **API Controller Class** (Classe controller API).</span><span class="sxs-lookup"><span data-stu-id="319e0-488">In the **Add New Item** dialog, select the **API Controller Class** template.</span></span>
* <span data-ttu-id="319e0-489">Assegnare alla classe il nome *TodoController* e selezionare **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="319e0-489">Name the class *TodoController*, and select **Add**.</span></span>

  ![Finestra di dialogo Aggiungi nuovo elemento con controller nella casella di ricerca e controller API Web selezionato](first-web-api/_static/new_controller.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="319e0-491">Visual Studio Code/Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="319e0-491">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="319e0-492">Nella cartella *Controllers* creare una classe denominata `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="319e0-492">In the *Controllers* folder, create a class named `TodoController`.</span></span>

---

* <span data-ttu-id="319e0-493">Sostituire il codice del modello con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="319e0-493">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="319e0-494">Il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="319e0-494">The preceding code:</span></span>

* <span data-ttu-id="319e0-495">Definisce una classe controller API senza metodi.</span><span class="sxs-lookup"><span data-stu-id="319e0-495">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="319e0-496">Contrassegna la classe con l'attributo [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute).</span><span class="sxs-lookup"><span data-stu-id="319e0-496">Decorates the class with the [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute.</span></span> <span data-ttu-id="319e0-497">L'attributo indica che il controller risponde alle richieste di API Web.</span><span class="sxs-lookup"><span data-stu-id="319e0-497">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="319e0-498">Per informazioni sui comportamenti specifici consentiti dall'attributo, vedere <xref:web-api/index>.</span><span class="sxs-lookup"><span data-stu-id="319e0-498">For information about specific behaviors that the attribute enables, see <xref:web-api/index>.</span></span>
* <span data-ttu-id="319e0-499">Usa l'inserimento delle dipendenze per inserire il contesto del database (`TodoContext`) nel controller.</span><span class="sxs-lookup"><span data-stu-id="319e0-499">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="319e0-500">Il contesto di database viene usato in ognuno dei metodi [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) nel controller.</span><span class="sxs-lookup"><span data-stu-id="319e0-500">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>
* <span data-ttu-id="319e0-501">Aggiunge un elemento denominato `Item1` al database se il database è vuoto.</span><span class="sxs-lookup"><span data-stu-id="319e0-501">Adds an item named `Item1` to the database if the database is empty.</span></span> <span data-ttu-id="319e0-502">Questo codice si trova nel costruttore, quindi viene eseguito ogni volta che è presente una nuova richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="319e0-502">This code is in the constructor, so it runs every time there's a new HTTP request.</span></span> <span data-ttu-id="319e0-503">Se si eliminano tutti gli elementi, il costruttore crea di nuovo `Item1` quando viene nuovamente chiamato un metodo API.</span><span class="sxs-lookup"><span data-stu-id="319e0-503">If you delete all items, the constructor creates `Item1` again the next time an API method is called.</span></span> <span data-ttu-id="319e0-504">Potrebbe pertanto sembrare che l'eliminazione non abbia funzionato, mentre di fatto ha funzionato.</span><span class="sxs-lookup"><span data-stu-id="319e0-504">So it may look like the deletion didn't work when it actually did work.</span></span>

## <a name="add-get-methods"></a><span data-ttu-id="319e0-505">Aggiungere metodi Get</span><span class="sxs-lookup"><span data-stu-id="319e0-505">Add Get methods</span></span>

<span data-ttu-id="319e0-506">Per specificare un'API che recupera elementi attività, aggiungere i metodi seguenti alla classe `TodoController`:</span><span class="sxs-lookup"><span data-stu-id="319e0-506">To provide an API that retrieves to-do items, add the following methods to the `TodoController` class:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

<span data-ttu-id="319e0-507">Questi metodi implementano due metodi GET:</span><span class="sxs-lookup"><span data-stu-id="319e0-507">These methods implement two GET endpoints:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="319e0-508">Arrestare l'app se è ancora in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="319e0-508">Stop the app if it's still running.</span></span> <span data-ttu-id="319e0-509">Quindi eseguirla di nuovo per includere le modifiche più recenti.</span><span class="sxs-lookup"><span data-stu-id="319e0-509">Then run it again to include the latest changes.</span></span>

<span data-ttu-id="319e0-510">Testare l'app chiamando i due endpoint da un browser.</span><span class="sxs-lookup"><span data-stu-id="319e0-510">Test the app by calling the two endpoints from a browser.</span></span> <span data-ttu-id="319e0-511">Esempio:</span><span class="sxs-lookup"><span data-stu-id="319e0-511">For example:</span></span>

* `https://localhost:<port>/api/todo`
* `https://localhost:<port>/api/todo/1`

<span data-ttu-id="319e0-512">La chiamata a `GetTodoItems` genera la risposta HTTP seguente:</span><span class="sxs-lookup"><span data-stu-id="319e0-512">The following HTTP response is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

## <a name="routing-and-url-paths"></a><span data-ttu-id="319e0-513">Routing e percorsi di URL</span><span class="sxs-lookup"><span data-stu-id="319e0-513">Routing and URL paths</span></span>

<span data-ttu-id="319e0-514">L'attributo [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) indica un metodo che risponde a una richiesta HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="319e0-514">The [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="319e0-515">Il percorso dell'URL per ogni metodo viene costruito nel modo seguente:</span><span class="sxs-lookup"><span data-stu-id="319e0-515">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="319e0-516">Iniziare con la stringa di modello nell'attributo `Route` del controller:</span><span class="sxs-lookup"><span data-stu-id="319e0-516">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

* <span data-ttu-id="319e0-517">Sostituire `[controller]` con il nome del controller, ovvero, per convenzione, il nome della classe controller meno il suffisso "Controller".</span><span class="sxs-lookup"><span data-stu-id="319e0-517">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="319e0-518">In questo esempio il nome della classe controller è **Todo**Controller, pertanto il nome del controller è "todo".</span><span class="sxs-lookup"><span data-stu-id="319e0-518">For this sample, the controller class name is **Todo**Controller, so the controller name is "todo".</span></span> <span data-ttu-id="319e0-519">Il [routing](xref:mvc/controllers/routing) ASP.NET Core non fa distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="319e0-519">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="319e0-520">Se l'attributo `[HttpGet]` ha un modello di route, ad esempio `[HttpGet("products")]`, aggiungerlo al percorso.</span><span class="sxs-lookup"><span data-stu-id="319e0-520">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="319e0-521">In questo esempio non si usa un modello.</span><span class="sxs-lookup"><span data-stu-id="319e0-521">This sample doesn't use a template.</span></span> <span data-ttu-id="319e0-522">Per altre informazioni, vedere [Routing con attributi Http[verb]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="319e0-522">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="319e0-523">Nel metodo `GetTodoItem` seguente, `"{id}"` è una variabile segnaposto per l'identificatore univoco dell'elemento attività.</span><span class="sxs-lookup"><span data-stu-id="319e0-523">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="319e0-524">Quando viene chiamato `GetTodoItem`, il valore di `"{id}"` viene specificato per il metodo nel rispettivo parametro `id`.</span><span class="sxs-lookup"><span data-stu-id="319e0-524">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its`id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a><span data-ttu-id="319e0-525">Valori restituiti</span><span class="sxs-lookup"><span data-stu-id="319e0-525">Return values</span></span>

<span data-ttu-id="319e0-526">Il tipo restituito dei metodi `GetTodoItems` e `GetTodoItem` è [ActionResult\<T>](xref:web-api/action-return-types#actionresultt-type).</span><span class="sxs-lookup"><span data-stu-id="319e0-526">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="319e0-527">ASP.NET Core serializza automaticamente l'oggetto su [JSON](https://www.json.org/) e scrive il codice JSON nel corpo del messaggio di risposta.</span><span class="sxs-lookup"><span data-stu-id="319e0-527">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="319e0-528">Il codice di risposta per questo tipo restituito è 200, presupponendo che non esistano eccezioni non gestite.</span><span class="sxs-lookup"><span data-stu-id="319e0-528">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="319e0-529">Le eccezioni non gestite vengono convertite in errori 5xx.</span><span class="sxs-lookup"><span data-stu-id="319e0-529">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="319e0-530">I tipi restituiti `ActionResult` possono rappresentare un ampio intervallo di codici di stato HTTP.</span><span class="sxs-lookup"><span data-stu-id="319e0-530">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="319e0-531">Ad esempio, `GetTodoItem` può restituire due valori di stato diversi:</span><span class="sxs-lookup"><span data-stu-id="319e0-531">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="319e0-532">Se nessun elemento corrisponde all'ID richiesto, il metodo restituisce un codice di errore 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound).</span><span class="sxs-lookup"><span data-stu-id="319e0-532">If no item matches the requested ID, the method returns a 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) error code.</span></span>
* <span data-ttu-id="319e0-533">In caso contrario, il metodo restituisce il codice 200 con un corpo della risposta JSON.</span><span class="sxs-lookup"><span data-stu-id="319e0-533">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="319e0-534">La restituzione di `item` risulta in una risposta HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="319e0-534">Returning `item` results in an HTTP 200 response.</span></span>

## <a name="test-the-gettodoitems-method"></a><span data-ttu-id="319e0-535">Testare il metodo GetTodoItems</span><span class="sxs-lookup"><span data-stu-id="319e0-535">Test the GetTodoItems method</span></span>

<span data-ttu-id="319e0-536">Questa esercitazione usa Postman per testare l'API Web.</span><span class="sxs-lookup"><span data-stu-id="319e0-536">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="319e0-537">Installare il [post](https://www.getpostman.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="319e0-537">Install [Postman](https://www.getpostman.com/downloads/).</span></span>
* <span data-ttu-id="319e0-538">Avviare l'app Web.</span><span class="sxs-lookup"><span data-stu-id="319e0-538">Start the web app.</span></span>
* <span data-ttu-id="319e0-539">Avviare Postman.</span><span class="sxs-lookup"><span data-stu-id="319e0-539">Start Postman.</span></span>
* <span data-ttu-id="319e0-540">Disabilitare la **Verifica del certificato SSL**.</span><span class="sxs-lookup"><span data-stu-id="319e0-540">Disable **SSL certificate verification**.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="319e0-541">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="319e0-541">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="319e0-542">Da **File** > **Settings** (Impostazioni) (scheda **General** (Generale)) disabilitare **SSL certificate verification** (Verifica certificato SSL).</span><span class="sxs-lookup"><span data-stu-id="319e0-542">From **File** > **Settings** (**General** tab), disable **SSL certificate verification**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="319e0-543">Visual Studio Code/Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="319e0-543">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="319e0-544">Da **Postman**  >  **Preferences** (Preferenze) (scheda **General** (Generale)) disabilitare **SSL certificate verification** (Verifica certificato SSL).</span><span class="sxs-lookup"><span data-stu-id="319e0-544">From **Postman** > **Preferences** (**General** tab), disable **SSL certificate verification**.</span></span> <span data-ttu-id="319e0-545">In alternativa, selezionare la chiave inglese e selezionare **Settings** (Impostazioni), quindi disabilitare la verifica del certificato SSL.</span><span class="sxs-lookup"><span data-stu-id="319e0-545">Alternatively, select the wrench and select **Settings**, then disable the SSL certificate verification.</span></span>

---
  
> [!WARNING]
> <span data-ttu-id="319e0-546">Riattivare la verifica dei certificati SSL al termine del test del controller.</span><span class="sxs-lookup"><span data-stu-id="319e0-546">Re-enable SSL certificate verification after testing the controller.</span></span>

* <span data-ttu-id="319e0-547">Creare una nuova richiesta.</span><span class="sxs-lookup"><span data-stu-id="319e0-547">Create a new request.</span></span>
  * <span data-ttu-id="319e0-548">Impostare il metodo HTTP su **GET**.</span><span class="sxs-lookup"><span data-stu-id="319e0-548">Set the HTTP method to **GET**.</span></span>
  * <span data-ttu-id="319e0-549">Impostare l'URL della richiesta su `https://localhost:<port>/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="319e0-549">Set the request URL to `https://localhost:<port>/api/todo`.</span></span> <span data-ttu-id="319e0-550">Ad esempio `https://localhost:5001/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="319e0-550">For example, `https://localhost:5001/api/todo`.</span></span>
* <span data-ttu-id="319e0-551">Impostare **Two pane view** (Visualizzazione in due riquadri) in Postman.</span><span class="sxs-lookup"><span data-stu-id="319e0-551">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="319e0-552">Selezionare **Send** (Invia).</span><span class="sxs-lookup"><span data-stu-id="319e0-552">Select **Send**.</span></span>

![Postman con richiesta GET](first-web-api/_static/2pv.png)

## <a name="add-a-create-method"></a><span data-ttu-id="319e0-554">Aggiungere un metodo Create</span><span class="sxs-lookup"><span data-stu-id="319e0-554">Add a Create method</span></span>

<span data-ttu-id="319e0-555">Aggiungere il metodo `PostTodoItem` seguente in *Controllers/TodoController.cs*:</span><span class="sxs-lookup"><span data-stu-id="319e0-555">Add the following `PostTodoItem` method inside of *Controllers/TodoController.cs*:</span></span> 

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="319e0-556">Il codice precedente è un metodo HTTP POST, come indicato dall'attributo [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute).</span><span class="sxs-lookup"><span data-stu-id="319e0-556">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="319e0-557">Il metodo ottiene il valore dell'elemento attività dal corpo della richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="319e0-557">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="319e0-558">Il metodo `CreatedAtAction`:</span><span class="sxs-lookup"><span data-stu-id="319e0-558">The `CreatedAtAction` method:</span></span>

* <span data-ttu-id="319e0-559">Restituisce un codice di stato HTTP 201 in caso di esito positivo.</span><span class="sxs-lookup"><span data-stu-id="319e0-559">Returns an HTTP 201 status code, if successful.</span></span> <span data-ttu-id="319e0-560">HTTP 201 è la risposta standard per un metodo HTTP POST che crea una nuova risorsa nel server.</span><span class="sxs-lookup"><span data-stu-id="319e0-560">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="319e0-561">Aggiunge un'intestazione `Location` alla risposta.</span><span class="sxs-lookup"><span data-stu-id="319e0-561">Adds a `Location` header to the response.</span></span> <span data-ttu-id="319e0-562">L'intestazione `Location` specifica l'URI dell'elemento attività appena creato.</span><span class="sxs-lookup"><span data-stu-id="319e0-562">The `Location` header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="319e0-563">Per altre informazioni, vedere [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="319e0-563">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="319e0-564">Fa riferimento all'azione `GetTodoItem` per creare l'URI dell'intestazione `Location`.</span><span class="sxs-lookup"><span data-stu-id="319e0-564">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="319e0-565">La parola chiave `nameof` C# viene usata per evitare di impostare il nome dell'azione come hardcoded nella chiamata a `CreatedAtAction`.</span><span class="sxs-lookup"><span data-stu-id="319e0-565">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

### <a name="test-the-posttodoitem-method"></a><span data-ttu-id="319e0-566">Testare il metodo PostTodoItem</span><span class="sxs-lookup"><span data-stu-id="319e0-566">Test the PostTodoItem method</span></span>

* <span data-ttu-id="319e0-567">Compilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="319e0-567">Build the project.</span></span>
* <span data-ttu-id="319e0-568">In Postman impostare il metodo HTTP su `POST`.</span><span class="sxs-lookup"><span data-stu-id="319e0-568">In Postman, set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="319e0-569">Fare clic sulla scheda **Body** (Corpo).</span><span class="sxs-lookup"><span data-stu-id="319e0-569">Select the **Body** tab.</span></span>
* <span data-ttu-id="319e0-570">Selezionare il pulsante di opzione **raw** (non elaborato).</span><span class="sxs-lookup"><span data-stu-id="319e0-570">Select the **raw** radio button.</span></span>
* <span data-ttu-id="319e0-571">Impostare il tipo su **JSON (application/json)** .</span><span class="sxs-lookup"><span data-stu-id="319e0-571">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="319e0-572">Nel corpo della richiesta immettere codice JSON per un elemento attività:</span><span class="sxs-lookup"><span data-stu-id="319e0-572">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="319e0-573">Selezionare **Send** (Invia).</span><span class="sxs-lookup"><span data-stu-id="319e0-573">Select **Send**.</span></span>

  ![Postman con richiesta di creazione](first-web-api/_static/create.png)

  <span data-ttu-id="319e0-575">Se viene visualizzato un errore HTTP 405 - Metodo non consentito, è probabile che il progetto non sia stato compilato dopo l'aggiunta del metodo `PostTodoItem`.</span><span class="sxs-lookup"><span data-stu-id="319e0-575">If you get a 405 Method Not Allowed error, it's probably the result of not compiling the project after adding the `PostTodoItem` method.</span></span>

### <a name="test-the-location-header-uri"></a><span data-ttu-id="319e0-576">Testare l'URI dell'intestazione della posizione</span><span class="sxs-lookup"><span data-stu-id="319e0-576">Test the location header URI</span></span>

* <span data-ttu-id="319e0-577">Selezionare la scheda **Headers** (Intestazioni) nel riquadro **Response** (Risposta).</span><span class="sxs-lookup"><span data-stu-id="319e0-577">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="319e0-578">Copiare il valore dell'intestazione **Location** (Posizione):</span><span class="sxs-lookup"><span data-stu-id="319e0-578">Copy the **Location** header value:</span></span>

  ![Scheda Headers (Intestazioni) della console Postman](first-web-api/_static/pmc2.png)

* <span data-ttu-id="319e0-580">Impostare il metodo su GET.</span><span class="sxs-lookup"><span data-stu-id="319e0-580">Set the method to GET.</span></span>
* <span data-ttu-id="319e0-581">Incollare l'URI (ad esempio, `https://localhost:5001/api/Todo/2`).</span><span class="sxs-lookup"><span data-stu-id="319e0-581">Paste the URI (for example, `https://localhost:5001/api/Todo/2`).</span></span>
* <span data-ttu-id="319e0-582">Selezionare **Send** (Invia).</span><span class="sxs-lookup"><span data-stu-id="319e0-582">Select **Send**.</span></span>

## <a name="add-a-puttodoitem-method"></a><span data-ttu-id="319e0-583">Aggiungere un metodo PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="319e0-583">Add a PutTodoItem method</span></span>

<span data-ttu-id="319e0-584">Aggiungere il metodo `PutTodoItem` seguente:</span><span class="sxs-lookup"><span data-stu-id="319e0-584">Add the following `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

<span data-ttu-id="319e0-585">`PutTodoItem` è simile a `PostTodoItem` ma usa la richiesta HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="319e0-585">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="319e0-586">La risposta è [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="319e0-586">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="319e0-587">In base alla specifica HTTP, una richiesta PUT richiede che il client invii l'intera entità aggiornata e non solo le modifiche.</span><span class="sxs-lookup"><span data-stu-id="319e0-587">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="319e0-588">Per supportare gli aggiornamenti parziali, usare [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span><span class="sxs-lookup"><span data-stu-id="319e0-588">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="319e0-589">Se si riceve un errore chiamando `PutTodoItem`, chiamare `GET` per verificare che il database includa un elemento.</span><span class="sxs-lookup"><span data-stu-id="319e0-589">If you get an error calling `PutTodoItem`, call `GET` to ensure there's an item in the database.</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="319e0-590">Testare il metodo PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="319e0-590">Test the PutTodoItem method</span></span>

<span data-ttu-id="319e0-591">Questo esempio usa un database in memoria che deve essere inizializzato ogni volta che l'app viene avviata.</span><span class="sxs-lookup"><span data-stu-id="319e0-591">This sample uses an in-memory database that must be initialed each time the app is started.</span></span> <span data-ttu-id="319e0-592">Deve esistere un elemento nel database prima di eseguire una chiamata PUT.</span><span class="sxs-lookup"><span data-stu-id="319e0-592">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="319e0-593">Chiamare GET per assicurarsi che sia presente un elemento nel database prima di eseguire una chiamata PUT.</span><span class="sxs-lookup"><span data-stu-id="319e0-593">Call GET to insure there's an item in the database before making a PUT call.</span></span>

<span data-ttu-id="319e0-594">Aggiornare l'elemento attività con id = 1 e impostarne il nome su "feed fish":</span><span class="sxs-lookup"><span data-stu-id="319e0-594">Update the to-do item that has id = 1 and set its name to "feed fish":</span></span>

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="319e0-595">L'immagine seguente visualizza l'aggiornamento di Postman:</span><span class="sxs-lookup"><span data-stu-id="319e0-595">The following image shows the Postman update:</span></span>

![Console Postman con risposta 204 (No Content)](first-web-api/_static/pmcput.png)

## <a name="add-a-deletetodoitem-method"></a><span data-ttu-id="319e0-597">Aggiungere un metodo DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="319e0-597">Add a DeleteTodoItem method</span></span>

<span data-ttu-id="319e0-598">Aggiungere il metodo `DeleteTodoItem` seguente:</span><span class="sxs-lookup"><span data-stu-id="319e0-598">Add the following `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="319e0-599">La risposta `DeleteTodoItem` è [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="319e0-599">The `DeleteTodoItem` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="319e0-600">Testare il metodo DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="319e0-600">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="319e0-601">Usare Postman per eliminare un elemento attività:</span><span class="sxs-lookup"><span data-stu-id="319e0-601">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="319e0-602">Impostare il metodo su `DELETE`.</span><span class="sxs-lookup"><span data-stu-id="319e0-602">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="319e0-603">Impostare l'URI dell'oggetto da eliminare, ad esempio `https://localhost:5001/api/todo/1`.</span><span class="sxs-lookup"><span data-stu-id="319e0-603">Set the URI of the object to delete (for example `https://localhost:5001/api/todo/1`).</span></span>
* <span data-ttu-id="319e0-604">Selezionare **Send** (Invia).</span><span class="sxs-lookup"><span data-stu-id="319e0-604">Select **Send**.</span></span>

<span data-ttu-id="319e0-605">L'app di esempio consente di eliminare tutti gli elementi.</span><span class="sxs-lookup"><span data-stu-id="319e0-605">The sample app allows you to delete all the items.</span></span> <span data-ttu-id="319e0-606">Quando viene eliminato l'ultimo elemento, tuttavia, ne viene creato uno nuovo dal costruttore della classe modello alla successiva chiamata dell'API.</span><span class="sxs-lookup"><span data-stu-id="319e0-606">However, when the last item is deleted, a new one is created by the model class constructor the next time the API is called.</span></span>

## <a name="call-the-web-api-with-javascript"></a><span data-ttu-id="319e0-607">Chiamare l'API Web con JavaScript</span><span class="sxs-lookup"><span data-stu-id="319e0-607">Call the web API with JavaScript</span></span>

<span data-ttu-id="319e0-608">In questa sezione viene aggiunta una pagina HTML che usa JavaScript per chiamare l'API Web.</span><span class="sxs-lookup"><span data-stu-id="319e0-608">In this section, an HTML page is added that uses JavaScript to call the web API.</span></span> <span data-ttu-id="319e0-609">L'API Fetch avvia la richiesta.</span><span class="sxs-lookup"><span data-stu-id="319e0-609">The Fetch API initiates the request.</span></span> <span data-ttu-id="319e0-610">JavaScript aggiorna la pagina con i dettagli della risposta dell'API Web.</span><span class="sxs-lookup"><span data-stu-id="319e0-610">JavaScript updates the page with the details from the web API's response.</span></span>

<span data-ttu-id="319e0-611">Configurare l'app per [servire file statici](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) e [abilitare il mapping del file predefinito](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) aggiornando *Startup.cs* con il codice evidenziato seguente:</span><span class="sxs-lookup"><span data-stu-id="319e0-611">Configure the app to [serve static files](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [enable default file mapping](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) by updating *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup.cs?highlight=14-15&name=snippet_configure)]

<span data-ttu-id="319e0-612">Creare una cartella *wwwroot* nella directory del progetto.</span><span class="sxs-lookup"><span data-stu-id="319e0-612">Create a *wwwroot* folder in the project directory.</span></span>

<span data-ttu-id="319e0-613">Aggiungere un file HTML denominato *index.html* alla directory *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="319e0-613">Add an HTML file named *index.html* to the *wwwroot* directory.</span></span> <span data-ttu-id="319e0-614">Sostituire il contenuto con il markup seguente:</span><span class="sxs-lookup"><span data-stu-id="319e0-614">Replace its contents with the following markup:</span></span>

[!code-html[](first-web-api/samples/2.2/TodoApi/wwwroot/index.html)]

<span data-ttu-id="319e0-615">Aggiungere un file JavaScript con nome *site.js* alla directory *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="319e0-615">Add a JavaScript file named *site.js* to the *wwwroot* directory.</span></span> <span data-ttu-id="319e0-616">Sostituire il contenuto con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="319e0-616">Replace its contents with the following code:</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_SiteJs)]

<span data-ttu-id="319e0-617">Può essere necessario modificare le impostazioni di avvio del progetto ASP.NET Core per il test della pagina HTML in locale:</span><span class="sxs-lookup"><span data-stu-id="319e0-617">A change to the ASP.NET Core project's launch settings may be required to test the HTML page locally:</span></span>

* <span data-ttu-id="319e0-618">Aprire *Properties\launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="319e0-618">Open *Properties\launchSettings.json*.</span></span>
* <span data-ttu-id="319e0-619">Rimuovere la proprietà `launchUrl` per forzare l'app ad aprire *index.html*, il file predefinito del progetto.</span><span class="sxs-lookup"><span data-stu-id="319e0-619">Remove the `launchUrl` property to force the app to open at *index.html*&mdash;the project's default file.</span></span>

<span data-ttu-id="319e0-620">In questo esempio vengono chiamati tutti i metodi CRUD dell'API Web.</span><span class="sxs-lookup"><span data-stu-id="319e0-620">This sample calls all of the CRUD methods of the web API.</span></span> <span data-ttu-id="319e0-621">Di seguito sono incluse le spiegazioni delle chiamate all'API.</span><span class="sxs-lookup"><span data-stu-id="319e0-621">Following are explanations of the calls to the API.</span></span>

### <a name="get-a-list-of-to-do-items"></a><span data-ttu-id="319e0-622">Ottenere un elenco di elementi attività</span><span class="sxs-lookup"><span data-stu-id="319e0-622">Get a list of to-do items</span></span>

<span data-ttu-id="319e0-623">Fetch invia una richiesta HTTP GET all'API Web, la quale restituisce codice JSON che rappresenta una matrice di elementi attività.</span><span class="sxs-lookup"><span data-stu-id="319e0-623">Fetch sends an HTTP GET request to the web API, which returns JSON representing an array of to-do items.</span></span> <span data-ttu-id="319e0-624">La funzione di callback `success` viene chiamata se la richiesta ha esito positivo.</span><span class="sxs-lookup"><span data-stu-id="319e0-624">The `success` callback function is invoked if the request succeeds.</span></span> <span data-ttu-id="319e0-625">Nel callback il modello DOM viene aggiornato con le informazioni dell'elemento attività.</span><span class="sxs-lookup"><span data-stu-id="319e0-625">In the callback, the DOM is updated with the to-do information.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_GetData)]

### <a name="add-a-to-do-item"></a><span data-ttu-id="319e0-626">Aggiungere un elemento attività</span><span class="sxs-lookup"><span data-stu-id="319e0-626">Add a to-do item</span></span>

<span data-ttu-id="319e0-627">Fetch invia una richiesta HTTP POST con l'elemento attività nel corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="319e0-627">Fetch sends an HTTP POST request with the to-do item in the request body.</span></span> <span data-ttu-id="319e0-628">Le opzioni `accepts` e `contentType` sono impostate su `application/json` per specificare il tipo di supporto ricevuto e inviato.</span><span class="sxs-lookup"><span data-stu-id="319e0-628">The `accepts` and `contentType` options are set to `application/json` to specify the media type being received and sent.</span></span> <span data-ttu-id="319e0-629">L'elemento attività viene convertito in JSON usando [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span><span class="sxs-lookup"><span data-stu-id="319e0-629">The to-do item is converted to JSON by using [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span></span> <span data-ttu-id="319e0-630">Quando l'API restituisce un codice di stato di esecuzione riuscita, viene chiamata la funzione `getData` per aggiornare la tabella HTML.</span><span class="sxs-lookup"><span data-stu-id="319e0-630">When the API returns a successful status code, the `getData` function is invoked to update the HTML table.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AddItem)]

### <a name="update-a-to-do-item"></a><span data-ttu-id="319e0-631">Aggiornare un elemento attività</span><span class="sxs-lookup"><span data-stu-id="319e0-631">Update a to-do item</span></span>

<span data-ttu-id="319e0-632">L'aggiornamento di un elemento attività è simile all'aggiunta di un elemento di questo tipo.</span><span class="sxs-lookup"><span data-stu-id="319e0-632">Updating a to-do item is similar to adding one.</span></span> <span data-ttu-id="319e0-633">L'`url` cambia per includere l'identificatore univoco dell'elemento e `type` è `PUT`.</span><span class="sxs-lookup"><span data-stu-id="319e0-633">The `url` changes to add the unique identifier of the item, and the `type` is `PUT`.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxPut)]

### <a name="delete-a-to-do-item"></a><span data-ttu-id="319e0-634">Eliminare un elemento attività</span><span class="sxs-lookup"><span data-stu-id="319e0-634">Delete a to-do item</span></span>

<span data-ttu-id="319e0-635">È possibile eliminare un elemento attività impostando `type` su `DELETE` nella chiamata AJAX e specificando l'identificatore univoco dell'elemento nell'URL.</span><span class="sxs-lookup"><span data-stu-id="319e0-635">Deleting a to-do item is accomplished by setting the `type` on the AJAX call to `DELETE` and specifying the item's unique identifier in the URL.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxDelete)]

::: moniker-end

<a name="auth"></a>

## <a name="add-authentication-support-to-a-web-api"></a><span data-ttu-id="319e0-636">Aggiungere il supporto per l'autenticazione a un'API Web</span><span class="sxs-lookup"><span data-stu-id="319e0-636">Add authentication support to a web API</span></span>

<span data-ttu-id="319e0-637">Vedere l'esercitazione su [IdentityServer4](https://identityserver4.readthedocs.io/en/latest/quickstarts/0_overview.html) .</span><span class="sxs-lookup"><span data-stu-id="319e0-637">See the [IdentityServer4](https://identityserver4.readthedocs.io/en/latest/quickstarts/0_overview.html) tutorial.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="319e0-638">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="319e0-638">Additional resources</span></span>

<span data-ttu-id="319e0-639">[Visualizzare o scaricare il codice di esempio per questa esercitazione](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span><span class="sxs-lookup"><span data-stu-id="319e0-639">[View or download sample code for this tutorial](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span></span> <span data-ttu-id="319e0-640">Vedere [come scaricare un esempio](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="319e0-640">See [how to download](xref:index#how-to-download-a-sample).</span></span>

<span data-ttu-id="319e0-641">Per altre informazioni, vedere le seguenti risorse:</span><span class="sxs-lookup"><span data-stu-id="319e0-641">For more information, see the following resources:</span></span>

* <xref:web-api/index>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:data/ef-rp/intro>
* <xref:mvc/controllers/routing>
* <xref:web-api/action-return-types>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/index>
* [<span data-ttu-id="319e0-642">Versione YouTube dell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="319e0-642">YouTube version of this tutorial</span></span>](https://www.youtube.com/watch?v=TTkhEyGBfAk)
