---
title: "Esercitazione: Creare un'API Web con ASP.NET Core"
author: rick-anderson
description: Informazioni su come creare un'API Web con ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 08/05/2019
uid: tutorials/first-web-api
ms.openlocfilehash: 855d05fa2b9c1a7572212c40adbe61bb396f4bac
ms.sourcegitcommit: 2eb605f4f20ac4dd9de6c3b3e3453e108a357a21
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/06/2019
ms.locfileid: "68819841"
---
# <a name="tutorial-create-a-web-api-with-aspnet-core"></a><span data-ttu-id="488b7-103">Esercitazione: Creare un'API Web con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="488b7-103">Tutorial: Create a web API with ASP.NET Core</span></span>

<span data-ttu-id="488b7-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT) e [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="488b7-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="488b7-105">Questa esercitazione illustra le nozioni di base della creazione di un'API Web con ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="488b7-105">This tutorial teaches the basics of building a web API with ASP.NET Core.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="488b7-106">In questa esercitazione si imparerà a:</span><span class="sxs-lookup"><span data-stu-id="488b7-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="488b7-107">Creare un progetto di API Web.</span><span class="sxs-lookup"><span data-stu-id="488b7-107">Create a web API project.</span></span>
> * <span data-ttu-id="488b7-108">Aggiungere una classe modello e un contesto di database.</span><span class="sxs-lookup"><span data-stu-id="488b7-108">Add a model class and a database context.</span></span>
> * <span data-ttu-id="488b7-109">Eseguire lo scaffolding di un controller con i metodi CRUD.</span><span class="sxs-lookup"><span data-stu-id="488b7-109">Scaffold a controller with CRUD methods.</span></span>
> * <span data-ttu-id="488b7-110">Configurare il routing, i percorsi URL e i valori restituiti.</span><span class="sxs-lookup"><span data-stu-id="488b7-110">Configure routing, URL paths, and return values.</span></span>
> * <span data-ttu-id="488b7-111">Chiamare l'API Web con Postman.</span><span class="sxs-lookup"><span data-stu-id="488b7-111">Call the web API with Postman.</span></span>

<span data-ttu-id="488b7-112">Al termine si avrà un'API Web che può gestire gli elementi di tipo "attività" archiviati in un database.</span><span class="sxs-lookup"><span data-stu-id="488b7-112">At the end, you have a web API that can manage "to-do" items stored in a database.</span></span>

## <a name="overview"></a><span data-ttu-id="488b7-113">Panoramica</span><span class="sxs-lookup"><span data-stu-id="488b7-113">Overview</span></span>

<span data-ttu-id="488b7-114">Questa esercitazione consente di creare l'API seguente:</span><span class="sxs-lookup"><span data-stu-id="488b7-114">This tutorial creates the following API:</span></span>

|<span data-ttu-id="488b7-115">API</span><span class="sxs-lookup"><span data-stu-id="488b7-115">API</span></span> | <span data-ttu-id="488b7-116">DESCRIZIONE</span><span class="sxs-lookup"><span data-stu-id="488b7-116">Description</span></span> | <span data-ttu-id="488b7-117">Corpo della richiesta</span><span class="sxs-lookup"><span data-stu-id="488b7-117">Request body</span></span> | <span data-ttu-id="488b7-118">Corpo della risposta</span><span class="sxs-lookup"><span data-stu-id="488b7-118">Response body</span></span> |
|--- | ---- | ---- | ---- |
|<span data-ttu-id="488b7-119">GET /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="488b7-119">GET /api/TodoItems</span></span> | <span data-ttu-id="488b7-120">Ottiene tutti gli elementi attività</span><span class="sxs-lookup"><span data-stu-id="488b7-120">Get all to-do items</span></span> | <span data-ttu-id="488b7-121">nessuno</span><span class="sxs-lookup"><span data-stu-id="488b7-121">None</span></span> | <span data-ttu-id="488b7-122">Matrice di elementi attività</span><span class="sxs-lookup"><span data-stu-id="488b7-122">Array of to-do items</span></span>|
|<span data-ttu-id="488b7-123">GET /api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="488b7-123">GET /api/TodoItems/{id}</span></span> | <span data-ttu-id="488b7-124">Ottiene un elemento in base all'ID</span><span class="sxs-lookup"><span data-stu-id="488b7-124">Get an item by ID</span></span> | <span data-ttu-id="488b7-125">nessuno</span><span class="sxs-lookup"><span data-stu-id="488b7-125">None</span></span> | <span data-ttu-id="488b7-126">Elemento attività</span><span class="sxs-lookup"><span data-stu-id="488b7-126">To-do item</span></span>|
|<span data-ttu-id="488b7-127">POST /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="488b7-127">POST /api/TodoItems</span></span> | <span data-ttu-id="488b7-128">Aggiunge un nuovo elemento</span><span class="sxs-lookup"><span data-stu-id="488b7-128">Add a new item</span></span> | <span data-ttu-id="488b7-129">Elemento attività</span><span class="sxs-lookup"><span data-stu-id="488b7-129">To-do item</span></span> | <span data-ttu-id="488b7-130">Elemento attività</span><span class="sxs-lookup"><span data-stu-id="488b7-130">To-do item</span></span> |
|<span data-ttu-id="488b7-131">PUT /api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="488b7-131">PUT /api/TodoItems/{id}</span></span> | <span data-ttu-id="488b7-132">Aggiorna un elemento esistente &nbsp;</span><span class="sxs-lookup"><span data-stu-id="488b7-132">Update an existing item &nbsp;</span></span> | <span data-ttu-id="488b7-133">Elemento attività</span><span class="sxs-lookup"><span data-stu-id="488b7-133">To-do item</span></span> | <span data-ttu-id="488b7-134">nessuno</span><span class="sxs-lookup"><span data-stu-id="488b7-134">None</span></span> |
|<span data-ttu-id="488b7-135">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="488b7-135">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span></span> | <span data-ttu-id="488b7-136">Elimina un elemento &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="488b7-136">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="488b7-137">nessuno</span><span class="sxs-lookup"><span data-stu-id="488b7-137">None</span></span> | <span data-ttu-id="488b7-138">nessuno</span><span class="sxs-lookup"><span data-stu-id="488b7-138">None</span></span>|

<span data-ttu-id="488b7-139">Il diagramma seguente visualizza la struttura dell'app.</span><span class="sxs-lookup"><span data-stu-id="488b7-139">The following diagram shows the design of the app.</span></span>

