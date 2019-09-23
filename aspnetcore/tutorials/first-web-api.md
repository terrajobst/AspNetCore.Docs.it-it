---
title: "Esercitazione: Creare un'API Web con ASP.NET Core"
author: rick-anderson
description: Informazioni su come creare un'API Web con ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 08/27/2019
uid: tutorials/first-web-api
ms.openlocfilehash: 2d0eb24641c3d1f795b9e85ce10d42ee96d30846
ms.sourcegitcommit: d34b2627a69bc8940b76a949de830335db9701d3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/23/2019
ms.locfileid: "71187304"
---
# <a name="tutorial-create-a-web-api-with-aspnet-core"></a><span data-ttu-id="f97e9-103">Esercitazione: Creare un'API Web con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f97e9-103">Tutorial: Create a web API with ASP.NET Core</span></span>

<span data-ttu-id="f97e9-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT) e [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="f97e9-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="f97e9-105">Questa esercitazione illustra le nozioni di base della creazione di un'API Web con ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f97e9-105">This tutorial teaches the basics of building a web API with ASP.NET Core.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="f97e9-106">In questa esercitazione si imparerà a:</span><span class="sxs-lookup"><span data-stu-id="f97e9-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="f97e9-107">Creare un progetto di API Web.</span><span class="sxs-lookup"><span data-stu-id="f97e9-107">Create a web API project.</span></span>
> * <span data-ttu-id="f97e9-108">Aggiungere una classe modello e un contesto di database.</span><span class="sxs-lookup"><span data-stu-id="f97e9-108">Add a model class and a database context.</span></span>
> * <span data-ttu-id="f97e9-109">Eseguire lo scaffolding di un controller con i metodi CRUD.</span><span class="sxs-lookup"><span data-stu-id="f97e9-109">Scaffold a controller with CRUD methods.</span></span>
> * <span data-ttu-id="f97e9-110">Configurare il routing, i percorsi URL e i valori restituiti.</span><span class="sxs-lookup"><span data-stu-id="f97e9-110">Configure routing, URL paths, and return values.</span></span>
> * <span data-ttu-id="f97e9-111">Chiamare l'API Web con Postman.</span><span class="sxs-lookup"><span data-stu-id="f97e9-111">Call the web API with Postman.</span></span>

<span data-ttu-id="f97e9-112">Al termine si avrà un'API Web che può gestire gli elementi di tipo "attività" archiviati in un database.</span><span class="sxs-lookup"><span data-stu-id="f97e9-112">At the end, you have a web API that can manage "to-do" items stored in a database.</span></span>

## <a name="overview"></a><span data-ttu-id="f97e9-113">Panoramica</span><span class="sxs-lookup"><span data-stu-id="f97e9-113">Overview</span></span>

<span data-ttu-id="f97e9-114">Questa esercitazione consente di creare l'API seguente:</span><span class="sxs-lookup"><span data-stu-id="f97e9-114">This tutorial creates the following API:</span></span>

|<span data-ttu-id="f97e9-115">API</span><span class="sxs-lookup"><span data-stu-id="f97e9-115">API</span></span> | <span data-ttu-id="f97e9-116">Descrizione</span><span class="sxs-lookup"><span data-stu-id="f97e9-116">Description</span></span> | <span data-ttu-id="f97e9-117">Corpo della richiesta</span><span class="sxs-lookup"><span data-stu-id="f97e9-117">Request body</span></span> | <span data-ttu-id="f97e9-118">Corpo della risposta</span><span class="sxs-lookup"><span data-stu-id="f97e9-118">Response body</span></span> |
|--- | ---- | ---- | ---- |
|<span data-ttu-id="f97e9-119">GET /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="f97e9-119">GET /api/TodoItems</span></span> | <span data-ttu-id="f97e9-120">Ottiene tutti gli elementi attività</span><span class="sxs-lookup"><span data-stu-id="f97e9-120">Get all to-do items</span></span> | <span data-ttu-id="f97e9-121">nessuno</span><span class="sxs-lookup"><span data-stu-id="f97e9-121">None</span></span> | <span data-ttu-id="f97e9-122">Matrice di elementi attività</span><span class="sxs-lookup"><span data-stu-id="f97e9-122">Array of to-do items</span></span>|
|<span data-ttu-id="f97e9-123">GET /api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="f97e9-123">GET /api/TodoItems/{id}</span></span> | <span data-ttu-id="f97e9-124">Ottiene un elemento in base all'ID</span><span class="sxs-lookup"><span data-stu-id="f97e9-124">Get an item by ID</span></span> | <span data-ttu-id="f97e9-125">nessuno</span><span class="sxs-lookup"><span data-stu-id="f97e9-125">None</span></span> | <span data-ttu-id="f97e9-126">Elemento attività</span><span class="sxs-lookup"><span data-stu-id="f97e9-126">To-do item</span></span>|
|<span data-ttu-id="f97e9-127">POST /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="f97e9-127">POST /api/TodoItems</span></span> | <span data-ttu-id="f97e9-128">Aggiunge un nuovo elemento</span><span class="sxs-lookup"><span data-stu-id="f97e9-128">Add a new item</span></span> | <span data-ttu-id="f97e9-129">Elemento attività</span><span class="sxs-lookup"><span data-stu-id="f97e9-129">To-do item</span></span> | <span data-ttu-id="f97e9-130">Elemento attività</span><span class="sxs-lookup"><span data-stu-id="f97e9-130">To-do item</span></span> |
|<span data-ttu-id="f97e9-131">PUT /api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="f97e9-131">PUT /api/TodoItems/{id}</span></span> | <span data-ttu-id="f97e9-132">Aggiorna un elemento esistente &nbsp;</span><span class="sxs-lookup"><span data-stu-id="f97e9-132">Update an existing item &nbsp;</span></span> | <span data-ttu-id="f97e9-133">Elemento attività</span><span class="sxs-lookup"><span data-stu-id="f97e9-133">To-do item</span></span> | <span data-ttu-id="f97e9-134">nessuno</span><span class="sxs-lookup"><span data-stu-id="f97e9-134">None</span></span> |
|<span data-ttu-id="f97e9-135">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="f97e9-135">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span></span> | <span data-ttu-id="f97e9-136">Elimina un elemento &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="f97e9-136">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="f97e9-137">nessuno</span><span class="sxs-lookup"><span data-stu-id="f97e9-137">None</span></span> | <span data-ttu-id="f97e9-138">nessuno</span><span class="sxs-lookup"><span data-stu-id="f97e9-138">None</span></span>|

<span data-ttu-id="f97e9-139">Il diagramma seguente visualizza la struttura dell'app.</span><span class="sxs-lookup"><span data-stu-id="f97e9-139">The following diagram shows the design of the app.</span></span>

