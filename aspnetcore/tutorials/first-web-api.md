---
title: "Esercitazione: Creare un'API Web con ASP.NET Core MVC"
author: rick-anderson
description: Creare un'API Web con ASP.NET Core MVC
ms.author: riande
ms.custom: mvc
ms.date: 12/10/2018
uid: tutorials/first-web-api
ms.openlocfilehash: c2b4dcddd5332330cd6e6abe7d3a12697cde845e
ms.sourcegitcommit: 4e87712029de2aceb1cf2c52e9e3dda8195a5b8e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/14/2018
ms.locfileid: "53382004"
---
# <a name="tutorial-create-a-web-api-with-aspnet-core-mvc"></a><span data-ttu-id="99ca7-103">Esercitazione: Creare un'API Web con ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="99ca7-103">Tutorial: Create a web API with ASP.NET Core MVC</span></span>

<span data-ttu-id="99ca7-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT) e [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="99ca7-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="99ca7-105">Questa esercitazione illustra le nozioni di base della creazione di un'API Web con ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="99ca7-105">This tutorial teaches the basics of building a web API with ASP.NET Core.</span></span>

<span data-ttu-id="99ca7-106">In questa esercitazione si imparerà a:</span><span class="sxs-lookup"><span data-stu-id="99ca7-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="99ca7-107">Creare un progetto di API Web.</span><span class="sxs-lookup"><span data-stu-id="99ca7-107">Create a web API project.</span></span>
> * <span data-ttu-id="99ca7-108">Aggiungere una classe modello.</span><span class="sxs-lookup"><span data-stu-id="99ca7-108">Add a model class.</span></span>
> * <span data-ttu-id="99ca7-109">Creare il contesto del database.</span><span class="sxs-lookup"><span data-stu-id="99ca7-109">Create the database context.</span></span>
> * <span data-ttu-id="99ca7-110">Registrare il contesto del database.</span><span class="sxs-lookup"><span data-stu-id="99ca7-110">Register the database context.</span></span>
> * <span data-ttu-id="99ca7-111">Aggiungere un controller.</span><span class="sxs-lookup"><span data-stu-id="99ca7-111">Add a controller.</span></span>
> * <span data-ttu-id="99ca7-112">Aggiungere metodi CRUD.</span><span class="sxs-lookup"><span data-stu-id="99ca7-112">Add CRUD methods.</span></span>
> * <span data-ttu-id="99ca7-113">Configurare routing e percorsi URL.</span><span class="sxs-lookup"><span data-stu-id="99ca7-113">Configure routing and URL paths.</span></span>
> * <span data-ttu-id="99ca7-114">Specificare valori restituiti.</span><span class="sxs-lookup"><span data-stu-id="99ca7-114">Specify return values.</span></span>
> * <span data-ttu-id="99ca7-115">Chiamare l'API Web con Postman.</span><span class="sxs-lookup"><span data-stu-id="99ca7-115">Call the web API with Postman.</span></span>
> * <span data-ttu-id="99ca7-116">Chiamare l'API Web con jQuery.</span><span class="sxs-lookup"><span data-stu-id="99ca7-116">Call the web API with jQuery.</span></span>

<span data-ttu-id="99ca7-117">Al termine si dispone di un'API web che può gestire gli elementi di tipo "attività" archiviati in un database relazionale.</span><span class="sxs-lookup"><span data-stu-id="99ca7-117">At the end, you have a web API that can manage "to-do" items stored in a relational database.</span></span>

## <a name="overview"></a><span data-ttu-id="99ca7-118">Panoramica</span><span class="sxs-lookup"><span data-stu-id="99ca7-118">Overview</span></span>

<span data-ttu-id="99ca7-119">Questa esercitazione consente di creare l'API seguente:</span><span class="sxs-lookup"><span data-stu-id="99ca7-119">This tutorial creates the following API:</span></span>

|<span data-ttu-id="99ca7-120">API</span><span class="sxs-lookup"><span data-stu-id="99ca7-120">API</span></span> | <span data-ttu-id="99ca7-121">Description</span><span class="sxs-lookup"><span data-stu-id="99ca7-121">Description</span></span> | <span data-ttu-id="99ca7-122">Corpo della richiesta</span><span class="sxs-lookup"><span data-stu-id="99ca7-122">Request body</span></span> | <span data-ttu-id="99ca7-123">Corpo della risposta</span><span class="sxs-lookup"><span data-stu-id="99ca7-123">Response body</span></span> |
|--- | ---- | ---- | ---- |
|<span data-ttu-id="99ca7-124">GET /api/todo</span><span class="sxs-lookup"><span data-stu-id="99ca7-124">GET /api/todo</span></span> | <span data-ttu-id="99ca7-125">Ottiene tutti gli elementi attività</span><span class="sxs-lookup"><span data-stu-id="99ca7-125">Get all to-do items</span></span> | <span data-ttu-id="99ca7-126">nessuno</span><span class="sxs-lookup"><span data-stu-id="99ca7-126">None</span></span> | <span data-ttu-id="99ca7-127">Matrice di elementi attività</span><span class="sxs-lookup"><span data-stu-id="99ca7-127">Array of to-do items</span></span>|
|<span data-ttu-id="99ca7-128">GET /api/todo/{id}</span><span class="sxs-lookup"><span data-stu-id="99ca7-128">GET /api/todo/{id}</span></span> | <span data-ttu-id="99ca7-129">Ottiene un elemento in base all'ID</span><span class="sxs-lookup"><span data-stu-id="99ca7-129">Get an item by ID</span></span> | <span data-ttu-id="99ca7-130">nessuno</span><span class="sxs-lookup"><span data-stu-id="99ca7-130">None</span></span> | <span data-ttu-id="99ca7-131">Elemento attività</span><span class="sxs-lookup"><span data-stu-id="99ca7-131">To-do item</span></span>|
|<span data-ttu-id="99ca7-132">POST /api/todo</span><span class="sxs-lookup"><span data-stu-id="99ca7-132">POST /api/todo</span></span> | <span data-ttu-id="99ca7-133">Aggiunge un nuovo elemento</span><span class="sxs-lookup"><span data-stu-id="99ca7-133">Add a new item</span></span> | <span data-ttu-id="99ca7-134">Elemento attività</span><span class="sxs-lookup"><span data-stu-id="99ca7-134">To-do item</span></span> | <span data-ttu-id="99ca7-135">Elemento attività</span><span class="sxs-lookup"><span data-stu-id="99ca7-135">To-do item</span></span> |
|<span data-ttu-id="99ca7-136">PUT /api/todo/{id}</span><span class="sxs-lookup"><span data-stu-id="99ca7-136">PUT /api/todo/{id}</span></span> | <span data-ttu-id="99ca7-137">Aggiorna un elemento esistente &nbsp;</span><span class="sxs-lookup"><span data-stu-id="99ca7-137">Update an existing item &nbsp;</span></span> | <span data-ttu-id="99ca7-138">Elemento attività</span><span class="sxs-lookup"><span data-stu-id="99ca7-138">To-do item</span></span> | <span data-ttu-id="99ca7-139">nessuno</span><span class="sxs-lookup"><span data-stu-id="99ca7-139">None</span></span> |
|<span data-ttu-id="99ca7-140">DELETE /api/todo/{id} &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="99ca7-140">DELETE /api/todo/{id} &nbsp; &nbsp;</span></span> | <span data-ttu-id="99ca7-141">Elimina un elemento &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="99ca7-141">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="99ca7-142">nessuno</span><span class="sxs-lookup"><span data-stu-id="99ca7-142">None</span></span> | <span data-ttu-id="99ca7-143">nessuno</span><span class="sxs-lookup"><span data-stu-id="99ca7-143">None</span></span>|

