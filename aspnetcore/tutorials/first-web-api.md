---
title: "Esercitazione: creare un'API Web con ASP.NET Core"
author: rick-anderson
description: Informazioni su come creare un'API Web con ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 10/15/2019
uid: tutorials/first-web-api
ms.openlocfilehash: b4c88f5dc7853396448a2a6122f3652f92079e68
ms.sourcegitcommit: dd026eceee79e943bd6b4a37b144803b50617583
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/15/2019
ms.locfileid: "72541796"
---
# <a name="tutorial-create-a-web-api-with-aspnet-core"></a><span data-ttu-id="93a41-103">Esercitazione: creare un'API Web con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="93a41-103">Tutorial: Create a web API with ASP.NET Core</span></span>

<span data-ttu-id="93a41-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT) e [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="93a41-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="93a41-105">Questa esercitazione illustra le nozioni di base della creazione di un'API Web con ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="93a41-105">This tutorial teaches the basics of building a web API with ASP.NET Core.</span></span>

<span data-ttu-id="93a41-106">In questa esercitazione si imparerà a:</span><span class="sxs-lookup"><span data-stu-id="93a41-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="93a41-107">Creare un progetto di API Web.</span><span class="sxs-lookup"><span data-stu-id="93a41-107">Create a web API project.</span></span>
> * <span data-ttu-id="93a41-108">Aggiungere una classe modello e un contesto di database.</span><span class="sxs-lookup"><span data-stu-id="93a41-108">Add a model class and a database context.</span></span>
> * <span data-ttu-id="93a41-109">Eseguire lo scaffolding di un controller con i metodi CRUD.</span><span class="sxs-lookup"><span data-stu-id="93a41-109">Scaffold a controller with CRUD methods.</span></span>
> * <span data-ttu-id="93a41-110">Configurare il routing, i percorsi URL e i valori restituiti.</span><span class="sxs-lookup"><span data-stu-id="93a41-110">Configure routing, URL paths, and return values.</span></span>
> * <span data-ttu-id="93a41-111">Chiamare l'API Web con Postman.</span><span class="sxs-lookup"><span data-stu-id="93a41-111">Call the web API with Postman.</span></span>

<span data-ttu-id="93a41-112">Al termine si avrà un'API Web che può gestire gli elementi di tipo "attività" archiviati in un database.</span><span class="sxs-lookup"><span data-stu-id="93a41-112">At the end, you have a web API that can manage "to-do" items stored in a database.</span></span>

## <a name="overview"></a><span data-ttu-id="93a41-113">Panoramica</span><span class="sxs-lookup"><span data-stu-id="93a41-113">Overview</span></span>

<span data-ttu-id="93a41-114">Questa esercitazione crea un'API Web contenente gli endpoint seguenti:</span><span class="sxs-lookup"><span data-stu-id="93a41-114">This tutorial creates a web API containing the following endpoints:</span></span>

|<span data-ttu-id="93a41-115">Endpoint</span><span class="sxs-lookup"><span data-stu-id="93a41-115">Endpoint</span></span>                  |<span data-ttu-id="93a41-116">Descrizione</span><span class="sxs-lookup"><span data-stu-id="93a41-116">Description</span></span>            |<span data-ttu-id="93a41-117">Corpo della richiesta</span><span class="sxs-lookup"><span data-stu-id="93a41-117">Request body</span></span>|<span data-ttu-id="93a41-118">Corpo della risposta</span><span class="sxs-lookup"><span data-stu-id="93a41-118">Response body</span></span>       |
|--------------------------|-----------------------|------------|--------------------|
|<span data-ttu-id="93a41-119">GET /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="93a41-119">GET /api/TodoItems</span></span>        |<span data-ttu-id="93a41-120">Ottiene tutti gli elementi attività</span><span class="sxs-lookup"><span data-stu-id="93a41-120">Get all to-do items</span></span>    |<span data-ttu-id="93a41-121">Nessuno</span><span class="sxs-lookup"><span data-stu-id="93a41-121">None</span></span>        |<span data-ttu-id="93a41-122">Matrice di elementi attività</span><span class="sxs-lookup"><span data-stu-id="93a41-122">Array of to-do items</span></span>|
|<span data-ttu-id="93a41-123">GET /api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="93a41-123">GET /api/TodoItems/{id}</span></span>   |<span data-ttu-id="93a41-124">Ottiene un elemento in base all'ID</span><span class="sxs-lookup"><span data-stu-id="93a41-124">Get an item by ID</span></span>      |<span data-ttu-id="93a41-125">Nessuno</span><span class="sxs-lookup"><span data-stu-id="93a41-125">None</span></span>        |<span data-ttu-id="93a41-126">Elemento attività</span><span class="sxs-lookup"><span data-stu-id="93a41-126">To-do item</span></span>          |
|<span data-ttu-id="93a41-127">POST /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="93a41-127">POST /api/TodoItems</span></span>       |<span data-ttu-id="93a41-128">Aggiunge un nuovo elemento</span><span class="sxs-lookup"><span data-stu-id="93a41-128">Add a new item</span></span>         |<span data-ttu-id="93a41-129">Elemento attività</span><span class="sxs-lookup"><span data-stu-id="93a41-129">To-do item</span></span>  |<span data-ttu-id="93a41-130">Elemento attività</span><span class="sxs-lookup"><span data-stu-id="93a41-130">To-do item</span></span>          |
|<span data-ttu-id="93a41-131">PUT /api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="93a41-131">PUT /api/TodoItems/{id}</span></span>   |<span data-ttu-id="93a41-132">Aggiorna un elemento esistente</span><span class="sxs-lookup"><span data-stu-id="93a41-132">Update an existing item</span></span>|<span data-ttu-id="93a41-133">Elemento attività</span><span class="sxs-lookup"><span data-stu-id="93a41-133">To-do item</span></span>  |<span data-ttu-id="93a41-134">Nessuno</span><span class="sxs-lookup"><span data-stu-id="93a41-134">None</span></span>                |
|<span data-ttu-id="93a41-135">Elimina/api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="93a41-135">DELETE /api/TodoItems/{id}</span></span>|<span data-ttu-id="93a41-136">Eliminare un elemento</span><span class="sxs-lookup"><span data-stu-id="93a41-136">Delete an item</span></span>         |<span data-ttu-id="93a41-137">Nessuno</span><span class="sxs-lookup"><span data-stu-id="93a41-137">None</span></span>        |<span data-ttu-id="93a41-138">Nessuno</span><span class="sxs-lookup"><span data-stu-id="93a41-138">None</span></span>                |