![Il client è rappresentato da una casella a sinistra.](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="488b7-145">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="488b7-145">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="488b7-146">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="488b7-146">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="488b7-147">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="488b7-147">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="488b7-148">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="488b7-148">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="488b7-149">Creare un progetto Web</span><span class="sxs-lookup"><span data-stu-id="488b7-149">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="488b7-150">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="488b7-150">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="488b7-151">Scegliere **Nuovo** > **Progetto** dal menu **File**.</span><span class="sxs-lookup"><span data-stu-id="488b7-151">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="488b7-152">Selezionare il modello **Applicazione Web ASP.NET Core** e fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="488b7-152">Select the **ASP.NET Core Web Application** template and click **Next**.</span></span>
* <span data-ttu-id="488b7-153">Assegnare al progetto il nome *TodoApi* e fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="488b7-153">Name the project *TodoApi* and click **Create**.</span></span>
* <span data-ttu-id="488b7-154">Nella finestra di dialogo **Crea una nuova applicazione Web ASP.NET Core** verificare che siano selezionati **.NET Core** e **ASP.NET Core 3.0**.</span><span class="sxs-lookup"><span data-stu-id="488b7-154">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 3.0** are selected.</span></span> <span data-ttu-id="488b7-155">Selezionare il modello **API** e fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="488b7-155">Select the **API** template and click **Create**.</span></span> <span data-ttu-id="488b7-156">**Non** selezionare **Abilita supporto Docker**.</span><span class="sxs-lookup"><span data-stu-id="488b7-156">**Don't** select **Enable Docker Support**.</span></span>

![Finestra di dialogo Nuovo progetto di Visual Studio](first-web-api/_static/vs3.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="488b7-158">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="488b7-158">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="488b7-159">Aprire il [terminale integrato](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="488b7-159">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="488b7-160">Cambiare directory (`cd`) e passare alla cartella che conterrà la cartella del progetto.</span><span class="sxs-lookup"><span data-stu-id="488b7-160">Change directories (`cd`) to the folder that will contain the project folder.</span></span>
* <span data-ttu-id="488b7-161">Eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="488b7-161">Run the following commands:</span></span>

   ```console
   dotnet new webapi -o TodoApi
   cd TodoAPI
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer --version 3.0.0-*
   dotnet add package Microsoft.EntityFrameworkCore.InMemory --version 3.0.0-*
   code -r ../TodoApi
   ```

* <span data-ttu-id="488b7-162">Quando una finestra di dialogo chiede se si vuole aggiungere gli asset necessari al progetto, selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="488b7-162">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

  <span data-ttu-id="488b7-163">I comandi precedenti:</span><span class="sxs-lookup"><span data-stu-id="488b7-163">The preceding commands:</span></span>

  * <span data-ttu-id="488b7-164">Crea un nuovo progetto API Web e lo apre in Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="488b7-164">Creates a new web API project and opens it in Visual Studio Code.</span></span>
  * <span data-ttu-id="488b7-165">Aggiunge i pacchetti NuGet necessari nella sezione successiva.</span><span class="sxs-lookup"><span data-stu-id="488b7-165">Adds the NuGet packages which are required in the next section.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="488b7-166">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="488b7-166">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="488b7-167">Selezionare **File** > **Nuova soluzione**.</span><span class="sxs-lookup"><span data-stu-id="488b7-167">Select **File** > **New Solution**.</span></span>

  ![Nuova soluzione macOS](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="488b7-169">Selezionare **.NET Core** > **App** > **API** > **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="488b7-169">Select **.NET Core** > **App** > **API** > **Next**.</span></span>

  ![Finestra di dialogo Nuovo progetto di macOS](first-web-api-mac/_static/1.png)
  
* <span data-ttu-id="488b7-171">Nella finestra di dialogo **Configura la nuova API Web ASP.NET Core** selezionare il **framework di destinazione** \* *.NET Core 3.0*.</span><span class="sxs-lookup"><span data-stu-id="488b7-171">In the **Configure your new ASP.NET Core Web API** dialog, select **Target Framework** of \**.NET Core 3.0*.</span></span>

* <span data-ttu-id="488b7-172">Immettere *TodoApi* in **Nome progetto**, quindi selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="488b7-172">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![Finestra di dialogo di configurazione](first-web-api-mac/_static/2.png)

---

### <a name="test-the-api"></a><span data-ttu-id="488b7-174">Testare l'API</span><span class="sxs-lookup"><span data-stu-id="488b7-174">Test the API</span></span>

<span data-ttu-id="488b7-175">Il modello di progetto crea un'API `WeatherForecast`.</span><span class="sxs-lookup"><span data-stu-id="488b7-175">The project template creates a `WeatherForecast` API.</span></span> <span data-ttu-id="488b7-176">Chiamare il metodo `Get` da un browser per eseguire il test dell'app.</span><span class="sxs-lookup"><span data-stu-id="488b7-176">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="488b7-177">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="488b7-177">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="488b7-178">Premere CTRL+F5 per eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="488b7-178">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="488b7-179">Visual Studio apre un browser e naviga all'indirizzo `https://localhost:<port>/WeatherForecast`, dove `<port>` è un numero di porta selezionato a caso.</span><span class="sxs-lookup"><span data-stu-id="488b7-179">Visual Studio launches a browser and navigates to `https://localhost:<port>/WeatherForecast`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="488b7-180">Se in una finestra di dialogo viene chiesto se considerare attendibile il certificato IIS Express, selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="488b7-180">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="488b7-181">Nella finestra di dialogo **Avviso di sicurezza** successiva selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="488b7-181">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="488b7-182">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="488b7-182">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="488b7-183">Premere CTRL+F5 per eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="488b7-183">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="488b7-184">In un browser passare all'URL seguente: [https://localhost:5001/WeatherForecast](https://localhost:5001/WeatherForecast).</span><span class="sxs-lookup"><span data-stu-id="488b7-184">In a browser, go to following URL: [https://localhost:5001/WeatherForecast](https://localhost:5001/WeatherForecast).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="488b7-185">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="488b7-185">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="488b7-186">Selezionare **Esegui** > **Avvia debug** per avviare l'app.</span><span class="sxs-lookup"><span data-stu-id="488b7-186">Select **Run** > **Start Debugging** to launch the app.</span></span> <span data-ttu-id="488b7-187">Visual Studio per Mac apre un browser e naviga all'indirizzo `https://localhost:<port>`, dove `<port>` è un numero di porta selezionato a caso.</span><span class="sxs-lookup"><span data-stu-id="488b7-187">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="488b7-188">Viene restituito un errore HTTP 404 (Non trovato).</span><span class="sxs-lookup"><span data-stu-id="488b7-188">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="488b7-189">Accodare `/WeatherForecast` all'URL (impostare l'URL su `https://localhost:<port>/WeatherForecast`).</span><span class="sxs-lookup"><span data-stu-id="488b7-189">Append `/WeatherForecast` to the URL (change the URL to `https://localhost:<port>/WeatherForecast`).</span></span>

---

<span data-ttu-id="488b7-190">Viene restituito un codice JSON simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="488b7-190">JSON similar to the following is returned:</span></span>

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

## <a name="add-a-model-class"></a><span data-ttu-id="488b7-191">Aggiungere una classe del modello</span><span class="sxs-lookup"><span data-stu-id="488b7-191">Add a model class</span></span>

<span data-ttu-id="488b7-192">Un *modello* è un set di classi che rappresentano i dati gestiti dall'app.</span><span class="sxs-lookup"><span data-stu-id="488b7-192">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="488b7-193">Il modello per questa app è una classe `TodoItem` singola.</span><span class="sxs-lookup"><span data-stu-id="488b7-193">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="488b7-194">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="488b7-194">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="488b7-195">In **Esplora soluzioni** fare clic con il pulsante destro del mouse sul progetto.</span><span class="sxs-lookup"><span data-stu-id="488b7-195">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="488b7-196">Selezionare **Aggiungi** > **Nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="488b7-196">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="488b7-197">Assegnare il nome *Models* alla cartella.</span><span class="sxs-lookup"><span data-stu-id="488b7-197">Name the folder *Models*.</span></span>

* <span data-ttu-id="488b7-198">Fare clic con il pulsante destro del mouse sulla cartella *Models* e scegliere **Aggiungi** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="488b7-198">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="488b7-199">Assegnare alla classe il nome *TodoItem* e selezionare **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="488b7-199">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="488b7-200">Sostituire il codice del modello con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="488b7-200">Replace the template code with the following code:</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="488b7-201">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="488b7-201">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="488b7-202">Aggiungere una cartella denominata *Models*.</span><span class="sxs-lookup"><span data-stu-id="488b7-202">Add a folder named *Models*.</span></span>

* <span data-ttu-id="488b7-203">Aggiungere una classe `TodoItem` alla cartella *Models* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="488b7-203">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="488b7-204">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="488b7-204">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="488b7-205">Fare clic con il pulsante destro del mouse sul progetto.</span><span class="sxs-lookup"><span data-stu-id="488b7-205">Right-click the project.</span></span> <span data-ttu-id="488b7-206">Selezionare **Aggiungi** > **Nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="488b7-206">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="488b7-207">Assegnare il nome *Models* alla cartella.</span><span class="sxs-lookup"><span data-stu-id="488b7-207">Name the folder *Models*.</span></span>

  ![Nuova cartella](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="488b7-209">Fare clic con il pulsante destro del mouse sulla cartella *Models* e scegliere **Aggiungi** > **Nuovo file** > **Generale** > **Classe vuota**.</span><span class="sxs-lookup"><span data-stu-id="488b7-209">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="488b7-210">Assegnare alla classe il nome *TodoItem* e fare clic su **Nuovo**.</span><span class="sxs-lookup"><span data-stu-id="488b7-210">Name the class *TodoItem*, and then click **New**.</span></span>

* <span data-ttu-id="488b7-211">Sostituire il codice del modello con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="488b7-211">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="488b7-212">La proprietà `Id` funziona come chiave univoca in un database relazionale.</span><span class="sxs-lookup"><span data-stu-id="488b7-212">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="488b7-213">È possibile inserire le classi del modello in qualsiasi punto del progetto, ma per convenzione viene usata la cartella *Models*.</span><span class="sxs-lookup"><span data-stu-id="488b7-213">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="488b7-214">Aggiungere un contesto di database</span><span class="sxs-lookup"><span data-stu-id="488b7-214">Add a database context</span></span>

<span data-ttu-id="488b7-215">Il *contesto di database* è la classe principale che coordina le funzionalità di Entity Framework per un modello di dati.</span><span class="sxs-lookup"><span data-stu-id="488b7-215">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="488b7-216">Questa classe viene creata mediante derivazione dalla classe `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="488b7-216">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="488b7-217">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="488b7-217">Visual Studio</span></span>](#tab/visual-studio)

### <a name="add-microsoftentityframeworkcoresqlserver"></a><span data-ttu-id="488b7-218">Aggiungere Microsoft.EntityFrameworkCore.SqlServer</span><span class="sxs-lookup"><span data-stu-id="488b7-218">Add Microsoft.EntityFrameworkCore.SqlServer</span></span>

* <span data-ttu-id="488b7-219">Dal menu **Strumenti** selezionare **Gestione pacchetti NuGet > Gestisci pacchetti NuGet per la soluzione**.</span><span class="sxs-lookup"><span data-stu-id="488b7-219">From the **Tools** menu, select **NuGet Package Manager > Manage NuGet Packages for Solution**.</span></span>
* <span data-ttu-id="488b7-220">Selezionare la casella di controllo **Includi versione preliminare**.</span><span class="sxs-lookup"><span data-stu-id="488b7-220">Select the **Include prerelease** checkbox.</span></span>
* <span data-ttu-id="488b7-221">Selezionare la scheda **Esplora** e quindi immettere **Microsoft.EntityFrameworkCore.SqlServer** nella casella di ricerca.</span><span class="sxs-lookup"><span data-stu-id="488b7-221">Select the **Browse** tab, and then enter **Microsoft.EntityFrameworkCore.SqlServer** in the search box.</span></span>
* <span data-ttu-id="488b7-222">Selezionare **Microsoft.EntityFrameworkCore.SqlServer V3.0.0-preview** nel riquadro sinistro.</span><span class="sxs-lookup"><span data-stu-id="488b7-222">Select  **Microsoft.EntityFrameworkCore.SqlServer V3.0.0-preview** in the left pane.</span></span>
* <span data-ttu-id="488b7-223">Selezionare la casella di controllo **Progetto** nel riquadro di destra e quindi selezionare **Installa**.</span><span class="sxs-lookup"><span data-stu-id="488b7-223">Select the **Project** check box in the right pane and then select **Install**.</span></span>
* <span data-ttu-id="488b7-224">Usare le istruzioni precedenti per aggiungere il pacchetto NuGet `Microsoft.EntityFrameworkCore.InMemory`.</span><span class="sxs-lookup"><span data-stu-id="488b7-224">Use the preceding instructions to add the `Microsoft.EntityFrameworkCore.InMemory` NuGet package.</span></span>

![Gestione pacchetti NuGet](first-web-api/_static/vs3NuGet.png)

## <a name="add-the-todocontext-database-context"></a><span data-ttu-id="488b7-226">Aggiungere il contesto del database TodoContext</span><span class="sxs-lookup"><span data-stu-id="488b7-226">Add the TodoContext database context</span></span>

* <span data-ttu-id="488b7-227">Fare clic con il pulsante destro del mouse sulla cartella *Models* e scegliere **Aggiungi** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="488b7-227">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="488b7-228">Assegnare alla classe il nome *TodoContext* e fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="488b7-228">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="488b7-229">Visual Studio Code/Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="488b7-229">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="488b7-230">Aggiungere una classe `TodoContext` alla cartella *Models*.</span><span class="sxs-lookup"><span data-stu-id="488b7-230">Add a `TodoContext` class to the *Models* folder.</span></span>

---

* <span data-ttu-id="488b7-231">Immettere il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="488b7-231">Enter the following code:</span></span>

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="488b7-232">Registrare il contesto del database</span><span class="sxs-lookup"><span data-stu-id="488b7-232">Register the database context</span></span>

<span data-ttu-id="488b7-233">In ASP.NET Core i servizi come il contesto del database devono essere registrati con il [contenitore di inserimento delle dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="488b7-233">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="488b7-234">Il contenitore rende disponibile il servizio ai controller.</span><span class="sxs-lookup"><span data-stu-id="488b7-234">The container provides the service to controllers.</span></span>

<span data-ttu-id="488b7-235">Aggiornare *Startup.cs* con il codice evidenziato seguente:</span><span class="sxs-lookup"><span data-stu-id="488b7-235">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Startup.cs?highlight=7-8,23-24&name=snippet_all)]

<span data-ttu-id="488b7-236">Il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="488b7-236">The preceding code:</span></span>

* <span data-ttu-id="488b7-237">Rimuove le dichiarazioni `using` inutilizzate.</span><span class="sxs-lookup"><span data-stu-id="488b7-237">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="488b7-238">Aggiunge il contesto del database al contenitore di inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="488b7-238">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="488b7-239">Specifica che il contesto del database userà un database in memoria.</span><span class="sxs-lookup"><span data-stu-id="488b7-239">Specifies that the database context will use an in-memory database.</span></span>

## <a name="scaffold-a-controller"></a><span data-ttu-id="488b7-240">Eseguire lo scaffolding di un controller</span><span class="sxs-lookup"><span data-stu-id="488b7-240">Scaffold a controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="488b7-241">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="488b7-241">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="488b7-242">Fare clic con il pulsante destro del mouse sulla cartella *Controllers*.</span><span class="sxs-lookup"><span data-stu-id="488b7-242">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="488b7-243">Selezionare **Aggiungi** > **Nuovo elemento di scaffolding**.</span><span class="sxs-lookup"><span data-stu-id="488b7-243">Select **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="488b7-244">Selezionare **Controller API con azioni, che usa Entity Framework** e quindi selezionare **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="488b7-244">Select **API Controller with actions, using Entity Framework**, and then select **Add**.</span></span>
* <span data-ttu-id="488b7-245">Nella finestra di dialogo **Add API Controller with actions, using Entity Framework** (Aggiungi controller API con azioni, che usa Entity Framework):</span><span class="sxs-lookup"><span data-stu-id="488b7-245">In the **Add API Controller with actions, using Entity Framework** dialog:</span></span>

  * <span data-ttu-id="488b7-246">Selezionare **TodoItem (TodoAPI.Models)** in **Classe modello**.</span><span class="sxs-lookup"><span data-stu-id="488b7-246">Select **TodoItem (TodoAPI.Models)** in the **Model class**.</span></span>
  * <span data-ttu-id="488b7-247">Selezionare **TodoContext (TodoAPI.Models)** in **Classe contesto dei dati**.</span><span class="sxs-lookup"><span data-stu-id="488b7-247">Select **TodoContext (TodoAPI.Models)** in the **Data context class**.</span></span>
  * <span data-ttu-id="488b7-248">Selezionare **Aggiungi**</span><span class="sxs-lookup"><span data-stu-id="488b7-248">Select **Add**</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="488b7-249">Visual Studio Code/Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="488b7-249">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="488b7-250">Eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="488b7-250">Run the following commands:</span></span>

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 3.0.0-*
dotnet add package Microsoft.EntityFrameworkCore.Design --version 3.0.0-*
dotnet tool install --global dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator controller -name TodoItemsController -async -api -m TodoItem -dc TodoContext  -outDir Controllers
```

<span data-ttu-id="488b7-251">I comandi precedenti:</span><span class="sxs-lookup"><span data-stu-id="488b7-251">The preceding commands:</span></span>

* <span data-ttu-id="488b7-252">Aggiungere i pacchetti NuGet necessari per lo scaffolding.</span><span class="sxs-lookup"><span data-stu-id="488b7-252">Add NuGet packages required for scaffolding.</span></span>
* <span data-ttu-id="488b7-253">Installa il motore di scaffolding (`dotnet-aspnet-codegenerator`).</span><span class="sxs-lookup"><span data-stu-id="488b7-253">Installs the scaffolding engine (`dotnet-aspnet-codegenerator`).</span></span>
* <span data-ttu-id="488b7-254">Esegue lo scaffolding di `TodoItemsController`.</span><span class="sxs-lookup"><span data-stu-id="488b7-254">Scaffolds the `TodoItemsController`.</span></span>

---

<span data-ttu-id="488b7-255">Il codice generato:</span><span class="sxs-lookup"><span data-stu-id="488b7-255">The generated code:</span></span>

* <span data-ttu-id="488b7-256">Definisce una classe controller API senza metodi.</span><span class="sxs-lookup"><span data-stu-id="488b7-256">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="488b7-257">Contrassegna la classe con l'attributo [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute).</span><span class="sxs-lookup"><span data-stu-id="488b7-257">Decorates the class with the [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute.</span></span> <span data-ttu-id="488b7-258">L'attributo indica che il controller risponde alle richieste di API Web.</span><span class="sxs-lookup"><span data-stu-id="488b7-258">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="488b7-259">Per informazioni sui comportamenti specifici consentiti dall'attributo, vedere <xref:web-api/index>.</span><span class="sxs-lookup"><span data-stu-id="488b7-259">For information about specific behaviors that the attribute enables, see <xref:web-api/index>.</span></span>
* <span data-ttu-id="488b7-260">Usa l'inserimento delle dipendenze per inserire il contesto del database (`TodoContext`) nel controller.</span><span class="sxs-lookup"><span data-stu-id="488b7-260">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="488b7-261">Il contesto di database viene usato in ognuno dei metodi [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) nel controller.</span><span class="sxs-lookup"><span data-stu-id="488b7-261">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

## <a name="examine-the-posttodoitem-create-method"></a><span data-ttu-id="488b7-262">Esaminare il metodo di creazione PostTodoItem</span><span class="sxs-lookup"><span data-stu-id="488b7-262">Examine the PostTodoItem create method</span></span>

<span data-ttu-id="488b7-263">Sostituire l'istruzione return in `PostTodoItem` per usare l'operatore [nameof](/dotnet/csharp/language-reference/operators/nameof):</span><span class="sxs-lookup"><span data-stu-id="488b7-263">Replace the return statement in the `PostTodoItem` to use the [nameof](/dotnet/csharp/language-reference/operators/nameof) operator:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Create)]

<span data-ttu-id="488b7-264">Il codice precedente è un metodo HTTP POST, come indicato dall'attributo [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute).</span><span class="sxs-lookup"><span data-stu-id="488b7-264">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="488b7-265">Il metodo ottiene il valore dell'elemento attività dal corpo della richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="488b7-265">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="488b7-266">Il metodo <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*>:</span><span class="sxs-lookup"><span data-stu-id="488b7-266">The <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> method:</span></span>

* <span data-ttu-id="488b7-267">Restituisce un codice di stato HTTP 201 in caso di esito positivo.</span><span class="sxs-lookup"><span data-stu-id="488b7-267">Returns an HTTP 201 status code if successful.</span></span> <span data-ttu-id="488b7-268">HTTP 201 è la risposta standard per un metodo HTTP POST che crea una nuova risorsa nel server.</span><span class="sxs-lookup"><span data-stu-id="488b7-268">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="488b7-269">Aggiunge un'intestazione [Location](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) alla risposta.</span><span class="sxs-lookup"><span data-stu-id="488b7-269">Adds a [Location](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) header to the response.</span></span> <span data-ttu-id="488b7-270">L'intestazione `Location` specifica l'[URI](https://developer.mozilla.org/docs/Glossary/URI) dell'elemento attività appena creato.</span><span class="sxs-lookup"><span data-stu-id="488b7-270">The `Location` header specifies the [URI](https://developer.mozilla.org/docs/Glossary/URI) of the newly created to-do item.</span></span> <span data-ttu-id="488b7-271">Per altre informazioni, vedere [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="488b7-271">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="488b7-272">Fa riferimento all'azione `GetTodoItem` per creare l'URI dell'intestazione `Location`.</span><span class="sxs-lookup"><span data-stu-id="488b7-272">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="488b7-273">La parola chiave `nameof` C# viene usata per evitare di impostare il nome dell'azione come hardcoded nella chiamata a `CreatedAtAction`.</span><span class="sxs-lookup"><span data-stu-id="488b7-273">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

### <a name="install-postman"></a><span data-ttu-id="488b7-274">Installare Postman</span><span class="sxs-lookup"><span data-stu-id="488b7-274">Install Postman</span></span>

<span data-ttu-id="488b7-275">Questa esercitazione usa Postman per testare l'API Web.</span><span class="sxs-lookup"><span data-stu-id="488b7-275">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="488b7-276">Installare [Postman](https://www.getpostman.com/downloads/)</span><span class="sxs-lookup"><span data-stu-id="488b7-276">Install [Postman](https://www.getpostman.com/downloads/)</span></span>
* <span data-ttu-id="488b7-277">Avviare l'app Web.</span><span class="sxs-lookup"><span data-stu-id="488b7-277">Start the web app.</span></span>
* <span data-ttu-id="488b7-278">Avviare Postman.</span><span class="sxs-lookup"><span data-stu-id="488b7-278">Start Postman.</span></span>
* <span data-ttu-id="488b7-279">Disattivare **SSL certificate verification** (Verifica certificato SSL)</span><span class="sxs-lookup"><span data-stu-id="488b7-279">Disable **SSL certificate verification**</span></span>
* <span data-ttu-id="488b7-280">From  **File > Settings** (File > Impostazioni) (scheda \**General* (Generale)), disattivare **SSL certificate verification** (Verifica certificato SSL).</span><span class="sxs-lookup"><span data-stu-id="488b7-280">From  **File > Settings** (\**General* tab), disable **SSL certificate verification**.</span></span>
    > [!WARNING]
    > <span data-ttu-id="488b7-281">Riattivare la verifica dei certificati SSL al termine del test del controller.</span><span class="sxs-lookup"><span data-stu-id="488b7-281">Re-enable SSL certificate verification after testing the controller.</span></span>

<a name="post"></a>

### <a name="test-posttodoitem-with-postman"></a><span data-ttu-id="488b7-282">Testare PostTodoItem con Postman</span><span class="sxs-lookup"><span data-stu-id="488b7-282">Test PostTodoItem with Postman</span></span>

* <span data-ttu-id="488b7-283">Creare una nuova richiesta.</span><span class="sxs-lookup"><span data-stu-id="488b7-283">Create a new request.</span></span>
* <span data-ttu-id="488b7-284">Impostare il metodo HTTP su `POST`.</span><span class="sxs-lookup"><span data-stu-id="488b7-284">Set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="488b7-285">Fare clic sulla scheda **Body** (Corpo).</span><span class="sxs-lookup"><span data-stu-id="488b7-285">Select the **Body** tab.</span></span>
* <span data-ttu-id="488b7-286">Selezionare il pulsante di opzione **raw** (non elaborato).</span><span class="sxs-lookup"><span data-stu-id="488b7-286">Select the **raw** radio button.</span></span>
* <span data-ttu-id="488b7-287">Impostare il tipo su **JSON (application/json)** .</span><span class="sxs-lookup"><span data-stu-id="488b7-287">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="488b7-288">Nel corpo della richiesta immettere codice JSON per un elemento attività:</span><span class="sxs-lookup"><span data-stu-id="488b7-288">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="488b7-289">Selezionare **Send** (Invia).</span><span class="sxs-lookup"><span data-stu-id="488b7-289">Select **Send**.</span></span>

  ![Postman con richiesta di creazione](first-web-api/_static/3/create.png)

### <a name="test-the-location-header-uri"></a><span data-ttu-id="488b7-291">Testare l'URI dell'intestazione della posizione</span><span class="sxs-lookup"><span data-stu-id="488b7-291">Test the location header URI</span></span>

* <span data-ttu-id="488b7-292">Selezionare la scheda **Headers** (Intestazioni) nel riquadro **Response** (Risposta).</span><span class="sxs-lookup"><span data-stu-id="488b7-292">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="488b7-293">Copiare il valore dell'intestazione **Location** (Posizione):</span><span class="sxs-lookup"><span data-stu-id="488b7-293">Copy the **Location** header value:</span></span>

  ![Scheda Headers (Intestazioni) della console Postman](first-web-api/_static/3/create.png)

* <span data-ttu-id="488b7-295">Impostare il metodo su GET.</span><span class="sxs-lookup"><span data-stu-id="488b7-295">Set the method to GET.</span></span>
* <span data-ttu-id="488b7-296">Incollare l'URI (ad esempio `https://localhost:5001/api/TodoItems/1`)</span><span class="sxs-lookup"><span data-stu-id="488b7-296">Paste the URI (for example, `https://localhost:5001/api/TodoItems/1`)</span></span>
* <span data-ttu-id="488b7-297">Selezionare **Send** (Invia).</span><span class="sxs-lookup"><span data-stu-id="488b7-297">Select **Send**.</span></span>

## <a name="examine-the-get-methods"></a><span data-ttu-id="488b7-298">Esaminare i metodi GET</span><span class="sxs-lookup"><span data-stu-id="488b7-298">Examine the GET methods</span></span>

<span data-ttu-id="488b7-299">Questi metodi implementano due metodi GET:</span><span class="sxs-lookup"><span data-stu-id="488b7-299">These methods implement two GET endpoints:</span></span>

* `GET /api/TodoItems`
* `GET /api/TodoItems/{id}`

<span data-ttu-id="488b7-300">Testare l'app chiamando i due endpoint da un browser o da Postman.</span><span class="sxs-lookup"><span data-stu-id="488b7-300">Test the app by calling the two endpoints from a browser or Postman.</span></span> <span data-ttu-id="488b7-301">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="488b7-301">For example:</span></span>

* [https://localhost:5001/api/TodoItems](https://localhost:5001/api/TodoItems)
* [https://localhost:5001/api/TodoItems/1](https://localhost:5001/api/TodoItems/1)

<span data-ttu-id="488b7-302">Una risposta simile alla seguente viene generata dalla chiamata a `GetTodoItems`:</span><span class="sxs-lookup"><span data-stu-id="488b7-302">A response similar to the following is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

### <a name="test-get-with-postman"></a><span data-ttu-id="488b7-303">Testare Get con Postman</span><span class="sxs-lookup"><span data-stu-id="488b7-303">Test Get with Postman</span></span>

* <span data-ttu-id="488b7-304">Creare una nuova richiesta.</span><span class="sxs-lookup"><span data-stu-id="488b7-304">Create a new request.</span></span>
* <span data-ttu-id="488b7-305">Impostare il metodo HTTP su **GET**.</span><span class="sxs-lookup"><span data-stu-id="488b7-305">Set the HTTP method to **GET**.</span></span>
* <span data-ttu-id="488b7-306">Impostare l'URL della richiesta su `https://localhost:<port>/api/TodoItems`.</span><span class="sxs-lookup"><span data-stu-id="488b7-306">Set the request URL to `https://localhost:<port>/api/TodoItems`.</span></span> <span data-ttu-id="488b7-307">Ad esempio `https://localhost:5001/api/TodoItems`.</span><span class="sxs-lookup"><span data-stu-id="488b7-307">For example, `https://localhost:5001/api/TodoItems`.</span></span>
* <span data-ttu-id="488b7-308">Impostare **Two pane view** (Visualizzazione in due riquadri) in Postman.</span><span class="sxs-lookup"><span data-stu-id="488b7-308">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="488b7-309">Selezionare **Send** (Invia).</span><span class="sxs-lookup"><span data-stu-id="488b7-309">Select **Send**.</span></span>

<span data-ttu-id="488b7-310">Questa app usa un database in memoria.</span><span class="sxs-lookup"><span data-stu-id="488b7-310">This app uses an in-memory database.</span></span> <span data-ttu-id="488b7-311">Se l'app viene arrestata e avviata, la richiesta GET precedente non restituirà alcun dato.</span><span class="sxs-lookup"><span data-stu-id="488b7-311">If the app is stopped and started, the preceding GET request will not return any data.</span></span> <span data-ttu-id="488b7-312">Se non vengono restituiti dati, eseguire [POST](#post) per pubblicare i dati nell'app.</span><span class="sxs-lookup"><span data-stu-id="488b7-312">If no data is returned, [POST](#post) data to the app.</span></span>

## <a name="routing-and-url-paths"></a><span data-ttu-id="488b7-313">Routing e percorsi di URL</span><span class="sxs-lookup"><span data-stu-id="488b7-313">Routing and URL paths</span></span>

<span data-ttu-id="488b7-314">L'attributo [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) indica un metodo che risponde a una richiesta HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="488b7-314">The [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="488b7-315">Il percorso dell'URL per ogni metodo viene costruito nel modo seguente:</span><span class="sxs-lookup"><span data-stu-id="488b7-315">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="488b7-316">Iniziare con la stringa di modello nell'attributo `Route` del controller:</span><span class="sxs-lookup"><span data-stu-id="488b7-316">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=TodoController&highlight=1)]

* <span data-ttu-id="488b7-317">Sostituire `[controller]` con il nome del controller, ovvero, per convenzione, il nome della classe controller meno il suffisso "Controller".</span><span class="sxs-lookup"><span data-stu-id="488b7-317">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="488b7-318">In questo esempio il nome della classe controller è **TodoItems**Controller, quindi il nome del controller è "TodoItems".</span><span class="sxs-lookup"><span data-stu-id="488b7-318">For this sample, the controller class name is **TodoItems**Controller, so the controller name is "TodoItems".</span></span> <span data-ttu-id="488b7-319">Il [routing](xref:mvc/controllers/routing) ASP.NET Core non fa distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="488b7-319">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="488b7-320">Se l'attributo `[HttpGet]` ha un modello di route, ad esempio `[HttpGet("products")]`, aggiungerlo al percorso.</span><span class="sxs-lookup"><span data-stu-id="488b7-320">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="488b7-321">In questo esempio non si usa un modello.</span><span class="sxs-lookup"><span data-stu-id="488b7-321">This sample doesn't use a template.</span></span> <span data-ttu-id="488b7-322">Per altre informazioni, vedere [Routing con attributi Http[verb]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="488b7-322">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="488b7-323">Nel metodo `GetTodoItem` seguente, `"{id}"` è una variabile segnaposto per l'identificatore univoco dell'elemento attività.</span><span class="sxs-lookup"><span data-stu-id="488b7-323">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="488b7-324">Quando viene chiamato `GetTodoItem`, il valore di `"{id}"` viene specificato per il metodo nel rispettivo parametro `id`.</span><span class="sxs-lookup"><span data-stu-id="488b7-324">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its`id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a><span data-ttu-id="488b7-325">Valori restituiti</span><span class="sxs-lookup"><span data-stu-id="488b7-325">Return values</span></span>

<span data-ttu-id="488b7-326">Il tipo restituito dei metodi `GetTodoItems` e `GetTodoItem` è [ActionResult\<T>](xref:web-api/action-return-types#actionresultt-type).</span><span class="sxs-lookup"><span data-stu-id="488b7-326">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="488b7-327">ASP.NET Core serializza automaticamente l'oggetto su [JSON](https://www.json.org/) e scrive il codice JSON nel corpo del messaggio di risposta.</span><span class="sxs-lookup"><span data-stu-id="488b7-327">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="488b7-328">Il codice di risposta per questo tipo restituito è 200, presupponendo che non esistano eccezioni non gestite.</span><span class="sxs-lookup"><span data-stu-id="488b7-328">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="488b7-329">Le eccezioni non gestite vengono convertite in errori 5xx.</span><span class="sxs-lookup"><span data-stu-id="488b7-329">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="488b7-330">I tipi restituiti `ActionResult` possono rappresentare un ampio intervallo di codici di stato HTTP.</span><span class="sxs-lookup"><span data-stu-id="488b7-330">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="488b7-331">Ad esempio, `GetTodoItem` può restituire due valori di stato diversi:</span><span class="sxs-lookup"><span data-stu-id="488b7-331">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="488b7-332">Se nessun elemento corrisponde all'ID richiesto, il metodo restituisce un codice di errore 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound).</span><span class="sxs-lookup"><span data-stu-id="488b7-332">If no item matches the requested ID, the method returns a 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) error code.</span></span>
* <span data-ttu-id="488b7-333">In caso contrario, il metodo restituisce il codice 200 con un corpo della risposta JSON.</span><span class="sxs-lookup"><span data-stu-id="488b7-333">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="488b7-334">La restituzione di `item` risulta in una risposta HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="488b7-334">Returning `item` results in an HTTP 200 response.</span></span>

## <a name="the-puttodoitem-method"></a><span data-ttu-id="488b7-335">Metodo PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="488b7-335">The PutTodoItem method</span></span>

<span data-ttu-id="488b7-336">Esaminare il metodo `PutTodoItem`:</span><span class="sxs-lookup"><span data-stu-id="488b7-336">Examine the `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Update)]

<span data-ttu-id="488b7-337">`PutTodoItem` è simile a `PostTodoItem` ma usa la richiesta HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="488b7-337">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="488b7-338">La risposta è [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="488b7-338">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="488b7-339">In base alla specifica HTTP, una richiesta PUT richiede che il client invii l'intera entità aggiornata e non solo le modifiche.</span><span class="sxs-lookup"><span data-stu-id="488b7-339">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="488b7-340">Per supportare gli aggiornamenti parziali, usare [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span><span class="sxs-lookup"><span data-stu-id="488b7-340">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="488b7-341">Se si riceve un errore chiamando `PutTodoItem`, chiamare `GET` per verificare che il database includa un elemento.</span><span class="sxs-lookup"><span data-stu-id="488b7-341">If you get an error calling `PutTodoItem`, call `GET` to ensure there's an item in the database.</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="488b7-342">Testare il metodo PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="488b7-342">Test the PutTodoItem method</span></span>

<span data-ttu-id="488b7-343">Questo esempio usa un database in memoria che deve essere inizializzato ogni volta che l'app viene avviata.</span><span class="sxs-lookup"><span data-stu-id="488b7-343">This sample uses an in-memory database that must be initialed each time the app is started.</span></span> <span data-ttu-id="488b7-344">Deve esistere un elemento nel database prima di eseguire una chiamata PUT.</span><span class="sxs-lookup"><span data-stu-id="488b7-344">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="488b7-345">Chiamare GET per assicurarsi che sia presente un elemento nel database prima di eseguire una chiamata PUT.</span><span class="sxs-lookup"><span data-stu-id="488b7-345">Call GET to insure there's an item in the database before making a PUT call.</span></span>

<span data-ttu-id="488b7-346">Aggiornare l'elemento attività con ID = 1 e impostarne il nome su "feed fish":</span><span class="sxs-lookup"><span data-stu-id="488b7-346">Update the to-do item that has ID = 1 and set its name to "feed fish":</span></span>

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="488b7-347">L'immagine seguente visualizza l'aggiornamento di Postman:</span><span class="sxs-lookup"><span data-stu-id="488b7-347">The following image shows the Postman update:</span></span>

![Console Postman con risposta 204 (No Content)](first-web-api/_static/3/pmcput.png)

## <a name="the-deletetodoitem-method"></a><span data-ttu-id="488b7-349">Metodo DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="488b7-349">The DeleteTodoItem method</span></span>

<span data-ttu-id="488b7-350">Esaminare il metodo `DeleteTodoItem`:</span><span class="sxs-lookup"><span data-stu-id="488b7-350">Examine the `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Delete)]

<span data-ttu-id="488b7-351">La risposta `DeleteTodoItem` è [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="488b7-351">The `DeleteTodoItem` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="488b7-352">Testare il metodo DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="488b7-352">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="488b7-353">Usare Postman per eliminare un elemento attività:</span><span class="sxs-lookup"><span data-stu-id="488b7-353">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="488b7-354">Impostare il metodo su `DELETE`.</span><span class="sxs-lookup"><span data-stu-id="488b7-354">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="488b7-355">Impostare l'URI dell'oggetto da eliminare, ad esempio `https://localhost:5001/api/TodoItems/1`</span><span class="sxs-lookup"><span data-stu-id="488b7-355">Set the URI of the object to delete, for example `https://localhost:5001/api/TodoItems/1`</span></span>
* <span data-ttu-id="488b7-356">Selezionare **Send** (Invia).</span><span class="sxs-lookup"><span data-stu-id="488b7-356">Select **Send**</span></span>

## <a name="call-the-api-from-jquery"></a><span data-ttu-id="488b7-357">Chiamare l'API da jQuery</span><span class="sxs-lookup"><span data-stu-id="488b7-357">Call the API from jQuery</span></span>

<span data-ttu-id="488b7-358">Per istruzioni dettagliate, vedere [Esercitazione: Chiamare un'app API Web ASP.NET Core con jQuery](xref:tutorials/web-api-jquery).</span><span class="sxs-lookup"><span data-stu-id="488b7-358">See [Tutorial: Call an ASP.NET Core web API with jQuery](xref:tutorials/web-api-jquery).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="488b7-359">In questa esercitazione si imparerà a:</span><span class="sxs-lookup"><span data-stu-id="488b7-359">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="488b7-360">Creare un progetto di API Web.</span><span class="sxs-lookup"><span data-stu-id="488b7-360">Create a web API project.</span></span>
> * <span data-ttu-id="488b7-361">Aggiungere una classe modello e un contesto di database.</span><span class="sxs-lookup"><span data-stu-id="488b7-361">Add a model class and a database context.</span></span>
> * <span data-ttu-id="488b7-362">Aggiungere un controller.</span><span class="sxs-lookup"><span data-stu-id="488b7-362">Add a controller.</span></span>
> * <span data-ttu-id="488b7-363">Aggiungere metodi CRUD.</span><span class="sxs-lookup"><span data-stu-id="488b7-363">Add CRUD methods.</span></span>
> * <span data-ttu-id="488b7-364">Configurare routing e percorsi URL.</span><span class="sxs-lookup"><span data-stu-id="488b7-364">Configure routing and URL paths.</span></span>
> * <span data-ttu-id="488b7-365">Specificare valori restituiti.</span><span class="sxs-lookup"><span data-stu-id="488b7-365">Specify return values.</span></span>
> * <span data-ttu-id="488b7-366">Chiamare l'API Web con Postman.</span><span class="sxs-lookup"><span data-stu-id="488b7-366">Call the web API with Postman.</span></span>
> * <span data-ttu-id="488b7-367">Chiamare l'API Web con jQuery.</span><span class="sxs-lookup"><span data-stu-id="488b7-367">Call the web API with jQuery.</span></span>

<span data-ttu-id="488b7-368">Al termine si dispone di un'API web che può gestire gli elementi di tipo "attività" archiviati in un database relazionale.</span><span class="sxs-lookup"><span data-stu-id="488b7-368">At the end, you have a web API that can manage "to-do" items stored in a relational database.</span></span>
## <a name="overview"></a><span data-ttu-id="488b7-369">Panoramica</span><span class="sxs-lookup"><span data-stu-id="488b7-369">Overview</span></span>

<span data-ttu-id="488b7-370">Questa esercitazione consente di creare l'API seguente:</span><span class="sxs-lookup"><span data-stu-id="488b7-370">This tutorial creates the following API:</span></span>

|<span data-ttu-id="488b7-371">API</span><span class="sxs-lookup"><span data-stu-id="488b7-371">API</span></span> | <span data-ttu-id="488b7-372">DESCRIZIONE</span><span class="sxs-lookup"><span data-stu-id="488b7-372">Description</span></span> | <span data-ttu-id="488b7-373">Corpo della richiesta</span><span class="sxs-lookup"><span data-stu-id="488b7-373">Request body</span></span> | <span data-ttu-id="488b7-374">Corpo della risposta</span><span class="sxs-lookup"><span data-stu-id="488b7-374">Response body</span></span> |
|--- | ---- | ---- | ---- |
|<span data-ttu-id="488b7-375">GET /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="488b7-375">GET /api/TodoItems</span></span> | <span data-ttu-id="488b7-376">Ottiene tutti gli elementi attività</span><span class="sxs-lookup"><span data-stu-id="488b7-376">Get all to-do items</span></span> | <span data-ttu-id="488b7-377">nessuno</span><span class="sxs-lookup"><span data-stu-id="488b7-377">None</span></span> | <span data-ttu-id="488b7-378">Matrice di elementi attività</span><span class="sxs-lookup"><span data-stu-id="488b7-378">Array of to-do items</span></span>|
|<span data-ttu-id="488b7-379">GET /api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="488b7-379">GET /api/TodoItems/{id}</span></span> | <span data-ttu-id="488b7-380">Ottiene un elemento in base all'ID</span><span class="sxs-lookup"><span data-stu-id="488b7-380">Get an item by ID</span></span> | <span data-ttu-id="488b7-381">nessuno</span><span class="sxs-lookup"><span data-stu-id="488b7-381">None</span></span> | <span data-ttu-id="488b7-382">Elemento attività</span><span class="sxs-lookup"><span data-stu-id="488b7-382">To-do item</span></span>|
|<span data-ttu-id="488b7-383">POST /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="488b7-383">POST /api/TodoItems</span></span> | <span data-ttu-id="488b7-384">Aggiunge un nuovo elemento</span><span class="sxs-lookup"><span data-stu-id="488b7-384">Add a new item</span></span> | <span data-ttu-id="488b7-385">Elemento attività</span><span class="sxs-lookup"><span data-stu-id="488b7-385">To-do item</span></span> | <span data-ttu-id="488b7-386">Elemento attività</span><span class="sxs-lookup"><span data-stu-id="488b7-386">To-do item</span></span> |
|<span data-ttu-id="488b7-387">PUT /api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="488b7-387">PUT /api/TodoItems/{id}</span></span> | <span data-ttu-id="488b7-388">Aggiorna un elemento esistente &nbsp;</span><span class="sxs-lookup"><span data-stu-id="488b7-388">Update an existing item &nbsp;</span></span> | <span data-ttu-id="488b7-389">Elemento attività</span><span class="sxs-lookup"><span data-stu-id="488b7-389">To-do item</span></span> | <span data-ttu-id="488b7-390">nessuno</span><span class="sxs-lookup"><span data-stu-id="488b7-390">None</span></span> |
|<span data-ttu-id="488b7-391">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="488b7-391">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span></span> | <span data-ttu-id="488b7-392">Elimina un elemento &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="488b7-392">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="488b7-393">nessuno</span><span class="sxs-lookup"><span data-stu-id="488b7-393">None</span></span> | <span data-ttu-id="488b7-394">nessuno</span><span class="sxs-lookup"><span data-stu-id="488b7-394">None</span></span>|

<span data-ttu-id="488b7-395">Il diagramma seguente visualizza la struttura dell'app.</span><span class="sxs-lookup"><span data-stu-id="488b7-395">The following diagram shows the design of the app.</span></span>

![Il client è rappresentato da una casella a sinistra.](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="488b7-401">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="488b7-401">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="488b7-402">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="488b7-402">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="488b7-403">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="488b7-403">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="488b7-404">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="488b7-404">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="488b7-405">Creare un progetto Web</span><span class="sxs-lookup"><span data-stu-id="488b7-405">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="488b7-406">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="488b7-406">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="488b7-407">Scegliere **Nuovo** > **Progetto** dal menu **File**.</span><span class="sxs-lookup"><span data-stu-id="488b7-407">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="488b7-408">Selezionare il modello **Applicazione Web ASP.NET Core** e fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="488b7-408">Select the **ASP.NET Core Web Application** template and click **Next**.</span></span>
* <span data-ttu-id="488b7-409">Assegnare al progetto il nome *TodoApi* e fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="488b7-409">Name the project *TodoApi* and click **Create**.</span></span>
* <span data-ttu-id="488b7-410">Nella finestra di dialogo **Crea una nuova applicazione Web ASP.NET Core** verificare che siano selezionati **.NET Core** e **ASP.NET Core 2.2**.</span><span class="sxs-lookup"><span data-stu-id="488b7-410">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 2.2** are selected.</span></span> <span data-ttu-id="488b7-411">Selezionare il modello **API** e fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="488b7-411">Select the **API** template and click **Create**.</span></span> <span data-ttu-id="488b7-412">**Non** selezionare **Abilita supporto Docker**.</span><span class="sxs-lookup"><span data-stu-id="488b7-412">**Don't** select **Enable Docker Support**.</span></span>

![Finestra di dialogo Nuovo progetto di Visual Studio](first-web-api/_static/vs.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="488b7-414">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="488b7-414">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="488b7-415">Aprire il [terminale integrato](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="488b7-415">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="488b7-416">Cambiare directory (`cd`) e passare alla cartella che conterrà la cartella del progetto.</span><span class="sxs-lookup"><span data-stu-id="488b7-416">Change directories (`cd`) to the folder that will contain the project folder.</span></span>
* <span data-ttu-id="488b7-417">Eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="488b7-417">Run the following commands:</span></span>

   ```console
   dotnet new webapi -o TodoApi
   code -r TodoApi
   ```

  <span data-ttu-id="488b7-418">Questi comandi creano un nuovo progetto API Web e aprono una nuova istanza di Visual Studio Code nella nuova cartella di progetto.</span><span class="sxs-lookup"><span data-stu-id="488b7-418">These commands create a new web API project and open a new instance of Visual Studio Code in the new project folder.</span></span>

* <span data-ttu-id="488b7-419">Quando una finestra di dialogo chiede se si vuole aggiungere gli asset necessari al progetto, selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="488b7-419">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="488b7-420">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="488b7-420">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="488b7-421">Selezionare **File** > **Nuova soluzione**.</span><span class="sxs-lookup"><span data-stu-id="488b7-421">Select **File** > **New Solution**.</span></span>

  ![Nuova soluzione macOS](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="488b7-423">Selezionare **.NET Core** > **App** > **API** > **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="488b7-423">Select **.NET Core** > **App** > **API** > **Next**.</span></span>

  ![Finestra di dialogo Nuovo progetto di macOS](first-web-api-mac/_static/1.png)
  
* <span data-ttu-id="488b7-425">Nella finestra di dialogo **Configure your new ASP.NET Core Web API** (Configura la nuova API Web ASP.NET Core), accettare l'impostazione predefinita di **Framework di destinazione**, ovvero \* *.NET Core 2.2*.</span><span class="sxs-lookup"><span data-stu-id="488b7-425">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of \**.NET Core 2.2*.</span></span>

* <span data-ttu-id="488b7-426">Immettere *TodoApi* in **Nome progetto**, quindi selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="488b7-426">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![Finestra di dialogo di configurazione](first-web-api-mac/_static/2.png)

---

### <a name="test-the-api"></a><span data-ttu-id="488b7-428">Testare l'API</span><span class="sxs-lookup"><span data-stu-id="488b7-428">Test the API</span></span>

<span data-ttu-id="488b7-429">Il modello di progetto crea un'API `values`.</span><span class="sxs-lookup"><span data-stu-id="488b7-429">The project template creates a `values` API.</span></span> <span data-ttu-id="488b7-430">Chiamare il metodo `Get` da un browser per eseguire il test dell'app.</span><span class="sxs-lookup"><span data-stu-id="488b7-430">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="488b7-431">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="488b7-431">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="488b7-432">Premere CTRL+F5 per eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="488b7-432">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="488b7-433">Visual Studio apre un browser e naviga all'indirizzo `https://localhost:<port>/api/values`, dove `<port>` è un numero di porta selezionato a caso.</span><span class="sxs-lookup"><span data-stu-id="488b7-433">Visual Studio launches a browser and navigates to `https://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="488b7-434">Se in una finestra di dialogo viene chiesto se considerare attendibile il certificato IIS Express, selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="488b7-434">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="488b7-435">Nella finestra di dialogo **Avviso di sicurezza** successiva selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="488b7-435">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="488b7-436">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="488b7-436">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="488b7-437">Premere CTRL+F5 per eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="488b7-437">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="488b7-438">In un browser passare all'URL seguente: [https://localhost:5001/api/values](https://localhost:5001/api/values).</span><span class="sxs-lookup"><span data-stu-id="488b7-438">In a browser, go to following URL: [https://localhost:5001/api/values](https://localhost:5001/api/values).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="488b7-439">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="488b7-439">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="488b7-440">Selezionare **Esegui** > **Avvia debug** per avviare l'app.</span><span class="sxs-lookup"><span data-stu-id="488b7-440">Select **Run** > **Start Debugging** to launch the app.</span></span> <span data-ttu-id="488b7-441">Visual Studio per Mac apre un browser e naviga all'indirizzo `https://localhost:<port>`, dove `<port>` è un numero di porta selezionato a caso.</span><span class="sxs-lookup"><span data-stu-id="488b7-441">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="488b7-442">Viene restituito un errore HTTP 404 (Non trovato).</span><span class="sxs-lookup"><span data-stu-id="488b7-442">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="488b7-443">Accodare `/api/values` all'URL (impostare l'URL su `https://localhost:<port>/api/values`).</span><span class="sxs-lookup"><span data-stu-id="488b7-443">Append `/api/values` to the URL (change the URL to `https://localhost:<port>/api/values`).</span></span>

---

<span data-ttu-id="488b7-444">Viene restituito il codice JSON seguente:</span><span class="sxs-lookup"><span data-stu-id="488b7-444">The following JSON is returned:</span></span>

```json
["value1","value2"]
```

## <a name="add-a-model-class"></a><span data-ttu-id="488b7-445">Aggiungere una classe del modello</span><span class="sxs-lookup"><span data-stu-id="488b7-445">Add a model class</span></span>

<span data-ttu-id="488b7-446">Un *modello* è un set di classi che rappresentano i dati gestiti dall'app.</span><span class="sxs-lookup"><span data-stu-id="488b7-446">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="488b7-447">Il modello per questa app è una classe `TodoItem` singola.</span><span class="sxs-lookup"><span data-stu-id="488b7-447">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="488b7-448">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="488b7-448">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="488b7-449">In **Esplora soluzioni** fare clic con il pulsante destro del mouse sul progetto.</span><span class="sxs-lookup"><span data-stu-id="488b7-449">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="488b7-450">Selezionare **Aggiungi** > **Nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="488b7-450">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="488b7-451">Assegnare il nome *Models* alla cartella.</span><span class="sxs-lookup"><span data-stu-id="488b7-451">Name the folder *Models*.</span></span>

* <span data-ttu-id="488b7-452">Fare clic con il pulsante destro del mouse sulla cartella *Models* e scegliere **Aggiungi** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="488b7-452">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="488b7-453">Assegnare alla classe il nome *TodoItem* e selezionare **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="488b7-453">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="488b7-454">Sostituire il codice del modello con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="488b7-454">Replace the template code with the following code:</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="488b7-455">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="488b7-455">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="488b7-456">Aggiungere una cartella denominata *Models*.</span><span class="sxs-lookup"><span data-stu-id="488b7-456">Add a folder named *Models*.</span></span>

* <span data-ttu-id="488b7-457">Aggiungere una classe `TodoItem` alla cartella *Models* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="488b7-457">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="488b7-458">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="488b7-458">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="488b7-459">Fare clic con il pulsante destro del mouse sul progetto.</span><span class="sxs-lookup"><span data-stu-id="488b7-459">Right-click the project.</span></span> <span data-ttu-id="488b7-460">Selezionare **Aggiungi** > **Nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="488b7-460">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="488b7-461">Assegnare il nome *Models* alla cartella.</span><span class="sxs-lookup"><span data-stu-id="488b7-461">Name the folder *Models*.</span></span>

  ![Nuova cartella](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="488b7-463">Fare clic con il pulsante destro del mouse sulla cartella *Models* e scegliere **Aggiungi** > **Nuovo file** > **Generale** > **Classe vuota**.</span><span class="sxs-lookup"><span data-stu-id="488b7-463">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="488b7-464">Assegnare alla classe il nome *TodoItem* e fare clic su **Nuovo**.</span><span class="sxs-lookup"><span data-stu-id="488b7-464">Name the class *TodoItem*, and then click **New**.</span></span>

* <span data-ttu-id="488b7-465">Sostituire il codice del modello con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="488b7-465">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="488b7-466">La proprietà `Id` funziona come chiave univoca in un database relazionale.</span><span class="sxs-lookup"><span data-stu-id="488b7-466">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="488b7-467">È possibile inserire le classi del modello in qualsiasi punto del progetto, ma per convenzione viene usata la cartella *Models*.</span><span class="sxs-lookup"><span data-stu-id="488b7-467">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="488b7-468">Aggiungere un contesto di database</span><span class="sxs-lookup"><span data-stu-id="488b7-468">Add a database context</span></span>

<span data-ttu-id="488b7-469">Il *contesto di database* è la classe principale che coordina le funzionalità di Entity Framework per un modello di dati.</span><span class="sxs-lookup"><span data-stu-id="488b7-469">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="488b7-470">Questa classe viene creata mediante derivazione dalla classe `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="488b7-470">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="488b7-471">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="488b7-471">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="488b7-472">Fare clic con il pulsante destro del mouse sulla cartella *Models* e scegliere **Aggiungi** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="488b7-472">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="488b7-473">Assegnare alla classe il nome *TodoContext* e fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="488b7-473">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="488b7-474">Visual Studio Code/Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="488b7-474">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="488b7-475">Aggiungere una classe `TodoContext` alla cartella *Models*.</span><span class="sxs-lookup"><span data-stu-id="488b7-475">Add a `TodoContext` class to the *Models* folder.</span></span>

---

* <span data-ttu-id="488b7-476">Sostituire il codice del modello con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="488b7-476">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="488b7-477">Registrare il contesto del database</span><span class="sxs-lookup"><span data-stu-id="488b7-477">Register the database context</span></span>

<span data-ttu-id="488b7-478">In ASP.NET Core i servizi come il contesto del database devono essere registrati con il [contenitore di inserimento delle dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="488b7-478">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="488b7-479">Il contenitore rende disponibile il servizio ai controller.</span><span class="sxs-lookup"><span data-stu-id="488b7-479">The container provides the service to controllers.</span></span>

<span data-ttu-id="488b7-480">Aggiornare *Startup.cs* con il codice evidenziato seguente:</span><span class="sxs-lookup"><span data-stu-id="488b7-480">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup1.cs?highlight=5,8,25-26&name=snippet_all)]

<span data-ttu-id="488b7-481">Il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="488b7-481">The preceding code:</span></span>

* <span data-ttu-id="488b7-482">Rimuove le dichiarazioni `using` inutilizzate.</span><span class="sxs-lookup"><span data-stu-id="488b7-482">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="488b7-483">Aggiunge il contesto del database al contenitore di inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="488b7-483">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="488b7-484">Specifica che il contesto del database userà un database in memoria.</span><span class="sxs-lookup"><span data-stu-id="488b7-484">Specifies that the database context will use an in-memory database.</span></span>

## <a name="add-a-controller"></a><span data-ttu-id="488b7-485">Aggiungere un controller</span><span class="sxs-lookup"><span data-stu-id="488b7-485">Add a controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="488b7-486">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="488b7-486">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="488b7-487">Fare clic con il pulsante destro del mouse sulla cartella *Controllers*.</span><span class="sxs-lookup"><span data-stu-id="488b7-487">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="488b7-488">Selezionare **Aggiungi** > **Nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="488b7-488">Select **Add** > **New Item**.</span></span>
* <span data-ttu-id="488b7-489">Nella finestra di dialogo **Aggiungi nuovo elemento** selezionare il modello **API Controller Class** (Classe controller API).</span><span class="sxs-lookup"><span data-stu-id="488b7-489">In the **Add New Item** dialog, select the **API Controller Class** template.</span></span>
* <span data-ttu-id="488b7-490">Assegnare alla classe il nome *TodoController* e selezionare **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="488b7-490">Name the class *TodoController*, and select **Add**.</span></span>

  ![Finestra di dialogo Aggiungi nuovo elemento con controller nella casella di ricerca e controller API Web selezionato](first-web-api/_static/new_controller.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="488b7-492">Visual Studio Code/Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="488b7-492">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="488b7-493">Nella cartella *Controllers* creare una classe denominata `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="488b7-493">In the *Controllers* folder, create a class named `TodoController`.</span></span>

---

* <span data-ttu-id="488b7-494">Sostituire il codice del modello con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="488b7-494">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="488b7-495">Il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="488b7-495">The preceding code:</span></span>

* <span data-ttu-id="488b7-496">Definisce una classe controller API senza metodi.</span><span class="sxs-lookup"><span data-stu-id="488b7-496">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="488b7-497">Contrassegna la classe con l'attributo [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute).</span><span class="sxs-lookup"><span data-stu-id="488b7-497">Decorates the class with the [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute.</span></span> <span data-ttu-id="488b7-498">L'attributo indica che il controller risponde alle richieste di API Web.</span><span class="sxs-lookup"><span data-stu-id="488b7-498">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="488b7-499">Per informazioni sui comportamenti specifici consentiti dall'attributo, vedere <xref:web-api/index>.</span><span class="sxs-lookup"><span data-stu-id="488b7-499">For information about specific behaviors that the attribute enables, see <xref:web-api/index>.</span></span>
* <span data-ttu-id="488b7-500">Usa l'inserimento delle dipendenze per inserire il contesto del database (`TodoContext`) nel controller.</span><span class="sxs-lookup"><span data-stu-id="488b7-500">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="488b7-501">Il contesto di database viene usato in ognuno dei metodi [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) nel controller.</span><span class="sxs-lookup"><span data-stu-id="488b7-501">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>
* <span data-ttu-id="488b7-502">Aggiunge un elemento denominato `Item1` al database se il database è vuoto.</span><span class="sxs-lookup"><span data-stu-id="488b7-502">Adds an item named `Item1` to the database if the database is empty.</span></span> <span data-ttu-id="488b7-503">Questo codice si trova nel costruttore, quindi viene eseguito ogni volta che è presente una nuova richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="488b7-503">This code is in the constructor, so it runs every time there's a new HTTP request.</span></span> <span data-ttu-id="488b7-504">Se si eliminano tutti gli elementi, il costruttore crea di nuovo `Item1` quando viene nuovamente chiamato un metodo API.</span><span class="sxs-lookup"><span data-stu-id="488b7-504">If you delete all items, the constructor creates `Item1` again the next time an API method is called.</span></span> <span data-ttu-id="488b7-505">Potrebbe pertanto sembrare che l'eliminazione non abbia funzionato, mentre di fatto ha funzionato.</span><span class="sxs-lookup"><span data-stu-id="488b7-505">So it may look like the deletion didn't work when it actually did work.</span></span>

## <a name="add-get-methods"></a><span data-ttu-id="488b7-506">Aggiungere metodi Get</span><span class="sxs-lookup"><span data-stu-id="488b7-506">Add Get methods</span></span>

<span data-ttu-id="488b7-507">Per specificare un'API che recupera elementi attività, aggiungere i metodi seguenti alla classe `TodoController`:</span><span class="sxs-lookup"><span data-stu-id="488b7-507">To provide an API that retrieves to-do items, add the following methods to the `TodoController` class:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

<span data-ttu-id="488b7-508">Questi metodi implementano due metodi GET:</span><span class="sxs-lookup"><span data-stu-id="488b7-508">These methods implement two GET endpoints:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="488b7-509">Arrestare l'app se è ancora in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="488b7-509">Stop the app if it's still running.</span></span> <span data-ttu-id="488b7-510">Quindi eseguirla di nuovo per includere le modifiche più recenti.</span><span class="sxs-lookup"><span data-stu-id="488b7-510">Then run it again to include the latest changes.</span></span>

<span data-ttu-id="488b7-511">Testare l'app chiamando i due endpoint da un browser.</span><span class="sxs-lookup"><span data-stu-id="488b7-511">Test the app by calling the two endpoints from a browser.</span></span> <span data-ttu-id="488b7-512">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="488b7-512">For example:</span></span>

* `https://localhost:<port>/api/todo`
* `https://localhost:<port>/api/todo/1`

<span data-ttu-id="488b7-513">La chiamata a `GetTodoItems` genera la risposta HTTP seguente:</span><span class="sxs-lookup"><span data-stu-id="488b7-513">The following HTTP response is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

## <a name="routing-and-url-paths"></a><span data-ttu-id="488b7-514">Routing e percorsi di URL</span><span class="sxs-lookup"><span data-stu-id="488b7-514">Routing and URL paths</span></span>

<span data-ttu-id="488b7-515">L'attributo [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) indica un metodo che risponde a una richiesta HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="488b7-515">The [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="488b7-516">Il percorso dell'URL per ogni metodo viene costruito nel modo seguente:</span><span class="sxs-lookup"><span data-stu-id="488b7-516">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="488b7-517">Iniziare con la stringa di modello nell'attributo `Route` del controller:</span><span class="sxs-lookup"><span data-stu-id="488b7-517">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

* <span data-ttu-id="488b7-518">Sostituire `[controller]` con il nome del controller, ovvero, per convenzione, il nome della classe controller meno il suffisso "Controller".</span><span class="sxs-lookup"><span data-stu-id="488b7-518">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="488b7-519">In questo esempio il nome della classe controller è **Todo**Controller, pertanto il nome del controller è "todo".</span><span class="sxs-lookup"><span data-stu-id="488b7-519">For this sample, the controller class name is **Todo**Controller, so the controller name is "todo".</span></span> <span data-ttu-id="488b7-520">Il [routing](xref:mvc/controllers/routing) ASP.NET Core non fa distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="488b7-520">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="488b7-521">Se l'attributo `[HttpGet]` ha un modello di route, ad esempio `[HttpGet("products")]`, aggiungerlo al percorso.</span><span class="sxs-lookup"><span data-stu-id="488b7-521">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="488b7-522">In questo esempio non si usa un modello.</span><span class="sxs-lookup"><span data-stu-id="488b7-522">This sample doesn't use a template.</span></span> <span data-ttu-id="488b7-523">Per altre informazioni, vedere [Routing con attributi Http[verb]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="488b7-523">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="488b7-524">Nel metodo `GetTodoItem` seguente, `"{id}"` è una variabile segnaposto per l'identificatore univoco dell'elemento attività.</span><span class="sxs-lookup"><span data-stu-id="488b7-524">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="488b7-525">Quando viene chiamato `GetTodoItem`, il valore di `"{id}"` viene specificato per il metodo nel rispettivo parametro `id`.</span><span class="sxs-lookup"><span data-stu-id="488b7-525">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its`id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a><span data-ttu-id="488b7-526">Valori restituiti</span><span class="sxs-lookup"><span data-stu-id="488b7-526">Return values</span></span>

<span data-ttu-id="488b7-527">Il tipo restituito dei metodi `GetTodoItems` e `GetTodoItem` è [ActionResult\<T>](xref:web-api/action-return-types#actionresultt-type).</span><span class="sxs-lookup"><span data-stu-id="488b7-527">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="488b7-528">ASP.NET Core serializza automaticamente l'oggetto su [JSON](https://www.json.org/) e scrive il codice JSON nel corpo del messaggio di risposta.</span><span class="sxs-lookup"><span data-stu-id="488b7-528">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="488b7-529">Il codice di risposta per questo tipo restituito è 200, presupponendo che non esistano eccezioni non gestite.</span><span class="sxs-lookup"><span data-stu-id="488b7-529">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="488b7-530">Le eccezioni non gestite vengono convertite in errori 5xx.</span><span class="sxs-lookup"><span data-stu-id="488b7-530">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="488b7-531">I tipi restituiti `ActionResult` possono rappresentare un ampio intervallo di codici di stato HTTP.</span><span class="sxs-lookup"><span data-stu-id="488b7-531">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="488b7-532">Ad esempio, `GetTodoItem` può restituire due valori di stato diversi:</span><span class="sxs-lookup"><span data-stu-id="488b7-532">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="488b7-533">Se nessun elemento corrisponde all'ID richiesto, il metodo restituisce un codice di errore 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound).</span><span class="sxs-lookup"><span data-stu-id="488b7-533">If no item matches the requested ID, the method returns a 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) error code.</span></span>
* <span data-ttu-id="488b7-534">In caso contrario, il metodo restituisce il codice 200 con un corpo della risposta JSON.</span><span class="sxs-lookup"><span data-stu-id="488b7-534">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="488b7-535">La restituzione di `item` risulta in una risposta HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="488b7-535">Returning `item` results in an HTTP 200 response.</span></span>


## <a name="test-the-gettodoitems-method"></a><span data-ttu-id="488b7-536">Testare il metodo GetTodoItems</span><span class="sxs-lookup"><span data-stu-id="488b7-536">Test the GetTodoItems method</span></span>

<span data-ttu-id="488b7-537">Questa esercitazione usa Postman per testare l'API Web.</span><span class="sxs-lookup"><span data-stu-id="488b7-537">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="488b7-538">Installare [Postman](https://www.getpostman.com/downloads/)</span><span class="sxs-lookup"><span data-stu-id="488b7-538">Install [Postman](https://www.getpostman.com/downloads/)</span></span>
* <span data-ttu-id="488b7-539">Avviare l'app Web.</span><span class="sxs-lookup"><span data-stu-id="488b7-539">Start the web app.</span></span>
* <span data-ttu-id="488b7-540">Avviare Postman.</span><span class="sxs-lookup"><span data-stu-id="488b7-540">Start Postman.</span></span>
* <span data-ttu-id="488b7-541">Disattivare **SSL certificate verification** (Verifica certificato SSL)</span><span class="sxs-lookup"><span data-stu-id="488b7-541">Disable **SSL certificate verification**</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="488b7-542">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="488b7-542">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="488b7-543">Da **File** > **Settings** (Impostazioni) (scheda **General** (Generale)) disabilitare **SSL certificate verification** (Verifica certificato SSL).</span><span class="sxs-lookup"><span data-stu-id="488b7-543">From **File** > **Settings** (**General** tab), disable **SSL certificate verification**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="488b7-544">Visual Studio Code/Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="488b7-544">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="488b7-545">Da **Postman**  >  **Preferences** (Preferenze) (scheda **General** (Generale)) disabilitare **SSL certificate verification** (Verifica certificato SSL).</span><span class="sxs-lookup"><span data-stu-id="488b7-545">From **Postman** > **Preferences** (**General** tab), disable **SSL certificate verification**.</span></span> <span data-ttu-id="488b7-546">In alternativa, selezionare la chiave inglese e selezionare **Settings** (Impostazioni), quindi disabilitare la verifica del certificato SSL.</span><span class="sxs-lookup"><span data-stu-id="488b7-546">Alternatively, select the wrench and select **Settings**, then disable the SSL certificate verification.</span></span>

---
  
> [!WARNING]
> <span data-ttu-id="488b7-547">Riattivare la verifica dei certificati SSL al termine del test del controller.</span><span class="sxs-lookup"><span data-stu-id="488b7-547">Re-enable SSL certificate verification after testing the controller.</span></span>

* <span data-ttu-id="488b7-548">Creare una nuova richiesta.</span><span class="sxs-lookup"><span data-stu-id="488b7-548">Create a new request.</span></span>
  * <span data-ttu-id="488b7-549">Impostare il metodo HTTP su **GET**.</span><span class="sxs-lookup"><span data-stu-id="488b7-549">Set the HTTP method to **GET**.</span></span>
  * <span data-ttu-id="488b7-550">Impostare l'URL della richiesta su `https://localhost:<port>/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="488b7-550">Set the request URL to `https://localhost:<port>/api/todo`.</span></span> <span data-ttu-id="488b7-551">Ad esempio `https://localhost:5001/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="488b7-551">For example, `https://localhost:5001/api/todo`.</span></span>
* <span data-ttu-id="488b7-552">Impostare **Two pane view** (Visualizzazione in due riquadri) in Postman.</span><span class="sxs-lookup"><span data-stu-id="488b7-552">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="488b7-553">Selezionare **Send** (Invia).</span><span class="sxs-lookup"><span data-stu-id="488b7-553">Select **Send**.</span></span>

![Postman con richiesta GET](first-web-api/_static/2pv.png)

## <a name="add-a-create-method"></a><span data-ttu-id="488b7-555">Aggiungere un metodo Create</span><span class="sxs-lookup"><span data-stu-id="488b7-555">Add a Create method</span></span>

<span data-ttu-id="488b7-556">Aggiungere il metodo `PostTodoItem` seguente in *Controllers/TodoController.cs*:</span><span class="sxs-lookup"><span data-stu-id="488b7-556">Add the following `PostTodoItem` method inside of *Controllers/TodoController.cs*:</span></span> 

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="488b7-557">Il codice precedente è un metodo HTTP POST, come indicato dall'attributo [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute).</span><span class="sxs-lookup"><span data-stu-id="488b7-557">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="488b7-558">Il metodo ottiene il valore dell'elemento attività dal corpo della richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="488b7-558">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="488b7-559">Il metodo `CreatedAtAction`:</span><span class="sxs-lookup"><span data-stu-id="488b7-559">The `CreatedAtAction` method:</span></span>

* <span data-ttu-id="488b7-560">Restituisce un codice di stato HTTP 201 in caso di esito positivo.</span><span class="sxs-lookup"><span data-stu-id="488b7-560">Returns an HTTP 201 status code, if successful.</span></span> <span data-ttu-id="488b7-561">HTTP 201 è la risposta standard per un metodo HTTP POST che crea una nuova risorsa nel server.</span><span class="sxs-lookup"><span data-stu-id="488b7-561">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="488b7-562">Aggiunge un'intestazione `Location` alla risposta.</span><span class="sxs-lookup"><span data-stu-id="488b7-562">Adds a `Location` header to the response.</span></span> <span data-ttu-id="488b7-563">L'intestazione `Location` specifica l'URI dell'elemento attività appena creato.</span><span class="sxs-lookup"><span data-stu-id="488b7-563">The `Location` header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="488b7-564">Per altre informazioni, vedere [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="488b7-564">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="488b7-565">Fa riferimento all'azione `GetTodoItem` per creare l'URI dell'intestazione `Location`.</span><span class="sxs-lookup"><span data-stu-id="488b7-565">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="488b7-566">La parola chiave `nameof` C# viene usata per evitare di impostare il nome dell'azione come hardcoded nella chiamata a `CreatedAtAction`.</span><span class="sxs-lookup"><span data-stu-id="488b7-566">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

### <a name="test-the-posttodoitem-method"></a><span data-ttu-id="488b7-567">Testare il metodo PostTodoItem</span><span class="sxs-lookup"><span data-stu-id="488b7-567">Test the PostTodoItem method</span></span>

* <span data-ttu-id="488b7-568">Compilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="488b7-568">Build the project.</span></span>
* <span data-ttu-id="488b7-569">In Postman impostare il metodo HTTP su `POST`.</span><span class="sxs-lookup"><span data-stu-id="488b7-569">In Postman, set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="488b7-570">Fare clic sulla scheda **Body** (Corpo).</span><span class="sxs-lookup"><span data-stu-id="488b7-570">Select the **Body** tab.</span></span>
* <span data-ttu-id="488b7-571">Selezionare il pulsante di opzione **raw** (non elaborato).</span><span class="sxs-lookup"><span data-stu-id="488b7-571">Select the **raw** radio button.</span></span>
* <span data-ttu-id="488b7-572">Impostare il tipo su **JSON (application/json)** .</span><span class="sxs-lookup"><span data-stu-id="488b7-572">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="488b7-573">Nel corpo della richiesta immettere codice JSON per un elemento attività:</span><span class="sxs-lookup"><span data-stu-id="488b7-573">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="488b7-574">Selezionare **Send** (Invia).</span><span class="sxs-lookup"><span data-stu-id="488b7-574">Select **Send**.</span></span>

  ![Postman con richiesta di creazione](first-web-api/_static/create.png)

  <span data-ttu-id="488b7-576">Se viene visualizzato un errore HTTP 405 - Metodo non consentito, è probabile che il progetto non sia stato compilato dopo l'aggiunta del metodo `PostTodoItem`.</span><span class="sxs-lookup"><span data-stu-id="488b7-576">If you get a 405 Method Not Allowed error, it's probably the result of not compiling the project after adding the `PostTodoItem` method.</span></span>

### <a name="test-the-location-header-uri"></a><span data-ttu-id="488b7-577">Testare l'URI dell'intestazione della posizione</span><span class="sxs-lookup"><span data-stu-id="488b7-577">Test the location header URI</span></span>

* <span data-ttu-id="488b7-578">Selezionare la scheda **Headers** (Intestazioni) nel riquadro **Response** (Risposta).</span><span class="sxs-lookup"><span data-stu-id="488b7-578">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="488b7-579">Copiare il valore dell'intestazione **Location** (Posizione):</span><span class="sxs-lookup"><span data-stu-id="488b7-579">Copy the **Location** header value:</span></span>

  ![Scheda Headers (Intestazioni) della console Postman](first-web-api/_static/pmc2.png)

* <span data-ttu-id="488b7-581">Impostare il metodo su GET.</span><span class="sxs-lookup"><span data-stu-id="488b7-581">Set the method to GET.</span></span>
* <span data-ttu-id="488b7-582">Incollare l'URI (ad esempio `https://localhost:5001/api/Todo/2`)</span><span class="sxs-lookup"><span data-stu-id="488b7-582">Paste the URI (for example, `https://localhost:5001/api/Todo/2`)</span></span>
* <span data-ttu-id="488b7-583">Selezionare **Send** (Invia).</span><span class="sxs-lookup"><span data-stu-id="488b7-583">Select **Send**.</span></span>

## <a name="add-a-puttodoitem-method"></a><span data-ttu-id="488b7-584">Aggiungere un metodo PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="488b7-584">Add a PutTodoItem method</span></span>

<span data-ttu-id="488b7-585">Aggiungere il metodo `PutTodoItem` seguente:</span><span class="sxs-lookup"><span data-stu-id="488b7-585">Add the following `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

<span data-ttu-id="488b7-586">`PutTodoItem` è simile a `PostTodoItem` ma usa la richiesta HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="488b7-586">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="488b7-587">La risposta è [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="488b7-587">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="488b7-588">In base alla specifica HTTP, una richiesta PUT richiede che il client invii l'intera entità aggiornata e non solo le modifiche.</span><span class="sxs-lookup"><span data-stu-id="488b7-588">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="488b7-589">Per supportare gli aggiornamenti parziali, usare [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span><span class="sxs-lookup"><span data-stu-id="488b7-589">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="488b7-590">Se si riceve un errore chiamando `PutTodoItem`, chiamare `GET` per verificare che il database includa un elemento.</span><span class="sxs-lookup"><span data-stu-id="488b7-590">If you get an error calling `PutTodoItem`, call `GET` to ensure there's an item in the database.</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="488b7-591">Testare il metodo PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="488b7-591">Test the PutTodoItem method</span></span>

<span data-ttu-id="488b7-592">Questo esempio usa un database in memoria che deve essere inizializzato ogni volta che l'app viene avviata.</span><span class="sxs-lookup"><span data-stu-id="488b7-592">This sample uses an in-memory database that must be initialed each time the app is started.</span></span> <span data-ttu-id="488b7-593">Deve esistere un elemento nel database prima di eseguire una chiamata PUT.</span><span class="sxs-lookup"><span data-stu-id="488b7-593">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="488b7-594">Chiamare GET per assicurarsi che sia presente un elemento nel database prima di eseguire una chiamata PUT.</span><span class="sxs-lookup"><span data-stu-id="488b7-594">Call GET to insure there's an item in the database before making a PUT call.</span></span>

<span data-ttu-id="488b7-595">Aggiornare l'elemento attività con id = 1 e impostarne il nome su "feed fish":</span><span class="sxs-lookup"><span data-stu-id="488b7-595">Update the to-do item that has id = 1 and set its name to "feed fish":</span></span>

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="488b7-596">L'immagine seguente visualizza l'aggiornamento di Postman:</span><span class="sxs-lookup"><span data-stu-id="488b7-596">The following image shows the Postman update:</span></span>

![Console Postman con risposta 204 (No Content)](first-web-api/_static/pmcput.png)

## <a name="add-a-deletetodoitem-method"></a><span data-ttu-id="488b7-598">Aggiungere un metodo DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="488b7-598">Add a DeleteTodoItem method</span></span>

<span data-ttu-id="488b7-599">Aggiungere il metodo `DeleteTodoItem` seguente:</span><span class="sxs-lookup"><span data-stu-id="488b7-599">Add the following `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="488b7-600">La risposta `DeleteTodoItem` è [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="488b7-600">The `DeleteTodoItem` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="488b7-601">Testare il metodo DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="488b7-601">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="488b7-602">Usare Postman per eliminare un elemento attività:</span><span class="sxs-lookup"><span data-stu-id="488b7-602">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="488b7-603">Impostare il metodo su `DELETE`.</span><span class="sxs-lookup"><span data-stu-id="488b7-603">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="488b7-604">Impostare l'URI dell'oggetto da eliminare, ad esempio `https://localhost:5001/api/todo/1`</span><span class="sxs-lookup"><span data-stu-id="488b7-604">Set the URI of the object to delete, for example `https://localhost:5001/api/todo/1`</span></span>
* <span data-ttu-id="488b7-605">Selezionare **Send** (Invia).</span><span class="sxs-lookup"><span data-stu-id="488b7-605">Select **Send**</span></span>

<span data-ttu-id="488b7-606">L'app di esempio consente di eliminare tutti gli elementi.</span><span class="sxs-lookup"><span data-stu-id="488b7-606">The sample app allows you to delete all the items.</span></span> <span data-ttu-id="488b7-607">Quando viene eliminato l'ultimo elemento, tuttavia, ne viene creato uno nuovo dal costruttore della classe modello alla successiva chiamata dell'API.</span><span class="sxs-lookup"><span data-stu-id="488b7-607">However, when the last item is deleted, a new one is created by the model class constructor the next time the API is called.</span></span>

## <a name="call-the-api-with-jquery"></a><span data-ttu-id="488b7-608">Chiamare l'API con jQuery</span><span class="sxs-lookup"><span data-stu-id="488b7-608">Call the API with jQuery</span></span>

<span data-ttu-id="488b7-609">In questa sezione viene aggiunta una pagina HTML che usa jQuery per chiamare l'API Web.</span><span class="sxs-lookup"><span data-stu-id="488b7-609">In this section, an HTML page is added that uses jQuery to call the web api.</span></span> <span data-ttu-id="488b7-610">jQuery avvia la richiesta e aggiorna la pagina con i dettagli ottenuti dalla risposta dell'API.</span><span class="sxs-lookup"><span data-stu-id="488b7-610">jQuery initiates the request and updates the page with the details from the API's response.</span></span>

<span data-ttu-id="488b7-611">Configurare l'app per [servire file statici](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) e [abilitare il mapping del file predefinito](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) aggiornando *Startup.cs* con il codice evidenziato seguente:</span><span class="sxs-lookup"><span data-stu-id="488b7-611">Configure the app to [serve static files](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [enable default file mapping](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) by updating *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup.cs?highlight=14-15&name=snippet_configure)]

<span data-ttu-id="488b7-612">Creare una cartella *wwwroot* nella directory del progetto.</span><span class="sxs-lookup"><span data-stu-id="488b7-612">Create a *wwwroot* folder in the project directory.</span></span>

<span data-ttu-id="488b7-613">Aggiungere un file HTML denominato *index.html* alla directory *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="488b7-613">Add an HTML file named *index.html* to the *wwwroot* directory.</span></span> <span data-ttu-id="488b7-614">Sostituire il contenuto con il markup seguente:</span><span class="sxs-lookup"><span data-stu-id="488b7-614">Replace its contents with the following markup:</span></span>

[!code-html[](first-web-api/samples/2.2/TodoApi/wwwroot/index.html)]

<span data-ttu-id="488b7-615">Aggiungere un file JavaScript con nome *site.js* alla directory *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="488b7-615">Add a JavaScript file named *site.js* to the *wwwroot* directory.</span></span> <span data-ttu-id="488b7-616">Sostituire il contenuto con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="488b7-616">Replace its contents with the following code:</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_SiteJs)]

<span data-ttu-id="488b7-617">Può essere necessario modificare le impostazioni di avvio del progetto ASP.NET Core per il test della pagina HTML in locale:</span><span class="sxs-lookup"><span data-stu-id="488b7-617">A change to the ASP.NET Core project's launch settings may be required to test the HTML page locally:</span></span>

* <span data-ttu-id="488b7-618">Aprire *Properties\launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="488b7-618">Open *Properties\launchSettings.json*.</span></span>
* <span data-ttu-id="488b7-619">Rimuovere la proprietà `launchUrl` per forzare l'app ad aprire *index.html*, il file predefinito del progetto.</span><span class="sxs-lookup"><span data-stu-id="488b7-619">Remove the `launchUrl` property to force the app to open at *index.html*&mdash;the project's default file.</span></span>

<span data-ttu-id="488b7-620">È possibile ottenere jQuery in vari modi.</span><span class="sxs-lookup"><span data-stu-id="488b7-620">There are several ways to get jQuery.</span></span> <span data-ttu-id="488b7-621">Nel frammento precedente la libreria viene caricata da una rete CDN.</span><span class="sxs-lookup"><span data-stu-id="488b7-621">In the preceding snippet, the library is loaded from a CDN.</span></span>

<span data-ttu-id="488b7-622">In questo esempio vengono chiamati tutti i metodi CRUD dell'API.</span><span class="sxs-lookup"><span data-stu-id="488b7-622">This sample calls all of the CRUD methods of the API.</span></span> <span data-ttu-id="488b7-623">Di seguito sono incluse le spiegazioni delle chiamate all'API.</span><span class="sxs-lookup"><span data-stu-id="488b7-623">Following are explanations of the calls to the API.</span></span>

### <a name="get-a-list-of-to-do-items"></a><span data-ttu-id="488b7-624">Ottenere un elenco di elementi attività</span><span class="sxs-lookup"><span data-stu-id="488b7-624">Get a list of to-do items</span></span>

<span data-ttu-id="488b7-625">La funzione [ajax](https://api.jquery.com/jquery.ajax/) di jQuery invia una richiesta `GET` all'API, la quale restituisce codice JSON che rappresenta una matrice di elementi attività.</span><span class="sxs-lookup"><span data-stu-id="488b7-625">The jQuery [ajax](https://api.jquery.com/jquery.ajax/) function sends a `GET` request to the API, which returns JSON representing an array of to-do items.</span></span> <span data-ttu-id="488b7-626">La funzione di callback `success` viene chiamata se la richiesta ha esito positivo.</span><span class="sxs-lookup"><span data-stu-id="488b7-626">The `success` callback function is invoked if the request succeeds.</span></span> <span data-ttu-id="488b7-627">Nel callback il modello DOM viene aggiornato con le informazioni dell'elemento attività.</span><span class="sxs-lookup"><span data-stu-id="488b7-627">In the callback, the DOM is updated with the to-do information.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_GetData)]

### <a name="add-a-to-do-item"></a><span data-ttu-id="488b7-628">Aggiungere un elemento attività</span><span class="sxs-lookup"><span data-stu-id="488b7-628">Add a to-do item</span></span>

<span data-ttu-id="488b7-629">La funzione [ajax](https://api.jquery.com/jquery.ajax/) invia una richiesta `POST` con l'elemento attività nel corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="488b7-629">The [ajax](https://api.jquery.com/jquery.ajax/) function sends a `POST` request with the to-do item in the request body.</span></span> <span data-ttu-id="488b7-630">Le opzioni `accepts` e `contentType` sono impostate su `application/json` per specificare il tipo di supporto ricevuto e inviato.</span><span class="sxs-lookup"><span data-stu-id="488b7-630">The `accepts` and `contentType` options are set to `application/json` to specify the media type being received and sent.</span></span> <span data-ttu-id="488b7-631">L'elemento attività viene convertito in JSON usando [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span><span class="sxs-lookup"><span data-stu-id="488b7-631">The to-do item is converted to JSON by using [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span></span> <span data-ttu-id="488b7-632">Quando l'API restituisce un codice di stato di esecuzione riuscita, viene chiamata la funzione `getData` per aggiornare la tabella HTML.</span><span class="sxs-lookup"><span data-stu-id="488b7-632">When the API returns a successful status code, the `getData` function is invoked to update the HTML table.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AddItem)]

### <a name="update-a-to-do-item"></a><span data-ttu-id="488b7-633">Aggiornare un elemento attività</span><span class="sxs-lookup"><span data-stu-id="488b7-633">Update a to-do item</span></span>

<span data-ttu-id="488b7-634">L'aggiornamento di un elemento attività è simile all'aggiunta di un elemento di questo tipo.</span><span class="sxs-lookup"><span data-stu-id="488b7-634">Updating a to-do item is similar to adding one.</span></span> <span data-ttu-id="488b7-635">L'`url` cambia per includere l'identificatore univoco dell'elemento e `type` è `PUT`.</span><span class="sxs-lookup"><span data-stu-id="488b7-635">The `url` changes to add the unique identifier of the item, and the `type` is `PUT`.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxPut)]

### <a name="delete-a-to-do-item"></a><span data-ttu-id="488b7-636">Eliminare un elemento attività</span><span class="sxs-lookup"><span data-stu-id="488b7-636">Delete a to-do item</span></span>

<span data-ttu-id="488b7-637">È possibile eliminare un elemento attività impostando `type` su `DELETE` nella chiamata AJAX e specificando l'identificatore univoco dell'elemento nell'URL.</span><span class="sxs-lookup"><span data-stu-id="488b7-637">Deleting a to-do item is accomplished by setting the `type` on the AJAX call to `DELETE` and specifying the item's unique identifier in the URL.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxDelete)]

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="488b7-638">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="488b7-638">Additional resources</span></span>

<span data-ttu-id="488b7-639">[Visualizzare o scaricare il codice di esempio per questa esercitazione](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span><span class="sxs-lookup"><span data-stu-id="488b7-639">[View or download sample code for this tutorial](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span></span> <span data-ttu-id="488b7-640">Vedere [come scaricare un esempio](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="488b7-640">See [how to download](xref:index#how-to-download-a-sample).</span></span>

<span data-ttu-id="488b7-641">Per altre informazioni, vedere le seguenti risorse:</span><span class="sxs-lookup"><span data-stu-id="488b7-641">For more information, see the following resources:</span></span>

* <xref:web-api/index>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:data/ef-rp/index>
* <xref:mvc/controllers/routing>
* <xref:web-api/action-return-types>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/index>
* [<span data-ttu-id="488b7-642">Versione YouTube dell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="488b7-642">YouTube version of this tutorial</span></span>](https://www.youtube.com/watch?v=TTkhEyGBfAk)