<span data-ttu-id="99ca7-144">Il diagramma seguente visualizza la struttura dell'app.</span><span class="sxs-lookup"><span data-stu-id="99ca7-144">The following diagram shows the design of the app.</span></span>

![Il client è rappresentato da una casella a sinistra e invia una richiesta e riceve una risposta dall'applicazione, una casella a destra.](first-web-api/_static/architecture.png)

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-web-project"></a><span data-ttu-id="99ca7-149">Creare un progetto Web</span><span class="sxs-lookup"><span data-stu-id="99ca7-149">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="99ca7-150">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="99ca7-150">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="99ca7-151">Scegliere **Nuovo** > **Progetto** dal menu **File**.</span><span class="sxs-lookup"><span data-stu-id="99ca7-151">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="99ca7-152">Selezionare il modello **Applicazione Web ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="99ca7-152">Select the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="99ca7-153">Assegnare al progetto il nome *TodoApi* e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="99ca7-153">Name the project *TodoApi* and click **OK**.</span></span>
* <span data-ttu-id="99ca7-154">Nella finestra di dialogo **Nuova Applicazione Web ASP.NET Core - TodoApi** scegliere la versione per ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="99ca7-154">In the **New ASP.NET Core Web Application - TodoApi** dialog, choose the ASP.NET Core version.</span></span> <span data-ttu-id="99ca7-155">Selezionare il modello **API**, quindi scegliere **OK**.</span><span class="sxs-lookup"><span data-stu-id="99ca7-155">Select the **API** template and click **OK**.</span></span> <span data-ttu-id="99ca7-156">**Non** selezionare **Abilita supporto Docker**.</span><span class="sxs-lookup"><span data-stu-id="99ca7-156">Do **not** select **Enable Docker Support**.</span></span>

![Finestra di dialogo Nuovo progetto di Visual Studio](first-web-api/_static/vs.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="99ca7-158">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="99ca7-158">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="99ca7-159">Aprire il [terminale integrato](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="99ca7-159">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="99ca7-160">Cambiare directory (`cd`) e passare alla cartella che conterrà la cartella del progetto.</span><span class="sxs-lookup"><span data-stu-id="99ca7-160">Change directories (`cd`) to the folder which will contain the project folder.</span></span>
* <span data-ttu-id="99ca7-161">Eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="99ca7-161">Run the following commands:</span></span>

   ```console
   dotnet new webapi -o TodoApi
   code -r TodoApi
   ```

  <span data-ttu-id="99ca7-162">Questi comandi creano un nuovo progetto API Web e aprono una nuova istanza di Visual Studio Code nella nuova cartella di progetto.</span><span class="sxs-lookup"><span data-stu-id="99ca7-162">These commands create a new web API project and open a new instance of Visual Studio Code in the new project folder.</span></span>

* <span data-ttu-id="99ca7-163">Quando una finestra di dialogo chiede se si vuole aggiungere gli asset necessari al progetto, selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="99ca7-163">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="99ca7-164">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="99ca7-164">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="99ca7-165">Selezionare **File** > **Nuova soluzione**.</span><span class="sxs-lookup"><span data-stu-id="99ca7-165">Select **File** > **New Solution**.</span></span>

  ![Nuova soluzione macOS](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="99ca7-167">Selezionare **.NET Core App** > **ASP.NET Core Web API** > **Next**.</span><span class="sxs-lookup"><span data-stu-id="99ca7-167">Select **.NET Core App** > **ASP.NET Core Web API** > **Next**.</span></span>

  ![Finestra di dialogo Nuovo progetto di macOS](first-web-api-mac/_static/1.png)
  
* <span data-ttu-id="99ca7-169">Nella finestra di dialogo **Configure your new ASP.NET Core Web API** (Configura la nuova API Web ASP.NET Core), accettare l'impostazione predefinita di **Framework di destinazione**, ovvero \**.NET Core 2.2*.</span><span class="sxs-lookup"><span data-stu-id="99ca7-169">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of \**.NET Core 2.2*.</span></span>

* <span data-ttu-id="99ca7-170">Immettere *TodoApi* in **Nome progetto**, quindi selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="99ca7-170">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![Finestra di dialogo di configurazione](first-web-api-mac/_static/2.png)

---

### <a name="test-the-api"></a><span data-ttu-id="99ca7-172">Testare l'API</span><span class="sxs-lookup"><span data-stu-id="99ca7-172">Test the API</span></span>

<span data-ttu-id="99ca7-173">Il modello di progetto crea un'API `values`.</span><span class="sxs-lookup"><span data-stu-id="99ca7-173">The project template creates a `values` API.</span></span> <span data-ttu-id="99ca7-174">Chiamare il metodo `Get` da un browser per eseguire il test dell'app.</span><span class="sxs-lookup"><span data-stu-id="99ca7-174">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="99ca7-175">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="99ca7-175">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="99ca7-176">Premere CTRL+F5 per eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="99ca7-176">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="99ca7-177">Visual Studio apre un browser e naviga all'indirizzo `https://localhost:<port>/api/values`, dove `<port>` è un numero di porta selezionato a caso.</span><span class="sxs-lookup"><span data-stu-id="99ca7-177">Visual Studio launches a browser and navigates to `https://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="99ca7-178">Se in una finestra di dialogo viene chiesto se considerare attendibile il certificato IIS Express, selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="99ca7-178">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="99ca7-179">Nella finestra di dialogo **Avviso di sicurezza** successiva selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="99ca7-179">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="99ca7-180">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="99ca7-180">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="99ca7-181">Premere CTRL+F5 per eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="99ca7-181">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="99ca7-182">In un browser passare all'URL seguente: [https://localhost:5001/api/values](https://localhost:5001/api/values).</span><span class="sxs-lookup"><span data-stu-id="99ca7-182">In a browser, go to following URL: [https://localhost:5001/api/values](https://localhost:5001/api/values).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="99ca7-183">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="99ca7-183">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="99ca7-184">Selezionare **Esegui** > **Avvia eseguendo il debug** per avviare l'app.</span><span class="sxs-lookup"><span data-stu-id="99ca7-184">Select **Run** > **Start With Debugging** to launch the app.</span></span> <span data-ttu-id="99ca7-185">Visual Studio per Mac apre un browser e naviga all'indirizzo `https://localhost:<port>`, dove `<port>` è un numero di porta selezionato a caso.</span><span class="sxs-lookup"><span data-stu-id="99ca7-185">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="99ca7-186">Viene restituito un errore HTTP 404 (Non trovato).</span><span class="sxs-lookup"><span data-stu-id="99ca7-186">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="99ca7-187">Accodare `/api/values` all'URL (impostare l'URL su `https://localhost:<port>/api/values`).</span><span class="sxs-lookup"><span data-stu-id="99ca7-187">Append `/api/values` to the URL (change the URL to `https://localhost:<port>/api/values`).</span></span>

---

<span data-ttu-id="99ca7-188">Viene restituito il codice JSON seguente:</span><span class="sxs-lookup"><span data-stu-id="99ca7-188">The following JSON is returned:</span></span>

```json
["value1","value2"]
```

## <a name="add-a-model-class"></a><span data-ttu-id="99ca7-189">Aggiungere una classe del modello</span><span class="sxs-lookup"><span data-stu-id="99ca7-189">Add a model class</span></span>

<span data-ttu-id="99ca7-190">Un *modello* è un set di classi che rappresentano i dati gestiti dall'app.</span><span class="sxs-lookup"><span data-stu-id="99ca7-190">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="99ca7-191">Il modello per questa app è una classe `TodoItem` singola.</span><span class="sxs-lookup"><span data-stu-id="99ca7-191">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="99ca7-192">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="99ca7-192">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="99ca7-193">In **Esplora soluzioni** fare clic con il pulsante destro del mouse sul progetto.</span><span class="sxs-lookup"><span data-stu-id="99ca7-193">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="99ca7-194">Selezionare **Aggiungi** > **Nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="99ca7-194">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="99ca7-195">Assegnare il nome *Modelli* alla cartella.</span><span class="sxs-lookup"><span data-stu-id="99ca7-195">Name the folder *Models*.</span></span>

* <span data-ttu-id="99ca7-196">Fare clic con il pulsante destro del mouse sulla cartella *Models* e scegliere **Aggiungi** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="99ca7-196">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="99ca7-197">Assegnare alla classe il nome *TodoItem* e selezionare **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="99ca7-197">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="99ca7-198">Sostituire il codice del modello con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="99ca7-198">Replace the template code with the following code:</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="99ca7-199">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="99ca7-199">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="99ca7-200">Aggiungere una cartella denominata *Modelli*.</span><span class="sxs-lookup"><span data-stu-id="99ca7-200">Add a folder named *Models*.</span></span>

* <span data-ttu-id="99ca7-201">Aggiungere una classe `TodoItem` alla cartella *Models* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="99ca7-201">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="99ca7-202">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="99ca7-202">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="99ca7-203">Fare clic con il pulsante destro del mouse sul progetto.</span><span class="sxs-lookup"><span data-stu-id="99ca7-203">Right-click the project.</span></span> <span data-ttu-id="99ca7-204">Selezionare **Aggiungi** > **Nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="99ca7-204">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="99ca7-205">Assegnare il nome *Models* alla cartella.</span><span class="sxs-lookup"><span data-stu-id="99ca7-205">Name the folder *Models*.</span></span>

  ![Nuova cartella](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="99ca7-207">Fare clic con il pulsante destro del mouse sulla cartella *Models* e selezionare **Aggiungi** > **Nuovo file** > **Generale** > **Classe vuota**.</span><span class="sxs-lookup"><span data-stu-id="99ca7-207">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="99ca7-208">Assegnare alla classe il nome *TodoItem* e fare clic su **Nuovo**.</span><span class="sxs-lookup"><span data-stu-id="99ca7-208">Name the class *TodoItem*, and then click **New**.</span></span>

* <span data-ttu-id="99ca7-209">Sostituire il codice del modello con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="99ca7-209">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="99ca7-210">La proprietà `Id` funziona come chiave univoca in un database relazionale.</span><span class="sxs-lookup"><span data-stu-id="99ca7-210">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="99ca7-211">È possibile inserire le classi del modello in qualsiasi punto del progetto, ma per convenzione viene usata la cartella *Models*.</span><span class="sxs-lookup"><span data-stu-id="99ca7-211">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="99ca7-212">Aggiungere un contesto di database</span><span class="sxs-lookup"><span data-stu-id="99ca7-212">Add a database context</span></span>

<span data-ttu-id="99ca7-213">Il *contesto di database* è la classe principale che coordina le funzionalità di Entity Framework per un modello di dati.</span><span class="sxs-lookup"><span data-stu-id="99ca7-213">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="99ca7-214">Questa classe viene creata mediante derivazione dalla classe `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="99ca7-214">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="99ca7-215">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="99ca7-215">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="99ca7-216">Fare clic con il pulsante destro del mouse sulla cartella *Models* e scegliere **Aggiungi** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="99ca7-216">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="99ca7-217">Assegnare alla classe il nome *TodoContext* e fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="99ca7-217">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="99ca7-218">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="99ca7-218">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="99ca7-219">Aggiungere una classe `TodoContext` alla cartella *Models*.</span><span class="sxs-lookup"><span data-stu-id="99ca7-219">Add a `TodoContext` class to the *Models* folder.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="99ca7-220">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="99ca7-220">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="99ca7-221">Aggiungere una classe `TodoContext` alla cartella *Models*:</span><span class="sxs-lookup"><span data-stu-id="99ca7-221">Add a `TodoContext` class in the *Models* folder:</span></span>

---

* <span data-ttu-id="99ca7-222">Sostituire il codice del modello con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="99ca7-222">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="99ca7-223">Registrare il contesto del database</span><span class="sxs-lookup"><span data-stu-id="99ca7-223">Register the database context</span></span>

<span data-ttu-id="99ca7-224">In ASP.NET Core i servizi come il contesto del database devono essere registrati con il [contenitore di inserimento delle dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="99ca7-224">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="99ca7-225">Il contenitore rende disponibile il servizio ai controller.</span><span class="sxs-lookup"><span data-stu-id="99ca7-225">The container provides the service to controllers.</span></span>

<span data-ttu-id="99ca7-226">Aggiornare *Startup.cs* con il codice evidenziato seguente:</span><span class="sxs-lookup"><span data-stu-id="99ca7-226">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup1.cs?highlight=5,8,25-26&name=snippet_all)]

<span data-ttu-id="99ca7-227">Il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="99ca7-227">The preceding code:</span></span>

* <span data-ttu-id="99ca7-228">Rimuove le dichiarazioni `using` inutilizzate.</span><span class="sxs-lookup"><span data-stu-id="99ca7-228">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="99ca7-229">Aggiunge il contesto del database al contenitore di inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="99ca7-229">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="99ca7-230">Specifica che il contesto del database userà un database in memoria.</span><span class="sxs-lookup"><span data-stu-id="99ca7-230">Specifies that the database context will use an in-memory database.</span></span>

## <a name="add-a-controller"></a><span data-ttu-id="99ca7-231">Aggiungere un controller</span><span class="sxs-lookup"><span data-stu-id="99ca7-231">Add a controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="99ca7-232">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="99ca7-232">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="99ca7-233">Fare clic con il pulsante destro del mouse sulla cartella *Controllers*.</span><span class="sxs-lookup"><span data-stu-id="99ca7-233">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="99ca7-234">Selezionare **Aggiungi** > **Nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="99ca7-234">Select **Add** > **New Item**.</span></span>
* <span data-ttu-id="99ca7-235">Nella finestra di dialogo **Aggiungi nuovo elemento** selezionare il modello **API Controller Class** (Classe controller API).</span><span class="sxs-lookup"><span data-stu-id="99ca7-235">In the **Add New Item** dialog, select the **API Controller Class** template.</span></span>
* <span data-ttu-id="99ca7-236">Assegnare alla classe il nome *TodoController* e selezionare **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="99ca7-236">Name the class *TodoController*, and select **Add**.</span></span>

  ![Finestra di dialogo Aggiungi nuovo elemento con controller nella casella di ricerca e controller API Web selezionato](first-web-api/_static/new_controller.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="99ca7-238">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="99ca7-238">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="99ca7-239">Nella cartella *Controllers* creare una classe denominata `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="99ca7-239">In the *Controllers* folder, create a class named `TodoController`.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="99ca7-240">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="99ca7-240">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="99ca7-241">Nella cartella *Controllers* aggiungere la classe `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="99ca7-241">In the *Controllers* folder, add the class `TodoController`.</span></span>

---

* <span data-ttu-id="99ca7-242">Sostituire il codice del modello con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="99ca7-242">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="99ca7-243">Il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="99ca7-243">The preceding code:</span></span>

* <span data-ttu-id="99ca7-244">Definisce una classe controller API senza metodi.</span><span class="sxs-lookup"><span data-stu-id="99ca7-244">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="99ca7-245">Contrassegnare la classe con l'attributo [`[ApiController]`](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute).</span><span class="sxs-lookup"><span data-stu-id="99ca7-245">Decorates the class with the [`[ApiController]`](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute.</span></span> <span data-ttu-id="99ca7-246">L'attributo indica che il controller risponde alle richieste di API Web.</span><span class="sxs-lookup"><span data-stu-id="99ca7-246">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="99ca7-247">Per informazioni sui comportamenti specifici consentiti dall'attributo, vedere [Annotation with ApiController attribute](xref:web-api/index#annotation-with-apicontroller-attribute) (Annotazione con l'attributo ApiController).</span><span class="sxs-lookup"><span data-stu-id="99ca7-247">For information about specific behaviors that the attribute enables, see [Annotation with ApiController attribute](xref:web-api/index#annotation-with-apicontroller-attribute).</span></span>
* <span data-ttu-id="99ca7-248">Usa l'inserimento delle dipendenze per inserire il contesto del database (`TodoContext`) nel controller.</span><span class="sxs-lookup"><span data-stu-id="99ca7-248">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="99ca7-249">Il contesto di database viene usato in ognuno dei metodi [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) nel controller.</span><span class="sxs-lookup"><span data-stu-id="99ca7-249">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>
* <span data-ttu-id="99ca7-250">Aggiunge un elemento denominato `Item1` al database se il database è vuoto.</span><span class="sxs-lookup"><span data-stu-id="99ca7-250">Adds an item named `Item1` to the database if the database is empty.</span></span> <span data-ttu-id="99ca7-251">Questo codice si trova nel costruttore, quindi viene eseguito ogni volta che è presente una nuova richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="99ca7-251">This code is in the constructor, so it runs every time there's a new HTTP request.</span></span> <span data-ttu-id="99ca7-252">Se si eliminano tutti gli elementi, il costruttore crea di nuovo `Item1` quando viene nuovamente chiamato un metodo API.</span><span class="sxs-lookup"><span data-stu-id="99ca7-252">If you delete all items, the constructor creates `Item1` again the next time an API method is called.</span></span> <span data-ttu-id="99ca7-253">Potrebbe pertanto sembrare che l'eliminazione non abbia funzionato, mentre di fatto ha funzionato.</span><span class="sxs-lookup"><span data-stu-id="99ca7-253">So it may look like the deletion didn't work when it actually did work.</span></span>

## <a name="add-get-methods"></a><span data-ttu-id="99ca7-254">Aggiungere metodi Get</span><span class="sxs-lookup"><span data-stu-id="99ca7-254">Add Get methods</span></span>

<span data-ttu-id="99ca7-255">Per specificare un'API che recupera elementi attività, aggiungere i metodi seguenti alla classe `TodoController`:</span><span class="sxs-lookup"><span data-stu-id="99ca7-255">To provide an API that retrieves to-do items, add the following methods to the `TodoController` class:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

<span data-ttu-id="99ca7-256">Questi metodi implementano due metodi GET:</span><span class="sxs-lookup"><span data-stu-id="99ca7-256">These methods implement two GET endpoints:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="99ca7-257">Testare l'app chiamando i due endpoint da un browser.</span><span class="sxs-lookup"><span data-stu-id="99ca7-257">Test the app by calling the two endpoints from a browser.</span></span> <span data-ttu-id="99ca7-258">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="99ca7-258">For example:</span></span>

* `https://localhost:<port>/api/todo`
* `https://localhost:<port>/api/todo/1`

<span data-ttu-id="99ca7-259">La chiamata a `GetTodoItems` genera la risposta HTTP seguente:</span><span class="sxs-lookup"><span data-stu-id="99ca7-259">The following HTTP response is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

## <a name="routing-and-url-paths"></a><span data-ttu-id="99ca7-260">Routing e percorsi di URL</span><span class="sxs-lookup"><span data-stu-id="99ca7-260">Routing and URL paths</span></span>

<span data-ttu-id="99ca7-261">L'attributo [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) indica un metodo che risponde a una richiesta HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="99ca7-261">The [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="99ca7-262">Il percorso dell'URL per ogni metodo viene costruito nel modo seguente:</span><span class="sxs-lookup"><span data-stu-id="99ca7-262">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="99ca7-263">Iniziare con la stringa di modello nell'attributo `Route` del controller:</span><span class="sxs-lookup"><span data-stu-id="99ca7-263">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

* <span data-ttu-id="99ca7-264">Sostituire `[controller]` con il nome del controller, ovvero, per convenzione, il nome della classe controller meno il suffisso "Controller".</span><span class="sxs-lookup"><span data-stu-id="99ca7-264">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="99ca7-265">In questo esempio il nome della classe controller è **Todo**Controller, pertanto il nome del controller è "todo".</span><span class="sxs-lookup"><span data-stu-id="99ca7-265">For this sample, the controller class name is **Todo**Controller, so the controller name is "todo".</span></span> <span data-ttu-id="99ca7-266">Il [routing](xref:mvc/controllers/routing) ASP.NET Core non fa distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="99ca7-266">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="99ca7-267">Se l'attributo `[HttpGet]` ha un modello di route, ad esempio `[HttpGet("/products")]`, aggiungerlo al percorso.</span><span class="sxs-lookup"><span data-stu-id="99ca7-267">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("/products")]`, append that to the path.</span></span> <span data-ttu-id="99ca7-268">In questo esempio non si usa un modello.</span><span class="sxs-lookup"><span data-stu-id="99ca7-268">This sample doesn't use a template.</span></span> <span data-ttu-id="99ca7-269">Per altre informazioni, vedere [Routing con attributi Http[verb]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="99ca7-269">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="99ca7-270">Nel metodo `GetTodoItem` seguente, `"{id}"` è una variabile segnaposto per l'identificatore univoco dell'elemento attività.</span><span class="sxs-lookup"><span data-stu-id="99ca7-270">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="99ca7-271">Quando viene chiamato `GetTodoItem`, il valore di `"{id}"` viene specificato per il metodo nel rispettivo parametro `id`.</span><span class="sxs-lookup"><span data-stu-id="99ca7-271">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its`id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

<span data-ttu-id="99ca7-272">Il parametro `Name = "GetTodo"` crea una route denominata.</span><span class="sxs-lookup"><span data-stu-id="99ca7-272">The `Name = "GetTodo"` parameter creates a named route.</span></span> <span data-ttu-id="99ca7-273">In seguito si vedrà come l'app può usare il nome per creare un collegamento HTTP usando il nome della route.</span><span class="sxs-lookup"><span data-stu-id="99ca7-273">You'll see later how the app can use the name to create an HTTP link using the route name.</span></span>

## <a name="return-values"></a><span data-ttu-id="99ca7-274">Valori restituiti</span><span class="sxs-lookup"><span data-stu-id="99ca7-274">Return values</span></span>

<span data-ttu-id="99ca7-275">Il tipo restituito dei metodi `GetTodoItems` e `GetTodoItem` è [ActionResult\<T>](xref:web-api/action-return-types#actionresultt-type).</span><span class="sxs-lookup"><span data-stu-id="99ca7-275">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="99ca7-276">ASP.NET Core serializza automaticamente l'oggetto su [JSON](https://www.json.org/) e scrive il codice JSON nel corpo del messaggio di risposta.</span><span class="sxs-lookup"><span data-stu-id="99ca7-276">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="99ca7-277">Il codice di risposta per questo tipo restituito è 200, presupponendo che non esistano eccezioni non gestite.</span><span class="sxs-lookup"><span data-stu-id="99ca7-277">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="99ca7-278">Le eccezioni non gestite vengono convertite in errori 5xx.</span><span class="sxs-lookup"><span data-stu-id="99ca7-278">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="99ca7-279">I tipi restituiti `ActionResult` possono rappresentare un ampio intervallo di codici di stato HTTP.</span><span class="sxs-lookup"><span data-stu-id="99ca7-279">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="99ca7-280">Ad esempio, `GetTodoItem` può restituire due valori di stato diversi:</span><span class="sxs-lookup"><span data-stu-id="99ca7-280">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="99ca7-281">Se nessun elemento corrisponde all'ID richiesto, il metodo restituisce un codice di errore 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound).</span><span class="sxs-lookup"><span data-stu-id="99ca7-281">If no item matches the requested ID, the method returns a 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) error code.</span></span>
* <span data-ttu-id="99ca7-282">In caso contrario, il metodo restituisce il codice 200 con un corpo della risposta JSON.</span><span class="sxs-lookup"><span data-stu-id="99ca7-282">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="99ca7-283">La restituzione di `item` risulta in una risposta HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="99ca7-283">Returning `item` results in an HTTP 200 response.</span></span>

## <a name="test-the-gettodoitems-method"></a><span data-ttu-id="99ca7-284">Testare il metodo GetTodoItems</span><span class="sxs-lookup"><span data-stu-id="99ca7-284">Test the GetTodoItems method</span></span>

<span data-ttu-id="99ca7-285">Questa esercitazione usa Postman per testare l'API Web.</span><span class="sxs-lookup"><span data-stu-id="99ca7-285">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="99ca7-286">Installare [Postman](https://www.getpostman.com/apps)</span><span class="sxs-lookup"><span data-stu-id="99ca7-286">Install [Postman](https://www.getpostman.com/apps)</span></span>
* <span data-ttu-id="99ca7-287">Avviare l'app Web.</span><span class="sxs-lookup"><span data-stu-id="99ca7-287">Start the web app.</span></span>
* <span data-ttu-id="99ca7-288">Avviare Postman.</span><span class="sxs-lookup"><span data-stu-id="99ca7-288">Start Postman.</span></span>
* <span data-ttu-id="99ca7-289">Disattivare **SSL certificate verification** (Verifica certificato SSL)</span><span class="sxs-lookup"><span data-stu-id="99ca7-289">Disable **SSL certificate verification**</span></span>
  
  * <span data-ttu-id="99ca7-290">From  **File > Settings** (File > Impostazioni) (scheda \**General* (Generale)), disattivare **SSL certificate verification** (Verifica certificato SSL).</span><span class="sxs-lookup"><span data-stu-id="99ca7-290">From  **File > Settings** (\**General* tab), disable **SSL certificate verification**.</span></span>
    > [!WARNING]
    > <span data-ttu-id="99ca7-291">Riattivare la verifica dei certificati SSL al termine del test del controller.</span><span class="sxs-lookup"><span data-stu-id="99ca7-291">Re-enable SSL certificate verification after testing the controller.</span></span>

* <span data-ttu-id="99ca7-292">Creare una nuova richiesta.</span><span class="sxs-lookup"><span data-stu-id="99ca7-292">Create a new request.</span></span>
  * <span data-ttu-id="99ca7-293">Impostare il metodo HTTP su **GET**.</span><span class="sxs-lookup"><span data-stu-id="99ca7-293">Set the HTTP method to **GET**.</span></span>
  * <span data-ttu-id="99ca7-294">Impostare l'URL della richiesta su `https://localhost:<port>/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="99ca7-294">Set the request URL to `https://localhost:<port>/api/todo`.</span></span> <span data-ttu-id="99ca7-295">Ad esempio `https://localhost:5001/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="99ca7-295">For example, `https://localhost:5001/api/todo`.</span></span>
* <span data-ttu-id="99ca7-296">Impostare **Two pane view** (Visualizzazione in due riquadri) in Postman.</span><span class="sxs-lookup"><span data-stu-id="99ca7-296">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="99ca7-297">Selezionare **Send** (Invia).</span><span class="sxs-lookup"><span data-stu-id="99ca7-297">Select **Send**.</span></span>

![Postman con richiesta GET](first-web-api/_static/2pv.png)

## <a name="add-a-create-method"></a><span data-ttu-id="99ca7-299">Aggiungere un metodo Create</span><span class="sxs-lookup"><span data-stu-id="99ca7-299">Add a Create method</span></span>

<span data-ttu-id="99ca7-300">Aggiungere il metodo `PostTodoItem` seguente:</span><span class="sxs-lookup"><span data-stu-id="99ca7-300">Add the following `PostTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="99ca7-301">Il codice precedente è un metodo HTTP POST, come indicato dall'attributo [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute).</span><span class="sxs-lookup"><span data-stu-id="99ca7-301">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="99ca7-302">Il metodo ottiene il valore dell'elemento attività dal corpo della richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="99ca7-302">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="99ca7-303">Il metodo `CreatedAtRoute`:</span><span class="sxs-lookup"><span data-stu-id="99ca7-303">The `CreatedAtRoute` method:</span></span>

* <span data-ttu-id="99ca7-304">Restituisce una risposta 201.</span><span class="sxs-lookup"><span data-stu-id="99ca7-304">Returns a 201 response.</span></span> <span data-ttu-id="99ca7-305">HTTP 201 è la risposta standard per un metodo HTTP POST che crea una nuova risorsa nel server.</span><span class="sxs-lookup"><span data-stu-id="99ca7-305">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="99ca7-306">Aggiunge un'intestazione Location alla risposta.</span><span class="sxs-lookup"><span data-stu-id="99ca7-306">Adds a Location header to the response.</span></span> <span data-ttu-id="99ca7-307">L'intestazione Location (Posizione) specifica l'URI dell'elemento attività appena creato.</span><span class="sxs-lookup"><span data-stu-id="99ca7-307">The Location header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="99ca7-308">Per altre informazioni, vedere [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="99ca7-308">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="99ca7-309">Usa la route denominata "GetTodo" per creare l'URL.</span><span class="sxs-lookup"><span data-stu-id="99ca7-309">Uses the "GetTodo" named route to create the URL.</span></span> <span data-ttu-id="99ca7-310">La route denominata "GetTodo" è definita in `GetTodoItem`:</span><span class="sxs-lookup"><span data-stu-id="99ca7-310">The "GetTodo" named route is defined in `GetTodoItem`:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

### <a name="test-the-posttodoitem-method"></a><span data-ttu-id="99ca7-311">Testare il metodo PostTodoItem</span><span class="sxs-lookup"><span data-stu-id="99ca7-311">Test the PostTodoItem method</span></span>

* <span data-ttu-id="99ca7-312">Compilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="99ca7-312">Build the project.</span></span>
* <span data-ttu-id="99ca7-313">In Postman impostare il metodo HTTP su `POST`.</span><span class="sxs-lookup"><span data-stu-id="99ca7-313">In Postman, set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="99ca7-314">Fare clic sulla scheda **Body** (Corpo).</span><span class="sxs-lookup"><span data-stu-id="99ca7-314">Select the **Body** tab.</span></span>
* <span data-ttu-id="99ca7-315">Selezionare il pulsante di opzione **raw** (non elaborato).</span><span class="sxs-lookup"><span data-stu-id="99ca7-315">Select the **raw** radio button.</span></span>
* <span data-ttu-id="99ca7-316">Impostare il tipo su **JSON (application/json)**.</span><span class="sxs-lookup"><span data-stu-id="99ca7-316">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="99ca7-317">Nel corpo della richiesta immettere codice JSON per un elemento attività:</span><span class="sxs-lookup"><span data-stu-id="99ca7-317">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="99ca7-318">Selezionare **Send** (Invia).</span><span class="sxs-lookup"><span data-stu-id="99ca7-318">Select **Send**.</span></span>

  ![Postman con richiesta di creazione](first-web-api/_static/create.png)

  <span data-ttu-id="99ca7-320">Se viene visualizzato un errore HTTP 405 - metodo non concesso, la causa probabile è che il progetto non è stato compilato dopo l'aggiunta del metodo `PostTodoItem`.</span><span class="sxs-lookup"><span data-stu-id="99ca7-320">If you get a 405 Method Not Allowed error, it's probably the result of not compiling the project after adding the after adding the `PostTodoItem` method.</span></span>

### <a name="test-the-location-header-uri"></a><span data-ttu-id="99ca7-321">Testare l'URI dell'intestazione della posizione</span><span class="sxs-lookup"><span data-stu-id="99ca7-321">Test the location header URI</span></span>

* <span data-ttu-id="99ca7-322">Selezionare la scheda **Headers** (Intestazioni) nel riquadro **Response** (Risposta).</span><span class="sxs-lookup"><span data-stu-id="99ca7-322">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="99ca7-323">Copiare il valore dell'intestazione **Location** (Posizione):</span><span class="sxs-lookup"><span data-stu-id="99ca7-323">Copy the **Location** header value:</span></span>

  ![Scheda Headers (Intestazioni) della console Postman](first-web-api/_static/pmc2.png)

* <span data-ttu-id="99ca7-325">Impostare il metodo su GET.</span><span class="sxs-lookup"><span data-stu-id="99ca7-325">Set the method to GET.</span></span>
* <span data-ttu-id="99ca7-326">Incollare l'URI (ad esempio `https://localhost:5001/api/Todo/2`)</span><span class="sxs-lookup"><span data-stu-id="99ca7-326">Paste the URI (for example, `https://localhost:5001/api/Todo/2`)</span></span>
* <span data-ttu-id="99ca7-327">Selezionare **Send** (Invia).</span><span class="sxs-lookup"><span data-stu-id="99ca7-327">Select **Send**.</span></span>

## <a name="add-a-puttodoitem-method"></a><span data-ttu-id="99ca7-328">Aggiungere un metodo PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="99ca7-328">Add a PutTodoItem method</span></span>

<span data-ttu-id="99ca7-329">Aggiungere il metodo `PutTodoItem` seguente:</span><span class="sxs-lookup"><span data-stu-id="99ca7-329">Add the following `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

<span data-ttu-id="99ca7-330">`PutTodoItem` è simile a `PostTodoItem` ma usa la richiesta HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="99ca7-330">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="99ca7-331">La risposta è [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="99ca7-331">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="99ca7-332">In base alla specifica HTTP, una richiesta PUT richiede che il client invii l'intera entità aggiornata e non solo le modifiche.</span><span class="sxs-lookup"><span data-stu-id="99ca7-332">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="99ca7-333">Per supportare gli aggiornamenti parziali, usare [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span><span class="sxs-lookup"><span data-stu-id="99ca7-333">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="99ca7-334">Testare il metodo PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="99ca7-334">Test the PutTodoItem method</span></span>

<span data-ttu-id="99ca7-335">Aggiornare l'elemento attività con id = 1 e impostarne il nome su "feed fish":</span><span class="sxs-lookup"><span data-stu-id="99ca7-335">Update the to-do item that has id = 1 and set its name to "feed fish":</span></span>

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="99ca7-336">L'immagine seguente visualizza l'aggiornamento di Postman:</span><span class="sxs-lookup"><span data-stu-id="99ca7-336">The following image shows the Postman update:</span></span>

![Console Postman con risposta 204 (No Content)](first-web-api/_static/pmcput.png)

## <a name="add-a-deletetodoitem-method"></a><span data-ttu-id="99ca7-338">Aggiungere un metodo DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="99ca7-338">Add a DeleteTodoItem method</span></span>

<span data-ttu-id="99ca7-339">Aggiungere il metodo `DeleteTodoItem` seguente:</span><span class="sxs-lookup"><span data-stu-id="99ca7-339">Add the following `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="99ca7-340">La risposta `DeleteTodoItem` è [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="99ca7-340">The `DeleteTodoItem` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="99ca7-341">Testare il metodo DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="99ca7-341">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="99ca7-342">Usare Postman per eliminare un elemento attività:</span><span class="sxs-lookup"><span data-stu-id="99ca7-342">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="99ca7-343">Impostare il metodo su `DELETE`.</span><span class="sxs-lookup"><span data-stu-id="99ca7-343">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="99ca7-344">Impostare l'URI dell'oggetto da eliminare, ad esempio `https://localhost:5001/api/todo/1`</span><span class="sxs-lookup"><span data-stu-id="99ca7-344">Set the URI of the object to delete, for example `https://localhost:5001/api/todo/1`</span></span>
* <span data-ttu-id="99ca7-345">Selezionare **Send** (Invia).</span><span class="sxs-lookup"><span data-stu-id="99ca7-345">Select **Send**</span></span>

<span data-ttu-id="99ca7-346">L'app di esempio consente di eliminare tutti gli elementi, ma quando viene eliminato l'ultimo elemento, ne viene creato uno nuovo dal costruttore della classe modello alla successiva chiamata dell'API.</span><span class="sxs-lookup"><span data-stu-id="99ca7-346">The sample app allows you to delete all the items, but when the last item is deleted, a new one is created by the model class constructor the next time the API is called.</span></span>

## <a name="call-the-api-with-jquery"></a><span data-ttu-id="99ca7-347">Chiamare l'API con jQuery</span><span class="sxs-lookup"><span data-stu-id="99ca7-347">Call the API with jQuery</span></span>

<span data-ttu-id="99ca7-348">In questa sezione viene aggiunta una pagina HTML che usa jQuery per chiamare l'API Web.</span><span class="sxs-lookup"><span data-stu-id="99ca7-348">In this section, an HTML page is added that uses jQuery to call the web api.</span></span> <span data-ttu-id="99ca7-349">jQuery avvia la richiesta e aggiorna la pagina con i dettagli ottenuti dalla risposta dell'API.</span><span class="sxs-lookup"><span data-stu-id="99ca7-349">jQuery initiates the request and updates the page with the details from the API's response.</span></span>

<span data-ttu-id="99ca7-350">Configurare l'app in modo che [gestisca i file statici](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) e [consenta il mapping di file predefinito](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_):</span><span class="sxs-lookup"><span data-stu-id="99ca7-350">Configure the app to [serve static files](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [enable default file mapping](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_):</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup.cs?highlight=14-15&name=snippet_configure)]

::: moniker range=">= aspnetcore-2.2"
<span data-ttu-id="99ca7-351">Creare una cartella *wwwroot* nella directory del progetto.</span><span class="sxs-lookup"><span data-stu-id="99ca7-351">Create a *wwwroot* folder in the project directory.</span></span>
::: moniker-end

<span data-ttu-id="99ca7-352">Aggiungere un file HTML denominato *index.html* alla directory *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="99ca7-352">Add an HTML file named *index.html* to the *wwwroot* directory.</span></span> <span data-ttu-id="99ca7-353">Sostituire il contenuto con il markup seguente:</span><span class="sxs-lookup"><span data-stu-id="99ca7-353">Replace its contents with the following markup:</span></span>

[!code-html[](first-web-api/samples/2.2/TodoApi/wwwroot/index.html)]

<span data-ttu-id="99ca7-354">Aggiungere un file JavaScript con nome *site.js* alla directory *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="99ca7-354">Add a JavaScript file named *site.js* to the *wwwroot* directory.</span></span> <span data-ttu-id="99ca7-355">Sostituire il contenuto con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="99ca7-355">Replace its contents with the following code:</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_SiteJs)]

<span data-ttu-id="99ca7-356">Può essere necessario modificare le impostazioni di avvio del progetto ASP.NET Core per il test della pagina HTML in locale:</span><span class="sxs-lookup"><span data-stu-id="99ca7-356">A change to the ASP.NET Core project's launch settings may be required to test the HTML page locally:</span></span>

* <span data-ttu-id="99ca7-357">Aprire *Properties\launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="99ca7-357">Open *Properties\launchSettings.json*.</span></span>
* <span data-ttu-id="99ca7-358">Rimuovere la proprietà `launchUrl` per forzare l'app ad aprire *index.html*, il file predefinito del progetto.</span><span class="sxs-lookup"><span data-stu-id="99ca7-358">Remove the `launchUrl` property to force the app to open at *index.html*&mdash;the project's default file.</span></span>

<span data-ttu-id="99ca7-359">È possibile ottenere jQuery in vari modi.</span><span class="sxs-lookup"><span data-stu-id="99ca7-359">There are several ways to get jQuery.</span></span> <span data-ttu-id="99ca7-360">Nel frammento precedente la libreria viene caricata da una rete CDN.</span><span class="sxs-lookup"><span data-stu-id="99ca7-360">In the preceding snippet, the library is loaded from a CDN.</span></span>

<span data-ttu-id="99ca7-361">In questo esempio vengono chiamati tutti i metodi CRUD dell'API.</span><span class="sxs-lookup"><span data-stu-id="99ca7-361">This sample calls all of the CRUD methods of the API.</span></span> <span data-ttu-id="99ca7-362">Di seguito sono incluse le spiegazioni delle chiamate all'API.</span><span class="sxs-lookup"><span data-stu-id="99ca7-362">Following are explanations of the calls to the API.</span></span>

### <a name="get-a-list-of-to-do-items"></a><span data-ttu-id="99ca7-363">Ottenere un elenco di elementi attività</span><span class="sxs-lookup"><span data-stu-id="99ca7-363">Get a list of to-do items</span></span>

<span data-ttu-id="99ca7-364">La funzione [ajax](https://api.jquery.com/jquery.ajax/) di jQuery invia una richiesta `GET` all'API, la quale restituisce codice JSON che rappresenta una matrice di elementi attività.</span><span class="sxs-lookup"><span data-stu-id="99ca7-364">The jQuery [ajax](https://api.jquery.com/jquery.ajax/) function sends a `GET` request to the API, which returns JSON representing an array of to-do items.</span></span> <span data-ttu-id="99ca7-365">La funzione di callback `success` viene chiamata se la richiesta ha esito positivo.</span><span class="sxs-lookup"><span data-stu-id="99ca7-365">The `success` callback function is invoked if the request succeeds.</span></span> <span data-ttu-id="99ca7-366">Nel callback il modello DOM viene aggiornato con le informazioni dell'elemento attività.</span><span class="sxs-lookup"><span data-stu-id="99ca7-366">In the callback, the DOM is updated with the to-do information.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_GetData)]

### <a name="add-a-to-do-item"></a><span data-ttu-id="99ca7-367">Aggiungere un elemento attività</span><span class="sxs-lookup"><span data-stu-id="99ca7-367">Add a to-do item</span></span>

<span data-ttu-id="99ca7-368">La funzione [ajax](https://api.jquery.com/jquery.ajax/) invia una richiesta `POST` con l'elemento attività nel corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="99ca7-368">The [ajax](https://api.jquery.com/jquery.ajax/) function sends a `POST` request with the to-do item in the request body.</span></span> <span data-ttu-id="99ca7-369">Le opzioni `accepts` e `contentType` sono impostate su `application/json` per specificare il tipo di supporto ricevuto e inviato.</span><span class="sxs-lookup"><span data-stu-id="99ca7-369">The `accepts` and `contentType` options are set to `application/json` to specify the media type being received and sent.</span></span> <span data-ttu-id="99ca7-370">L'elemento attività viene convertito in JSON usando [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span><span class="sxs-lookup"><span data-stu-id="99ca7-370">The to-do item is converted to JSON by using [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span></span> <span data-ttu-id="99ca7-371">Quando l'API restituisce un codice di stato di esecuzione riuscita, viene chiamata la funzione `getData` per aggiornare la tabella HTML.</span><span class="sxs-lookup"><span data-stu-id="99ca7-371">When the API returns a successful status code, the `getData` function is invoked to update the HTML table.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AddItem)]

### <a name="update-a-to-do-item"></a><span data-ttu-id="99ca7-372">Aggiornare un elemento attività</span><span class="sxs-lookup"><span data-stu-id="99ca7-372">Update a to-do item</span></span>

<span data-ttu-id="99ca7-373">L'aggiornamento di un elemento attività è simile all'aggiunta di un elemento di questo tipo.</span><span class="sxs-lookup"><span data-stu-id="99ca7-373">Updating a to-do item is similar to adding one.</span></span> <span data-ttu-id="99ca7-374">L'`url` cambia per includere l'identificatore univoco dell'elemento e `type` è `PUT`.</span><span class="sxs-lookup"><span data-stu-id="99ca7-374">The `url` changes to add the unique identifier of the item, and the `type` is `PUT`.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxPut)]

### <a name="delete-a-to-do-item"></a><span data-ttu-id="99ca7-375">Eliminare un elemento attività</span><span class="sxs-lookup"><span data-stu-id="99ca7-375">Delete a to-do item</span></span>

<span data-ttu-id="99ca7-376">È possibile eliminare un elemento attività impostando `type` su `DELETE` nella chiamata AJAX e specificando l'identificatore univoco dell'elemento nell'URL.</span><span class="sxs-lookup"><span data-stu-id="99ca7-376">Deleting a to-do item is accomplished by setting the `type` on the AJAX call to `DELETE` and specifying the item's unique identifier in the URL.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxDelete)]

## <a name="additional-resources"></a><span data-ttu-id="99ca7-377">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="99ca7-377">Additional resources</span></span>

<span data-ttu-id="99ca7-378">[Visualizzare o scaricare il codice di esempio per questa esercitazione](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span><span class="sxs-lookup"><span data-stu-id="99ca7-378">[View or download sample code for this tutorial](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span></span> <span data-ttu-id="99ca7-379">Vedere [come scaricare un esempio](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="99ca7-379">See [how to download](xref:index#how-to-download-a-sample).</span></span>

<span data-ttu-id="99ca7-380">Per altre informazioni, vedere le seguenti risorse:</span><span class="sxs-lookup"><span data-stu-id="99ca7-380">For more information, see the following resources:</span></span>

* <xref:web-api/index>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:data/ef-rp/index>
* <xref:mvc/controllers/routing>
* <xref:web-api/action-return-types>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/index>

## <a name="next-steps"></a><span data-ttu-id="99ca7-381">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="99ca7-381">Next steps</span></span>

<span data-ttu-id="99ca7-382">In questa esercitazione si è appreso come:</span><span class="sxs-lookup"><span data-stu-id="99ca7-382">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="99ca7-383">Creare un progetto di API Web.</span><span class="sxs-lookup"><span data-stu-id="99ca7-383">Create a web api project.</span></span>
> * <span data-ttu-id="99ca7-384">Aggiungere una classe modello.</span><span class="sxs-lookup"><span data-stu-id="99ca7-384">Add a model class.</span></span>
> * <span data-ttu-id="99ca7-385">Creare il contesto del database.</span><span class="sxs-lookup"><span data-stu-id="99ca7-385">Create the database context.</span></span>
> * <span data-ttu-id="99ca7-386">Registrare il contesto del database.</span><span class="sxs-lookup"><span data-stu-id="99ca7-386">Register the database context.</span></span>
> * <span data-ttu-id="99ca7-387">Aggiungere un controller.</span><span class="sxs-lookup"><span data-stu-id="99ca7-387">Add a controller.</span></span>
> * <span data-ttu-id="99ca7-388">Aggiungere metodi CRUD.</span><span class="sxs-lookup"><span data-stu-id="99ca7-388">Add CRUD methods.</span></span>
> * <span data-ttu-id="99ca7-389">Configurare routing e percorsi URL.</span><span class="sxs-lookup"><span data-stu-id="99ca7-389">Configure routing and URL paths.</span></span>
> * <span data-ttu-id="99ca7-390">Specificare valori restituiti.</span><span class="sxs-lookup"><span data-stu-id="99ca7-390">Specify return values.</span></span>
> * <span data-ttu-id="99ca7-391">Chiamare l'API Web con Postman.</span><span class="sxs-lookup"><span data-stu-id="99ca7-391">Call the web API with Postman.</span></span>
> * <span data-ttu-id="99ca7-392">Chiamare l'API Web con jQuery.</span><span class="sxs-lookup"><span data-stu-id="99ca7-392">Call the web api with jQuery.</span></span>

<span data-ttu-id="99ca7-393">Passare all'esercitazione successiva per apprendere come generare pagine della Guida dell'API:</span><span class="sxs-lookup"><span data-stu-id="99ca7-393">Advance to the next tutorial to learn how to generate API help pages:</span></span>

> [!div class="nextstepaction"]
> <xref:tutorials/get-started-with-swashbuckle>