![Il client è rappresentato da una casella a sinistra.](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="f97e9-145">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="f97e9-145">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f97e9-146">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f97e9-146">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f97e9-147">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f97e9-147">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f97e9-148">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="f97e9-148">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="f97e9-149">Creare un progetto Web</span><span class="sxs-lookup"><span data-stu-id="f97e9-149">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f97e9-150">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f97e9-150">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f97e9-151">Scegliere **Nuovo** > **Progetto** dal menu **File**.</span><span class="sxs-lookup"><span data-stu-id="f97e9-151">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="f97e9-152">Selezionare il modello **Applicazione Web ASP.NET Core** e fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="f97e9-152">Select the **ASP.NET Core Web Application** template and click **Next**.</span></span>
* <span data-ttu-id="f97e9-153">Assegnare al progetto il nome *TodoApi* e fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="f97e9-153">Name the project *TodoApi* and click **Create**.</span></span>
* <span data-ttu-id="f97e9-154">Nella finestra di dialogo **Crea una nuova applicazione Web ASP.NET Core** verificare che siano selezionati **.NET Core** e **ASP.NET Core 3.0**.</span><span class="sxs-lookup"><span data-stu-id="f97e9-154">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 3.0** are selected.</span></span> <span data-ttu-id="f97e9-155">Selezionare il modello **API** e fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="f97e9-155">Select the **API** template and click **Create**.</span></span> <span data-ttu-id="f97e9-156">**Non** selezionare **Abilita supporto Docker**.</span><span class="sxs-lookup"><span data-stu-id="f97e9-156">**Don't** select **Enable Docker Support**.</span></span>

![Finestra di dialogo Nuovo progetto di Visual Studio](first-web-api/_static/vs3.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f97e9-158">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f97e9-158">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="f97e9-159">Aprire il [terminale integrato](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="f97e9-159">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="f97e9-160">Cambiare directory (`cd`) e passare alla cartella che conterrà la cartella del progetto.</span><span class="sxs-lookup"><span data-stu-id="f97e9-160">Change directories (`cd`) to the folder that will contain the project folder.</span></span>
* <span data-ttu-id="f97e9-161">Eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="f97e9-161">Run the following commands:</span></span>

   ```dotnetcli
   dotnet new webapi -o TodoApi
   cd TodoAPI
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.EntityFrameworkCore.InMemory
   code -r ../TodoApi
   ```

* <span data-ttu-id="f97e9-162">Quando una finestra di dialogo chiede se si vuole aggiungere gli asset necessari al progetto, selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="f97e9-162">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

  <span data-ttu-id="f97e9-163">I comandi precedenti:</span><span class="sxs-lookup"><span data-stu-id="f97e9-163">The preceding commands:</span></span>

  * <span data-ttu-id="f97e9-164">Crea un nuovo progetto API Web e lo apre in Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="f97e9-164">Creates a new web API project and opens it in Visual Studio Code.</span></span>
  * <span data-ttu-id="f97e9-165">Aggiunge i pacchetti NuGet necessari nella sezione successiva.</span><span class="sxs-lookup"><span data-stu-id="f97e9-165">Adds the NuGet packages which are required in the next section.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f97e9-166">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="f97e9-166">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="f97e9-167">Selezionare **File** > **Nuova soluzione**.</span><span class="sxs-lookup"><span data-stu-id="f97e9-167">Select **File** > **New Solution**.</span></span>

  ![Nuova soluzione macOS](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="f97e9-169">Selezionare **.NET Core** > **App** > **API** > **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="f97e9-169">Select **.NET Core** > **App** > **API** > **Next**.</span></span>

  ![Finestra di dialogo Nuovo progetto di macOS](first-web-api-mac/_static/1.png)
  
* <span data-ttu-id="f97e9-171">Nella finestra di dialogo **Configura la nuova API Web ASP.NET Core** selezionare il **framework di destinazione** \* *.NET Core 3.0*.</span><span class="sxs-lookup"><span data-stu-id="f97e9-171">In the **Configure your new ASP.NET Core Web API** dialog, select **Target Framework** of \**.NET Core 3.0*.</span></span>

* <span data-ttu-id="f97e9-172">Immettere *TodoApi* in **Nome progetto**, quindi selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="f97e9-172">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![Finestra di dialogo di configurazione](first-web-api-mac/_static/2.png)

[!INCLUDE[](~/includes/mac-terminal-access.md)]

<span data-ttu-id="f97e9-174">Aprire un terminale di comando nella cartella di progetto ed eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="f97e9-174">Open a command terminal in the project folder and run the following commands:</span></span>

   ```dotnetcli
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.EntityFrameworkCore.InMemory
   ```

---

### <a name="test-the-api"></a><span data-ttu-id="f97e9-175">Testare l'API</span><span class="sxs-lookup"><span data-stu-id="f97e9-175">Test the API</span></span>

<span data-ttu-id="f97e9-176">Il modello di progetto crea un'API `WeatherForecast`.</span><span class="sxs-lookup"><span data-stu-id="f97e9-176">The project template creates a `WeatherForecast` API.</span></span> <span data-ttu-id="f97e9-177">Chiamare il metodo `Get` da un browser per eseguire il test dell'app.</span><span class="sxs-lookup"><span data-stu-id="f97e9-177">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f97e9-178">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f97e9-178">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="f97e9-179">Premere CTRL+F5 per eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="f97e9-179">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="f97e9-180">Visual Studio apre un browser e naviga all'indirizzo `https://localhost:<port>/WeatherForecast`, dove `<port>` è un numero di porta selezionato a caso.</span><span class="sxs-lookup"><span data-stu-id="f97e9-180">Visual Studio launches a browser and navigates to `https://localhost:<port>/WeatherForecast`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="f97e9-181">Se in una finestra di dialogo viene chiesto se considerare attendibile il certificato IIS Express, selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="f97e9-181">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="f97e9-182">Nella finestra di dialogo **Avviso di sicurezza** successiva selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="f97e9-182">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f97e9-183">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f97e9-183">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="f97e9-184">Premere CTRL+F5 per eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="f97e9-184">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="f97e9-185">In un browser passare all'URL seguente: [https://localhost:5001/WeatherForecast](https://localhost:5001/WeatherForecast).</span><span class="sxs-lookup"><span data-stu-id="f97e9-185">In a browser, go to following URL: [https://localhost:5001/WeatherForecast](https://localhost:5001/WeatherForecast).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f97e9-186">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="f97e9-186">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="f97e9-187">Selezionare **Esegui** > **Avvia debug** per avviare l'app.</span><span class="sxs-lookup"><span data-stu-id="f97e9-187">Select **Run** > **Start Debugging** to launch the app.</span></span> <span data-ttu-id="f97e9-188">Visual Studio per Mac apre un browser e naviga all'indirizzo `https://localhost:<port>`, dove `<port>` è un numero di porta selezionato a caso.</span><span class="sxs-lookup"><span data-stu-id="f97e9-188">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="f97e9-189">Viene restituito un errore HTTP 404 (Non trovato).</span><span class="sxs-lookup"><span data-stu-id="f97e9-189">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="f97e9-190">Accodare `/WeatherForecast` all'URL (impostare l'URL su `https://localhost:<port>/WeatherForecast`).</span><span class="sxs-lookup"><span data-stu-id="f97e9-190">Append `/WeatherForecast` to the URL (change the URL to `https://localhost:<port>/WeatherForecast`).</span></span>

---

<span data-ttu-id="f97e9-191">Viene restituito un codice JSON simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="f97e9-191">JSON similar to the following is returned:</span></span>

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

## <a name="add-a-model-class"></a><span data-ttu-id="f97e9-192">Aggiungere una classe del modello</span><span class="sxs-lookup"><span data-stu-id="f97e9-192">Add a model class</span></span>

<span data-ttu-id="f97e9-193">Un *modello* è un set di classi che rappresentano i dati gestiti dall'app.</span><span class="sxs-lookup"><span data-stu-id="f97e9-193">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="f97e9-194">Il modello per questa app è una classe `TodoItem` singola.</span><span class="sxs-lookup"><span data-stu-id="f97e9-194">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f97e9-195">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f97e9-195">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f97e9-196">In **Esplora soluzioni** fare clic con il pulsante destro del mouse sul progetto.</span><span class="sxs-lookup"><span data-stu-id="f97e9-196">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="f97e9-197">Selezionare **Aggiungi** > **Nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="f97e9-197">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="f97e9-198">Assegnare il nome *Models* alla cartella.</span><span class="sxs-lookup"><span data-stu-id="f97e9-198">Name the folder *Models*.</span></span>

* <span data-ttu-id="f97e9-199">Fare clic con il pulsante destro del mouse sulla cartella *Models* e scegliere **Aggiungi** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="f97e9-199">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="f97e9-200">Assegnare alla classe il nome *TodoItem* e selezionare **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="f97e9-200">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="f97e9-201">Sostituire il codice del modello con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="f97e9-201">Replace the template code with the following code:</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f97e9-202">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f97e9-202">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="f97e9-203">Aggiungere una cartella denominata *Models*.</span><span class="sxs-lookup"><span data-stu-id="f97e9-203">Add a folder named *Models*.</span></span>

* <span data-ttu-id="f97e9-204">Aggiungere una classe `TodoItem` alla cartella *Models* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="f97e9-204">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f97e9-205">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="f97e9-205">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="f97e9-206">Fare clic con il pulsante destro del mouse sul progetto.</span><span class="sxs-lookup"><span data-stu-id="f97e9-206">Right-click the project.</span></span> <span data-ttu-id="f97e9-207">Selezionare **Aggiungi** > **Nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="f97e9-207">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="f97e9-208">Assegnare il nome *Models* alla cartella.</span><span class="sxs-lookup"><span data-stu-id="f97e9-208">Name the folder *Models*.</span></span>

  ![Nuova cartella](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="f97e9-210">Fare clic con il pulsante destro del mouse sulla cartella *Models* e scegliere **Aggiungi** > **Nuovo file** > **Generale** > **Classe vuota**.</span><span class="sxs-lookup"><span data-stu-id="f97e9-210">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="f97e9-211">Assegnare alla classe il nome *TodoItem* e fare clic su **Nuovo**.</span><span class="sxs-lookup"><span data-stu-id="f97e9-211">Name the class *TodoItem*, and then click **New**.</span></span>

* <span data-ttu-id="f97e9-212">Sostituire il codice del modello con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="f97e9-212">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="f97e9-213">La proprietà `Id` funziona come chiave univoca in un database relazionale.</span><span class="sxs-lookup"><span data-stu-id="f97e9-213">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="f97e9-214">È possibile inserire le classi del modello in qualsiasi punto del progetto, ma per convenzione viene usata la cartella *Models*.</span><span class="sxs-lookup"><span data-stu-id="f97e9-214">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="f97e9-215">Aggiungere un contesto di database</span><span class="sxs-lookup"><span data-stu-id="f97e9-215">Add a database context</span></span>

<span data-ttu-id="f97e9-216">Il *contesto di database* è la classe principale che coordina le funzionalità di Entity Framework per un modello di dati.</span><span class="sxs-lookup"><span data-stu-id="f97e9-216">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="f97e9-217">Questa classe viene creata mediante derivazione dalla classe `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="f97e9-217">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f97e9-218">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f97e9-218">Visual Studio</span></span>](#tab/visual-studio)

### <a name="add-microsoftentityframeworkcoresqlserver"></a><span data-ttu-id="f97e9-219">Aggiungere Microsoft.EntityFrameworkCore.SqlServer</span><span class="sxs-lookup"><span data-stu-id="f97e9-219">Add Microsoft.EntityFrameworkCore.SqlServer</span></span>

* <span data-ttu-id="f97e9-220">Dal menu **Strumenti** selezionare **Gestione pacchetti NuGet > Gestisci pacchetti NuGet per la soluzione**.</span><span class="sxs-lookup"><span data-stu-id="f97e9-220">From the **Tools** menu, select **NuGet Package Manager > Manage NuGet Packages for Solution**.</span></span>
* <span data-ttu-id="f97e9-221">Selezionare la casella di controllo **Includi versione preliminare**.</span><span class="sxs-lookup"><span data-stu-id="f97e9-221">Select the **Include prerelease** checkbox.</span></span>
* <span data-ttu-id="f97e9-222">Selezionare la scheda **Esplora** e quindi immettere **Microsoft.EntityFrameworkCore.SqlServer** nella casella di ricerca.</span><span class="sxs-lookup"><span data-stu-id="f97e9-222">Select the **Browse** tab, and then enter **Microsoft.EntityFrameworkCore.SqlServer** in the search box.</span></span>
* <span data-ttu-id="f97e9-223">Selezionare **Microsoft.EntityFrameworkCore.SqlServer V3.0.0-preview** nel riquadro sinistro.</span><span class="sxs-lookup"><span data-stu-id="f97e9-223">Select  **Microsoft.EntityFrameworkCore.SqlServer V3.0.0-preview** in the left pane.</span></span>
* <span data-ttu-id="f97e9-224">Selezionare la casella di controllo **Progetto** nel riquadro di destra e quindi selezionare **Installa**.</span><span class="sxs-lookup"><span data-stu-id="f97e9-224">Select the **Project** check box in the right pane and then select **Install**.</span></span>
* <span data-ttu-id="f97e9-225">Usare le istruzioni precedenti per aggiungere il pacchetto NuGet `Microsoft.EntityFrameworkCore.InMemory`.</span><span class="sxs-lookup"><span data-stu-id="f97e9-225">Use the preceding instructions to add the `Microsoft.EntityFrameworkCore.InMemory` NuGet package.</span></span>

![Gestione pacchetti NuGet](first-web-api/_static/vs3NuGet.png)

## <a name="add-the-todocontext-database-context"></a><span data-ttu-id="f97e9-227">Aggiungere il contesto del database TodoContext</span><span class="sxs-lookup"><span data-stu-id="f97e9-227">Add the TodoContext database context</span></span>

* <span data-ttu-id="f97e9-228">Fare clic con il pulsante destro del mouse sulla cartella *Models* e scegliere **Aggiungi** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="f97e9-228">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="f97e9-229">Assegnare alla classe il nome *TodoContext* e fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="f97e9-229">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="f97e9-230">Visual Studio Code/Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="f97e9-230">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="f97e9-231">Aggiungere una classe `TodoContext` alla cartella *Models*.</span><span class="sxs-lookup"><span data-stu-id="f97e9-231">Add a `TodoContext` class to the *Models* folder.</span></span>

---

* <span data-ttu-id="f97e9-232">Immettere il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="f97e9-232">Enter the following code:</span></span>

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="f97e9-233">Registrare il contesto del database</span><span class="sxs-lookup"><span data-stu-id="f97e9-233">Register the database context</span></span>

<span data-ttu-id="f97e9-234">In ASP.NET Core i servizi come il contesto del database devono essere registrati con il [contenitore di inserimento delle dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="f97e9-234">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="f97e9-235">Il contenitore rende disponibile il servizio ai controller.</span><span class="sxs-lookup"><span data-stu-id="f97e9-235">The container provides the service to controllers.</span></span>

<span data-ttu-id="f97e9-236">Aggiornare *Startup.cs* con il codice evidenziato seguente:</span><span class="sxs-lookup"><span data-stu-id="f97e9-236">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Startup.cs?highlight=7-8,23-24&name=snippet_all)]

<span data-ttu-id="f97e9-237">Il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="f97e9-237">The preceding code:</span></span>

* <span data-ttu-id="f97e9-238">Rimuove le dichiarazioni `using` inutilizzate.</span><span class="sxs-lookup"><span data-stu-id="f97e9-238">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="f97e9-239">Aggiunge il contesto del database al contenitore di inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="f97e9-239">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="f97e9-240">Specifica che il contesto del database userà un database in memoria.</span><span class="sxs-lookup"><span data-stu-id="f97e9-240">Specifies that the database context will use an in-memory database.</span></span>

## <a name="scaffold-a-controller"></a><span data-ttu-id="f97e9-241">Eseguire lo scaffolding di un controller</span><span class="sxs-lookup"><span data-stu-id="f97e9-241">Scaffold a controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f97e9-242">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f97e9-242">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f97e9-243">Fare clic con il pulsante destro del mouse sulla cartella *Controllers*.</span><span class="sxs-lookup"><span data-stu-id="f97e9-243">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="f97e9-244">Selezionare **Aggiungi** > **Nuovo elemento di scaffolding**.</span><span class="sxs-lookup"><span data-stu-id="f97e9-244">Select **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="f97e9-245">Selezionare **Controller API con azioni, che usa Entity Framework** e quindi selezionare **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="f97e9-245">Select **API Controller with actions, using Entity Framework**, and then select **Add**.</span></span>
* <span data-ttu-id="f97e9-246">Nella finestra di dialogo **Add API Controller with actions, using Entity Framework** (Aggiungi controller API con azioni, che usa Entity Framework):</span><span class="sxs-lookup"><span data-stu-id="f97e9-246">In the **Add API Controller with actions, using Entity Framework** dialog:</span></span>

  * <span data-ttu-id="f97e9-247">Selezionare **TodoItem (TodoAPI.Models)** in **Classe modello**.</span><span class="sxs-lookup"><span data-stu-id="f97e9-247">Select **TodoItem (TodoAPI.Models)** in the **Model class**.</span></span>
  * <span data-ttu-id="f97e9-248">Selezionare **TodoContext (TodoAPI.Models)** in **Classe contesto dei dati**.</span><span class="sxs-lookup"><span data-stu-id="f97e9-248">Select **TodoContext (TodoAPI.Models)** in the **Data context class**.</span></span>
  * <span data-ttu-id="f97e9-249">Selezionare **Aggiungi**</span><span class="sxs-lookup"><span data-stu-id="f97e9-249">Select **Add**</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="f97e9-250">Visual Studio Code/Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="f97e9-250">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="f97e9-251">Eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="f97e9-251">Run the following commands:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet tool install --global dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator controller -name TodoItemsController -async -api -m TodoItem -dc TodoContext  -outDir Controllers
```

<span data-ttu-id="f97e9-252">I comandi precedenti:</span><span class="sxs-lookup"><span data-stu-id="f97e9-252">The preceding commands:</span></span>

* <span data-ttu-id="f97e9-253">Aggiungere i pacchetti NuGet necessari per lo scaffolding.</span><span class="sxs-lookup"><span data-stu-id="f97e9-253">Add NuGet packages required for scaffolding.</span></span>
* <span data-ttu-id="f97e9-254">Installa il motore di scaffolding (`dotnet-aspnet-codegenerator`).</span><span class="sxs-lookup"><span data-stu-id="f97e9-254">Installs the scaffolding engine (`dotnet-aspnet-codegenerator`).</span></span>
* <span data-ttu-id="f97e9-255">Esegue lo scaffolding di `TodoItemsController`.</span><span class="sxs-lookup"><span data-stu-id="f97e9-255">Scaffolds the `TodoItemsController`.</span></span>

---

<span data-ttu-id="f97e9-256">Il codice generato:</span><span class="sxs-lookup"><span data-stu-id="f97e9-256">The generated code:</span></span>

* <span data-ttu-id="f97e9-257">Definisce una classe controller API senza metodi.</span><span class="sxs-lookup"><span data-stu-id="f97e9-257">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="f97e9-258">Contrassegna la classe con l'attributo [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute).</span><span class="sxs-lookup"><span data-stu-id="f97e9-258">Decorates the class with the [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute.</span></span> <span data-ttu-id="f97e9-259">L'attributo indica che il controller risponde alle richieste di API Web.</span><span class="sxs-lookup"><span data-stu-id="f97e9-259">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="f97e9-260">Per informazioni sui comportamenti specifici consentiti dall'attributo, vedere <xref:web-api/index>.</span><span class="sxs-lookup"><span data-stu-id="f97e9-260">For information about specific behaviors that the attribute enables, see <xref:web-api/index>.</span></span>
* <span data-ttu-id="f97e9-261">Usa l'inserimento delle dipendenze per inserire il contesto del database (`TodoContext`) nel controller.</span><span class="sxs-lookup"><span data-stu-id="f97e9-261">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="f97e9-262">Il contesto di database viene usato in ognuno dei metodi [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) nel controller.</span><span class="sxs-lookup"><span data-stu-id="f97e9-262">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

## <a name="examine-the-posttodoitem-create-method"></a><span data-ttu-id="f97e9-263">Esaminare il metodo di creazione PostTodoItem</span><span class="sxs-lookup"><span data-stu-id="f97e9-263">Examine the PostTodoItem create method</span></span>

<span data-ttu-id="f97e9-264">Sostituire l'istruzione return in `PostTodoItem` per usare l'operatore [nameof](/dotnet/csharp/language-reference/operators/nameof):</span><span class="sxs-lookup"><span data-stu-id="f97e9-264">Replace the return statement in the `PostTodoItem` to use the [nameof](/dotnet/csharp/language-reference/operators/nameof) operator:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Create)]

<span data-ttu-id="f97e9-265">Il codice precedente è un metodo HTTP POST, come indicato dall'attributo [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute).</span><span class="sxs-lookup"><span data-stu-id="f97e9-265">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="f97e9-266">Il metodo ottiene il valore dell'elemento attività dal corpo della richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="f97e9-266">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="f97e9-267">Il metodo <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*>:</span><span class="sxs-lookup"><span data-stu-id="f97e9-267">The <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> method:</span></span>

* <span data-ttu-id="f97e9-268">Restituisce un codice di stato HTTP 201 in caso di esito positivo.</span><span class="sxs-lookup"><span data-stu-id="f97e9-268">Returns an HTTP 201 status code if successful.</span></span> <span data-ttu-id="f97e9-269">HTTP 201 è la risposta standard per un metodo HTTP POST che crea una nuova risorsa nel server.</span><span class="sxs-lookup"><span data-stu-id="f97e9-269">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="f97e9-270">Aggiunge un'intestazione [Location](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) alla risposta.</span><span class="sxs-lookup"><span data-stu-id="f97e9-270">Adds a [Location](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) header to the response.</span></span> <span data-ttu-id="f97e9-271">L'intestazione `Location` specifica l'[URI](https://developer.mozilla.org/docs/Glossary/URI) dell'elemento attività appena creato.</span><span class="sxs-lookup"><span data-stu-id="f97e9-271">The `Location` header specifies the [URI](https://developer.mozilla.org/docs/Glossary/URI) of the newly created to-do item.</span></span> <span data-ttu-id="f97e9-272">Per altre informazioni, vedere [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="f97e9-272">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="f97e9-273">Fa riferimento all'azione `GetTodoItem` per creare l'URI dell'intestazione `Location`.</span><span class="sxs-lookup"><span data-stu-id="f97e9-273">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="f97e9-274">La parola chiave `nameof` C# viene usata per evitare di impostare il nome dell'azione come hardcoded nella chiamata a `CreatedAtAction`.</span><span class="sxs-lookup"><span data-stu-id="f97e9-274">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

### <a name="install-postman"></a><span data-ttu-id="f97e9-275">Installare Postman</span><span class="sxs-lookup"><span data-stu-id="f97e9-275">Install Postman</span></span>

<span data-ttu-id="f97e9-276">Questa esercitazione usa Postman per testare l'API Web.</span><span class="sxs-lookup"><span data-stu-id="f97e9-276">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="f97e9-277">Installare [Postman](https://www.getpostman.com/downloads/)</span><span class="sxs-lookup"><span data-stu-id="f97e9-277">Install [Postman](https://www.getpostman.com/downloads/)</span></span>
* <span data-ttu-id="f97e9-278">Avviare l'app Web.</span><span class="sxs-lookup"><span data-stu-id="f97e9-278">Start the web app.</span></span>
* <span data-ttu-id="f97e9-279">Avviare Postman.</span><span class="sxs-lookup"><span data-stu-id="f97e9-279">Start Postman.</span></span>
* <span data-ttu-id="f97e9-280">Disattivare **SSL certificate verification** (Verifica certificato SSL)</span><span class="sxs-lookup"><span data-stu-id="f97e9-280">Disable **SSL certificate verification**</span></span>
* <span data-ttu-id="f97e9-281">From  **File > Settings** (File > Impostazioni) (scheda \**General* (Generale)), disattivare **SSL certificate verification** (Verifica certificato SSL).</span><span class="sxs-lookup"><span data-stu-id="f97e9-281">From  **File > Settings** (\**General* tab), disable **SSL certificate verification**.</span></span>
    > [!WARNING]
    > <span data-ttu-id="f97e9-282">Riattivare la verifica dei certificati SSL al termine del test del controller.</span><span class="sxs-lookup"><span data-stu-id="f97e9-282">Re-enable SSL certificate verification after testing the controller.</span></span>

<a name="post"></a>

### <a name="test-posttodoitem-with-postman"></a><span data-ttu-id="f97e9-283">Testare PostTodoItem con Postman</span><span class="sxs-lookup"><span data-stu-id="f97e9-283">Test PostTodoItem with Postman</span></span>

* <span data-ttu-id="f97e9-284">Creare una nuova richiesta.</span><span class="sxs-lookup"><span data-stu-id="f97e9-284">Create a new request.</span></span>
* <span data-ttu-id="f97e9-285">Impostare il metodo HTTP su `POST`.</span><span class="sxs-lookup"><span data-stu-id="f97e9-285">Set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="f97e9-286">Fare clic sulla scheda **Body** (Corpo).</span><span class="sxs-lookup"><span data-stu-id="f97e9-286">Select the **Body** tab.</span></span>
* <span data-ttu-id="f97e9-287">Selezionare il pulsante di opzione **raw** (non elaborato).</span><span class="sxs-lookup"><span data-stu-id="f97e9-287">Select the **raw** radio button.</span></span>
* <span data-ttu-id="f97e9-288">Impostare il tipo su **JSON (application/json)** .</span><span class="sxs-lookup"><span data-stu-id="f97e9-288">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="f97e9-289">Nel corpo della richiesta immettere codice JSON per un elemento attività:</span><span class="sxs-lookup"><span data-stu-id="f97e9-289">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="f97e9-290">Selezionare **Send** (Invia).</span><span class="sxs-lookup"><span data-stu-id="f97e9-290">Select **Send**.</span></span>

  ![Postman con richiesta di creazione](first-web-api/_static/3/create.png)

### <a name="test-the-location-header-uri"></a><span data-ttu-id="f97e9-292">Testare l'URI dell'intestazione della posizione</span><span class="sxs-lookup"><span data-stu-id="f97e9-292">Test the location header URI</span></span>

* <span data-ttu-id="f97e9-293">Selezionare la scheda **Headers** (Intestazioni) nel riquadro **Response** (Risposta).</span><span class="sxs-lookup"><span data-stu-id="f97e9-293">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="f97e9-294">Copiare il valore dell'intestazione **Location** (Posizione):</span><span class="sxs-lookup"><span data-stu-id="f97e9-294">Copy the **Location** header value:</span></span>

  ![Scheda Headers (Intestazioni) della console Postman](first-web-api/_static/3/create.png)

* <span data-ttu-id="f97e9-296">Impostare il metodo su GET.</span><span class="sxs-lookup"><span data-stu-id="f97e9-296">Set the method to GET.</span></span>
* <span data-ttu-id="f97e9-297">Incollare l'URI (ad esempio `https://localhost:5001/api/TodoItems/1`)</span><span class="sxs-lookup"><span data-stu-id="f97e9-297">Paste the URI (for example, `https://localhost:5001/api/TodoItems/1`)</span></span>
* <span data-ttu-id="f97e9-298">Selezionare **Send** (Invia).</span><span class="sxs-lookup"><span data-stu-id="f97e9-298">Select **Send**.</span></span>

## <a name="examine-the-get-methods"></a><span data-ttu-id="f97e9-299">Esaminare i metodi GET</span><span class="sxs-lookup"><span data-stu-id="f97e9-299">Examine the GET methods</span></span>

<span data-ttu-id="f97e9-300">Questi metodi implementano due metodi GET:</span><span class="sxs-lookup"><span data-stu-id="f97e9-300">These methods implement two GET endpoints:</span></span>

* `GET /api/TodoItems`
* `GET /api/TodoItems/{id}`

<span data-ttu-id="f97e9-301">Testare l'app chiamando i due endpoint da un browser o da Postman.</span><span class="sxs-lookup"><span data-stu-id="f97e9-301">Test the app by calling the two endpoints from a browser or Postman.</span></span> <span data-ttu-id="f97e9-302">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="f97e9-302">For example:</span></span>

* [https://localhost:5001/api/TodoItems](https://localhost:5001/api/TodoItems)
* [https://localhost:5001/api/TodoItems/1](https://localhost:5001/api/TodoItems/1)

<span data-ttu-id="f97e9-303">Una risposta simile alla seguente viene generata dalla chiamata a `GetTodoItems`:</span><span class="sxs-lookup"><span data-stu-id="f97e9-303">A response similar to the following is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

### <a name="test-get-with-postman"></a><span data-ttu-id="f97e9-304">Testare Get con Postman</span><span class="sxs-lookup"><span data-stu-id="f97e9-304">Test Get with Postman</span></span>

* <span data-ttu-id="f97e9-305">Creare una nuova richiesta.</span><span class="sxs-lookup"><span data-stu-id="f97e9-305">Create a new request.</span></span>
* <span data-ttu-id="f97e9-306">Impostare il metodo HTTP su **GET**.</span><span class="sxs-lookup"><span data-stu-id="f97e9-306">Set the HTTP method to **GET**.</span></span>
* <span data-ttu-id="f97e9-307">Impostare l'URL della richiesta su `https://localhost:<port>/api/TodoItems`.</span><span class="sxs-lookup"><span data-stu-id="f97e9-307">Set the request URL to `https://localhost:<port>/api/TodoItems`.</span></span> <span data-ttu-id="f97e9-308">Ad esempio `https://localhost:5001/api/TodoItems`.</span><span class="sxs-lookup"><span data-stu-id="f97e9-308">For example, `https://localhost:5001/api/TodoItems`.</span></span>
* <span data-ttu-id="f97e9-309">Impostare **Two pane view** (Visualizzazione in due riquadri) in Postman.</span><span class="sxs-lookup"><span data-stu-id="f97e9-309">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="f97e9-310">Selezionare **Send** (Invia).</span><span class="sxs-lookup"><span data-stu-id="f97e9-310">Select **Send**.</span></span>

<span data-ttu-id="f97e9-311">Questa app usa un database in memoria.</span><span class="sxs-lookup"><span data-stu-id="f97e9-311">This app uses an in-memory database.</span></span> <span data-ttu-id="f97e9-312">Se l'app viene arrestata e avviata, la richiesta GET precedente non restituirà alcun dato.</span><span class="sxs-lookup"><span data-stu-id="f97e9-312">If the app is stopped and started, the preceding GET request will not return any data.</span></span> <span data-ttu-id="f97e9-313">Se non vengono restituiti dati, eseguire [POST](#post) per pubblicare i dati nell'app.</span><span class="sxs-lookup"><span data-stu-id="f97e9-313">If no data is returned, [POST](#post) data to the app.</span></span>

## <a name="routing-and-url-paths"></a><span data-ttu-id="f97e9-314">Routing e percorsi di URL</span><span class="sxs-lookup"><span data-stu-id="f97e9-314">Routing and URL paths</span></span>

<span data-ttu-id="f97e9-315">L'attributo [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) indica un metodo che risponde a una richiesta HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="f97e9-315">The [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="f97e9-316">Il percorso dell'URL per ogni metodo viene costruito nel modo seguente:</span><span class="sxs-lookup"><span data-stu-id="f97e9-316">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="f97e9-317">Iniziare con la stringa di modello nell'attributo `Route` del controller:</span><span class="sxs-lookup"><span data-stu-id="f97e9-317">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=TodoController&highlight=1)]

* <span data-ttu-id="f97e9-318">Sostituire `[controller]` con il nome del controller, ovvero, per convenzione, il nome della classe controller meno il suffisso "Controller".</span><span class="sxs-lookup"><span data-stu-id="f97e9-318">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="f97e9-319">In questo esempio il nome della classe controller è **TodoItems**Controller, quindi il nome del controller è "TodoItems".</span><span class="sxs-lookup"><span data-stu-id="f97e9-319">For this sample, the controller class name is **TodoItems**Controller, so the controller name is "TodoItems".</span></span> <span data-ttu-id="f97e9-320">Il [routing](xref:mvc/controllers/routing) ASP.NET Core non fa distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="f97e9-320">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="f97e9-321">Se l'attributo `[HttpGet]` ha un modello di route, ad esempio `[HttpGet("products")]`, aggiungerlo al percorso.</span><span class="sxs-lookup"><span data-stu-id="f97e9-321">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="f97e9-322">In questo esempio non si usa un modello.</span><span class="sxs-lookup"><span data-stu-id="f97e9-322">This sample doesn't use a template.</span></span> <span data-ttu-id="f97e9-323">Per altre informazioni, vedere [Routing con attributi Http[verb]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="f97e9-323">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="f97e9-324">Nel metodo `GetTodoItem` seguente, `"{id}"` è una variabile segnaposto per l'identificatore univoco dell'elemento attività.</span><span class="sxs-lookup"><span data-stu-id="f97e9-324">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="f97e9-325">Quando viene chiamato `GetTodoItem`, il valore di `"{id}"` viene specificato per il metodo nel rispettivo parametro `id`.</span><span class="sxs-lookup"><span data-stu-id="f97e9-325">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its`id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a><span data-ttu-id="f97e9-326">Valori restituiti</span><span class="sxs-lookup"><span data-stu-id="f97e9-326">Return values</span></span>

<span data-ttu-id="f97e9-327">Il tipo restituito dei metodi `GetTodoItems` e `GetTodoItem` è [ActionResult\<T>](xref:web-api/action-return-types#actionresultt-type).</span><span class="sxs-lookup"><span data-stu-id="f97e9-327">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="f97e9-328">ASP.NET Core serializza automaticamente l'oggetto su [JSON](https://www.json.org/) e scrive il codice JSON nel corpo del messaggio di risposta.</span><span class="sxs-lookup"><span data-stu-id="f97e9-328">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="f97e9-329">Il codice di risposta per questo tipo restituito è 200, presupponendo che non esistano eccezioni non gestite.</span><span class="sxs-lookup"><span data-stu-id="f97e9-329">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="f97e9-330">Le eccezioni non gestite vengono convertite in errori 5xx.</span><span class="sxs-lookup"><span data-stu-id="f97e9-330">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="f97e9-331">I tipi restituiti `ActionResult` possono rappresentare un ampio intervallo di codici di stato HTTP.</span><span class="sxs-lookup"><span data-stu-id="f97e9-331">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="f97e9-332">Ad esempio, `GetTodoItem` può restituire due valori di stato diversi:</span><span class="sxs-lookup"><span data-stu-id="f97e9-332">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="f97e9-333">Se nessun elemento corrisponde all'ID richiesto, il metodo restituisce un codice di errore 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound).</span><span class="sxs-lookup"><span data-stu-id="f97e9-333">If no item matches the requested ID, the method returns a 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) error code.</span></span>
* <span data-ttu-id="f97e9-334">In caso contrario, il metodo restituisce il codice 200 con un corpo della risposta JSON.</span><span class="sxs-lookup"><span data-stu-id="f97e9-334">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="f97e9-335">La restituzione di `item` risulta in una risposta HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="f97e9-335">Returning `item` results in an HTTP 200 response.</span></span>

## <a name="the-puttodoitem-method"></a><span data-ttu-id="f97e9-336">Metodo PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="f97e9-336">The PutTodoItem method</span></span>

<span data-ttu-id="f97e9-337">Esaminare il metodo `PutTodoItem`:</span><span class="sxs-lookup"><span data-stu-id="f97e9-337">Examine the `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Update)]

<span data-ttu-id="f97e9-338">`PutTodoItem` è simile a `PostTodoItem` ma usa la richiesta HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="f97e9-338">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="f97e9-339">La risposta è [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="f97e9-339">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="f97e9-340">In base alla specifica HTTP, una richiesta PUT richiede che il client invii l'intera entità aggiornata e non solo le modifiche.</span><span class="sxs-lookup"><span data-stu-id="f97e9-340">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="f97e9-341">Per supportare gli aggiornamenti parziali, usare [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span><span class="sxs-lookup"><span data-stu-id="f97e9-341">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="f97e9-342">Se si riceve un errore chiamando `PutTodoItem`, chiamare `GET` per verificare che il database includa un elemento.</span><span class="sxs-lookup"><span data-stu-id="f97e9-342">If you get an error calling `PutTodoItem`, call `GET` to ensure there's an item in the database.</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="f97e9-343">Testare il metodo PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="f97e9-343">Test the PutTodoItem method</span></span>

<span data-ttu-id="f97e9-344">Questo esempio usa un database in memoria che deve essere inizializzato ogni volta che l'app viene avviata.</span><span class="sxs-lookup"><span data-stu-id="f97e9-344">This sample uses an in-memory database that must be initialed each time the app is started.</span></span> <span data-ttu-id="f97e9-345">Deve esistere un elemento nel database prima di eseguire una chiamata PUT.</span><span class="sxs-lookup"><span data-stu-id="f97e9-345">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="f97e9-346">Chiamare GET per assicurarsi che sia presente un elemento nel database prima di eseguire una chiamata PUT.</span><span class="sxs-lookup"><span data-stu-id="f97e9-346">Call GET to insure there's an item in the database before making a PUT call.</span></span>

<span data-ttu-id="f97e9-347">Aggiornare l'elemento attività con ID = 1 e impostarne il nome su "feed fish":</span><span class="sxs-lookup"><span data-stu-id="f97e9-347">Update the to-do item that has ID = 1 and set its name to "feed fish":</span></span>

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="f97e9-348">L'immagine seguente visualizza l'aggiornamento di Postman:</span><span class="sxs-lookup"><span data-stu-id="f97e9-348">The following image shows the Postman update:</span></span>

![Console Postman con risposta 204 (No Content)](first-web-api/_static/3/pmcput.png)

## <a name="the-deletetodoitem-method"></a><span data-ttu-id="f97e9-350">Metodo DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="f97e9-350">The DeleteTodoItem method</span></span>

<span data-ttu-id="f97e9-351">Esaminare il metodo `DeleteTodoItem`:</span><span class="sxs-lookup"><span data-stu-id="f97e9-351">Examine the `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Delete)]

<span data-ttu-id="f97e9-352">La risposta `DeleteTodoItem` è [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="f97e9-352">The `DeleteTodoItem` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="f97e9-353">Testare il metodo DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="f97e9-353">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="f97e9-354">Usare Postman per eliminare un elemento attività:</span><span class="sxs-lookup"><span data-stu-id="f97e9-354">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="f97e9-355">Impostare il metodo su `DELETE`.</span><span class="sxs-lookup"><span data-stu-id="f97e9-355">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="f97e9-356">Impostare l'URI dell'oggetto da eliminare, ad esempio `https://localhost:5001/api/TodoItems/1`</span><span class="sxs-lookup"><span data-stu-id="f97e9-356">Set the URI of the object to delete, for example `https://localhost:5001/api/TodoItems/1`</span></span>
* <span data-ttu-id="f97e9-357">Selezionare **Send** (Invia).</span><span class="sxs-lookup"><span data-stu-id="f97e9-357">Select **Send**</span></span>

## <a name="call-the-web-api-with-javascript"></a><span data-ttu-id="f97e9-358">Chiamare l'API Web con JavaScript</span><span class="sxs-lookup"><span data-stu-id="f97e9-358">Call the web API with JavaScript</span></span>

<span data-ttu-id="f97e9-359">Vedere [l'esercitazione: Chiamare un'API Web ASP.NET Core con JavaScript](xref:tutorials/web-api-javascript).</span><span class="sxs-lookup"><span data-stu-id="f97e9-359">See [Tutorial: Call an ASP.NET Core web API with JavaScript](xref:tutorials/web-api-javascript).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="f97e9-360">In questa esercitazione si imparerà a:</span><span class="sxs-lookup"><span data-stu-id="f97e9-360">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="f97e9-361">Creare un progetto di API Web.</span><span class="sxs-lookup"><span data-stu-id="f97e9-361">Create a web API project.</span></span>
> * <span data-ttu-id="f97e9-362">Aggiungere una classe modello e un contesto di database.</span><span class="sxs-lookup"><span data-stu-id="f97e9-362">Add a model class and a database context.</span></span>
> * <span data-ttu-id="f97e9-363">Aggiungere un controller.</span><span class="sxs-lookup"><span data-stu-id="f97e9-363">Add a controller.</span></span>
> * <span data-ttu-id="f97e9-364">Aggiungere metodi CRUD.</span><span class="sxs-lookup"><span data-stu-id="f97e9-364">Add CRUD methods.</span></span>
> * <span data-ttu-id="f97e9-365">Configurare routing e percorsi URL.</span><span class="sxs-lookup"><span data-stu-id="f97e9-365">Configure routing and URL paths.</span></span>
> * <span data-ttu-id="f97e9-366">Specificare valori restituiti.</span><span class="sxs-lookup"><span data-stu-id="f97e9-366">Specify return values.</span></span>
> * <span data-ttu-id="f97e9-367">Chiamare l'API Web con Postman.</span><span class="sxs-lookup"><span data-stu-id="f97e9-367">Call the web API with Postman.</span></span>
> * <span data-ttu-id="f97e9-368">Chiamare l'API Web con JavaScript.</span><span class="sxs-lookup"><span data-stu-id="f97e9-368">Call the web API with JavaScript.</span></span>

<span data-ttu-id="f97e9-369">Al termine si dispone di un'API web che può gestire gli elementi di tipo "attività" archiviati in un database relazionale.</span><span class="sxs-lookup"><span data-stu-id="f97e9-369">At the end, you have a web API that can manage "to-do" items stored in a relational database.</span></span>

## <a name="overview"></a><span data-ttu-id="f97e9-370">Panoramica</span><span class="sxs-lookup"><span data-stu-id="f97e9-370">Overview</span></span>

<span data-ttu-id="f97e9-371">Questa esercitazione consente di creare l'API seguente:</span><span class="sxs-lookup"><span data-stu-id="f97e9-371">This tutorial creates the following API:</span></span>

|<span data-ttu-id="f97e9-372">API</span><span class="sxs-lookup"><span data-stu-id="f97e9-372">API</span></span> | <span data-ttu-id="f97e9-373">Descrizione</span><span class="sxs-lookup"><span data-stu-id="f97e9-373">Description</span></span> | <span data-ttu-id="f97e9-374">Corpo della richiesta</span><span class="sxs-lookup"><span data-stu-id="f97e9-374">Request body</span></span> | <span data-ttu-id="f97e9-375">Corpo della risposta</span><span class="sxs-lookup"><span data-stu-id="f97e9-375">Response body</span></span> |
|--- | ---- | ---- | ---- |
|<span data-ttu-id="f97e9-376">GET /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="f97e9-376">GET /api/TodoItems</span></span> | <span data-ttu-id="f97e9-377">Ottiene tutti gli elementi attività</span><span class="sxs-lookup"><span data-stu-id="f97e9-377">Get all to-do items</span></span> | <span data-ttu-id="f97e9-378">nessuno</span><span class="sxs-lookup"><span data-stu-id="f97e9-378">None</span></span> | <span data-ttu-id="f97e9-379">Matrice di elementi attività</span><span class="sxs-lookup"><span data-stu-id="f97e9-379">Array of to-do items</span></span>|
|<span data-ttu-id="f97e9-380">GET /api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="f97e9-380">GET /api/TodoItems/{id}</span></span> | <span data-ttu-id="f97e9-381">Ottiene un elemento in base all'ID</span><span class="sxs-lookup"><span data-stu-id="f97e9-381">Get an item by ID</span></span> | <span data-ttu-id="f97e9-382">nessuno</span><span class="sxs-lookup"><span data-stu-id="f97e9-382">None</span></span> | <span data-ttu-id="f97e9-383">Elemento attività</span><span class="sxs-lookup"><span data-stu-id="f97e9-383">To-do item</span></span>|
|<span data-ttu-id="f97e9-384">POST /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="f97e9-384">POST /api/TodoItems</span></span> | <span data-ttu-id="f97e9-385">Aggiunge un nuovo elemento</span><span class="sxs-lookup"><span data-stu-id="f97e9-385">Add a new item</span></span> | <span data-ttu-id="f97e9-386">Elemento attività</span><span class="sxs-lookup"><span data-stu-id="f97e9-386">To-do item</span></span> | <span data-ttu-id="f97e9-387">Elemento attività</span><span class="sxs-lookup"><span data-stu-id="f97e9-387">To-do item</span></span> |
|<span data-ttu-id="f97e9-388">PUT /api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="f97e9-388">PUT /api/TodoItems/{id}</span></span> | <span data-ttu-id="f97e9-389">Aggiorna un elemento esistente &nbsp;</span><span class="sxs-lookup"><span data-stu-id="f97e9-389">Update an existing item &nbsp;</span></span> | <span data-ttu-id="f97e9-390">Elemento attività</span><span class="sxs-lookup"><span data-stu-id="f97e9-390">To-do item</span></span> | <span data-ttu-id="f97e9-391">nessuno</span><span class="sxs-lookup"><span data-stu-id="f97e9-391">None</span></span> |
|<span data-ttu-id="f97e9-392">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="f97e9-392">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span></span> | <span data-ttu-id="f97e9-393">Elimina un elemento &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="f97e9-393">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="f97e9-394">nessuno</span><span class="sxs-lookup"><span data-stu-id="f97e9-394">None</span></span> | <span data-ttu-id="f97e9-395">nessuno</span><span class="sxs-lookup"><span data-stu-id="f97e9-395">None</span></span>|

<span data-ttu-id="f97e9-396">Il diagramma seguente visualizza la struttura dell'app.</span><span class="sxs-lookup"><span data-stu-id="f97e9-396">The following diagram shows the design of the app.</span></span>

![Il client è rappresentato da una casella a sinistra.](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="f97e9-402">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="f97e9-402">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f97e9-403">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f97e9-403">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f97e9-404">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f97e9-404">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f97e9-405">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="f97e9-405">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="f97e9-406">Creare un progetto Web</span><span class="sxs-lookup"><span data-stu-id="f97e9-406">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f97e9-407">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f97e9-407">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f97e9-408">Scegliere **Nuovo** > **Progetto** dal menu **File**.</span><span class="sxs-lookup"><span data-stu-id="f97e9-408">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="f97e9-409">Selezionare il modello **Applicazione Web ASP.NET Core** e fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="f97e9-409">Select the **ASP.NET Core Web Application** template and click **Next**.</span></span>
* <span data-ttu-id="f97e9-410">Assegnare al progetto il nome *TodoApi* e fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="f97e9-410">Name the project *TodoApi* and click **Create**.</span></span>
* <span data-ttu-id="f97e9-411">Nella finestra di dialogo **Crea una nuova applicazione Web ASP.NET Core** verificare che siano selezionati **.NET Core** e **ASP.NET Core 2.2**.</span><span class="sxs-lookup"><span data-stu-id="f97e9-411">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 2.2** are selected.</span></span> <span data-ttu-id="f97e9-412">Selezionare il modello **API** e fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="f97e9-412">Select the **API** template and click **Create**.</span></span> <span data-ttu-id="f97e9-413">**Non** selezionare **Abilita supporto Docker**.</span><span class="sxs-lookup"><span data-stu-id="f97e9-413">**Don't** select **Enable Docker Support**.</span></span>

![Finestra di dialogo Nuovo progetto di Visual Studio](first-web-api/_static/vs.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f97e9-415">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f97e9-415">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="f97e9-416">Aprire il [terminale integrato](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="f97e9-416">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="f97e9-417">Cambiare directory (`cd`) e passare alla cartella che conterrà la cartella del progetto.</span><span class="sxs-lookup"><span data-stu-id="f97e9-417">Change directories (`cd`) to the folder that will contain the project folder.</span></span>
* <span data-ttu-id="f97e9-418">Eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="f97e9-418">Run the following commands:</span></span>

   ```dotnetcli
   dotnet new webapi -o TodoApi
   code -r TodoApi
   ```

  <span data-ttu-id="f97e9-419">Questi comandi creano un nuovo progetto API Web e aprono una nuova istanza di Visual Studio Code nella nuova cartella di progetto.</span><span class="sxs-lookup"><span data-stu-id="f97e9-419">These commands create a new web API project and open a new instance of Visual Studio Code in the new project folder.</span></span>

* <span data-ttu-id="f97e9-420">Quando una finestra di dialogo chiede se si vuole aggiungere gli asset necessari al progetto, selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="f97e9-420">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f97e9-421">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="f97e9-421">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="f97e9-422">Selezionare **File** > **Nuova soluzione**.</span><span class="sxs-lookup"><span data-stu-id="f97e9-422">Select **File** > **New Solution**.</span></span>

  ![Nuova soluzione macOS](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="f97e9-424">Selezionare **.NET Core** > **App** > **API** > **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="f97e9-424">Select **.NET Core** > **App** > **API** > **Next**.</span></span>

  ![Finestra di dialogo Nuovo progetto di macOS](first-web-api-mac/_static/1.png)
  
* <span data-ttu-id="f97e9-426">Nella finestra di dialogo **Configure your new ASP.NET Core Web API** (Configura la nuova API Web ASP.NET Core), accettare l'impostazione predefinita di **Framework di destinazione**, ovvero \* *.NET Core 2.2*.</span><span class="sxs-lookup"><span data-stu-id="f97e9-426">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of \**.NET Core 2.2*.</span></span>

* <span data-ttu-id="f97e9-427">Immettere *TodoApi* in **Nome progetto**, quindi selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="f97e9-427">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![Finestra di dialogo di configurazione](first-web-api-mac/_static/2.png)

---

### <a name="test-the-api"></a><span data-ttu-id="f97e9-429">Testare l'API</span><span class="sxs-lookup"><span data-stu-id="f97e9-429">Test the API</span></span>

<span data-ttu-id="f97e9-430">Il modello di progetto crea un'API `values`.</span><span class="sxs-lookup"><span data-stu-id="f97e9-430">The project template creates a `values` API.</span></span> <span data-ttu-id="f97e9-431">Chiamare il metodo `Get` da un browser per eseguire il test dell'app.</span><span class="sxs-lookup"><span data-stu-id="f97e9-431">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f97e9-432">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f97e9-432">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="f97e9-433">Premere CTRL+F5 per eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="f97e9-433">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="f97e9-434">Visual Studio apre un browser e naviga all'indirizzo `https://localhost:<port>/api/values`, dove `<port>` è un numero di porta selezionato a caso.</span><span class="sxs-lookup"><span data-stu-id="f97e9-434">Visual Studio launches a browser and navigates to `https://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="f97e9-435">Se in una finestra di dialogo viene chiesto se considerare attendibile il certificato IIS Express, selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="f97e9-435">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="f97e9-436">Nella finestra di dialogo **Avviso di sicurezza** successiva selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="f97e9-436">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f97e9-437">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f97e9-437">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="f97e9-438">Premere CTRL+F5 per eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="f97e9-438">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="f97e9-439">In un browser passare all'URL seguente: [https://localhost:5001/api/values](https://localhost:5001/api/values).</span><span class="sxs-lookup"><span data-stu-id="f97e9-439">In a browser, go to following URL: [https://localhost:5001/api/values](https://localhost:5001/api/values).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f97e9-440">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="f97e9-440">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="f97e9-441">Selezionare **Esegui** > **Avvia debug** per avviare l'app.</span><span class="sxs-lookup"><span data-stu-id="f97e9-441">Select **Run** > **Start Debugging** to launch the app.</span></span> <span data-ttu-id="f97e9-442">Visual Studio per Mac apre un browser e naviga all'indirizzo `https://localhost:<port>`, dove `<port>` è un numero di porta selezionato a caso.</span><span class="sxs-lookup"><span data-stu-id="f97e9-442">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="f97e9-443">Viene restituito un errore HTTP 404 (Non trovato).</span><span class="sxs-lookup"><span data-stu-id="f97e9-443">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="f97e9-444">Accodare `/api/values` all'URL (impostare l'URL su `https://localhost:<port>/api/values`).</span><span class="sxs-lookup"><span data-stu-id="f97e9-444">Append `/api/values` to the URL (change the URL to `https://localhost:<port>/api/values`).</span></span>

---

<span data-ttu-id="f97e9-445">Viene restituito il codice JSON seguente:</span><span class="sxs-lookup"><span data-stu-id="f97e9-445">The following JSON is returned:</span></span>

```json
["value1","value2"]
```

## <a name="add-a-model-class"></a><span data-ttu-id="f97e9-446">Aggiungere una classe del modello</span><span class="sxs-lookup"><span data-stu-id="f97e9-446">Add a model class</span></span>

<span data-ttu-id="f97e9-447">Un *modello* è un set di classi che rappresentano i dati gestiti dall'app.</span><span class="sxs-lookup"><span data-stu-id="f97e9-447">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="f97e9-448">Il modello per questa app è una classe `TodoItem` singola.</span><span class="sxs-lookup"><span data-stu-id="f97e9-448">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f97e9-449">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f97e9-449">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f97e9-450">In **Esplora soluzioni** fare clic con il pulsante destro del mouse sul progetto.</span><span class="sxs-lookup"><span data-stu-id="f97e9-450">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="f97e9-451">Selezionare **Aggiungi** > **Nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="f97e9-451">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="f97e9-452">Assegnare il nome *Models* alla cartella.</span><span class="sxs-lookup"><span data-stu-id="f97e9-452">Name the folder *Models*.</span></span>

* <span data-ttu-id="f97e9-453">Fare clic con il pulsante destro del mouse sulla cartella *Models* e scegliere **Aggiungi** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="f97e9-453">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="f97e9-454">Assegnare alla classe il nome *TodoItem* e selezionare **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="f97e9-454">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="f97e9-455">Sostituire il codice del modello con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="f97e9-455">Replace the template code with the following code:</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f97e9-456">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f97e9-456">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="f97e9-457">Aggiungere una cartella denominata *Models*.</span><span class="sxs-lookup"><span data-stu-id="f97e9-457">Add a folder named *Models*.</span></span>

* <span data-ttu-id="f97e9-458">Aggiungere una classe `TodoItem` alla cartella *Models* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="f97e9-458">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f97e9-459">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="f97e9-459">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="f97e9-460">Fare clic con il pulsante destro del mouse sul progetto.</span><span class="sxs-lookup"><span data-stu-id="f97e9-460">Right-click the project.</span></span> <span data-ttu-id="f97e9-461">Selezionare **Aggiungi** > **Nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="f97e9-461">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="f97e9-462">Assegnare il nome *Models* alla cartella.</span><span class="sxs-lookup"><span data-stu-id="f97e9-462">Name the folder *Models*.</span></span>

  ![Nuova cartella](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="f97e9-464">Fare clic con il pulsante destro del mouse sulla cartella *Models* e scegliere **Aggiungi** > **Nuovo file** > **Generale** > **Classe vuota**.</span><span class="sxs-lookup"><span data-stu-id="f97e9-464">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="f97e9-465">Assegnare alla classe il nome *TodoItem* e fare clic su **Nuovo**.</span><span class="sxs-lookup"><span data-stu-id="f97e9-465">Name the class *TodoItem*, and then click **New**.</span></span>

* <span data-ttu-id="f97e9-466">Sostituire il codice del modello con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="f97e9-466">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="f97e9-467">La proprietà `Id` funziona come chiave univoca in un database relazionale.</span><span class="sxs-lookup"><span data-stu-id="f97e9-467">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="f97e9-468">È possibile inserire le classi del modello in qualsiasi punto del progetto, ma per convenzione viene usata la cartella *Models*.</span><span class="sxs-lookup"><span data-stu-id="f97e9-468">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="f97e9-469">Aggiungere un contesto di database</span><span class="sxs-lookup"><span data-stu-id="f97e9-469">Add a database context</span></span>

<span data-ttu-id="f97e9-470">Il *contesto di database* è la classe principale che coordina le funzionalità di Entity Framework per un modello di dati.</span><span class="sxs-lookup"><span data-stu-id="f97e9-470">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="f97e9-471">Questa classe viene creata mediante derivazione dalla classe `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="f97e9-471">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f97e9-472">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f97e9-472">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f97e9-473">Fare clic con il pulsante destro del mouse sulla cartella *Models* e scegliere **Aggiungi** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="f97e9-473">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="f97e9-474">Assegnare alla classe il nome *TodoContext* e fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="f97e9-474">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="f97e9-475">Visual Studio Code/Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="f97e9-475">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="f97e9-476">Aggiungere una classe `TodoContext` alla cartella *Models*.</span><span class="sxs-lookup"><span data-stu-id="f97e9-476">Add a `TodoContext` class to the *Models* folder.</span></span>

---

* <span data-ttu-id="f97e9-477">Sostituire il codice del modello con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="f97e9-477">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="f97e9-478">Registrare il contesto del database</span><span class="sxs-lookup"><span data-stu-id="f97e9-478">Register the database context</span></span>

<span data-ttu-id="f97e9-479">In ASP.NET Core i servizi come il contesto del database devono essere registrati con il [contenitore di inserimento delle dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="f97e9-479">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="f97e9-480">Il contenitore rende disponibile il servizio ai controller.</span><span class="sxs-lookup"><span data-stu-id="f97e9-480">The container provides the service to controllers.</span></span>

<span data-ttu-id="f97e9-481">Aggiornare *Startup.cs* con il codice evidenziato seguente:</span><span class="sxs-lookup"><span data-stu-id="f97e9-481">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup1.cs?highlight=5,8,25-26&name=snippet_all)]

<span data-ttu-id="f97e9-482">Il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="f97e9-482">The preceding code:</span></span>

* <span data-ttu-id="f97e9-483">Rimuove le dichiarazioni `using` inutilizzate.</span><span class="sxs-lookup"><span data-stu-id="f97e9-483">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="f97e9-484">Aggiunge il contesto del database al contenitore di inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="f97e9-484">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="f97e9-485">Specifica che il contesto del database userà un database in memoria.</span><span class="sxs-lookup"><span data-stu-id="f97e9-485">Specifies that the database context will use an in-memory database.</span></span>

## <a name="add-a-controller"></a><span data-ttu-id="f97e9-486">Aggiungere un controller</span><span class="sxs-lookup"><span data-stu-id="f97e9-486">Add a controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f97e9-487">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f97e9-487">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f97e9-488">Fare clic con il pulsante destro del mouse sulla cartella *Controllers*.</span><span class="sxs-lookup"><span data-stu-id="f97e9-488">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="f97e9-489">Selezionare **Aggiungi** > **Nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="f97e9-489">Select **Add** > **New Item**.</span></span>
* <span data-ttu-id="f97e9-490">Nella finestra di dialogo **Aggiungi nuovo elemento** selezionare il modello **API Controller Class** (Classe controller API).</span><span class="sxs-lookup"><span data-stu-id="f97e9-490">In the **Add New Item** dialog, select the **API Controller Class** template.</span></span>
* <span data-ttu-id="f97e9-491">Assegnare alla classe il nome *TodoController* e selezionare **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="f97e9-491">Name the class *TodoController*, and select **Add**.</span></span>

  ![Finestra di dialogo Aggiungi nuovo elemento con controller nella casella di ricerca e controller API Web selezionato](first-web-api/_static/new_controller.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="f97e9-493">Visual Studio Code/Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="f97e9-493">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="f97e9-494">Nella cartella *Controllers* creare una classe denominata `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="f97e9-494">In the *Controllers* folder, create a class named `TodoController`.</span></span>

---

* <span data-ttu-id="f97e9-495">Sostituire il codice del modello con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="f97e9-495">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="f97e9-496">Il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="f97e9-496">The preceding code:</span></span>

* <span data-ttu-id="f97e9-497">Definisce una classe controller API senza metodi.</span><span class="sxs-lookup"><span data-stu-id="f97e9-497">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="f97e9-498">Contrassegna la classe con l'attributo [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute).</span><span class="sxs-lookup"><span data-stu-id="f97e9-498">Decorates the class with the [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute.</span></span> <span data-ttu-id="f97e9-499">L'attributo indica che il controller risponde alle richieste di API Web.</span><span class="sxs-lookup"><span data-stu-id="f97e9-499">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="f97e9-500">Per informazioni sui comportamenti specifici consentiti dall'attributo, vedere <xref:web-api/index>.</span><span class="sxs-lookup"><span data-stu-id="f97e9-500">For information about specific behaviors that the attribute enables, see <xref:web-api/index>.</span></span>
* <span data-ttu-id="f97e9-501">Usa l'inserimento delle dipendenze per inserire il contesto del database (`TodoContext`) nel controller.</span><span class="sxs-lookup"><span data-stu-id="f97e9-501">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="f97e9-502">Il contesto di database viene usato in ognuno dei metodi [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) nel controller.</span><span class="sxs-lookup"><span data-stu-id="f97e9-502">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>
* <span data-ttu-id="f97e9-503">Aggiunge un elemento denominato `Item1` al database se il database è vuoto.</span><span class="sxs-lookup"><span data-stu-id="f97e9-503">Adds an item named `Item1` to the database if the database is empty.</span></span> <span data-ttu-id="f97e9-504">Questo codice si trova nel costruttore, quindi viene eseguito ogni volta che è presente una nuova richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="f97e9-504">This code is in the constructor, so it runs every time there's a new HTTP request.</span></span> <span data-ttu-id="f97e9-505">Se si eliminano tutti gli elementi, il costruttore crea di nuovo `Item1` quando viene nuovamente chiamato un metodo API.</span><span class="sxs-lookup"><span data-stu-id="f97e9-505">If you delete all items, the constructor creates `Item1` again the next time an API method is called.</span></span> <span data-ttu-id="f97e9-506">Potrebbe pertanto sembrare che l'eliminazione non abbia funzionato, mentre di fatto ha funzionato.</span><span class="sxs-lookup"><span data-stu-id="f97e9-506">So it may look like the deletion didn't work when it actually did work.</span></span>

## <a name="add-get-methods"></a><span data-ttu-id="f97e9-507">Aggiungere metodi Get</span><span class="sxs-lookup"><span data-stu-id="f97e9-507">Add Get methods</span></span>

<span data-ttu-id="f97e9-508">Per specificare un'API che recupera elementi attività, aggiungere i metodi seguenti alla classe `TodoController`:</span><span class="sxs-lookup"><span data-stu-id="f97e9-508">To provide an API that retrieves to-do items, add the following methods to the `TodoController` class:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

<span data-ttu-id="f97e9-509">Questi metodi implementano due metodi GET:</span><span class="sxs-lookup"><span data-stu-id="f97e9-509">These methods implement two GET endpoints:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="f97e9-510">Arrestare l'app se è ancora in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="f97e9-510">Stop the app if it's still running.</span></span> <span data-ttu-id="f97e9-511">Quindi eseguirla di nuovo per includere le modifiche più recenti.</span><span class="sxs-lookup"><span data-stu-id="f97e9-511">Then run it again to include the latest changes.</span></span>

<span data-ttu-id="f97e9-512">Testare l'app chiamando i due endpoint da un browser.</span><span class="sxs-lookup"><span data-stu-id="f97e9-512">Test the app by calling the two endpoints from a browser.</span></span> <span data-ttu-id="f97e9-513">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="f97e9-513">For example:</span></span>

* `https://localhost:<port>/api/todo`
* `https://localhost:<port>/api/todo/1`

<span data-ttu-id="f97e9-514">La chiamata a `GetTodoItems` genera la risposta HTTP seguente:</span><span class="sxs-lookup"><span data-stu-id="f97e9-514">The following HTTP response is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

## <a name="routing-and-url-paths"></a><span data-ttu-id="f97e9-515">Routing e percorsi di URL</span><span class="sxs-lookup"><span data-stu-id="f97e9-515">Routing and URL paths</span></span>

<span data-ttu-id="f97e9-516">L'attributo [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) indica un metodo che risponde a una richiesta HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="f97e9-516">The [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="f97e9-517">Il percorso dell'URL per ogni metodo viene costruito nel modo seguente:</span><span class="sxs-lookup"><span data-stu-id="f97e9-517">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="f97e9-518">Iniziare con la stringa di modello nell'attributo `Route` del controller:</span><span class="sxs-lookup"><span data-stu-id="f97e9-518">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

* <span data-ttu-id="f97e9-519">Sostituire `[controller]` con il nome del controller, ovvero, per convenzione, il nome della classe controller meno il suffisso "Controller".</span><span class="sxs-lookup"><span data-stu-id="f97e9-519">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="f97e9-520">In questo esempio il nome della classe controller è **Todo**Controller, pertanto il nome del controller è "todo".</span><span class="sxs-lookup"><span data-stu-id="f97e9-520">For this sample, the controller class name is **Todo**Controller, so the controller name is "todo".</span></span> <span data-ttu-id="f97e9-521">Il [routing](xref:mvc/controllers/routing) ASP.NET Core non fa distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="f97e9-521">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="f97e9-522">Se l'attributo `[HttpGet]` ha un modello di route, ad esempio `[HttpGet("products")]`, aggiungerlo al percorso.</span><span class="sxs-lookup"><span data-stu-id="f97e9-522">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="f97e9-523">In questo esempio non si usa un modello.</span><span class="sxs-lookup"><span data-stu-id="f97e9-523">This sample doesn't use a template.</span></span> <span data-ttu-id="f97e9-524">Per altre informazioni, vedere [Routing con attributi Http[verb]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="f97e9-524">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="f97e9-525">Nel metodo `GetTodoItem` seguente, `"{id}"` è una variabile segnaposto per l'identificatore univoco dell'elemento attività.</span><span class="sxs-lookup"><span data-stu-id="f97e9-525">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="f97e9-526">Quando viene chiamato `GetTodoItem`, il valore di `"{id}"` viene specificato per il metodo nel rispettivo parametro `id`.</span><span class="sxs-lookup"><span data-stu-id="f97e9-526">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its`id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a><span data-ttu-id="f97e9-527">Valori restituiti</span><span class="sxs-lookup"><span data-stu-id="f97e9-527">Return values</span></span>

<span data-ttu-id="f97e9-528">Il tipo restituito dei metodi `GetTodoItems` e `GetTodoItem` è [ActionResult\<T>](xref:web-api/action-return-types#actionresultt-type).</span><span class="sxs-lookup"><span data-stu-id="f97e9-528">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="f97e9-529">ASP.NET Core serializza automaticamente l'oggetto su [JSON](https://www.json.org/) e scrive il codice JSON nel corpo del messaggio di risposta.</span><span class="sxs-lookup"><span data-stu-id="f97e9-529">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="f97e9-530">Il codice di risposta per questo tipo restituito è 200, presupponendo che non esistano eccezioni non gestite.</span><span class="sxs-lookup"><span data-stu-id="f97e9-530">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="f97e9-531">Le eccezioni non gestite vengono convertite in errori 5xx.</span><span class="sxs-lookup"><span data-stu-id="f97e9-531">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="f97e9-532">I tipi restituiti `ActionResult` possono rappresentare un ampio intervallo di codici di stato HTTP.</span><span class="sxs-lookup"><span data-stu-id="f97e9-532">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="f97e9-533">Ad esempio, `GetTodoItem` può restituire due valori di stato diversi:</span><span class="sxs-lookup"><span data-stu-id="f97e9-533">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="f97e9-534">Se nessun elemento corrisponde all'ID richiesto, il metodo restituisce un codice di errore 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound).</span><span class="sxs-lookup"><span data-stu-id="f97e9-534">If no item matches the requested ID, the method returns a 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) error code.</span></span>
* <span data-ttu-id="f97e9-535">In caso contrario, il metodo restituisce il codice 200 con un corpo della risposta JSON.</span><span class="sxs-lookup"><span data-stu-id="f97e9-535">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="f97e9-536">La restituzione di `item` risulta in una risposta HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="f97e9-536">Returning `item` results in an HTTP 200 response.</span></span>

## <a name="test-the-gettodoitems-method"></a><span data-ttu-id="f97e9-537">Testare il metodo GetTodoItems</span><span class="sxs-lookup"><span data-stu-id="f97e9-537">Test the GetTodoItems method</span></span>

<span data-ttu-id="f97e9-538">Questa esercitazione usa Postman per testare l'API Web.</span><span class="sxs-lookup"><span data-stu-id="f97e9-538">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="f97e9-539">Installare [Postman](https://www.getpostman.com/downloads/)</span><span class="sxs-lookup"><span data-stu-id="f97e9-539">Install [Postman](https://www.getpostman.com/downloads/)</span></span>
* <span data-ttu-id="f97e9-540">Avviare l'app Web.</span><span class="sxs-lookup"><span data-stu-id="f97e9-540">Start the web app.</span></span>
* <span data-ttu-id="f97e9-541">Avviare Postman.</span><span class="sxs-lookup"><span data-stu-id="f97e9-541">Start Postman.</span></span>
* <span data-ttu-id="f97e9-542">Disattivare **SSL certificate verification** (Verifica certificato SSL)</span><span class="sxs-lookup"><span data-stu-id="f97e9-542">Disable **SSL certificate verification**</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f97e9-543">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f97e9-543">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f97e9-544">Da **File** > **Settings** (Impostazioni) (scheda **General** (Generale)) disabilitare **SSL certificate verification** (Verifica certificato SSL).</span><span class="sxs-lookup"><span data-stu-id="f97e9-544">From **File** > **Settings** (**General** tab), disable **SSL certificate verification**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="f97e9-545">Visual Studio Code/Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="f97e9-545">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="f97e9-546">Da **Postman**  >  **Preferences** (Preferenze) (scheda **General** (Generale)) disabilitare **SSL certificate verification** (Verifica certificato SSL).</span><span class="sxs-lookup"><span data-stu-id="f97e9-546">From **Postman** > **Preferences** (**General** tab), disable **SSL certificate verification**.</span></span> <span data-ttu-id="f97e9-547">In alternativa, selezionare la chiave inglese e selezionare **Settings** (Impostazioni), quindi disabilitare la verifica del certificato SSL.</span><span class="sxs-lookup"><span data-stu-id="f97e9-547">Alternatively, select the wrench and select **Settings**, then disable the SSL certificate verification.</span></span>

---
  
> [!WARNING]
> <span data-ttu-id="f97e9-548">Riattivare la verifica dei certificati SSL al termine del test del controller.</span><span class="sxs-lookup"><span data-stu-id="f97e9-548">Re-enable SSL certificate verification after testing the controller.</span></span>

* <span data-ttu-id="f97e9-549">Creare una nuova richiesta.</span><span class="sxs-lookup"><span data-stu-id="f97e9-549">Create a new request.</span></span>
  * <span data-ttu-id="f97e9-550">Impostare il metodo HTTP su **GET**.</span><span class="sxs-lookup"><span data-stu-id="f97e9-550">Set the HTTP method to **GET**.</span></span>
  * <span data-ttu-id="f97e9-551">Impostare l'URL della richiesta su `https://localhost:<port>/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="f97e9-551">Set the request URL to `https://localhost:<port>/api/todo`.</span></span> <span data-ttu-id="f97e9-552">Ad esempio `https://localhost:5001/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="f97e9-552">For example, `https://localhost:5001/api/todo`.</span></span>
* <span data-ttu-id="f97e9-553">Impostare **Two pane view** (Visualizzazione in due riquadri) in Postman.</span><span class="sxs-lookup"><span data-stu-id="f97e9-553">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="f97e9-554">Selezionare **Send** (Invia).</span><span class="sxs-lookup"><span data-stu-id="f97e9-554">Select **Send**.</span></span>

![Postman con richiesta GET](first-web-api/_static/2pv.png)

## <a name="add-a-create-method"></a><span data-ttu-id="f97e9-556">Aggiungere un metodo Create</span><span class="sxs-lookup"><span data-stu-id="f97e9-556">Add a Create method</span></span>

<span data-ttu-id="f97e9-557">Aggiungere il metodo `PostTodoItem` seguente in *Controllers/TodoController.cs*:</span><span class="sxs-lookup"><span data-stu-id="f97e9-557">Add the following `PostTodoItem` method inside of *Controllers/TodoController.cs*:</span></span> 

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="f97e9-558">Il codice precedente è un metodo HTTP POST, come indicato dall'attributo [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute).</span><span class="sxs-lookup"><span data-stu-id="f97e9-558">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="f97e9-559">Il metodo ottiene il valore dell'elemento attività dal corpo della richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="f97e9-559">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="f97e9-560">Il metodo `CreatedAtAction`:</span><span class="sxs-lookup"><span data-stu-id="f97e9-560">The `CreatedAtAction` method:</span></span>

* <span data-ttu-id="f97e9-561">Restituisce un codice di stato HTTP 201 in caso di esito positivo.</span><span class="sxs-lookup"><span data-stu-id="f97e9-561">Returns an HTTP 201 status code, if successful.</span></span> <span data-ttu-id="f97e9-562">HTTP 201 è la risposta standard per un metodo HTTP POST che crea una nuova risorsa nel server.</span><span class="sxs-lookup"><span data-stu-id="f97e9-562">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="f97e9-563">Aggiunge un'intestazione `Location` alla risposta.</span><span class="sxs-lookup"><span data-stu-id="f97e9-563">Adds a `Location` header to the response.</span></span> <span data-ttu-id="f97e9-564">L'intestazione `Location` specifica l'URI dell'elemento attività appena creato.</span><span class="sxs-lookup"><span data-stu-id="f97e9-564">The `Location` header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="f97e9-565">Per altre informazioni, vedere [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="f97e9-565">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="f97e9-566">Fa riferimento all'azione `GetTodoItem` per creare l'URI dell'intestazione `Location`.</span><span class="sxs-lookup"><span data-stu-id="f97e9-566">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="f97e9-567">La parola chiave `nameof` C# viene usata per evitare di impostare il nome dell'azione come hardcoded nella chiamata a `CreatedAtAction`.</span><span class="sxs-lookup"><span data-stu-id="f97e9-567">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

### <a name="test-the-posttodoitem-method"></a><span data-ttu-id="f97e9-568">Testare il metodo PostTodoItem</span><span class="sxs-lookup"><span data-stu-id="f97e9-568">Test the PostTodoItem method</span></span>

* <span data-ttu-id="f97e9-569">Compilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="f97e9-569">Build the project.</span></span>
* <span data-ttu-id="f97e9-570">In Postman impostare il metodo HTTP su `POST`.</span><span class="sxs-lookup"><span data-stu-id="f97e9-570">In Postman, set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="f97e9-571">Fare clic sulla scheda **Body** (Corpo).</span><span class="sxs-lookup"><span data-stu-id="f97e9-571">Select the **Body** tab.</span></span>
* <span data-ttu-id="f97e9-572">Selezionare il pulsante di opzione **raw** (non elaborato).</span><span class="sxs-lookup"><span data-stu-id="f97e9-572">Select the **raw** radio button.</span></span>
* <span data-ttu-id="f97e9-573">Impostare il tipo su **JSON (application/json)** .</span><span class="sxs-lookup"><span data-stu-id="f97e9-573">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="f97e9-574">Nel corpo della richiesta immettere codice JSON per un elemento attività:</span><span class="sxs-lookup"><span data-stu-id="f97e9-574">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="f97e9-575">Selezionare **Send** (Invia).</span><span class="sxs-lookup"><span data-stu-id="f97e9-575">Select **Send**.</span></span>

  ![Postman con richiesta di creazione](first-web-api/_static/create.png)

  <span data-ttu-id="f97e9-577">Se viene visualizzato un errore HTTP 405 - Metodo non consentito, è probabile che il progetto non sia stato compilato dopo l'aggiunta del metodo `PostTodoItem`.</span><span class="sxs-lookup"><span data-stu-id="f97e9-577">If you get a 405 Method Not Allowed error, it's probably the result of not compiling the project after adding the `PostTodoItem` method.</span></span>

### <a name="test-the-location-header-uri"></a><span data-ttu-id="f97e9-578">Testare l'URI dell'intestazione della posizione</span><span class="sxs-lookup"><span data-stu-id="f97e9-578">Test the location header URI</span></span>

* <span data-ttu-id="f97e9-579">Selezionare la scheda **Headers** (Intestazioni) nel riquadro **Response** (Risposta).</span><span class="sxs-lookup"><span data-stu-id="f97e9-579">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="f97e9-580">Copiare il valore dell'intestazione **Location** (Posizione):</span><span class="sxs-lookup"><span data-stu-id="f97e9-580">Copy the **Location** header value:</span></span>

  ![Scheda Headers (Intestazioni) della console Postman](first-web-api/_static/pmc2.png)

* <span data-ttu-id="f97e9-582">Impostare il metodo su GET.</span><span class="sxs-lookup"><span data-stu-id="f97e9-582">Set the method to GET.</span></span>
* <span data-ttu-id="f97e9-583">Incollare l'URI (ad esempio `https://localhost:5001/api/Todo/2`)</span><span class="sxs-lookup"><span data-stu-id="f97e9-583">Paste the URI (for example, `https://localhost:5001/api/Todo/2`)</span></span>
* <span data-ttu-id="f97e9-584">Selezionare **Send** (Invia).</span><span class="sxs-lookup"><span data-stu-id="f97e9-584">Select **Send**.</span></span>

## <a name="add-a-puttodoitem-method"></a><span data-ttu-id="f97e9-585">Aggiungere un metodo PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="f97e9-585">Add a PutTodoItem method</span></span>

<span data-ttu-id="f97e9-586">Aggiungere il metodo `PutTodoItem` seguente:</span><span class="sxs-lookup"><span data-stu-id="f97e9-586">Add the following `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

<span data-ttu-id="f97e9-587">`PutTodoItem` è simile a `PostTodoItem` ma usa la richiesta HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="f97e9-587">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="f97e9-588">La risposta è [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="f97e9-588">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="f97e9-589">In base alla specifica HTTP, una richiesta PUT richiede che il client invii l'intera entità aggiornata e non solo le modifiche.</span><span class="sxs-lookup"><span data-stu-id="f97e9-589">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="f97e9-590">Per supportare gli aggiornamenti parziali, usare [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span><span class="sxs-lookup"><span data-stu-id="f97e9-590">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="f97e9-591">Se si riceve un errore chiamando `PutTodoItem`, chiamare `GET` per verificare che il database includa un elemento.</span><span class="sxs-lookup"><span data-stu-id="f97e9-591">If you get an error calling `PutTodoItem`, call `GET` to ensure there's an item in the database.</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="f97e9-592">Testare il metodo PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="f97e9-592">Test the PutTodoItem method</span></span>

<span data-ttu-id="f97e9-593">Questo esempio usa un database in memoria che deve essere inizializzato ogni volta che l'app viene avviata.</span><span class="sxs-lookup"><span data-stu-id="f97e9-593">This sample uses an in-memory database that must be initialed each time the app is started.</span></span> <span data-ttu-id="f97e9-594">Deve esistere un elemento nel database prima di eseguire una chiamata PUT.</span><span class="sxs-lookup"><span data-stu-id="f97e9-594">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="f97e9-595">Chiamare GET per assicurarsi che sia presente un elemento nel database prima di eseguire una chiamata PUT.</span><span class="sxs-lookup"><span data-stu-id="f97e9-595">Call GET to insure there's an item in the database before making a PUT call.</span></span>

<span data-ttu-id="f97e9-596">Aggiornare l'elemento attività con id = 1 e impostarne il nome su "feed fish":</span><span class="sxs-lookup"><span data-stu-id="f97e9-596">Update the to-do item that has id = 1 and set its name to "feed fish":</span></span>

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="f97e9-597">L'immagine seguente visualizza l'aggiornamento di Postman:</span><span class="sxs-lookup"><span data-stu-id="f97e9-597">The following image shows the Postman update:</span></span>

![Console Postman con risposta 204 (No Content)](first-web-api/_static/pmcput.png)

## <a name="add-a-deletetodoitem-method"></a><span data-ttu-id="f97e9-599">Aggiungere un metodo DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="f97e9-599">Add a DeleteTodoItem method</span></span>

<span data-ttu-id="f97e9-600">Aggiungere il metodo `DeleteTodoItem` seguente:</span><span class="sxs-lookup"><span data-stu-id="f97e9-600">Add the following `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="f97e9-601">La risposta `DeleteTodoItem` è [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="f97e9-601">The `DeleteTodoItem` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="f97e9-602">Testare il metodo DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="f97e9-602">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="f97e9-603">Usare Postman per eliminare un elemento attività:</span><span class="sxs-lookup"><span data-stu-id="f97e9-603">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="f97e9-604">Impostare il metodo su `DELETE`.</span><span class="sxs-lookup"><span data-stu-id="f97e9-604">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="f97e9-605">Impostare l'URI dell'oggetto da eliminare, ad esempio `https://localhost:5001/api/todo/1`</span><span class="sxs-lookup"><span data-stu-id="f97e9-605">Set the URI of the object to delete, for example `https://localhost:5001/api/todo/1`</span></span>
* <span data-ttu-id="f97e9-606">Selezionare **Send** (Invia).</span><span class="sxs-lookup"><span data-stu-id="f97e9-606">Select **Send**</span></span>

<span data-ttu-id="f97e9-607">L'app di esempio consente di eliminare tutti gli elementi.</span><span class="sxs-lookup"><span data-stu-id="f97e9-607">The sample app allows you to delete all the items.</span></span> <span data-ttu-id="f97e9-608">Quando viene eliminato l'ultimo elemento, tuttavia, ne viene creato uno nuovo dal costruttore della classe modello alla successiva chiamata dell'API.</span><span class="sxs-lookup"><span data-stu-id="f97e9-608">However, when the last item is deleted, a new one is created by the model class constructor the next time the API is called.</span></span>

## <a name="call-the-web-api-with-javascript"></a><span data-ttu-id="f97e9-609">Chiamare l'API Web con JavaScript</span><span class="sxs-lookup"><span data-stu-id="f97e9-609">Call the web API with JavaScript</span></span>

<span data-ttu-id="f97e9-610">In questa sezione viene aggiunta una pagina HTML che usa JavaScript per chiamare l'API Web.</span><span class="sxs-lookup"><span data-stu-id="f97e9-610">In this section, an HTML page is added that uses JavaScript to call the web API.</span></span> <span data-ttu-id="f97e9-611">L'API Fetch avvia la richiesta.</span><span class="sxs-lookup"><span data-stu-id="f97e9-611">The Fetch API initiates the request.</span></span> <span data-ttu-id="f97e9-612">JavaScript aggiorna la pagina con i dettagli della risposta dell'API Web.</span><span class="sxs-lookup"><span data-stu-id="f97e9-612">JavaScript updates the page with the details from the web API's response.</span></span>

<span data-ttu-id="f97e9-613">Configurare l'app per [servire file statici](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) e [abilitare il mapping del file predefinito](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) aggiornando *Startup.cs* con il codice evidenziato seguente:</span><span class="sxs-lookup"><span data-stu-id="f97e9-613">Configure the app to [serve static files](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [enable default file mapping](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) by updating *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup.cs?highlight=14-15&name=snippet_configure)]

<span data-ttu-id="f97e9-614">Creare una cartella *wwwroot* nella directory del progetto.</span><span class="sxs-lookup"><span data-stu-id="f97e9-614">Create a *wwwroot* folder in the project directory.</span></span>

<span data-ttu-id="f97e9-615">Aggiungere un file HTML denominato *index.html* alla directory *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="f97e9-615">Add an HTML file named *index.html* to the *wwwroot* directory.</span></span> <span data-ttu-id="f97e9-616">Sostituire il contenuto con il markup seguente:</span><span class="sxs-lookup"><span data-stu-id="f97e9-616">Replace its contents with the following markup:</span></span>

[!code-html[](first-web-api/samples/2.2/TodoApi/wwwroot/index.html)]

<span data-ttu-id="f97e9-617">Aggiungere un file JavaScript con nome *site.js* alla directory *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="f97e9-617">Add a JavaScript file named *site.js* to the *wwwroot* directory.</span></span> <span data-ttu-id="f97e9-618">Sostituire il contenuto con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="f97e9-618">Replace its contents with the following code:</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_SiteJs)]

<span data-ttu-id="f97e9-619">Può essere necessario modificare le impostazioni di avvio del progetto ASP.NET Core per il test della pagina HTML in locale:</span><span class="sxs-lookup"><span data-stu-id="f97e9-619">A change to the ASP.NET Core project's launch settings may be required to test the HTML page locally:</span></span>

* <span data-ttu-id="f97e9-620">Aprire *Properties\launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="f97e9-620">Open *Properties\launchSettings.json*.</span></span>
* <span data-ttu-id="f97e9-621">Rimuovere la proprietà `launchUrl` per forzare l'app ad aprire *index.html*, il file predefinito del progetto.</span><span class="sxs-lookup"><span data-stu-id="f97e9-621">Remove the `launchUrl` property to force the app to open at *index.html*&mdash;the project's default file.</span></span>

<span data-ttu-id="f97e9-622">In questo esempio vengono chiamati tutti i metodi CRUD dell'API Web.</span><span class="sxs-lookup"><span data-stu-id="f97e9-622">This sample calls all of the CRUD methods of the web API.</span></span> <span data-ttu-id="f97e9-623">Di seguito sono incluse le spiegazioni delle chiamate all'API.</span><span class="sxs-lookup"><span data-stu-id="f97e9-623">Following are explanations of the calls to the API.</span></span>

### <a name="get-a-list-of-to-do-items"></a><span data-ttu-id="f97e9-624">Ottenere un elenco di elementi attività</span><span class="sxs-lookup"><span data-stu-id="f97e9-624">Get a list of to-do items</span></span>

<span data-ttu-id="f97e9-625">Fetch invia una richiesta HTTP GET all'API Web, la quale restituisce codice JSON che rappresenta una matrice di elementi attività.</span><span class="sxs-lookup"><span data-stu-id="f97e9-625">Fetch sends an HTTP GET request to the web API, which returns JSON representing an array of to-do items.</span></span> <span data-ttu-id="f97e9-626">La funzione di callback `success` viene chiamata se la richiesta ha esito positivo.</span><span class="sxs-lookup"><span data-stu-id="f97e9-626">The `success` callback function is invoked if the request succeeds.</span></span> <span data-ttu-id="f97e9-627">Nel callback il modello DOM viene aggiornato con le informazioni dell'elemento attività.</span><span class="sxs-lookup"><span data-stu-id="f97e9-627">In the callback, the DOM is updated with the to-do information.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_GetData)]

### <a name="add-a-to-do-item"></a><span data-ttu-id="f97e9-628">Aggiungere un elemento attività</span><span class="sxs-lookup"><span data-stu-id="f97e9-628">Add a to-do item</span></span>

<span data-ttu-id="f97e9-629">Fetch invia una richiesta HTTP POST con l'elemento attività nel corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="f97e9-629">Fetch sends an HTTP POST request with the to-do item in the request body.</span></span> <span data-ttu-id="f97e9-630">Le opzioni `accepts` e `contentType` sono impostate su `application/json` per specificare il tipo di supporto ricevuto e inviato.</span><span class="sxs-lookup"><span data-stu-id="f97e9-630">The `accepts` and `contentType` options are set to `application/json` to specify the media type being received and sent.</span></span> <span data-ttu-id="f97e9-631">L'elemento attività viene convertito in JSON usando [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span><span class="sxs-lookup"><span data-stu-id="f97e9-631">The to-do item is converted to JSON by using [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span></span> <span data-ttu-id="f97e9-632">Quando l'API restituisce un codice di stato di esecuzione riuscita, viene chiamata la funzione `getData` per aggiornare la tabella HTML.</span><span class="sxs-lookup"><span data-stu-id="f97e9-632">When the API returns a successful status code, the `getData` function is invoked to update the HTML table.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AddItem)]

### <a name="update-a-to-do-item"></a><span data-ttu-id="f97e9-633">Aggiornare un elemento attività</span><span class="sxs-lookup"><span data-stu-id="f97e9-633">Update a to-do item</span></span>

<span data-ttu-id="f97e9-634">L'aggiornamento di un elemento attività è simile all'aggiunta di un elemento di questo tipo.</span><span class="sxs-lookup"><span data-stu-id="f97e9-634">Updating a to-do item is similar to adding one.</span></span> <span data-ttu-id="f97e9-635">L'`url` cambia per includere l'identificatore univoco dell'elemento e `type` è `PUT`.</span><span class="sxs-lookup"><span data-stu-id="f97e9-635">The `url` changes to add the unique identifier of the item, and the `type` is `PUT`.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxPut)]

### <a name="delete-a-to-do-item"></a><span data-ttu-id="f97e9-636">Eliminare un elemento attività</span><span class="sxs-lookup"><span data-stu-id="f97e9-636">Delete a to-do item</span></span>

<span data-ttu-id="f97e9-637">È possibile eliminare un elemento attività impostando `type` su `DELETE` nella chiamata AJAX e specificando l'identificatore univoco dell'elemento nell'URL.</span><span class="sxs-lookup"><span data-stu-id="f97e9-637">Deleting a to-do item is accomplished by setting the `type` on the AJAX call to `DELETE` and specifying the item's unique identifier in the URL.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxDelete)]

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="f97e9-638">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="f97e9-638">Additional resources</span></span>

<span data-ttu-id="f97e9-639">[Visualizzare o scaricare il codice di esempio per questa esercitazione](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span><span class="sxs-lookup"><span data-stu-id="f97e9-639">[View or download sample code for this tutorial](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span></span> <span data-ttu-id="f97e9-640">Vedere [come scaricare un esempio](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="f97e9-640">See [how to download](xref:index#how-to-download-a-sample).</span></span>

<span data-ttu-id="f97e9-641">Per altre informazioni, vedere le seguenti risorse:</span><span class="sxs-lookup"><span data-stu-id="f97e9-641">For more information, see the following resources:</span></span>

* <xref:web-api/index>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:data/ef-rp/intro>
* <xref:mvc/controllers/routing>
* <xref:web-api/action-return-types>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/index>
* [<span data-ttu-id="f97e9-642">Versione YouTube dell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="f97e9-642">YouTube version of this tutorial</span></span>](https://www.youtube.com/watch?v=TTkhEyGBfAk)