<span data-ttu-id="93a41-139">Il diagramma seguente visualizza la struttura dell'app.</span><span class="sxs-lookup"><span data-stu-id="93a41-139">The following diagram shows the design of the app.</span></span>

![Il client è rappresentato da una casella a sinistra.](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="93a41-145">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="93a41-145">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="93a41-146">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="93a41-146">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="93a41-147">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="93a41-147">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="93a41-148">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="93a41-148">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="93a41-149">Creare un progetto Web</span><span class="sxs-lookup"><span data-stu-id="93a41-149">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="93a41-150">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="93a41-150">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="93a41-151">Scegliere **Nuovo** > **Progetto** dal menu **File**.</span><span class="sxs-lookup"><span data-stu-id="93a41-151">From the **File** menu, select **New** > **Project**.</span></span>
1. <span data-ttu-id="93a41-152">Selezionare il modello **Applicazione Web ASP.NET Core** e fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="93a41-152">Select the **ASP.NET Core Web Application** template and click **Next**.</span></span>
1. <span data-ttu-id="93a41-153">Assegnare al progetto il nome *TodoApi* e fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="93a41-153">Name the project *TodoApi* and click **Create**.</span></span>
1. <span data-ttu-id="93a41-154">Nella finestra di dialogo **Crea una nuova applicazione Web ASP.NET Core** verificare che siano selezionati **.NET Core** e **ASP.NET Core 3.0**.</span><span class="sxs-lookup"><span data-stu-id="93a41-154">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 3.0** are selected.</span></span> <span data-ttu-id="93a41-155">Selezionare il modello **API** e fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="93a41-155">Select the **API** template and click **Create**.</span></span>

![Finestra di dialogo Nuovo progetto di Visual Studio](first-web-api/_static/vs3.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="93a41-157">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="93a41-157">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="93a41-158">Aprire il [terminale integrato](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="93a41-158">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
1. <span data-ttu-id="93a41-159">Cambiare directory (`cd`) e passare alla cartella che conterrà la cartella del progetto.</span><span class="sxs-lookup"><span data-stu-id="93a41-159">Change directories (`cd`) to the folder that will contain the project folder.</span></span>
1. <span data-ttu-id="93a41-160">Eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="93a41-160">Run the following commands:</span></span>

   ```dotnetcli
   dotnet new webapi -o TodoApi
   cd TodoApi
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.EntityFrameworkCore.InMemory
   code -r ../TodoApi
   ```

1. <span data-ttu-id="93a41-161">Quando una finestra di dialogo chiede se si vuole aggiungere gli asset necessari al progetto, selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="93a41-161">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

  <span data-ttu-id="93a41-162">I comandi precedenti:</span><span class="sxs-lookup"><span data-stu-id="93a41-162">The preceding commands:</span></span>

  * <span data-ttu-id="93a41-163">Creare un nuovo progetto API Web e aprirlo in Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="93a41-163">Create a new web API project and open it in Visual Studio Code.</span></span>
  * <span data-ttu-id="93a41-164">Aggiungere i pacchetti NuGet necessari nella sezione successiva.</span><span class="sxs-lookup"><span data-stu-id="93a41-164">Add the NuGet packages which are required in the next section.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="93a41-165">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="93a41-165">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="93a41-166">Selezionare **File** > **Nuova soluzione**.</span><span class="sxs-lookup"><span data-stu-id="93a41-166">Select **File** > **New Solution**.</span></span>

    ![Nuova soluzione macOS](first-web-api-mac/_static/sln.png)

1. <span data-ttu-id="93a41-168">Selezionare **.NET Core** > **App** > **API** > **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="93a41-168">Select **.NET Core** > **App** > **API** > **Next**.</span></span>

    ![Finestra di dialogo Nuovo progetto di macOS](first-web-api-mac/_static/1.png)
  
1. <span data-ttu-id="93a41-170">Nella finestra di dialogo **Configura la nuova API Web ASP.NET Core** selezionare il **framework di destinazione** \* *.NET Core 3.0*.</span><span class="sxs-lookup"><span data-stu-id="93a41-170">In the **Configure your new ASP.NET Core Web API** dialog, select **Target Framework** of \**.NET Core 3.0*.</span></span>
1. <span data-ttu-id="93a41-171">Immettere *TodoApi* in **Nome progetto**, quindi selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="93a41-171">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![Finestra di dialogo di configurazione](first-web-api-mac/_static/2.png)

[!INCLUDE[](~/includes/mac-terminal-access.md)]

<span data-ttu-id="93a41-173">Aprire un terminale di comando nella cartella di progetto ed eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="93a41-173">Open a command terminal in the project folder and run the following commands:</span></span>

   ```dotnetcli
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.EntityFrameworkCore.InMemory
   ```

---

### <a name="test-the-api"></a><span data-ttu-id="93a41-174">Testare l'API</span><span class="sxs-lookup"><span data-stu-id="93a41-174">Test the API</span></span>

<span data-ttu-id="93a41-175">Il modello di progetto crea un'API `WeatherForecast`.</span><span class="sxs-lookup"><span data-stu-id="93a41-175">The project template creates a `WeatherForecast` API.</span></span> <span data-ttu-id="93a41-176">Chiamare il metodo `Get` da un browser per eseguire il test dell'app.</span><span class="sxs-lookup"><span data-stu-id="93a41-176">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="93a41-177">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="93a41-177">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="93a41-178">Premere <kbd>CTRL + F5</kbd> per eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="93a41-178">Press <kbd>Ctrl+F5</kbd> to run the app.</span></span> <span data-ttu-id="93a41-179">Visual Studio apre un browser e naviga all'indirizzo `https://localhost:<port>/WeatherForecast`, dove `<port>` è un numero di porta selezionato a caso.</span><span class="sxs-lookup"><span data-stu-id="93a41-179">Visual Studio launches a browser and navigates to `https://localhost:<port>/WeatherForecast`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="93a41-180">Se in una finestra di dialogo viene chiesto se considerare attendibile il certificato IIS Express, selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="93a41-180">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="93a41-181">Nella finestra di dialogo **Avviso di sicurezza** successiva selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="93a41-181">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="93a41-182">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="93a41-182">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="93a41-183">Premere <kbd>CTRL + F5</kbd> per eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="93a41-183">Press <kbd>Ctrl+F5</kbd> to run the app.</span></span> <span data-ttu-id="93a41-184">In un browser passare all'URL seguente: [https://localhost:5001/WeatherForecast](https://localhost:5001/WeatherForecast).</span><span class="sxs-lookup"><span data-stu-id="93a41-184">In a browser, go to following URL: [https://localhost:5001/WeatherForecast](https://localhost:5001/WeatherForecast).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="93a41-185">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="93a41-185">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="93a41-186">Selezionare **Esegui** > **Avvia debug** per avviare l'app.</span><span class="sxs-lookup"><span data-stu-id="93a41-186">Select **Run** > **Start Debugging** to launch the app.</span></span> <span data-ttu-id="93a41-187">Visual Studio per Mac apre un browser e naviga all'indirizzo `https://localhost:<port>`, dove `<port>` è un numero di porta selezionato a caso.</span><span class="sxs-lookup"><span data-stu-id="93a41-187">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="93a41-188">Viene restituito un errore HTTP 404 (Non trovato).</span><span class="sxs-lookup"><span data-stu-id="93a41-188">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="93a41-189">Accodare `/WeatherForecast` all'URL (impostare l'URL su `https://localhost:<port>/WeatherForecast`).</span><span class="sxs-lookup"><span data-stu-id="93a41-189">Append `/WeatherForecast` to the URL (change the URL to `https://localhost:<port>/WeatherForecast`).</span></span>

---

<span data-ttu-id="93a41-190">Viene restituito un codice JSON simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="93a41-190">JSON similar to the following is returned:</span></span>

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

## <a name="add-a-model-class"></a><span data-ttu-id="93a41-191">Aggiungere una classe modello</span><span class="sxs-lookup"><span data-stu-id="93a41-191">Add a model class</span></span>

<span data-ttu-id="93a41-192">Un *modello* è un set di classi che rappresentano i dati gestiti dall'app.</span><span class="sxs-lookup"><span data-stu-id="93a41-192">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="93a41-193">Il modello per questa app è una classe `TodoItem` singola.</span><span class="sxs-lookup"><span data-stu-id="93a41-193">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="93a41-194">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="93a41-194">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="93a41-195">In **Esplora soluzioni** fare clic con il pulsante destro del mouse sul progetto.</span><span class="sxs-lookup"><span data-stu-id="93a41-195">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="93a41-196">Selezionare **Aggiungi** > **Nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="93a41-196">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="93a41-197">Assegnare il nome *Models* alla cartella.</span><span class="sxs-lookup"><span data-stu-id="93a41-197">Name the folder *Models*.</span></span>
1. <span data-ttu-id="93a41-198">Fare clic con il pulsante destro del mouse sulla cartella *Models* e scegliere **Aggiungi** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="93a41-198">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="93a41-199">Assegnare alla classe il nome *TodoItem* e selezionare **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="93a41-199">Name the class *TodoItem* and select **Add**.</span></span>
1. <span data-ttu-id="93a41-200">Sostituire il codice del modello con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="93a41-200">Replace the template code with the following code:</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="93a41-201">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="93a41-201">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="93a41-202">Aggiungere una cartella denominata *Modelli*.</span><span class="sxs-lookup"><span data-stu-id="93a41-202">Add a folder named *Models*.</span></span>
1. <span data-ttu-id="93a41-203">Aggiungere una classe `TodoItem` alla cartella *Models* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="93a41-203">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="93a41-204">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="93a41-204">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="93a41-205">Fare clic con il pulsante destro del mouse sul progetto.</span><span class="sxs-lookup"><span data-stu-id="93a41-205">Right-click the project.</span></span> <span data-ttu-id="93a41-206">Selezionare **Aggiungi** > **Nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="93a41-206">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="93a41-207">Assegnare il nome *Models* alla cartella.</span><span class="sxs-lookup"><span data-stu-id="93a41-207">Name the folder *Models*.</span></span>

    ![Nuova cartella](first-web-api-mac/_static/folder.png)

1. <span data-ttu-id="93a41-209">Fare clic con il pulsante destro del mouse sulla cartella *Models* e selezionare **Aggiungi** > **Nuovo file** > **Generale** > **Classe vuota**.</span><span class="sxs-lookup"><span data-stu-id="93a41-209">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>
1. <span data-ttu-id="93a41-210">Assegnare alla classe il nome *TodoItem* e fare clic su **Nuovo**.</span><span class="sxs-lookup"><span data-stu-id="93a41-210">Name the class *TodoItem*, and then click **New**.</span></span>
1. <span data-ttu-id="93a41-211">Sostituire il codice del modello con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="93a41-211">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="93a41-212">La proprietà `Id` funziona come chiave univoca in un database relazionale.</span><span class="sxs-lookup"><span data-stu-id="93a41-212">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="93a41-213">È possibile inserire le classi del modello in qualsiasi punto del progetto, ma per convenzione viene usata la cartella *Models*.</span><span class="sxs-lookup"><span data-stu-id="93a41-213">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="93a41-214">Aggiungere un contesto di database</span><span class="sxs-lookup"><span data-stu-id="93a41-214">Add a database context</span></span>

<span data-ttu-id="93a41-215">Il *contesto di database* è la classe principale che coordina le funzionalità di Entity Framework per un modello di dati.</span><span class="sxs-lookup"><span data-stu-id="93a41-215">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="93a41-216">Questa classe viene creata mediante derivazione dalla classe `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="93a41-216">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="93a41-217">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="93a41-217">Visual Studio</span></span>](#tab/visual-studio)

### <a name="add-microsoftentityframeworkcoresqlserver"></a><span data-ttu-id="93a41-218">Aggiungere Microsoft.EntityFrameworkCore.SqlServer</span><span class="sxs-lookup"><span data-stu-id="93a41-218">Add Microsoft.EntityFrameworkCore.SqlServer</span></span>

1. <span data-ttu-id="93a41-219">Dal menu **Strumenti** selezionare **Gestione pacchetti NuGet > Gestisci pacchetti NuGet per la soluzione**.</span><span class="sxs-lookup"><span data-stu-id="93a41-219">From the **Tools** menu, select **NuGet Package Manager > Manage NuGet Packages for Solution**.</span></span>
1. <span data-ttu-id="93a41-220">Selezionare la scheda **Esplora** e quindi immettere **Microsoft.EntityFrameworkCore.SqlServer** nella casella di ricerca.</span><span class="sxs-lookup"><span data-stu-id="93a41-220">Select the **Browse** tab, and then enter **Microsoft.EntityFrameworkCore.SqlServer** in the search box.</span></span>
1. <span data-ttu-id="93a41-221">Nel riquadro sinistro selezionare **Microsoft. EntityFrameworkCore. SqlServer** .</span><span class="sxs-lookup"><span data-stu-id="93a41-221">Select **Microsoft.EntityFrameworkCore.SqlServer** in the left pane.</span></span>
1. <span data-ttu-id="93a41-222">Selezionare la casella di controllo **Progetto** nel riquadro di destra e quindi selezionare **Installa**.</span><span class="sxs-lookup"><span data-stu-id="93a41-222">Select the **Project** check box in the right pane and then select **Install**.</span></span>
1. <span data-ttu-id="93a41-223">Usare le istruzioni precedenti per aggiungere il pacchetto NuGet `Microsoft.EntityFrameworkCore.InMemory`.</span><span class="sxs-lookup"><span data-stu-id="93a41-223">Use the preceding instructions to add the `Microsoft.EntityFrameworkCore.InMemory` NuGet package.</span></span>

![Gestione pacchetti NuGet](first-web-api/_static/vs3nuget.png)

## <a name="add-the-todocontext-database-context"></a><span data-ttu-id="93a41-225">Aggiungere il contesto del database TodoContext</span><span class="sxs-lookup"><span data-stu-id="93a41-225">Add the TodoContext database context</span></span>

* <span data-ttu-id="93a41-226">Fare clic con il pulsante destro del mouse sulla cartella *Models* e scegliere **Aggiungi** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="93a41-226">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="93a41-227">Assegnare alla classe il nome *TodoContext* e fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="93a41-227">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="93a41-228">Visual Studio Code/Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="93a41-228">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="93a41-229">Aggiungere una classe `TodoContext` alla cartella *Models*.</span><span class="sxs-lookup"><span data-stu-id="93a41-229">Add a `TodoContext` class to the *Models* folder.</span></span>

---

* <span data-ttu-id="93a41-230">Immettere il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="93a41-230">Enter the following code:</span></span>

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="93a41-231">Registrare il contesto del database</span><span class="sxs-lookup"><span data-stu-id="93a41-231">Register the database context</span></span>

<span data-ttu-id="93a41-232">In ASP.NET Core, i servizi, ad esempio il contesto DI database, devono essere registrati con il contenitore [di inserimento delle dipendenze](xref:fundamentals/dependency-injection) .</span><span class="sxs-lookup"><span data-stu-id="93a41-232">In ASP.NET Core, services such as the database context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="93a41-233">Il contenitore rende disponibile il servizio ai controller.</span><span class="sxs-lookup"><span data-stu-id="93a41-233">The container provides the service to controllers.</span></span>

<span data-ttu-id="93a41-234">Aggiornare *Startup.cs* con il codice evidenziato seguente:</span><span class="sxs-lookup"><span data-stu-id="93a41-234">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Startup.cs?highlight=7-8,23-24&name=snippet_all)]

<span data-ttu-id="93a41-235">Il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="93a41-235">The preceding code:</span></span>

* <span data-ttu-id="93a41-236">Rimuove le dichiarazioni `using` inutilizzate.</span><span class="sxs-lookup"><span data-stu-id="93a41-236">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="93a41-237">Aggiunge il contesto del database al contenitore di inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="93a41-237">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="93a41-238">Specifica che il contesto del database userà un database in memoria.</span><span class="sxs-lookup"><span data-stu-id="93a41-238">Specifies that the database context will use an in-memory database.</span></span>

## <a name="scaffold-a-controller"></a><span data-ttu-id="93a41-239">Eseguire lo scaffolding di un controller</span><span class="sxs-lookup"><span data-stu-id="93a41-239">Scaffold a controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="93a41-240">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="93a41-240">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="93a41-241">Fare clic con il pulsante destro del mouse sulla cartella *Controllers*.</span><span class="sxs-lookup"><span data-stu-id="93a41-241">Right-click the *Controllers* folder.</span></span>
1. <span data-ttu-id="93a41-242">Selezionare **aggiungi**  > **nuovo elemento con impalcatura**.</span><span class="sxs-lookup"><span data-stu-id="93a41-242">Select **Add** > **New Scaffolded Item**.</span></span>
1. <span data-ttu-id="93a41-243">Selezionare **Controller API con azioni, che usa Entity Framework** e quindi selezionare **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="93a41-243">Select **API Controller with actions, using Entity Framework**, and then select **Add**.</span></span>
1. <span data-ttu-id="93a41-244">Nella finestra di dialogo **Add API Controller with actions, using Entity Framework** (Aggiungi controller API con azioni, che usa Entity Framework):</span><span class="sxs-lookup"><span data-stu-id="93a41-244">In the **Add API Controller with actions, using Entity Framework** dialog:</span></span>
    * <span data-ttu-id="93a41-245">Selezionare **TodoItem (TodoApi. Models)** nella **classe del modello**.</span><span class="sxs-lookup"><span data-stu-id="93a41-245">Select **TodoItem (TodoApi.Models)** in the **Model class**.</span></span>
    * <span data-ttu-id="93a41-246">Selezionare **TodoContext (TodoApi. Models)** nella **classe del contesto dati**.</span><span class="sxs-lookup"><span data-stu-id="93a41-246">Select **TodoContext (TodoApi.Models)** in the **Data context class**.</span></span>
    * <span data-ttu-id="93a41-247">Selezionare **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="93a41-247">Select **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="93a41-248">Visual Studio Code/Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="93a41-248">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="93a41-249">Eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="93a41-249">Run the following commands:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet tool install --global dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator controller -name TodoItemsController -async -api -m TodoItem -dc TodoContext -outDir Controllers
```

<span data-ttu-id="93a41-250">I comandi precedenti:</span><span class="sxs-lookup"><span data-stu-id="93a41-250">The preceding commands:</span></span>

* <span data-ttu-id="93a41-251">Aggiungere i pacchetti NuGet necessari per lo scaffolding.</span><span class="sxs-lookup"><span data-stu-id="93a41-251">Add NuGet packages required for scaffolding.</span></span>
* <span data-ttu-id="93a41-252">Installa il motore di scaffolding (`dotnet-aspnet-codegenerator`).</span><span class="sxs-lookup"><span data-stu-id="93a41-252">Installs the scaffolding engine (`dotnet-aspnet-codegenerator`).</span></span>
* <span data-ttu-id="93a41-253">Esegue lo scaffolding di `TodoItemsController`.</span><span class="sxs-lookup"><span data-stu-id="93a41-253">Scaffolds the `TodoItemsController`.</span></span>

---

<span data-ttu-id="93a41-254">Il codice generato:</span><span class="sxs-lookup"><span data-stu-id="93a41-254">The generated code:</span></span>

* <span data-ttu-id="93a41-255">Definisce una classe controller API senza metodi.</span><span class="sxs-lookup"><span data-stu-id="93a41-255">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="93a41-256">Contrassegna la classe con l'attributo [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute).</span><span class="sxs-lookup"><span data-stu-id="93a41-256">Decorates the class with the [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute.</span></span> <span data-ttu-id="93a41-257">L'attributo indica che il controller risponde alle richieste di API Web.</span><span class="sxs-lookup"><span data-stu-id="93a41-257">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="93a41-258">Per informazioni sui comportamenti specifici consentiti dall'attributo, vedere <xref:web-api/index>.</span><span class="sxs-lookup"><span data-stu-id="93a41-258">For information about specific behaviors that the attribute enables, see <xref:web-api/index>.</span></span>
* <span data-ttu-id="93a41-259">Usa l'inserimento delle dipendenze per inserire il contesto del database (`TodoContext`) nel controller.</span><span class="sxs-lookup"><span data-stu-id="93a41-259">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="93a41-260">Il contesto di database viene usato in ognuno dei metodi [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) nel controller.</span><span class="sxs-lookup"><span data-stu-id="93a41-260">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

## <a name="examine-the-posttodoitem-create-method"></a><span data-ttu-id="93a41-261">Esaminare il metodo di creazione PostTodoItem</span><span class="sxs-lookup"><span data-stu-id="93a41-261">Examine the PostTodoItem create method</span></span>

<span data-ttu-id="93a41-262">Sostituire l'istruzione return in `PostTodoItem` per usare l'operatore [nameof](/dotnet/csharp/language-reference/operators/nameof):</span><span class="sxs-lookup"><span data-stu-id="93a41-262">Replace the return statement in the `PostTodoItem` to use the [nameof](/dotnet/csharp/language-reference/operators/nameof) operator:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Create)]

<span data-ttu-id="93a41-263">Il codice precedente è un metodo HTTP POST, come indicato dall'attributo [[HttpPost]](xref:Microsoft.AspNetCore.Mvc.HttpPostAttribute).</span><span class="sxs-lookup"><span data-stu-id="93a41-263">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](xref:Microsoft.AspNetCore.Mvc.HttpPostAttribute) attribute.</span></span> <span data-ttu-id="93a41-264">Il metodo ottiene il valore dell'elemento attività dal corpo della richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="93a41-264">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="93a41-265">Il metodo <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*>:</span><span class="sxs-lookup"><span data-stu-id="93a41-265">The <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> method:</span></span>

* <span data-ttu-id="93a41-266">Restituisce un codice di stato HTTP 201 in caso di esito positivo.</span><span class="sxs-lookup"><span data-stu-id="93a41-266">Returns an HTTP 201 status code if successful.</span></span> <span data-ttu-id="93a41-267">HTTP 201 è la risposta standard per un metodo HTTP POST che crea una nuova risorsa nel server.</span><span class="sxs-lookup"><span data-stu-id="93a41-267">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="93a41-268">Aggiunge un'intestazione [Location](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) alla risposta.</span><span class="sxs-lookup"><span data-stu-id="93a41-268">Adds a [Location](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) header to the response.</span></span> <span data-ttu-id="93a41-269">L'intestazione `Location` specifica l'[URI](https://developer.mozilla.org/docs/Glossary/URI) dell'elemento attività appena creato.</span><span class="sxs-lookup"><span data-stu-id="93a41-269">The `Location` header specifies the [URI](https://developer.mozilla.org/docs/Glossary/URI) of the newly created to-do item.</span></span> <span data-ttu-id="93a41-270">Per altre informazioni, vedere [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="93a41-270">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="93a41-271">Fa riferimento all'azione `GetTodoItem` per creare l'URI dell'intestazione `Location`.</span><span class="sxs-lookup"><span data-stu-id="93a41-271">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="93a41-272">La parola chiave `nameof` C# viene usata per evitare di impostare il nome dell'azione come hardcoded nella chiamata a `CreatedAtAction`.</span><span class="sxs-lookup"><span data-stu-id="93a41-272">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

### <a name="install-postman"></a><span data-ttu-id="93a41-273">Installare Postman</span><span class="sxs-lookup"><span data-stu-id="93a41-273">Install Postman</span></span>

<span data-ttu-id="93a41-274">Questa esercitazione usa Postman per testare l'API Web.</span><span class="sxs-lookup"><span data-stu-id="93a41-274">This tutorial uses Postman to test the web API.</span></span>

1. <span data-ttu-id="93a41-275">Installare [Postman](https://www.getpostman.com/downloads/)</span><span class="sxs-lookup"><span data-stu-id="93a41-275">Install [Postman](https://www.getpostman.com/downloads/)</span></span>
1. <span data-ttu-id="93a41-276">Avviare l'app Web.</span><span class="sxs-lookup"><span data-stu-id="93a41-276">Start the web app.</span></span>
1. <span data-ttu-id="93a41-277">Avviare Postman.</span><span class="sxs-lookup"><span data-stu-id="93a41-277">Start Postman.</span></span>
1. <span data-ttu-id="93a41-278">Disattivare **SSL certificate verification** (Verifica certificato SSL)</span><span class="sxs-lookup"><span data-stu-id="93a41-278">Disable **SSL certificate verification**</span></span>
1. <span data-ttu-id="93a41-279">In **File**  > **Impostazioni** (scheda**generale** ) disabilitare la **Verifica del certificato SSL**.</span><span class="sxs-lookup"><span data-stu-id="93a41-279">From **File** > **Settings** (**General** tab), disable **SSL certificate verification**.</span></span>

    > [!WARNING]
    > <span data-ttu-id="93a41-280">Riattivare la verifica dei certificati SSL al termine del test del controller.</span><span class="sxs-lookup"><span data-stu-id="93a41-280">Re-enable SSL certificate verification after testing the controller.</span></span>

<a name="post"></a>

### <a name="test-posttodoitem-with-postman"></a><span data-ttu-id="93a41-281">Testare PostTodoItem con Postman</span><span class="sxs-lookup"><span data-stu-id="93a41-281">Test PostTodoItem with Postman</span></span>

1. <span data-ttu-id="93a41-282">Creare una nuova richiesta.</span><span class="sxs-lookup"><span data-stu-id="93a41-282">Create a new request.</span></span>
1. <span data-ttu-id="93a41-283">Impostare il metodo HTTP su `POST`.</span><span class="sxs-lookup"><span data-stu-id="93a41-283">Set the HTTP method to `POST`.</span></span>
1. <span data-ttu-id="93a41-284">Fare clic sulla scheda **Body** (Corpo).</span><span class="sxs-lookup"><span data-stu-id="93a41-284">Select the **Body** tab.</span></span>
1. <span data-ttu-id="93a41-285">Selezionare il pulsante di opzione **raw** (non elaborato).</span><span class="sxs-lookup"><span data-stu-id="93a41-285">Select the **raw** radio button.</span></span>
1. <span data-ttu-id="93a41-286">Impostare il tipo su **JSON (application/json)** .</span><span class="sxs-lookup"><span data-stu-id="93a41-286">Set the type to **JSON (application/json)**.</span></span>
1. <span data-ttu-id="93a41-287">Nel corpo della richiesta immettere JSON per un elemento attività:</span><span class="sxs-lookup"><span data-stu-id="93a41-287">In the request body, enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

1. <span data-ttu-id="93a41-288">Selezionare **Send** (Invia).</span><span class="sxs-lookup"><span data-stu-id="93a41-288">Select **Send**.</span></span>

  ![Postman con richiesta di creazione](first-web-api/_static/create.png)

### <a name="test-the-location-header-uri"></a><span data-ttu-id="93a41-290">Testare l'URI dell'intestazione della posizione</span><span class="sxs-lookup"><span data-stu-id="93a41-290">Test the location header URI</span></span>

1. <span data-ttu-id="93a41-291">Selezionare la scheda **Headers** (Intestazioni) nel riquadro **Response** (Risposta).</span><span class="sxs-lookup"><span data-stu-id="93a41-291">Select the **Headers** tab in the **Response** pane.</span></span>
1. <span data-ttu-id="93a41-292">Copiare il valore dell'intestazione **Location** (Posizione):</span><span class="sxs-lookup"><span data-stu-id="93a41-292">Copy the **Location** header value:</span></span>

    ![Scheda Headers (Intestazioni) della console Postman](first-web-api/_static/create.png)

1. <span data-ttu-id="93a41-294">Impostare il metodo su GET.</span><span class="sxs-lookup"><span data-stu-id="93a41-294">Set the method to GET.</span></span>
1. <span data-ttu-id="93a41-295">Incollare l'URI, ad esempio `https://localhost:5001/api/TodoItems/1`.</span><span class="sxs-lookup"><span data-stu-id="93a41-295">Paste the URI (for example, `https://localhost:5001/api/TodoItems/1`).</span></span>
1. <span data-ttu-id="93a41-296">Selezionare **Send** (Invia).</span><span class="sxs-lookup"><span data-stu-id="93a41-296">Select **Send**.</span></span>

## <a name="examine-the-get-methods"></a><span data-ttu-id="93a41-297">Esaminare i metodi GET</span><span class="sxs-lookup"><span data-stu-id="93a41-297">Examine the GET methods</span></span>

<span data-ttu-id="93a41-298">Questi metodi implementano due metodi GET:</span><span class="sxs-lookup"><span data-stu-id="93a41-298">These methods implement two GET endpoints:</span></span>

* `GET /api/TodoItems`
* `GET /api/TodoItems/{id}`

<span data-ttu-id="93a41-299">Testare l'app chiamando i due endpoint da un browser o da Postman.</span><span class="sxs-lookup"><span data-stu-id="93a41-299">Test the app by calling the two endpoints from a browser or Postman.</span></span> <span data-ttu-id="93a41-300">Esempio:</span><span class="sxs-lookup"><span data-stu-id="93a41-300">For example:</span></span>

* [https://localhost:5001/api/TodoItems](https://localhost:5001/api/TodoItems)
* [https://localhost:5001/api/TodoItems/1](https://localhost:5001/api/TodoItems/1)

<span data-ttu-id="93a41-301">Una risposta simile alla seguente viene generata dalla chiamata a `GetTodoItems`:</span><span class="sxs-lookup"><span data-stu-id="93a41-301">A response similar to the following is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

### <a name="test-get-with-postman"></a><span data-ttu-id="93a41-302">Testare Get con Postman</span><span class="sxs-lookup"><span data-stu-id="93a41-302">Test Get with Postman</span></span>

1. <span data-ttu-id="93a41-303">Creare una nuova richiesta.</span><span class="sxs-lookup"><span data-stu-id="93a41-303">Create a new request.</span></span>
1. <span data-ttu-id="93a41-304">Impostare il metodo HTTP su **GET**.</span><span class="sxs-lookup"><span data-stu-id="93a41-304">Set the HTTP method to **GET**.</span></span>
1. <span data-ttu-id="93a41-305">Impostare l'URL della richiesta su `https://localhost:<port>/api/TodoItems`.</span><span class="sxs-lookup"><span data-stu-id="93a41-305">Set the request URL to `https://localhost:<port>/api/TodoItems`.</span></span> <span data-ttu-id="93a41-306">Ad esempio `https://localhost:5001/api/TodoItems`.</span><span class="sxs-lookup"><span data-stu-id="93a41-306">For example, `https://localhost:5001/api/TodoItems`.</span></span>
1. <span data-ttu-id="93a41-307">Impostare **Two pane view** (Visualizzazione in due riquadri) in Postman.</span><span class="sxs-lookup"><span data-stu-id="93a41-307">Set **Two pane view** in Postman.</span></span>
1. <span data-ttu-id="93a41-308">Selezionare **Send** (Invia).</span><span class="sxs-lookup"><span data-stu-id="93a41-308">Select **Send**.</span></span>

<span data-ttu-id="93a41-309">Questa app usa un database in memoria.</span><span class="sxs-lookup"><span data-stu-id="93a41-309">This app uses an in-memory database.</span></span> <span data-ttu-id="93a41-310">Se l'app viene arrestata e avviata, la richiesta GET precedente non restituirà alcun dato.</span><span class="sxs-lookup"><span data-stu-id="93a41-310">If the app is stopped and started, the preceding GET request won't return any data.</span></span> <span data-ttu-id="93a41-311">Se non vengono restituiti dati, eseguire [POST](#post) per pubblicare i dati nell'app.</span><span class="sxs-lookup"><span data-stu-id="93a41-311">If no data is returned, [POST](#post) data to the app.</span></span>

## <a name="routing-and-url-paths"></a><span data-ttu-id="93a41-312">Routing e percorsi di URL</span><span class="sxs-lookup"><span data-stu-id="93a41-312">Routing and URL paths</span></span>

<span data-ttu-id="93a41-313">L'attributo [[HttpGet]](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute) denota un metodo che risponde a una richiesta HTTP Get.</span><span class="sxs-lookup"><span data-stu-id="93a41-313">The [[HttpGet]](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="93a41-314">Il percorso dell'URL per ogni metodo viene costruito nel modo seguente:</span><span class="sxs-lookup"><span data-stu-id="93a41-314">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="93a41-315">Iniziare con la stringa di modello nell'attributo `Route` del controller:</span><span class="sxs-lookup"><span data-stu-id="93a41-315">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=TodoController&highlight=1)]

* <span data-ttu-id="93a41-316">Sostituire `[controller]` con il nome del controller, ovvero, per convenzione, il nome della classe controller meno il suffisso "Controller".</span><span class="sxs-lookup"><span data-stu-id="93a41-316">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="93a41-317">In questo esempio il nome della classe controller è **TodoItems**Controller, quindi il nome del controller è "TodoItems".</span><span class="sxs-lookup"><span data-stu-id="93a41-317">For this sample, the controller class name is **TodoItems**Controller, so the controller name is "TodoItems".</span></span> <span data-ttu-id="93a41-318">Il [routing](xref:mvc/controllers/routing) ASP.NET Core non fa distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="93a41-318">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="93a41-319">Se l'attributo `[HttpGet]` ha un modello di route, ad esempio `[HttpGet("products")]`, aggiungerlo al percorso.</span><span class="sxs-lookup"><span data-stu-id="93a41-319">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="93a41-320">In questo esempio non si usa un modello.</span><span class="sxs-lookup"><span data-stu-id="93a41-320">This sample doesn't use a template.</span></span> <span data-ttu-id="93a41-321">Per altre informazioni, vedere [Routing con attributi Http[verb]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="93a41-321">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="93a41-322">Nel metodo `GetTodoItem` seguente, `"{id}"` è una variabile segnaposto per l'identificatore univoco dell'elemento attività.</span><span class="sxs-lookup"><span data-stu-id="93a41-322">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="93a41-323">Quando viene richiamato `GetTodoItem`, il valore di `"{id}"` nell'URL viene fornito al metodo nel parametro `id`.</span><span class="sxs-lookup"><span data-stu-id="93a41-323">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its `id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a><span data-ttu-id="93a41-324">Valori restituiti</span><span class="sxs-lookup"><span data-stu-id="93a41-324">Return values</span></span>

<span data-ttu-id="93a41-325">Il tipo restituito dei metodi `GetTodoItems` e `GetTodoItem` è [ActionResult\<T>](xref:web-api/action-return-types#actionresultt-type).</span><span class="sxs-lookup"><span data-stu-id="93a41-325">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="93a41-326">ASP.NET Core serializza automaticamente l'oggetto su [JSON](https://www.json.org/) e scrive il codice JSON nel corpo del messaggio di risposta.</span><span class="sxs-lookup"><span data-stu-id="93a41-326">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="93a41-327">Il codice di risposta per questo tipo restituito è 200, presupponendo che non esistano eccezioni non gestite.</span><span class="sxs-lookup"><span data-stu-id="93a41-327">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="93a41-328">Le eccezioni non gestite vengono convertite in errori 5xx.</span><span class="sxs-lookup"><span data-stu-id="93a41-328">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="93a41-329">I tipi restituiti `ActionResult` possono rappresentare un ampio intervallo di codici di stato HTTP.</span><span class="sxs-lookup"><span data-stu-id="93a41-329">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="93a41-330">Ad esempio, `GetTodoItem` può restituire due valori di stato diversi:</span><span class="sxs-lookup"><span data-stu-id="93a41-330">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="93a41-331">Se nessun elemento corrisponde all'ID richiesto, il metodo restituisce un codice di errore 404 <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound>.</span><span class="sxs-lookup"><span data-stu-id="93a41-331">If no item matches the requested ID, the method returns a 404 <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound> error code.</span></span>
* <span data-ttu-id="93a41-332">In caso contrario, il metodo restituisce il codice 200 con un corpo della risposta JSON.</span><span class="sxs-lookup"><span data-stu-id="93a41-332">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="93a41-333">La restituzione di `item` risulta in una risposta HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="93a41-333">Returning `item` results in an HTTP 200 response.</span></span>

## <a name="the-puttodoitem-method"></a><span data-ttu-id="93a41-334">Metodo PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="93a41-334">The PutTodoItem method</span></span>

<span data-ttu-id="93a41-335">Esaminare il metodo `PutTodoItem`:</span><span class="sxs-lookup"><span data-stu-id="93a41-335">Examine the `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Update)]

<span data-ttu-id="93a41-336">`PutTodoItem` è simile a `PostTodoItem` ma usa la richiesta HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="93a41-336">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="93a41-337">La risposta è [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="93a41-337">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="93a41-338">In base alla specifica HTTP, una richiesta PUT richiede che il client invii l'intera entità aggiornata e non solo le modifiche.</span><span class="sxs-lookup"><span data-stu-id="93a41-338">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="93a41-339">Per supportare gli aggiornamenti parziali, usare [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span><span class="sxs-lookup"><span data-stu-id="93a41-339">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="93a41-340">Se si riceve un errore chiamando `PutTodoItem`, chiamare `GET` per verificare che il database includa un elemento.</span><span class="sxs-lookup"><span data-stu-id="93a41-340">If you get an error calling `PutTodoItem`, call `GET` to ensure there's an item in the database.</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="93a41-341">Testare il metodo PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="93a41-341">Test the PutTodoItem method</span></span>

<span data-ttu-id="93a41-342">Questo esempio usa un database in memoria che deve essere inizializzato ogni volta che l'app viene avviata.</span><span class="sxs-lookup"><span data-stu-id="93a41-342">This sample uses an in-memory database that must be initialized each time the app is started.</span></span> <span data-ttu-id="93a41-343">Deve esistere un elemento nel database prima di eseguire una chiamata PUT.</span><span class="sxs-lookup"><span data-stu-id="93a41-343">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="93a41-344">Chiamare GET per assicurarsi che sia presente un elemento nel database prima di eseguire una chiamata PUT.</span><span class="sxs-lookup"><span data-stu-id="93a41-344">Call GET to insure there's an item in the database before making a PUT call.</span></span>

<span data-ttu-id="93a41-345">Aggiornare l'attività con ID 1.</span><span class="sxs-lookup"><span data-stu-id="93a41-345">Update the to-do item that has an ID of 1.</span></span> <span data-ttu-id="93a41-346">Impostarne il nome su "feed Fish":</span><span class="sxs-lookup"><span data-stu-id="93a41-346">Set its name to "feed fish":</span></span>

```json
{
  "ID":1,
  "name":"feed fish",
  "isComplete":true
}
```

<span data-ttu-id="93a41-347">L'immagine seguente visualizza l'aggiornamento di Postman:</span><span class="sxs-lookup"><span data-stu-id="93a41-347">The following image shows the Postman update:</span></span>

![Console Postman con risposta 204 (No Content)](first-web-api/_static/pmcput.png)

## <a name="the-deletetodoitem-method"></a><span data-ttu-id="93a41-349">Metodo DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="93a41-349">The DeleteTodoItem method</span></span>

<span data-ttu-id="93a41-350">Esaminare il metodo `DeleteTodoItem`:</span><span class="sxs-lookup"><span data-stu-id="93a41-350">Examine the `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Delete)]

<span data-ttu-id="93a41-351">La risposta `DeleteTodoItem` è [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="93a41-351">The `DeleteTodoItem` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="93a41-352">Testare il metodo DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="93a41-352">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="93a41-353">Usare Postman per eliminare un elemento attività:</span><span class="sxs-lookup"><span data-stu-id="93a41-353">Use Postman to delete a to-do item:</span></span>

1. <span data-ttu-id="93a41-354">Impostare il metodo su `DELETE`.</span><span class="sxs-lookup"><span data-stu-id="93a41-354">Set the method to `DELETE`.</span></span>
1. <span data-ttu-id="93a41-355">Impostare l'URI dell'oggetto da eliminare, ad esempio `https://localhost:5001/api/TodoItems/1`.</span><span class="sxs-lookup"><span data-stu-id="93a41-355">Set the URI of the object to delete (for example, `https://localhost:5001/api/TodoItems/1`).</span></span>
1. <span data-ttu-id="93a41-356">Selezionare **Send** (Invia).</span><span class="sxs-lookup"><span data-stu-id="93a41-356">Select **Send**.</span></span>

## <a name="call-the-web-api-with-javascript"></a><span data-ttu-id="93a41-357">Chiamare l'API Web con JavaScript</span><span class="sxs-lookup"><span data-stu-id="93a41-357">Call the web API with JavaScript</span></span>

<span data-ttu-id="93a41-358">Vedere [esercitazione: chiamare un'API web ASP.NET Core con JavaScript](xref:tutorials/web-api-javascript).</span><span class="sxs-lookup"><span data-stu-id="93a41-358">See [Tutorial: Call an ASP.NET Core web API with JavaScript](xref:tutorials/web-api-javascript).</span></span>

<a name="auth"></a>

## <a name="add-authentication-support-to-a-web-api"></a><span data-ttu-id="93a41-359">Aggiungere il supporto per l'autenticazione a un'API Web</span><span class="sxs-lookup"><span data-stu-id="93a41-359">Add authentication support to a web API</span></span>

<span data-ttu-id="93a41-360">Vedere l'esercitazione su [IdentityServer4](https://identityserver4.readthedocs.io/en/latest/quickstarts/0_overview.html) .</span><span class="sxs-lookup"><span data-stu-id="93a41-360">See the [IdentityServer4](https://identityserver4.readthedocs.io/en/latest/quickstarts/0_overview.html) tutorial.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="93a41-361">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="93a41-361">Additional resources</span></span>

<span data-ttu-id="93a41-362">[Visualizzare o scaricare il codice di esempio per questa esercitazione](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span><span class="sxs-lookup"><span data-stu-id="93a41-362">[View or download sample code for this tutorial](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span></span> <span data-ttu-id="93a41-363">Vedere [come scaricare un esempio](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="93a41-363">See [how to download](xref:index#how-to-download-a-sample).</span></span>

<span data-ttu-id="93a41-364">Per altre informazioni, vedere le seguenti risorse:</span><span class="sxs-lookup"><span data-stu-id="93a41-364">For more information, see the following resources:</span></span>

* <xref:web-api/index>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:data/ef-rp/intro>
* <xref:mvc/controllers/routing>
* <xref:web-api/action-return-types>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/index>
* [<span data-ttu-id="93a41-365">Versione YouTube dell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="93a41-365">YouTube version of this tutorial</span></span>](https://www.youtube.com/watch?v=TTkhEyGBfAk)
