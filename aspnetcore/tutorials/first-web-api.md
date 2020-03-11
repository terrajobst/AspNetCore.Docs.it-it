---
title: "Esercitazione: creare un'API Web con ASP.NET Core"
author: rick-anderson
description: Informazioni su come creare un'API Web con ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 2/25/2020
uid: tutorials/first-web-api
ms.openlocfilehash: 55dfc05b5c96f7fa060d537745bac969e92daa9b
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78655589"
---
# <a name="tutorial-create-a-web-api-with-aspnet-core"></a><span data-ttu-id="de640-103">Esercitazione: creare un'API Web con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="de640-103">Tutorial: Create a web API with ASP.NET Core</span></span>

<span data-ttu-id="de640-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT), [Kirk Larkin](https://twitter.com/serpent5)e [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="de640-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Kirk Larkin](https://twitter.com/serpent5),  and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="de640-105">Questa esercitazione illustra le nozioni di base della creazione di un'API Web con ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="de640-105">This tutorial teaches the basics of building a web API with ASP.NET Core.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="de640-106">In questa esercitazione verranno illustrate le procedure per:</span><span class="sxs-lookup"><span data-stu-id="de640-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="de640-107">Creare un progetto di API Web.</span><span class="sxs-lookup"><span data-stu-id="de640-107">Create a web API project.</span></span>
> * <span data-ttu-id="de640-108">Aggiungere una classe modello e un contesto di database.</span><span class="sxs-lookup"><span data-stu-id="de640-108">Add a model class and a database context.</span></span>
> * <span data-ttu-id="de640-109">Eseguire lo scaffolding di un controller con i metodi CRUD.</span><span class="sxs-lookup"><span data-stu-id="de640-109">Scaffold a controller with CRUD methods.</span></span>
> * <span data-ttu-id="de640-110">Configurare il routing, i percorsi URL e i valori restituiti.</span><span class="sxs-lookup"><span data-stu-id="de640-110">Configure routing, URL paths, and return values.</span></span>
> * <span data-ttu-id="de640-111">Chiamare l'API Web con Postman.</span><span class="sxs-lookup"><span data-stu-id="de640-111">Call the web API with Postman.</span></span>

<span data-ttu-id="de640-112">Al termine si avrà un'API Web che può gestire gli elementi di tipo "attività" archiviati in un database.</span><span class="sxs-lookup"><span data-stu-id="de640-112">At the end, you have a web API that can manage "to-do" items stored in a database.</span></span>

## <a name="overview"></a><span data-ttu-id="de640-113">Panoramica</span><span class="sxs-lookup"><span data-stu-id="de640-113">Overview</span></span>

<span data-ttu-id="de640-114">Questa esercitazione consente di creare l'API seguente:</span><span class="sxs-lookup"><span data-stu-id="de640-114">This tutorial creates the following API:</span></span>

|<span data-ttu-id="de640-115">API</span><span class="sxs-lookup"><span data-stu-id="de640-115">API</span></span> | <span data-ttu-id="de640-116">Descrizione</span><span class="sxs-lookup"><span data-stu-id="de640-116">Description</span></span> | <span data-ttu-id="de640-117">Corpo della richiesta</span><span class="sxs-lookup"><span data-stu-id="de640-117">Request body</span></span> | <span data-ttu-id="de640-118">Corpo della risposta</span><span class="sxs-lookup"><span data-stu-id="de640-118">Response body</span></span> |
|--- | ---- | ---- | ---- |
|<span data-ttu-id="de640-119">GET /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="de640-119">GET /api/TodoItems</span></span> | <span data-ttu-id="de640-120">Ottiene tutti gli elementi attività</span><span class="sxs-lookup"><span data-stu-id="de640-120">Get all to-do items</span></span> | <span data-ttu-id="de640-121">nessuno</span><span class="sxs-lookup"><span data-stu-id="de640-121">None</span></span> | <span data-ttu-id="de640-122">Matrice di elementi attività</span><span class="sxs-lookup"><span data-stu-id="de640-122">Array of to-do items</span></span>|
|<span data-ttu-id="de640-123">GET /api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="de640-123">GET /api/TodoItems/{id}</span></span> | <span data-ttu-id="de640-124">Ottiene un elemento in base all'ID</span><span class="sxs-lookup"><span data-stu-id="de640-124">Get an item by ID</span></span> | <span data-ttu-id="de640-125">nessuno</span><span class="sxs-lookup"><span data-stu-id="de640-125">None</span></span> | <span data-ttu-id="de640-126">Elemento attività</span><span class="sxs-lookup"><span data-stu-id="de640-126">To-do item</span></span>|
|<span data-ttu-id="de640-127">POST /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="de640-127">POST /api/TodoItems</span></span> | <span data-ttu-id="de640-128">Aggiunge un nuovo elemento</span><span class="sxs-lookup"><span data-stu-id="de640-128">Add a new item</span></span> | <span data-ttu-id="de640-129">Elemento attività</span><span class="sxs-lookup"><span data-stu-id="de640-129">To-do item</span></span> | <span data-ttu-id="de640-130">Elemento attività</span><span class="sxs-lookup"><span data-stu-id="de640-130">To-do item</span></span> |
|<span data-ttu-id="de640-131">PUT /api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="de640-131">PUT /api/TodoItems/{id}</span></span> | <span data-ttu-id="de640-132">Aggiorna un elemento esistente &nbsp;</span><span class="sxs-lookup"><span data-stu-id="de640-132">Update an existing item &nbsp;</span></span> | <span data-ttu-id="de640-133">Elemento attività</span><span class="sxs-lookup"><span data-stu-id="de640-133">To-do item</span></span> | <span data-ttu-id="de640-134">nessuno</span><span class="sxs-lookup"><span data-stu-id="de640-134">None</span></span> |
|<span data-ttu-id="de640-135">Elimina &nbsp;/api/TodoItems/{id} &nbsp;</span><span class="sxs-lookup"><span data-stu-id="de640-135">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span></span> | <span data-ttu-id="de640-136">Eliminare un elemento &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="de640-136">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="de640-137">nessuno</span><span class="sxs-lookup"><span data-stu-id="de640-137">None</span></span> | <span data-ttu-id="de640-138">nessuno</span><span class="sxs-lookup"><span data-stu-id="de640-138">None</span></span>|

<span data-ttu-id="de640-139">Il diagramma seguente visualizza la struttura dell'app.</span><span class="sxs-lookup"><span data-stu-id="de640-139">The following diagram shows the design of the app.</span></span>

![Il client è rappresentato da una casella a sinistra.](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="de640-145">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="de640-145">Prerequisites</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="de640-146">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="de640-146">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.1.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="de640-147">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="de640-147">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.1.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="de640-148">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="de640-148">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.1.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="de640-149">Creare un progetto Web</span><span class="sxs-lookup"><span data-stu-id="de640-149">Create a web project</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="de640-150">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="de640-150">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="de640-151">Scegliere **nuovo** > **progetto**dal menu **file** .</span><span class="sxs-lookup"><span data-stu-id="de640-151">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="de640-152">Selezionare il modello **Applicazione Web ASP.NET Core** e fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="de640-152">Select the **ASP.NET Core Web Application** template and click **Next**.</span></span>
* <span data-ttu-id="de640-153">Assegnare al progetto il nome *TodoApi* e fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="de640-153">Name the project *TodoApi* and click **Create**.</span></span>
* <span data-ttu-id="de640-154">Nella finestra di dialogo **Crea una nuova applicazione Web ASP.NET Core** verificare che siano selezionati **.net Core** e **ASP.NET Core 3,1** .</span><span class="sxs-lookup"><span data-stu-id="de640-154">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 3.1** are selected.</span></span> <span data-ttu-id="de640-155">Selezionare il modello **API** e fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="de640-155">Select the **API** template and click **Create**.</span></span>

![Finestra di dialogo Nuovo progetto di Visual Studio](first-web-api/_static/vs3.png)

# <a name="visual-studio-code"></a>[<span data-ttu-id="de640-157">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="de640-157">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="de640-158">Aprire il [terminale integrato](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="de640-158">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="de640-159">Cambiare directory (`cd`) e passare alla cartella che conterrà la cartella del progetto.</span><span class="sxs-lookup"><span data-stu-id="de640-159">Change directories (`cd`) to the folder that will contain the project folder.</span></span>
* <span data-ttu-id="de640-160">Eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="de640-160">Run the following commands:</span></span>

   ```dotnetcli
   dotnet new webapi -o TodoApi
   cd TodoApi
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.EntityFrameworkCore.InMemory
   code -r ../TodoApi
   ```

* <span data-ttu-id="de640-161">Quando una finestra di dialogo chiede se si vuole aggiungere gli asset necessari al progetto, selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="de640-161">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

  <span data-ttu-id="de640-162">I comandi precedenti:</span><span class="sxs-lookup"><span data-stu-id="de640-162">The preceding commands:</span></span>

  * <span data-ttu-id="de640-163">Crea un nuovo progetto API Web e lo apre in Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="de640-163">Creates a new web API project and opens it in Visual Studio Code.</span></span>
  * <span data-ttu-id="de640-164">Aggiunge i pacchetti NuGet necessari nella sezione successiva.</span><span class="sxs-lookup"><span data-stu-id="de640-164">Adds the NuGet packages which are required in the next section.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="de640-165">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="de640-165">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="de640-166">Selezionare **File** > **nuova soluzione**.</span><span class="sxs-lookup"><span data-stu-id="de640-166">Select **File** > **New Solution**.</span></span>

  ![Nuova soluzione macOS](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="de640-168">Selezionare **.NET Core** > **App** > **API** > **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="de640-168">Select **.NET Core** > **App** > **API** > **Next**.</span></span>

  ![Finestra di dialogo Nuovo progetto di macOS](first-web-api-mac/_static/1.png)
  
* <span data-ttu-id="de640-170">Nella finestra di dialogo **Configura nuova ASP.NET Core API Web** selezionare **Framework di destinazione** di \* *.NET Core 3,1*.</span><span class="sxs-lookup"><span data-stu-id="de640-170">In the **Configure your new ASP.NET Core Web API** dialog, select **Target Framework** of \**.NET Core 3.1*.</span></span>

* <span data-ttu-id="de640-171">Immettere *TodoApi* in **Nome progetto**, quindi selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="de640-171">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![Finestra di dialogo di configurazione](first-web-api-mac/_static/2.png)

[!INCLUDE[](~/includes/mac-terminal-access.md)]

<span data-ttu-id="de640-173">Aprire un terminale di comando nella cartella di progetto ed eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="de640-173">Open a command terminal in the project folder and run the following commands:</span></span>

   ```dotnetcli
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.EntityFrameworkCore.InMemory
   ```

---

### <a name="test-the-api"></a><span data-ttu-id="de640-174">Testare l'API</span><span class="sxs-lookup"><span data-stu-id="de640-174">Test the API</span></span>

<span data-ttu-id="de640-175">Il modello di progetto crea un'API `WeatherForecast`.</span><span class="sxs-lookup"><span data-stu-id="de640-175">The project template creates a `WeatherForecast` API.</span></span> <span data-ttu-id="de640-176">Chiamare il metodo `Get` da un browser per eseguire il test dell'app.</span><span class="sxs-lookup"><span data-stu-id="de640-176">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="de640-177">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="de640-177">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="de640-178">Premere CTRL+F5 per eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="de640-178">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="de640-179">Visual Studio apre un browser e naviga all'indirizzo `https://localhost:<port>/WeatherForecast`, dove `<port>` è un numero di porta selezionato a caso.</span><span class="sxs-lookup"><span data-stu-id="de640-179">Visual Studio launches a browser and navigates to `https://localhost:<port>/WeatherForecast`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="de640-180">Se in una finestra di dialogo viene chiesto se considerare attendibile il certificato IIS Express, selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="de640-180">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="de640-181">Nella finestra di dialogo **Avviso di sicurezza** successiva selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="de640-181">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="de640-182">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="de640-182">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="de640-183">Premere CTRL+F5 per eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="de640-183">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="de640-184">In un browser passare all'URL seguente: [https://localhost:5001/WeatherForecast](https://localhost:5001/WeatherForecast).</span><span class="sxs-lookup"><span data-stu-id="de640-184">In a browser, go to following URL: [https://localhost:5001/WeatherForecast](https://localhost:5001/WeatherForecast).</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="de640-185">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="de640-185">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="de640-186">Selezionare **Esegui** > **Avvia debug** per avviare l'app.</span><span class="sxs-lookup"><span data-stu-id="de640-186">Select **Run** > **Start Debugging** to launch the app.</span></span> <span data-ttu-id="de640-187">Visual Studio per Mac apre un browser e naviga all'indirizzo `https://localhost:<port>`, dove `<port>` è un numero di porta selezionato a caso.</span><span class="sxs-lookup"><span data-stu-id="de640-187">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="de640-188">Viene restituito un errore HTTP 404 (Non trovato).</span><span class="sxs-lookup"><span data-stu-id="de640-188">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="de640-189">Accodare `/WeatherForecast` all'URL (impostare l'URL su `https://localhost:<port>/WeatherForecast`).</span><span class="sxs-lookup"><span data-stu-id="de640-189">Append `/WeatherForecast` to the URL (change the URL to `https://localhost:<port>/WeatherForecast`).</span></span>

---

<span data-ttu-id="de640-190">Viene restituito un codice JSON simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="de640-190">JSON similar to the following is returned:</span></span>

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

## <a name="add-a-model-class"></a><span data-ttu-id="de640-191">Aggiungere una classe modello</span><span class="sxs-lookup"><span data-stu-id="de640-191">Add a model class</span></span>

<span data-ttu-id="de640-192">Un *modello* è un set di classi che rappresentano i dati gestiti dall'app.</span><span class="sxs-lookup"><span data-stu-id="de640-192">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="de640-193">Il modello per questa app è una classe `TodoItem` singola.</span><span class="sxs-lookup"><span data-stu-id="de640-193">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="de640-194">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="de640-194">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="de640-195">In **Esplora soluzioni** fare clic con il pulsante destro del mouse sul progetto.</span><span class="sxs-lookup"><span data-stu-id="de640-195">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="de640-196">Selezionare **Aggiungi** > **Nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="de640-196">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="de640-197">Assegnare il nome *Models* alla cartella.</span><span class="sxs-lookup"><span data-stu-id="de640-197">Name the folder *Models*.</span></span>

* <span data-ttu-id="de640-198">Fare clic con il pulsante destro del mouse sulla cartella *Models* e scegliere **Aggiungi** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="de640-198">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="de640-199">Assegnare alla classe il nome *TodoItem* e selezionare **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="de640-199">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="de640-200">Sostituire il codice del modello con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="de640-200">Replace the template code with the following code:</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="de640-201">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="de640-201">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="de640-202">Aggiungere una cartella denominata *Models*.</span><span class="sxs-lookup"><span data-stu-id="de640-202">Add a folder named *Models*.</span></span>

* <span data-ttu-id="de640-203">Aggiungere una classe `TodoItem` alla cartella *Models* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="de640-203">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="de640-204">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="de640-204">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="de640-205">Fare clic con il pulsante destro del mouse sul progetto.</span><span class="sxs-lookup"><span data-stu-id="de640-205">Right-click the project.</span></span> <span data-ttu-id="de640-206">Selezionare **Aggiungi** > **Nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="de640-206">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="de640-207">Assegnare il nome *Models* alla cartella.</span><span class="sxs-lookup"><span data-stu-id="de640-207">Name the folder *Models*.</span></span>

  ![Nuova cartella](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="de640-209">Fare clic con il pulsante destro del mouse sulla cartella *Models* e scegliere **Aggiungi** > **nuovo file** > **generale** > **classe vuota**.</span><span class="sxs-lookup"><span data-stu-id="de640-209">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="de640-210">Assegnare alla classe il nome *TodoItem* e fare clic su **Nuovo**.</span><span class="sxs-lookup"><span data-stu-id="de640-210">Name the class *TodoItem*, and then click **New**.</span></span>

* <span data-ttu-id="de640-211">Sostituire il codice del modello con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="de640-211">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoItem.cs?name=snippet)]

<span data-ttu-id="de640-212">La proprietà `Id` funziona come chiave univoca in un database relazionale.</span><span class="sxs-lookup"><span data-stu-id="de640-212">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="de640-213">È possibile inserire le classi del modello in qualsiasi punto del progetto, ma per convenzione viene usata la cartella *Models*.</span><span class="sxs-lookup"><span data-stu-id="de640-213">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="de640-214">Aggiungere un contesto di database</span><span class="sxs-lookup"><span data-stu-id="de640-214">Add a database context</span></span>

<span data-ttu-id="de640-215">Il *contesto di database* è la classe principale che coordina le funzionalità di Entity Framework per un modello di dati.</span><span class="sxs-lookup"><span data-stu-id="de640-215">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="de640-216">Questa classe viene creata mediante derivazione dalla classe `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="de640-216">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="de640-217">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="de640-217">Visual Studio</span></span>](#tab/visual-studio)

### <a name="add-microsoftentityframeworkcoresqlserver"></a><span data-ttu-id="de640-218">Aggiungere Microsoft.EntityFrameworkCore.SqlServer</span><span class="sxs-lookup"><span data-stu-id="de640-218">Add Microsoft.EntityFrameworkCore.SqlServer</span></span>

* <span data-ttu-id="de640-219">Dal menu **Strumenti** selezionare **Gestione pacchetti NuGet > Gestisci pacchetti NuGet per la soluzione**.</span><span class="sxs-lookup"><span data-stu-id="de640-219">From the **Tools** menu, select **NuGet Package Manager > Manage NuGet Packages for Solution**.</span></span>
* <span data-ttu-id="de640-220">Selezionare la scheda **Esplora** e quindi immettere **Microsoft.EntityFrameworkCore.SqlServer** nella casella di ricerca.</span><span class="sxs-lookup"><span data-stu-id="de640-220">Select the **Browse** tab, and then enter **Microsoft.EntityFrameworkCore.SqlServer** in the search box.</span></span>
* <span data-ttu-id="de640-221">Nel riquadro sinistro selezionare **Microsoft. EntityFrameworkCore. SqlServer** .</span><span class="sxs-lookup"><span data-stu-id="de640-221">Select **Microsoft.EntityFrameworkCore.SqlServer** in the left pane.</span></span>
* <span data-ttu-id="de640-222">Selezionare la casella di controllo **Progetto** nel riquadro di destra e quindi selezionare **Installa**.</span><span class="sxs-lookup"><span data-stu-id="de640-222">Select the **Project** check box in the right pane and then select **Install**.</span></span>
* <span data-ttu-id="de640-223">Usare le istruzioni precedenti per aggiungere il pacchetto NuGet `Microsoft.EntityFrameworkCore.InMemory`.</span><span class="sxs-lookup"><span data-stu-id="de640-223">Use the preceding instructions to add the `Microsoft.EntityFrameworkCore.InMemory` NuGet package.</span></span>

![Gestione pacchetti NuGet](first-web-api/_static/vs3NuGet.png)

## <a name="add-the-todocontext-database-context"></a><span data-ttu-id="de640-225">Aggiungere il contesto del database TodoContext</span><span class="sxs-lookup"><span data-stu-id="de640-225">Add the TodoContext database context</span></span>

* <span data-ttu-id="de640-226">Fare clic con il pulsante destro del mouse sulla cartella *Models* e scegliere **Aggiungi** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="de640-226">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="de640-227">Assegnare alla classe il nome *TodoContext* e fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="de640-227">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mac"></a>[<span data-ttu-id="de640-228">Visual Studio Code/Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="de640-228">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="de640-229">Aggiungere una classe `TodoContext` alla cartella *Models*.</span><span class="sxs-lookup"><span data-stu-id="de640-229">Add a `TodoContext` class to the *Models* folder.</span></span>

---

* <span data-ttu-id="de640-230">Immettere il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="de640-230">Enter the following code:</span></span>

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="de640-231">Registrare il contesto del database</span><span class="sxs-lookup"><span data-stu-id="de640-231">Register the database context</span></span>

<span data-ttu-id="de640-232">In ASP.NET Core i servizi come il contesto del database devono essere registrati con il [contenitore di inserimento delle dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="de640-232">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="de640-233">Il contenitore rende disponibile il servizio ai controller.</span><span class="sxs-lookup"><span data-stu-id="de640-233">The container provides the service to controllers.</span></span>

<span data-ttu-id="de640-234">Aggiornare *Startup.cs* con il codice evidenziato seguente:</span><span class="sxs-lookup"><span data-stu-id="de640-234">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Startup.cs?highlight=7-8,23-24&name=snippet_all)]

<span data-ttu-id="de640-235">Il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="de640-235">The preceding code:</span></span>

* <span data-ttu-id="de640-236">Rimuove le dichiarazioni `using` inutilizzate.</span><span class="sxs-lookup"><span data-stu-id="de640-236">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="de640-237">Aggiunge il contesto del database al contenitore di inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="de640-237">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="de640-238">Specifica che il contesto del database userà un database in memoria.</span><span class="sxs-lookup"><span data-stu-id="de640-238">Specifies that the database context will use an in-memory database.</span></span>

## <a name="scaffold-a-controller"></a><span data-ttu-id="de640-239">Eseguire lo scaffolding di un controller</span><span class="sxs-lookup"><span data-stu-id="de640-239">Scaffold a controller</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="de640-240">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="de640-240">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="de640-241">Fare clic con il pulsante destro del mouse sulla cartella *Controllers*.</span><span class="sxs-lookup"><span data-stu-id="de640-241">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="de640-242">Selezionare **aggiungi** > **nuovo elemento con impalcatura**.</span><span class="sxs-lookup"><span data-stu-id="de640-242">Select **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="de640-243">Selezionare **Controller API con azioni, che usa Entity Framework** e quindi selezionare **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="de640-243">Select **API Controller with actions, using Entity Framework**, and then select **Add**.</span></span>
* <span data-ttu-id="de640-244">Nella finestra di dialogo **Add API Controller with actions, using Entity Framework** (Aggiungi controller API con azioni, che usa Entity Framework):</span><span class="sxs-lookup"><span data-stu-id="de640-244">In the **Add API Controller with actions, using Entity Framework** dialog:</span></span>

  * <span data-ttu-id="de640-245">Selezionare **TodoItem (TodoApi. Models)** nella **classe del modello**.</span><span class="sxs-lookup"><span data-stu-id="de640-245">Select **TodoItem (TodoApi.Models)** in the **Model class**.</span></span>
  * <span data-ttu-id="de640-246">Selezionare **TodoContext (TodoApi. Models)** nella **classe del contesto dati**.</span><span class="sxs-lookup"><span data-stu-id="de640-246">Select **TodoContext (TodoApi.Models)** in the **Data context class**.</span></span>
  * <span data-ttu-id="de640-247">Selezionare **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="de640-247">Select **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mac"></a>[<span data-ttu-id="de640-248">Visual Studio Code/Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="de640-248">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="de640-249">Eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="de640-249">Run the following commands:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet tool install --global dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator controller -name TodoItemsController -async -api -m TodoItem -dc TodoContext -outDir Controllers
```

<span data-ttu-id="de640-250">I comandi precedenti:</span><span class="sxs-lookup"><span data-stu-id="de640-250">The preceding commands:</span></span>

* <span data-ttu-id="de640-251">Aggiungere i pacchetti NuGet necessari per lo scaffolding.</span><span class="sxs-lookup"><span data-stu-id="de640-251">Add NuGet packages required for scaffolding.</span></span>
* <span data-ttu-id="de640-252">Installa il motore di scaffolding (`dotnet-aspnet-codegenerator`).</span><span class="sxs-lookup"><span data-stu-id="de640-252">Installs the scaffolding engine (`dotnet-aspnet-codegenerator`).</span></span>
* <span data-ttu-id="de640-253">Esegue lo scaffolding di `TodoItemsController`.</span><span class="sxs-lookup"><span data-stu-id="de640-253">Scaffolds the `TodoItemsController`.</span></span>

---

<span data-ttu-id="de640-254">Il codice generato:</span><span class="sxs-lookup"><span data-stu-id="de640-254">The generated code:</span></span>

* <span data-ttu-id="de640-255">Contrassegna la classe con l'attributo [`[ApiController]`](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) .</span><span class="sxs-lookup"><span data-stu-id="de640-255">Marks the class with the [`[ApiController]`](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute.</span></span> <span data-ttu-id="de640-256">L'attributo indica che il controller risponde alle richieste di API Web.</span><span class="sxs-lookup"><span data-stu-id="de640-256">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="de640-257">Per informazioni sui comportamenti specifici consentiti dall'attributo, vedere <xref:web-api/index>.</span><span class="sxs-lookup"><span data-stu-id="de640-257">For information about specific behaviors that the attribute enables, see <xref:web-api/index>.</span></span>
* <span data-ttu-id="de640-258">Usa l'inserimento delle dipendenze per inserire il contesto del database (`TodoContext`) nel controller.</span><span class="sxs-lookup"><span data-stu-id="de640-258">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="de640-259">Il contesto di database viene usato in ognuno dei metodi [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) nel controller.</span><span class="sxs-lookup"><span data-stu-id="de640-259">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

<span data-ttu-id="de640-260">I modelli di ASP.NET Core per:</span><span class="sxs-lookup"><span data-stu-id="de640-260">The ASP.NET Core templates for:</span></span>

* <span data-ttu-id="de640-261">I controller con viste includono `[action]` nel modello di route.</span><span class="sxs-lookup"><span data-stu-id="de640-261">Controllers with views include `[action]` in the route template.</span></span>
* <span data-ttu-id="de640-262">I controller API non includono `[action]` nel modello di route.</span><span class="sxs-lookup"><span data-stu-id="de640-262">API controllers don't include `[action]` in the route template.</span></span>

<span data-ttu-id="de640-263">Quando il token `[action]` non è presente nel modello di route, il nome dell' [azione](xref:mvc/controllers/routing#action) viene escluso dalla route.</span><span class="sxs-lookup"><span data-stu-id="de640-263">When the `[action]` token isn't in the route template, the [action](xref:mvc/controllers/routing#action) name is excluded from the route.</span></span> <span data-ttu-id="de640-264">Ovvero, il nome del metodo associato all'azione non viene usato nella route corrispondente.</span><span class="sxs-lookup"><span data-stu-id="de640-264">That is, the action's associated method name isn't used in the matching route.</span></span>

## <a name="examine-the-posttodoitem-create-method"></a><span data-ttu-id="de640-265">Esaminare il metodo di creazione PostTodoItem</span><span class="sxs-lookup"><span data-stu-id="de640-265">Examine the PostTodoItem create method</span></span>

<span data-ttu-id="de640-266">Sostituire l'istruzione return in `PostTodoItem` per usare l'operatore [nameof](/dotnet/csharp/language-reference/operators/nameof):</span><span class="sxs-lookup"><span data-stu-id="de640-266">Replace the return statement in the `PostTodoItem` to use the [nameof](/dotnet/csharp/language-reference/operators/nameof) operator:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Create)]

<span data-ttu-id="de640-267">Il codice precedente è un metodo HTTP POST, come indicato dall'attributo [`[HttpPost]`](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) .</span><span class="sxs-lookup"><span data-stu-id="de640-267">The preceding code is an HTTP POST method, as indicated by the [`[HttpPost]`](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="de640-268">Il metodo ottiene il valore dell'elemento attività dal corpo della richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="de640-268">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="de640-269">Il metodo <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*>:</span><span class="sxs-lookup"><span data-stu-id="de640-269">The <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> method:</span></span>

* <span data-ttu-id="de640-270">Restituisce un codice di stato HTTP 201 in caso di esito positivo.</span><span class="sxs-lookup"><span data-stu-id="de640-270">Returns an HTTP 201 status code if successful.</span></span> <span data-ttu-id="de640-271">HTTP 201 è la risposta standard per un metodo HTTP POST che crea una nuova risorsa nel server.</span><span class="sxs-lookup"><span data-stu-id="de640-271">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="de640-272">Aggiunge un'intestazione [Location](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) alla risposta.</span><span class="sxs-lookup"><span data-stu-id="de640-272">Adds a [Location](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) header to the response.</span></span> <span data-ttu-id="de640-273">L'intestazione `Location` specifica l'[URI](https://developer.mozilla.org/docs/Glossary/URI) dell'elemento attività appena creato.</span><span class="sxs-lookup"><span data-stu-id="de640-273">The `Location` header specifies the [URI](https://developer.mozilla.org/docs/Glossary/URI) of the newly created to-do item.</span></span> <span data-ttu-id="de640-274">Per altre informazioni, vedere [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="de640-274">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="de640-275">Fa riferimento all'azione `GetTodoItem` per creare l'URI dell'intestazione `Location`.</span><span class="sxs-lookup"><span data-stu-id="de640-275">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="de640-276">La parola chiave `nameof` C# viene usata per evitare di impostare il nome dell'azione come hardcoded nella chiamata a `CreatedAtAction`.</span><span class="sxs-lookup"><span data-stu-id="de640-276">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

### <a name="install-postman"></a><span data-ttu-id="de640-277">Installare Postman</span><span class="sxs-lookup"><span data-stu-id="de640-277">Install Postman</span></span>

<span data-ttu-id="de640-278">Questa esercitazione usa Postman per testare l'API Web.</span><span class="sxs-lookup"><span data-stu-id="de640-278">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="de640-279">Installare [Postman](https://www.getpostman.com/downloads/)</span><span class="sxs-lookup"><span data-stu-id="de640-279">Install [Postman](https://www.getpostman.com/downloads/)</span></span>
* <span data-ttu-id="de640-280">Avviare l'app Web.</span><span class="sxs-lookup"><span data-stu-id="de640-280">Start the web app.</span></span>
* <span data-ttu-id="de640-281">Avviare Postman.</span><span class="sxs-lookup"><span data-stu-id="de640-281">Start Postman.</span></span>
* <span data-ttu-id="de640-282">Disattivare **SSL certificate verification** (Verifica certificato SSL)</span><span class="sxs-lookup"><span data-stu-id="de640-282">Disable **SSL certificate verification**</span></span>
  * <span data-ttu-id="de640-283">In **File** > **Impostazioni** (scheda**generale** ) disabilitare la **Verifica del certificato SSL**.</span><span class="sxs-lookup"><span data-stu-id="de640-283">From **File** > **Settings** (**General** tab), disable **SSL certificate verification**.</span></span>
    > [!WARNING]
    > <span data-ttu-id="de640-284">Riattivare la verifica dei certificati SSL al termine del test del controller.</span><span class="sxs-lookup"><span data-stu-id="de640-284">Re-enable SSL certificate verification after testing the controller.</span></span>

<a name="post"></a>

### <a name="test-posttodoitem-with-postman"></a><span data-ttu-id="de640-285">Testare PostTodoItem con Postman</span><span class="sxs-lookup"><span data-stu-id="de640-285">Test PostTodoItem with Postman</span></span>

* <span data-ttu-id="de640-286">Creare una nuova richiesta.</span><span class="sxs-lookup"><span data-stu-id="de640-286">Create a new request.</span></span>
* <span data-ttu-id="de640-287">Impostare il metodo HTTP su `POST`.</span><span class="sxs-lookup"><span data-stu-id="de640-287">Set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="de640-288">Selezionare la scheda **Corpo**.</span><span class="sxs-lookup"><span data-stu-id="de640-288">Select the **Body** tab.</span></span>
* <span data-ttu-id="de640-289">Selezionare il pulsante di opzione **raw** (non elaborato).</span><span class="sxs-lookup"><span data-stu-id="de640-289">Select the **raw** radio button.</span></span>
* <span data-ttu-id="de640-290">Impostare il tipo su **JSON (application/json)** .</span><span class="sxs-lookup"><span data-stu-id="de640-290">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="de640-291">Nel corpo della richiesta immettere codice JSON per un elemento attività:</span><span class="sxs-lookup"><span data-stu-id="de640-291">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="de640-292">Selezionare **Send** (Invia).</span><span class="sxs-lookup"><span data-stu-id="de640-292">Select **Send**.</span></span>

  ![Postman con richiesta di creazione](first-web-api/_static/3/create.png)

### <a name="test-the-location-header-uri"></a><span data-ttu-id="de640-294">Testare l'URI dell'intestazione della posizione</span><span class="sxs-lookup"><span data-stu-id="de640-294">Test the location header URI</span></span>

* <span data-ttu-id="de640-295">Selezionare la scheda **Headers** (Intestazioni) nel riquadro **Response** (Risposta).</span><span class="sxs-lookup"><span data-stu-id="de640-295">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="de640-296">Copiare il valore dell'intestazione **Location** (Posizione):</span><span class="sxs-lookup"><span data-stu-id="de640-296">Copy the **Location** header value:</span></span>

  ![Scheda Headers (Intestazioni) della console Postman](first-web-api/_static/3/create.png)

* <span data-ttu-id="de640-298">Impostare il metodo su GET.</span><span class="sxs-lookup"><span data-stu-id="de640-298">Set the method to GET.</span></span>
* <span data-ttu-id="de640-299">Incollare l'URI, ad esempio `https://localhost:5001/api/TodoItems/1`.</span><span class="sxs-lookup"><span data-stu-id="de640-299">Paste the URI (for example, `https://localhost:5001/api/TodoItems/1`).</span></span>
* <span data-ttu-id="de640-300">Selezionare **Send** (Invia).</span><span class="sxs-lookup"><span data-stu-id="de640-300">Select **Send**.</span></span>

## <a name="examine-the-get-methods"></a><span data-ttu-id="de640-301">Esaminare i metodi GET</span><span class="sxs-lookup"><span data-stu-id="de640-301">Examine the GET methods</span></span>

<span data-ttu-id="de640-302">Questi metodi implementano due metodi GET:</span><span class="sxs-lookup"><span data-stu-id="de640-302">These methods implement two GET endpoints:</span></span>

* `GET /api/TodoItems`
* `GET /api/TodoItems/{id}`

<span data-ttu-id="de640-303">Testare l'app chiamando i due endpoint da un browser o da Postman.</span><span class="sxs-lookup"><span data-stu-id="de640-303">Test the app by calling the two endpoints from a browser or Postman.</span></span> <span data-ttu-id="de640-304">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="de640-304">For example:</span></span>

* [https://localhost:5001/api/TodoItems](https://localhost:5001/api/TodoItems)
* [https://localhost:5001/api/TodoItems/1](https://localhost:5001/api/TodoItems/1)

<span data-ttu-id="de640-305">Una risposta simile alla seguente viene generata dalla chiamata a `GetTodoItems`:</span><span class="sxs-lookup"><span data-stu-id="de640-305">A response similar to the following is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

### <a name="test-get-with-postman"></a><span data-ttu-id="de640-306">Testare Get con Postman</span><span class="sxs-lookup"><span data-stu-id="de640-306">Test Get with Postman</span></span>

* <span data-ttu-id="de640-307">Creare una nuova richiesta.</span><span class="sxs-lookup"><span data-stu-id="de640-307">Create a new request.</span></span>
* <span data-ttu-id="de640-308">Impostare il metodo HTTP su **GET**.</span><span class="sxs-lookup"><span data-stu-id="de640-308">Set the HTTP method to **GET**.</span></span>
* <span data-ttu-id="de640-309">Impostare l'URL della richiesta su `https://localhost:<port>/api/TodoItems`.</span><span class="sxs-lookup"><span data-stu-id="de640-309">Set the request URL to `https://localhost:<port>/api/TodoItems`.</span></span> <span data-ttu-id="de640-310">Ad esempio: `https://localhost:5001/api/TodoItems`.</span><span class="sxs-lookup"><span data-stu-id="de640-310">For example, `https://localhost:5001/api/TodoItems`.</span></span>
* <span data-ttu-id="de640-311">Impostare **Two pane view** (Visualizzazione in due riquadri) in Postman.</span><span class="sxs-lookup"><span data-stu-id="de640-311">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="de640-312">Selezionare **Send** (Invia).</span><span class="sxs-lookup"><span data-stu-id="de640-312">Select **Send**.</span></span>

<span data-ttu-id="de640-313">Questa app usa un database in memoria.</span><span class="sxs-lookup"><span data-stu-id="de640-313">This app uses an in-memory database.</span></span> <span data-ttu-id="de640-314">Se l'app viene arrestata e avviata, la richiesta GET precedente non restituirà alcun dato.</span><span class="sxs-lookup"><span data-stu-id="de640-314">If the app is stopped and started, the preceding GET request will not return any data.</span></span> <span data-ttu-id="de640-315">Se non vengono restituiti dati, eseguire [POST](#post) per pubblicare i dati nell'app.</span><span class="sxs-lookup"><span data-stu-id="de640-315">If no data is returned, [POST](#post) data to the app.</span></span>

## <a name="routing-and-url-paths"></a><span data-ttu-id="de640-316">Routing e percorsi di URL</span><span class="sxs-lookup"><span data-stu-id="de640-316">Routing and URL paths</span></span>

<span data-ttu-id="de640-317">L'attributo [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) indica un metodo che risponde a una richiesta HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="de640-317">The [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="de640-318">Il percorso dell'URL per ogni metodo viene costruito nel modo seguente:</span><span class="sxs-lookup"><span data-stu-id="de640-318">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="de640-319">Iniziare con la stringa di modello nell'attributo `Route` del controller:</span><span class="sxs-lookup"><span data-stu-id="de640-319">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=TodoController&highlight=1)]

* <span data-ttu-id="de640-320">Sostituire `[controller]` con il nome del controller, ovvero, per convenzione, il nome della classe controller meno il suffisso "Controller".</span><span class="sxs-lookup"><span data-stu-id="de640-320">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="de640-321">In questo esempio il nome della classe controller è **TodoItems**Controller, quindi il nome del controller è "TodoItems".</span><span class="sxs-lookup"><span data-stu-id="de640-321">For this sample, the controller class name is **TodoItems**Controller, so the controller name is "TodoItems".</span></span> <span data-ttu-id="de640-322">Il [routing](xref:mvc/controllers/routing) ASP.NET Core non fa distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="de640-322">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="de640-323">Se l'attributo `[HttpGet]` ha un modello di route, ad esempio `[HttpGet("products")]`, aggiungerlo al percorso.</span><span class="sxs-lookup"><span data-stu-id="de640-323">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="de640-324">In questo esempio non si usa un modello.</span><span class="sxs-lookup"><span data-stu-id="de640-324">This sample doesn't use a template.</span></span> <span data-ttu-id="de640-325">Per altre informazioni, vedere [Routing con attributi Http[verb]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="de640-325">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="de640-326">Nel metodo `GetTodoItem` seguente, `"{id}"` è una variabile segnaposto per l'identificatore univoco dell'elemento attività.</span><span class="sxs-lookup"><span data-stu-id="de640-326">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="de640-327">Quando viene richiamato `GetTodoItem`, il valore di `"{id}"` nell'URL viene fornito al metodo nel parametro `id`.</span><span class="sxs-lookup"><span data-stu-id="de640-327">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its `id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a><span data-ttu-id="de640-328">Valori restituiti</span><span class="sxs-lookup"><span data-stu-id="de640-328">Return values</span></span>

<span data-ttu-id="de640-329">Il tipo restituito dei metodi `GetTodoItems` e `GetTodoItem` è [ActionResult\<T>](xref:web-api/action-return-types#actionresultt-type).</span><span class="sxs-lookup"><span data-stu-id="de640-329">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="de640-330">ASP.NET Core serializza automaticamente l'oggetto su [JSON](https://www.json.org/) e scrive il codice JSON nel corpo del messaggio di risposta.</span><span class="sxs-lookup"><span data-stu-id="de640-330">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="de640-331">Il codice di risposta per questo tipo restituito è 200, presupponendo che non esistano eccezioni non gestite.</span><span class="sxs-lookup"><span data-stu-id="de640-331">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="de640-332">Le eccezioni non gestite vengono convertite in errori 5xx.</span><span class="sxs-lookup"><span data-stu-id="de640-332">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="de640-333">I tipi restituiti `ActionResult` possono rappresentare un ampio intervallo di codici di stato HTTP.</span><span class="sxs-lookup"><span data-stu-id="de640-333">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="de640-334">Ad esempio, `GetTodoItem` può restituire due valori di stato diversi:</span><span class="sxs-lookup"><span data-stu-id="de640-334">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="de640-335">Se nessun elemento corrisponde all'ID richiesto, il metodo restituisce un codice di errore 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound).</span><span class="sxs-lookup"><span data-stu-id="de640-335">If no item matches the requested ID, the method returns a 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) error code.</span></span>
* <span data-ttu-id="de640-336">In caso contrario, il metodo restituisce il codice 200 con un corpo della risposta JSON.</span><span class="sxs-lookup"><span data-stu-id="de640-336">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="de640-337">La restituzione di `item` risulta in una risposta HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="de640-337">Returning `item` results in an HTTP 200 response.</span></span>

## <a name="the-puttodoitem-method"></a><span data-ttu-id="de640-338">Metodo PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="de640-338">The PutTodoItem method</span></span>

<span data-ttu-id="de640-339">Esaminare il metodo `PutTodoItem`:</span><span class="sxs-lookup"><span data-stu-id="de640-339">Examine the `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Update)]

<span data-ttu-id="de640-340">`PutTodoItem` è simile a `PostTodoItem` ma usa la richiesta HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="de640-340">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="de640-341">La risposta è [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="de640-341">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="de640-342">In base alla specifica HTTP, una richiesta PUT richiede che il client invii l'intera entità aggiornata e non solo le modifiche.</span><span class="sxs-lookup"><span data-stu-id="de640-342">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="de640-343">Per supportare gli aggiornamenti parziali, usare [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span><span class="sxs-lookup"><span data-stu-id="de640-343">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="de640-344">Se si riceve un errore chiamando `PutTodoItem`, chiamare `GET` per verificare che il database includa un elemento.</span><span class="sxs-lookup"><span data-stu-id="de640-344">If you get an error calling `PutTodoItem`, call `GET` to ensure there's an item in the database.</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="de640-345">Testare il metodo PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="de640-345">Test the PutTodoItem method</span></span>

<span data-ttu-id="de640-346">Questo esempio usa un database in memoria che deve essere inizializzato ogni volta che l'app viene avviata.</span><span class="sxs-lookup"><span data-stu-id="de640-346">This sample uses an in-memory database that must be initialized each time the app is started.</span></span> <span data-ttu-id="de640-347">Deve esistere un elemento nel database prima di eseguire una chiamata PUT.</span><span class="sxs-lookup"><span data-stu-id="de640-347">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="de640-348">Chiamare GET per assicurarsi che sia presente un elemento nel database prima di eseguire una chiamata PUT.</span><span class="sxs-lookup"><span data-stu-id="de640-348">Call GET to insure there's an item in the database before making a PUT call.</span></span>

<span data-ttu-id="de640-349">Aggiornare l'elemento attività con ID = 1 e impostarne il nome su "feed fish":</span><span class="sxs-lookup"><span data-stu-id="de640-349">Update the to-do item that has ID = 1 and set its name to "feed fish":</span></span>

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="de640-350">L'immagine seguente visualizza l'aggiornamento di Postman:</span><span class="sxs-lookup"><span data-stu-id="de640-350">The following image shows the Postman update:</span></span>

![Console Postman con risposta 204 (No Content)](first-web-api/_static/3/pmcput.png)

## <a name="the-deletetodoitem-method"></a><span data-ttu-id="de640-352">Metodo DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="de640-352">The DeleteTodoItem method</span></span>

<span data-ttu-id="de640-353">Esaminare il metodo `DeleteTodoItem`:</span><span class="sxs-lookup"><span data-stu-id="de640-353">Examine the `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Delete)]

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="de640-354">Testare il metodo DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="de640-354">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="de640-355">Usare Postman per eliminare un elemento attività:</span><span class="sxs-lookup"><span data-stu-id="de640-355">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="de640-356">Impostare il metodo su `DELETE`.</span><span class="sxs-lookup"><span data-stu-id="de640-356">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="de640-357">Impostare l'URI dell'oggetto da eliminare, ad esempio `https://localhost:5001/api/TodoItems/1`.</span><span class="sxs-lookup"><span data-stu-id="de640-357">Set the URI of the object to delete (for example `https://localhost:5001/api/TodoItems/1`).</span></span>
* <span data-ttu-id="de640-358">Selezionare **Send** (Invia).</span><span class="sxs-lookup"><span data-stu-id="de640-358">Select **Send**.</span></span>

<a name="over-post"></a>

## <a name="prevent-over-posting"></a><span data-ttu-id="de640-359">Impedisci overposting</span><span class="sxs-lookup"><span data-stu-id="de640-359">Prevent over-posting</span></span>

<span data-ttu-id="de640-360">Attualmente l'app di esempio espone l'intero oggetto `TodoItem`.</span><span class="sxs-lookup"><span data-stu-id="de640-360">Currently the sample app exposes the entire `TodoItem` object.</span></span> <span data-ttu-id="de640-361">Le app Productions limitano in genere i dati di input e restituiti usando un subset del modello.</span><span class="sxs-lookup"><span data-stu-id="de640-361">Productions apps typically limit the data that's input and returned using a subset of the model.</span></span> <span data-ttu-id="de640-362">Esistono diversi motivi alla base di questo e la sicurezza è una delle principali.</span><span class="sxs-lookup"><span data-stu-id="de640-362">There are multiple reasons behind this and security is a major one.</span></span> <span data-ttu-id="de640-363">Il subset di un modello è in genere indicato come oggetto Trasferimento dati (DTO), modello di input o modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="de640-363">The subset of a model is usually referred to as a Data Transfer Object (DTO), input model, or view model.</span></span> <span data-ttu-id="de640-364">**Dto** viene usato in questo articolo.</span><span class="sxs-lookup"><span data-stu-id="de640-364">**DTO** is used in this article.</span></span>

<span data-ttu-id="de640-365">Un DTO può essere usato per:</span><span class="sxs-lookup"><span data-stu-id="de640-365">A DTO may be used to:</span></span>

* <span data-ttu-id="de640-366">Impedisci l'overposting.</span><span class="sxs-lookup"><span data-stu-id="de640-366">Prevent over-posting.</span></span>
* <span data-ttu-id="de640-367">Nascondere le proprietà che i client non dovrebbero visualizzare.</span><span class="sxs-lookup"><span data-stu-id="de640-367">Hide properties that clients are not supposed to view.</span></span>
* <span data-ttu-id="de640-368">Omettere alcune proprietà per ridurre le dimensioni del payload.</span><span class="sxs-lookup"><span data-stu-id="de640-368">Omit some properties in order to reduce payload size.</span></span>
* <span data-ttu-id="de640-369">Rendere flat gli oggetti grafici che contengono oggetti annidati.</span><span class="sxs-lookup"><span data-stu-id="de640-369">Flatten object graphs that contain nested objects.</span></span> <span data-ttu-id="de640-370">I grafici a oggetti flat possono essere più pratici per i client.</span><span class="sxs-lookup"><span data-stu-id="de640-370">Flattened object graphs can be more convenient for clients.</span></span>

<span data-ttu-id="de640-371">Per illustrare l'approccio DTO, aggiornare la classe `TodoItem` per includere un campo Secret:</span><span class="sxs-lookup"><span data-stu-id="de640-371">To demonstrate the DTO approach, update the `TodoItem` class to include a secret field:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApiDTO/Models/TodoItem.cs?name=snippet&highlight=6)]

<span data-ttu-id="de640-372">Il campo segreto deve essere nascosto da questa app, ma un'app amministrativa può scegliere di esporla.</span><span class="sxs-lookup"><span data-stu-id="de640-372">The secret field needs to be hidden from this app, but an administrative app could choose to expose it.</span></span>

<span data-ttu-id="de640-373">Verificare che sia possibile pubblicare e ottenere il campo secret.</span><span class="sxs-lookup"><span data-stu-id="de640-373">Verify you can post and get the secret field.</span></span>

<span data-ttu-id="de640-374">Creare un modello DTO:</span><span class="sxs-lookup"><span data-stu-id="de640-374">Create a DTO model:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApiDTO/Models/TodoItemDTO.cs?name=snippet)]

<span data-ttu-id="de640-375">Aggiornare la `TodoItemsController` per usare `TodoItemDTO`:</span><span class="sxs-lookup"><span data-stu-id="de640-375">Update the `TodoItemsController` to use `TodoItemDTO`:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApiDTO/Controllers/TodoItemsController.cs?name=snippet)]

<span data-ttu-id="de640-376">Verificare che non sia possibile inserire o ottenere il campo secret.</span><span class="sxs-lookup"><span data-stu-id="de640-376">Verify you can't post or get the secret field.</span></span>

## <a name="call-the-web-api-with-javascript"></a><span data-ttu-id="de640-377">Chiamare l'API Web con JavaScript</span><span class="sxs-lookup"><span data-stu-id="de640-377">Call the web API with JavaScript</span></span>

<span data-ttu-id="de640-378">Vedere [esercitazione: chiamare un'API web ASP.NET Core con JavaScript](xref:tutorials/web-api-javascript).</span><span class="sxs-lookup"><span data-stu-id="de640-378">See [Tutorial: Call an ASP.NET Core web API with JavaScript](xref:tutorials/web-api-javascript).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="de640-379">In questa esercitazione verranno illustrate le procedure per:</span><span class="sxs-lookup"><span data-stu-id="de640-379">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="de640-380">Creare un progetto di API Web.</span><span class="sxs-lookup"><span data-stu-id="de640-380">Create a web API project.</span></span>
> * <span data-ttu-id="de640-381">Aggiungere una classe modello e un contesto di database.</span><span class="sxs-lookup"><span data-stu-id="de640-381">Add a model class and a database context.</span></span>
> * <span data-ttu-id="de640-382">Aggiungere un controller.</span><span class="sxs-lookup"><span data-stu-id="de640-382">Add a controller.</span></span>
> * <span data-ttu-id="de640-383">Aggiungere metodi CRUD.</span><span class="sxs-lookup"><span data-stu-id="de640-383">Add CRUD methods.</span></span>
> * <span data-ttu-id="de640-384">Configurare routing e percorsi URL.</span><span class="sxs-lookup"><span data-stu-id="de640-384">Configure routing and URL paths.</span></span>
> * <span data-ttu-id="de640-385">Specificare valori restituiti.</span><span class="sxs-lookup"><span data-stu-id="de640-385">Specify return values.</span></span>
> * <span data-ttu-id="de640-386">Chiamare l'API Web con Postman.</span><span class="sxs-lookup"><span data-stu-id="de640-386">Call the web API with Postman.</span></span>
> * <span data-ttu-id="de640-387">Chiamare l'API Web con JavaScript.</span><span class="sxs-lookup"><span data-stu-id="de640-387">Call the web API with JavaScript.</span></span>

<span data-ttu-id="de640-388">Al termine si dispone di un'API web che può gestire gli elementi di tipo "attività" archiviati in un database relazionale.</span><span class="sxs-lookup"><span data-stu-id="de640-388">At the end, you have a web API that can manage "to-do" items stored in a relational database.</span></span>

## <a name="overview"></a><span data-ttu-id="de640-389">Panoramica</span><span class="sxs-lookup"><span data-stu-id="de640-389">Overview</span></span>

<span data-ttu-id="de640-390">Questa esercitazione consente di creare l'API seguente:</span><span class="sxs-lookup"><span data-stu-id="de640-390">This tutorial creates the following API:</span></span>

|<span data-ttu-id="de640-391">API</span><span class="sxs-lookup"><span data-stu-id="de640-391">API</span></span> | <span data-ttu-id="de640-392">Descrizione</span><span class="sxs-lookup"><span data-stu-id="de640-392">Description</span></span> | <span data-ttu-id="de640-393">Corpo della richiesta</span><span class="sxs-lookup"><span data-stu-id="de640-393">Request body</span></span> | <span data-ttu-id="de640-394">Corpo della risposta</span><span class="sxs-lookup"><span data-stu-id="de640-394">Response body</span></span> |
|--- | ---- | ---- | ---- |
|<span data-ttu-id="de640-395">GET /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="de640-395">GET /api/TodoItems</span></span> | <span data-ttu-id="de640-396">Ottiene tutti gli elementi attività</span><span class="sxs-lookup"><span data-stu-id="de640-396">Get all to-do items</span></span> | <span data-ttu-id="de640-397">nessuno</span><span class="sxs-lookup"><span data-stu-id="de640-397">None</span></span> | <span data-ttu-id="de640-398">Matrice di elementi attività</span><span class="sxs-lookup"><span data-stu-id="de640-398">Array of to-do items</span></span>|
|<span data-ttu-id="de640-399">GET /api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="de640-399">GET /api/TodoItems/{id}</span></span> | <span data-ttu-id="de640-400">Ottiene un elemento in base all'ID</span><span class="sxs-lookup"><span data-stu-id="de640-400">Get an item by ID</span></span> | <span data-ttu-id="de640-401">nessuno</span><span class="sxs-lookup"><span data-stu-id="de640-401">None</span></span> | <span data-ttu-id="de640-402">Elemento attività</span><span class="sxs-lookup"><span data-stu-id="de640-402">To-do item</span></span>|
|<span data-ttu-id="de640-403">POST /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="de640-403">POST /api/TodoItems</span></span> | <span data-ttu-id="de640-404">Aggiunge un nuovo elemento</span><span class="sxs-lookup"><span data-stu-id="de640-404">Add a new item</span></span> | <span data-ttu-id="de640-405">Elemento attività</span><span class="sxs-lookup"><span data-stu-id="de640-405">To-do item</span></span> | <span data-ttu-id="de640-406">Elemento attività</span><span class="sxs-lookup"><span data-stu-id="de640-406">To-do item</span></span> |
|<span data-ttu-id="de640-407">PUT /api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="de640-407">PUT /api/TodoItems/{id}</span></span> | <span data-ttu-id="de640-408">Aggiorna un elemento esistente &nbsp;</span><span class="sxs-lookup"><span data-stu-id="de640-408">Update an existing item &nbsp;</span></span> | <span data-ttu-id="de640-409">Elemento attività</span><span class="sxs-lookup"><span data-stu-id="de640-409">To-do item</span></span> | <span data-ttu-id="de640-410">nessuno</span><span class="sxs-lookup"><span data-stu-id="de640-410">None</span></span> |
|<span data-ttu-id="de640-411">Elimina &nbsp;/api/TodoItems/{id} &nbsp;</span><span class="sxs-lookup"><span data-stu-id="de640-411">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span></span> | <span data-ttu-id="de640-412">Eliminare un elemento &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="de640-412">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="de640-413">nessuno</span><span class="sxs-lookup"><span data-stu-id="de640-413">None</span></span> | <span data-ttu-id="de640-414">nessuno</span><span class="sxs-lookup"><span data-stu-id="de640-414">None</span></span>|

<span data-ttu-id="de640-415">Il diagramma seguente visualizza la struttura dell'app.</span><span class="sxs-lookup"><span data-stu-id="de640-415">The following diagram shows the design of the app.</span></span>

![Il client è rappresentato da una casella a sinistra.](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="de640-421">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="de640-421">Prerequisites</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="de640-422">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="de640-422">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="de640-423">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="de640-423">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="de640-424">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="de640-424">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="de640-425">Creare un progetto Web</span><span class="sxs-lookup"><span data-stu-id="de640-425">Create a web project</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="de640-426">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="de640-426">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="de640-427">Scegliere **nuovo** > **progetto**dal menu **file** .</span><span class="sxs-lookup"><span data-stu-id="de640-427">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="de640-428">Selezionare il modello **Applicazione Web ASP.NET Core** e fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="de640-428">Select the **ASP.NET Core Web Application** template and click **Next**.</span></span>
* <span data-ttu-id="de640-429">Assegnare al progetto il nome *TodoApi* e fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="de640-429">Name the project *TodoApi* and click **Create**.</span></span>
* <span data-ttu-id="de640-430">Nella finestra di dialogo **Crea una nuova applicazione Web ASP.NET Core** verificare che siano selezionati **.NET Core** e **ASP.NET Core 2.2**.</span><span class="sxs-lookup"><span data-stu-id="de640-430">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 2.2** are selected.</span></span> <span data-ttu-id="de640-431">Selezionare il modello **API** e fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="de640-431">Select the **API** template and click **Create**.</span></span> <span data-ttu-id="de640-432">**Non** selezionare **Abilita supporto Docker**.</span><span class="sxs-lookup"><span data-stu-id="de640-432">**Don't** select **Enable Docker Support**.</span></span>

![Finestra di dialogo Nuovo progetto di Visual Studio](first-web-api/_static/vs.png)

# <a name="visual-studio-code"></a>[<span data-ttu-id="de640-434">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="de640-434">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="de640-435">Aprire il [terminale integrato](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="de640-435">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="de640-436">Cambiare directory (`cd`) e passare alla cartella che conterrà la cartella del progetto.</span><span class="sxs-lookup"><span data-stu-id="de640-436">Change directories (`cd`) to the folder that will contain the project folder.</span></span>
* <span data-ttu-id="de640-437">Eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="de640-437">Run the following commands:</span></span>

   ```dotnetcli
   dotnet new webapi -o TodoApi
   code -r TodoApi
   ```

  <span data-ttu-id="de640-438">Questi comandi creano un nuovo progetto API Web e aprono una nuova istanza di Visual Studio Code nella nuova cartella di progetto.</span><span class="sxs-lookup"><span data-stu-id="de640-438">These commands create a new web API project and open a new instance of Visual Studio Code in the new project folder.</span></span>

* <span data-ttu-id="de640-439">Quando una finestra di dialogo chiede se si vuole aggiungere gli asset necessari al progetto, selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="de640-439">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="de640-440">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="de640-440">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="de640-441">Selezionare **File** > **nuova soluzione**.</span><span class="sxs-lookup"><span data-stu-id="de640-441">Select **File** > **New Solution**.</span></span>

  ![Nuova soluzione macOS](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="de640-443">Selezionare **.NET Core** > **App** > **API** > **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="de640-443">Select **.NET Core** > **App** > **API** > **Next**.</span></span>

  ![Finestra di dialogo Nuovo progetto di macOS](first-web-api-mac/_static/1.png)
  
* <span data-ttu-id="de640-445">Nella finestra di dialogo **Configure your new ASP.NET Core Web API** (Configura la nuova API Web ASP.NET Core), accettare l'impostazione predefinita di **Framework di destinazione**, ovvero \* *.NET Core 2.2*.</span><span class="sxs-lookup"><span data-stu-id="de640-445">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of \**.NET Core 2.2*.</span></span>

* <span data-ttu-id="de640-446">Immettere *TodoApi* in **Nome progetto**, quindi selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="de640-446">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![Finestra di dialogo di configurazione](first-web-api-mac/_static/2.png)

---

### <a name="test-the-api"></a><span data-ttu-id="de640-448">Testare l'API</span><span class="sxs-lookup"><span data-stu-id="de640-448">Test the API</span></span>

<span data-ttu-id="de640-449">Il modello di progetto crea un'API `values`.</span><span class="sxs-lookup"><span data-stu-id="de640-449">The project template creates a `values` API.</span></span> <span data-ttu-id="de640-450">Chiamare il metodo `Get` da un browser per eseguire il test dell'app.</span><span class="sxs-lookup"><span data-stu-id="de640-450">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="de640-451">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="de640-451">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="de640-452">Premere CTRL+F5 per eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="de640-452">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="de640-453">Visual Studio apre un browser e naviga all'indirizzo `https://localhost:<port>/api/values`, dove `<port>` è un numero di porta selezionato a caso.</span><span class="sxs-lookup"><span data-stu-id="de640-453">Visual Studio launches a browser and navigates to `https://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="de640-454">Se in una finestra di dialogo viene chiesto se considerare attendibile il certificato IIS Express, selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="de640-454">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="de640-455">Nella finestra di dialogo **Avviso di sicurezza** successiva selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="de640-455">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="de640-456">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="de640-456">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="de640-457">Premere CTRL+F5 per eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="de640-457">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="de640-458">In un browser passare all'URL seguente: [https://localhost:5001/api/values](https://localhost:5001/api/values).</span><span class="sxs-lookup"><span data-stu-id="de640-458">In a browser, go to following URL: [https://localhost:5001/api/values](https://localhost:5001/api/values).</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="de640-459">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="de640-459">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="de640-460">Selezionare **Esegui** > **Avvia debug** per avviare l'app.</span><span class="sxs-lookup"><span data-stu-id="de640-460">Select **Run** > **Start Debugging** to launch the app.</span></span> <span data-ttu-id="de640-461">Visual Studio per Mac apre un browser e naviga all'indirizzo `https://localhost:<port>`, dove `<port>` è un numero di porta selezionato a caso.</span><span class="sxs-lookup"><span data-stu-id="de640-461">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="de640-462">Viene restituito un errore HTTP 404 (Non trovato).</span><span class="sxs-lookup"><span data-stu-id="de640-462">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="de640-463">Accodare `/api/values` all'URL (impostare l'URL su `https://localhost:<port>/api/values`).</span><span class="sxs-lookup"><span data-stu-id="de640-463">Append `/api/values` to the URL (change the URL to `https://localhost:<port>/api/values`).</span></span>

---

<span data-ttu-id="de640-464">Viene restituito il codice JSON seguente:</span><span class="sxs-lookup"><span data-stu-id="de640-464">The following JSON is returned:</span></span>

```json
["value1","value2"]
```

## <a name="add-a-model-class"></a><span data-ttu-id="de640-465">Aggiungere una classe modello</span><span class="sxs-lookup"><span data-stu-id="de640-465">Add a model class</span></span>

<span data-ttu-id="de640-466">Un *modello* è un set di classi che rappresentano i dati gestiti dall'app.</span><span class="sxs-lookup"><span data-stu-id="de640-466">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="de640-467">Il modello per questa app è una classe `TodoItem` singola.</span><span class="sxs-lookup"><span data-stu-id="de640-467">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="de640-468">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="de640-468">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="de640-469">In **Esplora soluzioni** fare clic con il pulsante destro del mouse sul progetto.</span><span class="sxs-lookup"><span data-stu-id="de640-469">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="de640-470">Selezionare **Aggiungi** > **Nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="de640-470">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="de640-471">Assegnare il nome *Models* alla cartella.</span><span class="sxs-lookup"><span data-stu-id="de640-471">Name the folder *Models*.</span></span>

* <span data-ttu-id="de640-472">Fare clic con il pulsante destro del mouse sulla cartella *Models* e scegliere **Aggiungi** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="de640-472">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="de640-473">Assegnare alla classe il nome *TodoItem* e selezionare **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="de640-473">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="de640-474">Sostituire il codice del modello con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="de640-474">Replace the template code with the following code:</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="de640-475">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="de640-475">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="de640-476">Aggiungere una cartella denominata *Models*.</span><span class="sxs-lookup"><span data-stu-id="de640-476">Add a folder named *Models*.</span></span>

* <span data-ttu-id="de640-477">Aggiungere una classe `TodoItem` alla cartella *Models* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="de640-477">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="de640-478">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="de640-478">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="de640-479">Fare clic con il pulsante destro del mouse sul progetto.</span><span class="sxs-lookup"><span data-stu-id="de640-479">Right-click the project.</span></span> <span data-ttu-id="de640-480">Selezionare **Aggiungi** > **Nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="de640-480">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="de640-481">Assegnare il nome *Models* alla cartella.</span><span class="sxs-lookup"><span data-stu-id="de640-481">Name the folder *Models*.</span></span>

  ![Nuova cartella](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="de640-483">Fare clic con il pulsante destro del mouse sulla cartella *Models* e scegliere **Aggiungi** > **nuovo file** > **generale** > **classe vuota**.</span><span class="sxs-lookup"><span data-stu-id="de640-483">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="de640-484">Assegnare alla classe il nome *TodoItem* e fare clic su **Nuovo**.</span><span class="sxs-lookup"><span data-stu-id="de640-484">Name the class *TodoItem*, and then click **New**.</span></span>

* <span data-ttu-id="de640-485">Sostituire il codice del modello con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="de640-485">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="de640-486">La proprietà `Id` funziona come chiave univoca in un database relazionale.</span><span class="sxs-lookup"><span data-stu-id="de640-486">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="de640-487">È possibile inserire le classi del modello in qualsiasi punto del progetto, ma per convenzione viene usata la cartella *Models*.</span><span class="sxs-lookup"><span data-stu-id="de640-487">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="de640-488">Aggiungere un contesto di database</span><span class="sxs-lookup"><span data-stu-id="de640-488">Add a database context</span></span>

<span data-ttu-id="de640-489">Il *contesto di database* è la classe principale che coordina le funzionalità di Entity Framework per un modello di dati.</span><span class="sxs-lookup"><span data-stu-id="de640-489">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="de640-490">Questa classe viene creata mediante derivazione dalla classe `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="de640-490">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="de640-491">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="de640-491">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="de640-492">Fare clic con il pulsante destro del mouse sulla cartella *Models* e scegliere **Aggiungi** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="de640-492">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="de640-493">Assegnare alla classe il nome *TodoContext* e fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="de640-493">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mac"></a>[<span data-ttu-id="de640-494">Visual Studio Code/Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="de640-494">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="de640-495">Aggiungere una classe `TodoContext` alla cartella *Models*.</span><span class="sxs-lookup"><span data-stu-id="de640-495">Add a `TodoContext` class to the *Models* folder.</span></span>

---

* <span data-ttu-id="de640-496">Sostituire il codice del modello con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="de640-496">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="de640-497">Registrare il contesto del database</span><span class="sxs-lookup"><span data-stu-id="de640-497">Register the database context</span></span>

<span data-ttu-id="de640-498">In ASP.NET Core i servizi come il contesto del database devono essere registrati con il [contenitore di inserimento delle dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="de640-498">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="de640-499">Il contenitore rende disponibile il servizio ai controller.</span><span class="sxs-lookup"><span data-stu-id="de640-499">The container provides the service to controllers.</span></span>

<span data-ttu-id="de640-500">Aggiornare *Startup.cs* con il codice evidenziato seguente:</span><span class="sxs-lookup"><span data-stu-id="de640-500">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup1.cs?highlight=5,8,25-26&name=snippet_all)]

<span data-ttu-id="de640-501">Il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="de640-501">The preceding code:</span></span>

* <span data-ttu-id="de640-502">Rimuove le dichiarazioni `using` inutilizzate.</span><span class="sxs-lookup"><span data-stu-id="de640-502">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="de640-503">Aggiunge il contesto del database al contenitore di inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="de640-503">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="de640-504">Specifica che il contesto del database userà un database in memoria.</span><span class="sxs-lookup"><span data-stu-id="de640-504">Specifies that the database context will use an in-memory database.</span></span>

## <a name="add-a-controller"></a><span data-ttu-id="de640-505">Aggiunta di un controller</span><span class="sxs-lookup"><span data-stu-id="de640-505">Add a controller</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="de640-506">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="de640-506">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="de640-507">Fare clic con il pulsante destro del mouse sulla cartella *Controllers*.</span><span class="sxs-lookup"><span data-stu-id="de640-507">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="de640-508">Selezionare **aggiungi** > **nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="de640-508">Select **Add** > **New Item**.</span></span>
* <span data-ttu-id="de640-509">Nella finestra di dialogo **Aggiungi nuovo elemento** selezionare il modello **API Controller Class** (Classe controller API).</span><span class="sxs-lookup"><span data-stu-id="de640-509">In the **Add New Item** dialog, select the **API Controller Class** template.</span></span>
* <span data-ttu-id="de640-510">Assegnare alla classe il nome *TodoController* e selezionare **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="de640-510">Name the class *TodoController*, and select **Add**.</span></span>

  ![Finestra di dialogo Aggiungi nuovo elemento con controller nella casella di ricerca e controller API Web selezionato](first-web-api/_static/new_controller.png)

# <a name="visual-studio-code--visual-studio-for-mac"></a>[<span data-ttu-id="de640-512">Visual Studio Code/Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="de640-512">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="de640-513">Nella cartella *Controllers* creare una classe denominata `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="de640-513">In the *Controllers* folder, create a class named `TodoController`.</span></span>

---

* <span data-ttu-id="de640-514">Sostituire il codice del modello con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="de640-514">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="de640-515">Il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="de640-515">The preceding code:</span></span>

* <span data-ttu-id="de640-516">Definisce una classe controller API senza metodi.</span><span class="sxs-lookup"><span data-stu-id="de640-516">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="de640-517">Contrassegna la classe con l'attributo [`[ApiController]`](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) .</span><span class="sxs-lookup"><span data-stu-id="de640-517">Marks the class with the [`[ApiController]`](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute.</span></span> <span data-ttu-id="de640-518">L'attributo indica che il controller risponde alle richieste di API Web.</span><span class="sxs-lookup"><span data-stu-id="de640-518">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="de640-519">Per informazioni sui comportamenti specifici consentiti dall'attributo, vedere <xref:web-api/index>.</span><span class="sxs-lookup"><span data-stu-id="de640-519">For information about specific behaviors that the attribute enables, see <xref:web-api/index>.</span></span>
* <span data-ttu-id="de640-520">Usa l'inserimento delle dipendenze per inserire il contesto del database (`TodoContext`) nel controller.</span><span class="sxs-lookup"><span data-stu-id="de640-520">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="de640-521">Il contesto di database viene usato in ognuno dei metodi [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) nel controller.</span><span class="sxs-lookup"><span data-stu-id="de640-521">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>
* <span data-ttu-id="de640-522">Aggiunge un elemento denominato `Item1` al database se il database è vuoto.</span><span class="sxs-lookup"><span data-stu-id="de640-522">Adds an item named `Item1` to the database if the database is empty.</span></span> <span data-ttu-id="de640-523">Questo codice si trova nel costruttore, quindi viene eseguito ogni volta che è presente una nuova richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="de640-523">This code is in the constructor, so it runs every time there's a new HTTP request.</span></span> <span data-ttu-id="de640-524">Se si eliminano tutti gli elementi, il costruttore crea di nuovo `Item1` quando viene nuovamente chiamato un metodo API.</span><span class="sxs-lookup"><span data-stu-id="de640-524">If you delete all items, the constructor creates `Item1` again the next time an API method is called.</span></span> <span data-ttu-id="de640-525">Potrebbe pertanto sembrare che l'eliminazione non abbia funzionato, mentre di fatto ha funzionato.</span><span class="sxs-lookup"><span data-stu-id="de640-525">So it may look like the deletion didn't work when it actually did work.</span></span>

## <a name="add-get-methods"></a><span data-ttu-id="de640-526">Aggiungere metodi Get</span><span class="sxs-lookup"><span data-stu-id="de640-526">Add Get methods</span></span>

<span data-ttu-id="de640-527">Per specificare un'API che recupera elementi attività, aggiungere i metodi seguenti alla classe `TodoController`:</span><span class="sxs-lookup"><span data-stu-id="de640-527">To provide an API that retrieves to-do items, add the following methods to the `TodoController` class:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

<span data-ttu-id="de640-528">Questi metodi implementano due metodi GET:</span><span class="sxs-lookup"><span data-stu-id="de640-528">These methods implement two GET endpoints:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="de640-529">Arrestare l'app se è ancora in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="de640-529">Stop the app if it's still running.</span></span> <span data-ttu-id="de640-530">Quindi eseguirla di nuovo per includere le modifiche più recenti.</span><span class="sxs-lookup"><span data-stu-id="de640-530">Then run it again to include the latest changes.</span></span>

<span data-ttu-id="de640-531">Testare l'app chiamando i due endpoint da un browser.</span><span class="sxs-lookup"><span data-stu-id="de640-531">Test the app by calling the two endpoints from a browser.</span></span> <span data-ttu-id="de640-532">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="de640-532">For example:</span></span>

* `https://localhost:<port>/api/todo`
* `https://localhost:<port>/api/todo/1`

<span data-ttu-id="de640-533">La chiamata a `GetTodoItems` genera la risposta HTTP seguente:</span><span class="sxs-lookup"><span data-stu-id="de640-533">The following HTTP response is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

## <a name="routing-and-url-paths"></a><span data-ttu-id="de640-534">Routing e percorsi di URL</span><span class="sxs-lookup"><span data-stu-id="de640-534">Routing and URL paths</span></span>

<span data-ttu-id="de640-535">L'attributo [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) indica un metodo che risponde a una richiesta HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="de640-535">The [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="de640-536">Il percorso dell'URL per ogni metodo viene costruito nel modo seguente:</span><span class="sxs-lookup"><span data-stu-id="de640-536">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="de640-537">Iniziare con la stringa di modello nell'attributo `Route` del controller:</span><span class="sxs-lookup"><span data-stu-id="de640-537">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

* <span data-ttu-id="de640-538">Sostituire `[controller]` con il nome del controller, ovvero, per convenzione, il nome della classe controller meno il suffisso "Controller".</span><span class="sxs-lookup"><span data-stu-id="de640-538">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="de640-539">In questo esempio il nome della classe controller è **Todo**Controller, pertanto il nome del controller è "todo".</span><span class="sxs-lookup"><span data-stu-id="de640-539">For this sample, the controller class name is **Todo**Controller, so the controller name is "todo".</span></span> <span data-ttu-id="de640-540">Il [routing](xref:mvc/controllers/routing) ASP.NET Core non fa distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="de640-540">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="de640-541">Se l'attributo `[HttpGet]` ha un modello di route, ad esempio `[HttpGet("products")]`, aggiungerlo al percorso.</span><span class="sxs-lookup"><span data-stu-id="de640-541">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="de640-542">In questo esempio non si usa un modello.</span><span class="sxs-lookup"><span data-stu-id="de640-542">This sample doesn't use a template.</span></span> <span data-ttu-id="de640-543">Per altre informazioni, vedere [Routing con attributi Http[verb]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="de640-543">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="de640-544">Nel metodo `GetTodoItem` seguente, `"{id}"` è una variabile segnaposto per l'identificatore univoco dell'elemento attività.</span><span class="sxs-lookup"><span data-stu-id="de640-544">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="de640-545">Quando viene chiamato `GetTodoItem`, il valore di `"{id}"` viene specificato per il metodo nel rispettivo parametro `id`.</span><span class="sxs-lookup"><span data-stu-id="de640-545">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its`id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a><span data-ttu-id="de640-546">Valori restituiti</span><span class="sxs-lookup"><span data-stu-id="de640-546">Return values</span></span>

<span data-ttu-id="de640-547">Il tipo restituito dei metodi `GetTodoItems` e `GetTodoItem` è [ActionResult\<T>](xref:web-api/action-return-types#actionresultt-type).</span><span class="sxs-lookup"><span data-stu-id="de640-547">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="de640-548">ASP.NET Core serializza automaticamente l'oggetto su [JSON](https://www.json.org/) e scrive il codice JSON nel corpo del messaggio di risposta.</span><span class="sxs-lookup"><span data-stu-id="de640-548">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="de640-549">Il codice di risposta per questo tipo restituito è 200, presupponendo che non esistano eccezioni non gestite.</span><span class="sxs-lookup"><span data-stu-id="de640-549">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="de640-550">Le eccezioni non gestite vengono convertite in errori 5xx.</span><span class="sxs-lookup"><span data-stu-id="de640-550">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="de640-551">I tipi restituiti `ActionResult` possono rappresentare un ampio intervallo di codici di stato HTTP.</span><span class="sxs-lookup"><span data-stu-id="de640-551">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="de640-552">Ad esempio, `GetTodoItem` può restituire due valori di stato diversi:</span><span class="sxs-lookup"><span data-stu-id="de640-552">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="de640-553">Se nessun elemento corrisponde all'ID richiesto, il metodo restituisce un codice di errore 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound).</span><span class="sxs-lookup"><span data-stu-id="de640-553">If no item matches the requested ID, the method returns a 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) error code.</span></span>
* <span data-ttu-id="de640-554">In caso contrario, il metodo restituisce il codice 200 con un corpo della risposta JSON.</span><span class="sxs-lookup"><span data-stu-id="de640-554">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="de640-555">La restituzione di `item` risulta in una risposta HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="de640-555">Returning `item` results in an HTTP 200 response.</span></span>

## <a name="test-the-gettodoitems-method"></a><span data-ttu-id="de640-556">Testare il metodo GetTodoItems</span><span class="sxs-lookup"><span data-stu-id="de640-556">Test the GetTodoItems method</span></span>

<span data-ttu-id="de640-557">Questa esercitazione usa Postman per testare l'API Web.</span><span class="sxs-lookup"><span data-stu-id="de640-557">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="de640-558">Installare il [post](https://www.getpostman.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="de640-558">Install [Postman](https://www.getpostman.com/downloads/).</span></span>
* <span data-ttu-id="de640-559">Avviare l'app Web.</span><span class="sxs-lookup"><span data-stu-id="de640-559">Start the web app.</span></span>
* <span data-ttu-id="de640-560">Avviare Postman.</span><span class="sxs-lookup"><span data-stu-id="de640-560">Start Postman.</span></span>
* <span data-ttu-id="de640-561">Disabilitare la **Verifica del certificato SSL**.</span><span class="sxs-lookup"><span data-stu-id="de640-561">Disable **SSL certificate verification**.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="de640-562">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="de640-562">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="de640-563">In **File** > **Impostazioni** (scheda**generale** ) disabilitare la **Verifica del certificato SSL**.</span><span class="sxs-lookup"><span data-stu-id="de640-563">From **File** > **Settings** (**General** tab), disable **SSL certificate verification**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mac"></a>[<span data-ttu-id="de640-564">Visual Studio Code/Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="de640-564">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="de640-565">Da **Postman** > **Preferences** (Preferenze) (scheda **General** (Generale)) disabilitare **SSL certificate verification** (Verifica certificato SSL).</span><span class="sxs-lookup"><span data-stu-id="de640-565">From **Postman** > **Preferences** (**General** tab), disable **SSL certificate verification**.</span></span> <span data-ttu-id="de640-566">In alternativa, selezionare la chiave inglese e selezionare **Settings** (Impostazioni), quindi disabilitare la verifica del certificato SSL.</span><span class="sxs-lookup"><span data-stu-id="de640-566">Alternatively, select the wrench and select **Settings**, then disable the SSL certificate verification.</span></span>

---
  
> [!WARNING]
> <span data-ttu-id="de640-567">Riattivare la verifica dei certificati SSL al termine del test del controller.</span><span class="sxs-lookup"><span data-stu-id="de640-567">Re-enable SSL certificate verification after testing the controller.</span></span>

* <span data-ttu-id="de640-568">Creare una nuova richiesta.</span><span class="sxs-lookup"><span data-stu-id="de640-568">Create a new request.</span></span>
  * <span data-ttu-id="de640-569">Impostare il metodo HTTP su **GET**.</span><span class="sxs-lookup"><span data-stu-id="de640-569">Set the HTTP method to **GET**.</span></span>
  * <span data-ttu-id="de640-570">Impostare l'URL della richiesta su `https://localhost:<port>/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="de640-570">Set the request URL to `https://localhost:<port>/api/todo`.</span></span> <span data-ttu-id="de640-571">Ad esempio: `https://localhost:5001/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="de640-571">For example, `https://localhost:5001/api/todo`.</span></span>
* <span data-ttu-id="de640-572">Impostare **Two pane view** (Visualizzazione in due riquadri) in Postman.</span><span class="sxs-lookup"><span data-stu-id="de640-572">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="de640-573">Selezionare **Send** (Invia).</span><span class="sxs-lookup"><span data-stu-id="de640-573">Select **Send**.</span></span>

![Postman con richiesta GET](first-web-api/_static/2pv.png)

## <a name="add-a-create-method"></a><span data-ttu-id="de640-575">Aggiungere un metodo Create</span><span class="sxs-lookup"><span data-stu-id="de640-575">Add a Create method</span></span>

<span data-ttu-id="de640-576">Aggiungere il metodo `PostTodoItem` seguente in *Controllers/TodoController.cs*:</span><span class="sxs-lookup"><span data-stu-id="de640-576">Add the following `PostTodoItem` method inside of *Controllers/TodoController.cs*:</span></span> 

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="de640-577">Il codice precedente è un metodo HTTP POST, come indicato dall'attributo [`[HttpPost]`](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) .</span><span class="sxs-lookup"><span data-stu-id="de640-577">The preceding code is an HTTP POST method, as indicated by the [`[HttpPost]`](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="de640-578">Il metodo ottiene il valore dell'elemento attività dal corpo della richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="de640-578">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="de640-579">Il metodo `CreatedAtAction`:</span><span class="sxs-lookup"><span data-stu-id="de640-579">The `CreatedAtAction` method:</span></span>

* <span data-ttu-id="de640-580">Restituisce un codice di stato HTTP 201 in caso di esito positivo.</span><span class="sxs-lookup"><span data-stu-id="de640-580">Returns an HTTP 201 status code, if successful.</span></span> <span data-ttu-id="de640-581">HTTP 201 è la risposta standard per un metodo HTTP POST che crea una nuova risorsa nel server.</span><span class="sxs-lookup"><span data-stu-id="de640-581">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="de640-582">Aggiunge un'intestazione `Location` alla risposta.</span><span class="sxs-lookup"><span data-stu-id="de640-582">Adds a `Location` header to the response.</span></span> <span data-ttu-id="de640-583">L'intestazione `Location` specifica l'URI dell'elemento attività appena creato.</span><span class="sxs-lookup"><span data-stu-id="de640-583">The `Location` header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="de640-584">Per altre informazioni, vedere [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="de640-584">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="de640-585">Fa riferimento all'azione `GetTodoItem` per creare l'URI dell'intestazione `Location`.</span><span class="sxs-lookup"><span data-stu-id="de640-585">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="de640-586">La parola chiave `nameof` C# viene usata per evitare di impostare il nome dell'azione come hardcoded nella chiamata a `CreatedAtAction`.</span><span class="sxs-lookup"><span data-stu-id="de640-586">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

### <a name="test-the-posttodoitem-method"></a><span data-ttu-id="de640-587">Testare il metodo PostTodoItem</span><span class="sxs-lookup"><span data-stu-id="de640-587">Test the PostTodoItem method</span></span>

* <span data-ttu-id="de640-588">Compilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="de640-588">Build the project.</span></span>
* <span data-ttu-id="de640-589">In Postman impostare il metodo HTTP su `POST`.</span><span class="sxs-lookup"><span data-stu-id="de640-589">In Postman, set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="de640-590">Selezionare la scheda **Corpo**.</span><span class="sxs-lookup"><span data-stu-id="de640-590">Select the **Body** tab.</span></span>
* <span data-ttu-id="de640-591">Selezionare il pulsante di opzione **raw** (non elaborato).</span><span class="sxs-lookup"><span data-stu-id="de640-591">Select the **raw** radio button.</span></span>
* <span data-ttu-id="de640-592">Impostare il tipo su **JSON (application/json)** .</span><span class="sxs-lookup"><span data-stu-id="de640-592">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="de640-593">Nel corpo della richiesta immettere codice JSON per un elemento attività:</span><span class="sxs-lookup"><span data-stu-id="de640-593">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="de640-594">Selezionare **Send** (Invia).</span><span class="sxs-lookup"><span data-stu-id="de640-594">Select **Send**.</span></span>

  ![Postman con richiesta di creazione](first-web-api/_static/create.png)

  <span data-ttu-id="de640-596">Se viene visualizzato un errore HTTP 405 - Metodo non consentito, è probabile che il progetto non sia stato compilato dopo l'aggiunta del metodo `PostTodoItem`.</span><span class="sxs-lookup"><span data-stu-id="de640-596">If you get a 405 Method Not Allowed error, it's probably the result of not compiling the project after adding the `PostTodoItem` method.</span></span>

### <a name="test-the-location-header-uri"></a><span data-ttu-id="de640-597">Testare l'URI dell'intestazione della posizione</span><span class="sxs-lookup"><span data-stu-id="de640-597">Test the location header URI</span></span>

* <span data-ttu-id="de640-598">Selezionare la scheda **Headers** (Intestazioni) nel riquadro **Response** (Risposta).</span><span class="sxs-lookup"><span data-stu-id="de640-598">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="de640-599">Copiare il valore dell'intestazione **Location** (Posizione):</span><span class="sxs-lookup"><span data-stu-id="de640-599">Copy the **Location** header value:</span></span>

  ![Scheda Headers (Intestazioni) della console Postman](first-web-api/_static/pmc2.png)

* <span data-ttu-id="de640-601">Impostare il metodo su GET.</span><span class="sxs-lookup"><span data-stu-id="de640-601">Set the method to GET.</span></span>
* <span data-ttu-id="de640-602">Incollare l'URI, ad esempio `https://localhost:5001/api/Todo/2`.</span><span class="sxs-lookup"><span data-stu-id="de640-602">Paste the URI (for example, `https://localhost:5001/api/Todo/2`).</span></span>
* <span data-ttu-id="de640-603">Selezionare **Send** (Invia).</span><span class="sxs-lookup"><span data-stu-id="de640-603">Select **Send**.</span></span>

## <a name="add-a-puttodoitem-method"></a><span data-ttu-id="de640-604">Aggiungere un metodo PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="de640-604">Add a PutTodoItem method</span></span>

<span data-ttu-id="de640-605">Aggiungere il metodo `PutTodoItem` seguente:</span><span class="sxs-lookup"><span data-stu-id="de640-605">Add the following `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

<span data-ttu-id="de640-606">`PutTodoItem` è simile a `PostTodoItem` ma usa la richiesta HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="de640-606">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="de640-607">La risposta è [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="de640-607">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="de640-608">In base alla specifica HTTP, una richiesta PUT richiede che il client invii l'intera entità aggiornata e non solo le modifiche.</span><span class="sxs-lookup"><span data-stu-id="de640-608">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="de640-609">Per supportare gli aggiornamenti parziali, usare [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span><span class="sxs-lookup"><span data-stu-id="de640-609">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="de640-610">Se si riceve un errore chiamando `PutTodoItem`, chiamare `GET` per verificare che il database includa un elemento.</span><span class="sxs-lookup"><span data-stu-id="de640-610">If you get an error calling `PutTodoItem`, call `GET` to ensure there's an item in the database.</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="de640-611">Testare il metodo PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="de640-611">Test the PutTodoItem method</span></span>

<span data-ttu-id="de640-612">Questo esempio usa un database in memoria che deve essere inizializzato ogni volta che l'app viene avviata.</span><span class="sxs-lookup"><span data-stu-id="de640-612">This sample uses an in-memory database that must be initialized each time the app is started.</span></span> <span data-ttu-id="de640-613">Deve esistere un elemento nel database prima di eseguire una chiamata PUT.</span><span class="sxs-lookup"><span data-stu-id="de640-613">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="de640-614">Chiamare GET per assicurarsi che sia presente un elemento nel database prima di eseguire una chiamata PUT.</span><span class="sxs-lookup"><span data-stu-id="de640-614">Call GET to insure there's an item in the database before making a PUT call.</span></span>

<span data-ttu-id="de640-615">Aggiornare l'elemento attività con id = 1 e impostarne il nome su "feed fish":</span><span class="sxs-lookup"><span data-stu-id="de640-615">Update the to-do item that has id = 1 and set its name to "feed fish":</span></span>

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="de640-616">L'immagine seguente visualizza l'aggiornamento di Postman:</span><span class="sxs-lookup"><span data-stu-id="de640-616">The following image shows the Postman update:</span></span>

![Console Postman con risposta 204 (No Content)](first-web-api/_static/pmcput.png)

## <a name="add-a-deletetodoitem-method"></a><span data-ttu-id="de640-618">Aggiungere un metodo DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="de640-618">Add a DeleteTodoItem method</span></span>

<span data-ttu-id="de640-619">Aggiungere il metodo `DeleteTodoItem` seguente:</span><span class="sxs-lookup"><span data-stu-id="de640-619">Add the following `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="de640-620">La risposta `DeleteTodoItem` è [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="de640-620">The `DeleteTodoItem` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="de640-621">Testare il metodo DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="de640-621">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="de640-622">Usare Postman per eliminare un elemento attività:</span><span class="sxs-lookup"><span data-stu-id="de640-622">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="de640-623">Impostare il metodo su `DELETE`.</span><span class="sxs-lookup"><span data-stu-id="de640-623">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="de640-624">Impostare l'URI dell'oggetto da eliminare, ad esempio `https://localhost:5001/api/todo/1`.</span><span class="sxs-lookup"><span data-stu-id="de640-624">Set the URI of the object to delete (for example `https://localhost:5001/api/todo/1`).</span></span>
* <span data-ttu-id="de640-625">Selezionare **Send** (Invia).</span><span class="sxs-lookup"><span data-stu-id="de640-625">Select **Send**.</span></span>

<span data-ttu-id="de640-626">L'app di esempio consente di eliminare tutti gli elementi.</span><span class="sxs-lookup"><span data-stu-id="de640-626">The sample app allows you to delete all the items.</span></span> <span data-ttu-id="de640-627">Quando viene eliminato l'ultimo elemento, tuttavia, ne viene creato uno nuovo dal costruttore della classe modello alla successiva chiamata dell'API.</span><span class="sxs-lookup"><span data-stu-id="de640-627">However, when the last item is deleted, a new one is created by the model class constructor the next time the API is called.</span></span>

## <a name="call-the-web-api-with-javascript"></a><span data-ttu-id="de640-628">Chiamare l'API Web con JavaScript</span><span class="sxs-lookup"><span data-stu-id="de640-628">Call the web API with JavaScript</span></span>

<span data-ttu-id="de640-629">In questa sezione viene aggiunta una pagina HTML che usa JavaScript per chiamare l'API Web.</span><span class="sxs-lookup"><span data-stu-id="de640-629">In this section, an HTML page is added that uses JavaScript to call the web API.</span></span> <span data-ttu-id="de640-630">jQuery avvia la richiesta.</span><span class="sxs-lookup"><span data-stu-id="de640-630">jQuery initiates the request.</span></span> <span data-ttu-id="de640-631">JavaScript aggiorna la pagina con i dettagli della risposta dell'API Web.</span><span class="sxs-lookup"><span data-stu-id="de640-631">JavaScript updates the page with the details from the web API's response.</span></span>

<span data-ttu-id="de640-632">Configurare l'app per [servire file statici](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) e [abilitare il mapping del file predefinito](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) aggiornando *Startup.cs* con il codice evidenziato seguente:</span><span class="sxs-lookup"><span data-stu-id="de640-632">Configure the app to [serve static files](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [enable default file mapping](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) by updating *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup.cs?highlight=14-15&name=snippet_configure)]

<span data-ttu-id="de640-633">Creare una cartella *wwwroot* nella directory del progetto.</span><span class="sxs-lookup"><span data-stu-id="de640-633">Create a *wwwroot* folder in the project directory.</span></span>

<span data-ttu-id="de640-634">Aggiungere un file HTML denominato *index.html* alla directory *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="de640-634">Add an HTML file named *index.html* to the *wwwroot* directory.</span></span> <span data-ttu-id="de640-635">Sostituire il contenuto con il markup seguente:</span><span class="sxs-lookup"><span data-stu-id="de640-635">Replace its contents with the following markup:</span></span>

[!code-html[](first-web-api/samples/2.2/TodoApi/wwwroot/index.html)]

<span data-ttu-id="de640-636">Aggiungere un file JavaScript con nome *site.js* alla directory *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="de640-636">Add a JavaScript file named *site.js* to the *wwwroot* directory.</span></span> <span data-ttu-id="de640-637">Sostituire il contenuto con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="de640-637">Replace its contents with the following code:</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_SiteJs)]

<span data-ttu-id="de640-638">Può essere necessario modificare le impostazioni di avvio del progetto ASP.NET Core per il test della pagina HTML in locale:</span><span class="sxs-lookup"><span data-stu-id="de640-638">A change to the ASP.NET Core project's launch settings may be required to test the HTML page locally:</span></span>

* <span data-ttu-id="de640-639">Aprire *Properties\launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="de640-639">Open *Properties\launchSettings.json*.</span></span>
* <span data-ttu-id="de640-640">Rimuovere la proprietà `launchUrl` per forzare l'app ad aprire *index.html*, il file predefinito del progetto.</span><span class="sxs-lookup"><span data-stu-id="de640-640">Remove the `launchUrl` property to force the app to open at *index.html*&mdash;the project's default file.</span></span>

<span data-ttu-id="de640-641">In questo esempio vengono chiamati tutti i metodi CRUD dell'API Web.</span><span class="sxs-lookup"><span data-stu-id="de640-641">This sample calls all of the CRUD methods of the web API.</span></span> <span data-ttu-id="de640-642">Di seguito sono incluse le spiegazioni delle chiamate all'API.</span><span class="sxs-lookup"><span data-stu-id="de640-642">Following are explanations of the calls to the API.</span></span>

### <a name="get-a-list-of-to-do-items"></a><span data-ttu-id="de640-643">Ottenere un elenco di elementi attività</span><span class="sxs-lookup"><span data-stu-id="de640-643">Get a list of to-do items</span></span>

<span data-ttu-id="de640-644">jQuery invia una richiesta HTTP GET all'API Web, che restituisce JSON che rappresenta una matrice di elementi attività.</span><span class="sxs-lookup"><span data-stu-id="de640-644">jQuery sends an HTTP GET request to the web API, which returns JSON representing an array of to-do items.</span></span> <span data-ttu-id="de640-645">La funzione di callback `success` viene chiamata se la richiesta ha esito positivo.</span><span class="sxs-lookup"><span data-stu-id="de640-645">The `success` callback function is invoked if the request succeeds.</span></span> <span data-ttu-id="de640-646">Nel callback il modello DOM viene aggiornato con le informazioni dell'elemento attività.</span><span class="sxs-lookup"><span data-stu-id="de640-646">In the callback, the DOM is updated with the to-do information.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_GetData)]

### <a name="add-a-to-do-item"></a><span data-ttu-id="de640-647">Aggiungere un elemento attività</span><span class="sxs-lookup"><span data-stu-id="de640-647">Add a to-do item</span></span>

<span data-ttu-id="de640-648">jQuery invia una richiesta HTTP POST con l'elemento attività nel corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="de640-648">jQuery sends an HTTP POST request with the to-do item in the request body.</span></span> <span data-ttu-id="de640-649">Le opzioni `accepts` e `contentType` sono impostate su `application/json` per specificare il tipo di supporto ricevuto e inviato.</span><span class="sxs-lookup"><span data-stu-id="de640-649">The `accepts` and `contentType` options are set to `application/json` to specify the media type being received and sent.</span></span> <span data-ttu-id="de640-650">L'elemento attività viene convertito in JSON usando [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span><span class="sxs-lookup"><span data-stu-id="de640-650">The to-do item is converted to JSON by using [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span></span> <span data-ttu-id="de640-651">Quando l'API restituisce un codice di stato di esecuzione riuscita, viene chiamata la funzione `getData` per aggiornare la tabella HTML.</span><span class="sxs-lookup"><span data-stu-id="de640-651">When the API returns a successful status code, the `getData` function is invoked to update the HTML table.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AddItem)]

### <a name="update-a-to-do-item"></a><span data-ttu-id="de640-652">Aggiornare un elemento attività</span><span class="sxs-lookup"><span data-stu-id="de640-652">Update a to-do item</span></span>

<span data-ttu-id="de640-653">L'aggiornamento di un elemento attività è simile all'aggiunta di un elemento di questo tipo.</span><span class="sxs-lookup"><span data-stu-id="de640-653">Updating a to-do item is similar to adding one.</span></span> <span data-ttu-id="de640-654">L'`url` cambia per includere l'identificatore univoco dell'elemento e `type` è `PUT`.</span><span class="sxs-lookup"><span data-stu-id="de640-654">The `url` changes to add the unique identifier of the item, and the `type` is `PUT`.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxPut)]

### <a name="delete-a-to-do-item"></a><span data-ttu-id="de640-655">Eliminare un elemento attività</span><span class="sxs-lookup"><span data-stu-id="de640-655">Delete a to-do item</span></span>

<span data-ttu-id="de640-656">È possibile eliminare un elemento attività impostando `type` su `DELETE` nella chiamata AJAX e specificando l'identificatore univoco dell'elemento nell'URL.</span><span class="sxs-lookup"><span data-stu-id="de640-656">Deleting a to-do item is accomplished by setting the `type` on the AJAX call to `DELETE` and specifying the item's unique identifier in the URL.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxDelete)]

::: moniker-end

<a name="auth"></a>

## <a name="add-authentication-support-to-a-web-api"></a><span data-ttu-id="de640-657">Aggiungere il supporto per l'autenticazione a un'API Web</span><span class="sxs-lookup"><span data-stu-id="de640-657">Add authentication support to a web API</span></span>

[!INCLUDE[](~/includes/IdentityServer4.md)]

## <a name="additional-resources"></a><span data-ttu-id="de640-658">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="de640-658">Additional resources</span></span>

<span data-ttu-id="de640-659">[Visualizzare o scaricare il codice di esempio per questa esercitazione](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span><span class="sxs-lookup"><span data-stu-id="de640-659">[View or download sample code for this tutorial](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span></span> <span data-ttu-id="de640-660">Vedere [come scaricare un esempio](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="de640-660">See [how to download](xref:index#how-to-download-a-sample).</span></span>

<span data-ttu-id="de640-661">Per altre informazioni, vedere le seguenti risorse:</span><span class="sxs-lookup"><span data-stu-id="de640-661">For more information, see the following resources:</span></span>

* <xref:web-api/index>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:data/ef-rp/intro>
* <xref:mvc/controllers/routing>
* <xref:web-api/action-return-types>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/index>
* [<span data-ttu-id="de640-662">Versione YouTube dell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="de640-662">YouTube version of this tutorial</span></span>](https://www.youtube.com/watch?v=TTkhEyGBfAk)
