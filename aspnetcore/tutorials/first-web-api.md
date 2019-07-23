---
title: "Esercitazione: Creare un'API Web con ASP.NET Core"
author: rick-anderson
description: Informazioni su come creare un'API Web con ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 07/11/2019
uid: tutorials/first-web-api
ms.openlocfilehash: 95410cef9753fbb0eda6136320b59682e0553ea7
ms.sourcegitcommit: 040aedca220ed24ee1726e6886daf6906f95a028
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/15/2019
ms.locfileid: "67893126"
---
# <a name="tutorial-create-a-web-api-with-aspnet-core"></a><span data-ttu-id="3c621-103">Esercitazione: Creare un'API Web con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3c621-103">Tutorial: Create a web API with ASP.NET Core</span></span>

<span data-ttu-id="3c621-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT) e [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="3c621-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="3c621-105">Questa esercitazione illustra le nozioni di base della creazione di un'API Web con ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3c621-105">This tutorial teaches the basics of building a web API with ASP.NET Core.</span></span>

<span data-ttu-id="3c621-106">In questa esercitazione si imparerà a:</span><span class="sxs-lookup"><span data-stu-id="3c621-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="3c621-107">Creare un progetto di API Web.</span><span class="sxs-lookup"><span data-stu-id="3c621-107">Create a web API project.</span></span>
> * <span data-ttu-id="3c621-108">Aggiungere una classe modello.</span><span class="sxs-lookup"><span data-stu-id="3c621-108">Add a model class.</span></span>
> * <span data-ttu-id="3c621-109">Creare il contesto del database.</span><span class="sxs-lookup"><span data-stu-id="3c621-109">Create the database context.</span></span>
> * <span data-ttu-id="3c621-110">Registrare il contesto del database.</span><span class="sxs-lookup"><span data-stu-id="3c621-110">Register the database context.</span></span>
> * <span data-ttu-id="3c621-111">Aggiungere un controller.</span><span class="sxs-lookup"><span data-stu-id="3c621-111">Add a controller.</span></span>
> * <span data-ttu-id="3c621-112">Aggiungere metodi CRUD.</span><span class="sxs-lookup"><span data-stu-id="3c621-112">Add CRUD methods.</span></span>
> * <span data-ttu-id="3c621-113">Configurare routing e percorsi URL.</span><span class="sxs-lookup"><span data-stu-id="3c621-113">Configure routing and URL paths.</span></span>
> * <span data-ttu-id="3c621-114">Specificare valori restituiti.</span><span class="sxs-lookup"><span data-stu-id="3c621-114">Specify return values.</span></span>
> * <span data-ttu-id="3c621-115">Chiamare l'API Web con Postman.</span><span class="sxs-lookup"><span data-stu-id="3c621-115">Call the web API with Postman.</span></span>
> * <span data-ttu-id="3c621-116">Chiamare l'API Web con jQuery.</span><span class="sxs-lookup"><span data-stu-id="3c621-116">Call the web API with jQuery.</span></span>

<span data-ttu-id="3c621-117">Al termine si dispone di un'API web che può gestire gli elementi di tipo "attività" archiviati in un database relazionale.</span><span class="sxs-lookup"><span data-stu-id="3c621-117">At the end, you have a web API that can manage "to-do" items stored in a relational database.</span></span>

## <a name="overview"></a><span data-ttu-id="3c621-118">Panoramica</span><span class="sxs-lookup"><span data-stu-id="3c621-118">Overview</span></span>

<span data-ttu-id="3c621-119">Questa esercitazione consente di creare l'API seguente:</span><span class="sxs-lookup"><span data-stu-id="3c621-119">This tutorial creates the following API:</span></span>

|<span data-ttu-id="3c621-120">API</span><span class="sxs-lookup"><span data-stu-id="3c621-120">API</span></span> | <span data-ttu-id="3c621-121">DESCRIZIONE</span><span class="sxs-lookup"><span data-stu-id="3c621-121">Description</span></span> | <span data-ttu-id="3c621-122">Corpo della richiesta</span><span class="sxs-lookup"><span data-stu-id="3c621-122">Request body</span></span> | <span data-ttu-id="3c621-123">Corpo della risposta</span><span class="sxs-lookup"><span data-stu-id="3c621-123">Response body</span></span> |
|--- | ---- | ---- | ---- |
|<span data-ttu-id="3c621-124">GET /api/todo</span><span class="sxs-lookup"><span data-stu-id="3c621-124">GET /api/todo</span></span> | <span data-ttu-id="3c621-125">Ottiene tutti gli elementi attività</span><span class="sxs-lookup"><span data-stu-id="3c621-125">Get all to-do items</span></span> | <span data-ttu-id="3c621-126">nessuno</span><span class="sxs-lookup"><span data-stu-id="3c621-126">None</span></span> | <span data-ttu-id="3c621-127">Matrice di elementi attività</span><span class="sxs-lookup"><span data-stu-id="3c621-127">Array of to-do items</span></span>|
|<span data-ttu-id="3c621-128">GET /api/todo/{id}</span><span class="sxs-lookup"><span data-stu-id="3c621-128">GET /api/todo/{id}</span></span> | <span data-ttu-id="3c621-129">Ottiene un elemento in base all'ID</span><span class="sxs-lookup"><span data-stu-id="3c621-129">Get an item by ID</span></span> | <span data-ttu-id="3c621-130">nessuno</span><span class="sxs-lookup"><span data-stu-id="3c621-130">None</span></span> | <span data-ttu-id="3c621-131">Elemento attività</span><span class="sxs-lookup"><span data-stu-id="3c621-131">To-do item</span></span>|
|<span data-ttu-id="3c621-132">POST /api/todo</span><span class="sxs-lookup"><span data-stu-id="3c621-132">POST /api/todo</span></span> | <span data-ttu-id="3c621-133">Aggiunge un nuovo elemento</span><span class="sxs-lookup"><span data-stu-id="3c621-133">Add a new item</span></span> | <span data-ttu-id="3c621-134">Elemento attività</span><span class="sxs-lookup"><span data-stu-id="3c621-134">To-do item</span></span> | <span data-ttu-id="3c621-135">Elemento attività</span><span class="sxs-lookup"><span data-stu-id="3c621-135">To-do item</span></span> |
|<span data-ttu-id="3c621-136">PUT /api/todo/{id}</span><span class="sxs-lookup"><span data-stu-id="3c621-136">PUT /api/todo/{id}</span></span> | <span data-ttu-id="3c621-137">Aggiorna un elemento esistente &nbsp;</span><span class="sxs-lookup"><span data-stu-id="3c621-137">Update an existing item &nbsp;</span></span> | <span data-ttu-id="3c621-138">Elemento attività</span><span class="sxs-lookup"><span data-stu-id="3c621-138">To-do item</span></span> | <span data-ttu-id="3c621-139">nessuno</span><span class="sxs-lookup"><span data-stu-id="3c621-139">None</span></span> |
|<span data-ttu-id="3c621-140">DELETE /api/todo/{id} &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="3c621-140">DELETE /api/todo/{id} &nbsp; &nbsp;</span></span> | <span data-ttu-id="3c621-141">Elimina un elemento &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="3c621-141">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="3c621-142">nessuno</span><span class="sxs-lookup"><span data-stu-id="3c621-142">None</span></span> | <span data-ttu-id="3c621-143">nessuno</span><span class="sxs-lookup"><span data-stu-id="3c621-143">None</span></span>|

<span data-ttu-id="3c621-144">Il diagramma seguente visualizza la struttura dell'app.</span><span class="sxs-lookup"><span data-stu-id="3c621-144">The following diagram shows the design of the app.</span></span>

![Il client è rappresentato da una casella a sinistra.](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="3c621-150">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="3c621-150">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3c621-151">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3c621-151">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="3c621-152">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="3c621-152">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="3c621-153">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="3c621-153">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="3c621-154">Creare un progetto Web</span><span class="sxs-lookup"><span data-stu-id="3c621-154">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3c621-155">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3c621-155">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="3c621-156">Scegliere **Nuovo** > **Progetto** dal menu **File**.</span><span class="sxs-lookup"><span data-stu-id="3c621-156">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="3c621-157">Selezionare il modello **Applicazione Web ASP.NET Core** e fare clic su **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="3c621-157">Select the **ASP.NET Core Web Application** template and click **Next**.</span></span>
* <span data-ttu-id="3c621-158">Assegnare al progetto il nome *TodoApi* e fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="3c621-158">Name the project *TodoApi* and click **Create**.</span></span>
* <span data-ttu-id="3c621-159">Nella finestra di dialogo **Crea una nuova applicazione Web ASP.NET Core** verificare che siano selezionati **.NET Core** e **ASP.NET Core 2.2**.</span><span class="sxs-lookup"><span data-stu-id="3c621-159">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 2.2** are selected.</span></span> <span data-ttu-id="3c621-160">Selezionare il modello **API** e fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="3c621-160">Select the **API** template and click **Create**.</span></span> <span data-ttu-id="3c621-161">**Non** selezionare **Abilita supporto Docker**.</span><span class="sxs-lookup"><span data-stu-id="3c621-161">**Don't** select **Enable Docker Support**.</span></span>

![Finestra di dialogo Nuovo progetto di Visual Studio](first-web-api/_static/vs.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="3c621-163">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="3c621-163">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="3c621-164">Aprire il [terminale integrato](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="3c621-164">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="3c621-165">Cambiare directory (`cd`) e passare alla cartella che conterrà la cartella del progetto.</span><span class="sxs-lookup"><span data-stu-id="3c621-165">Change directories (`cd`) to the folder that will contain the project folder.</span></span>
* <span data-ttu-id="3c621-166">Eseguire i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="3c621-166">Run the following commands:</span></span>

   ```console
   dotnet new webapi -o TodoApi
   code -r TodoApi
   ```

  <span data-ttu-id="3c621-167">Questi comandi creano un nuovo progetto API Web e aprono una nuova istanza di Visual Studio Code nella nuova cartella di progetto.</span><span class="sxs-lookup"><span data-stu-id="3c621-167">These commands create a new web API project and open a new instance of Visual Studio Code in the new project folder.</span></span>

* <span data-ttu-id="3c621-168">Quando una finestra di dialogo chiede se si vuole aggiungere gli asset necessari al progetto, selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="3c621-168">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="3c621-169">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="3c621-169">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="3c621-170">Selezionare **File** > **Nuova soluzione**.</span><span class="sxs-lookup"><span data-stu-id="3c621-170">Select **File** > **New Solution**.</span></span>

  ![Nuova soluzione macOS](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="3c621-172">Selezionare **.NET Core** > **App** > **API** > **Avanti**.</span><span class="sxs-lookup"><span data-stu-id="3c621-172">Select **.NET Core** > **App** > **API** > **Next**.</span></span>

  ![Finestra di dialogo Nuovo progetto di macOS](first-web-api-mac/_static/1.png)
  
* <span data-ttu-id="3c621-174">Nella finestra di dialogo **Configure your new ASP.NET Core Web API** (Configura la nuova API Web ASP.NET Core), accettare l'impostazione predefinita di **Framework di destinazione**, ovvero \* *.NET Core 2.2*.</span><span class="sxs-lookup"><span data-stu-id="3c621-174">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of \**.NET Core 2.2*.</span></span>

* <span data-ttu-id="3c621-175">Immettere *TodoApi* in **Nome progetto**, quindi selezionare **Crea**.</span><span class="sxs-lookup"><span data-stu-id="3c621-175">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![Finestra di dialogo di configurazione](first-web-api-mac/_static/2.png)

---

### <a name="test-the-api"></a><span data-ttu-id="3c621-177">Testare l'API</span><span class="sxs-lookup"><span data-stu-id="3c621-177">Test the API</span></span>

<span data-ttu-id="3c621-178">Il modello di progetto crea un'API `values`.</span><span class="sxs-lookup"><span data-stu-id="3c621-178">The project template creates a `values` API.</span></span> <span data-ttu-id="3c621-179">Chiamare il metodo `Get` da un browser per eseguire il test dell'app.</span><span class="sxs-lookup"><span data-stu-id="3c621-179">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3c621-180">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3c621-180">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="3c621-181">Premere CTRL+F5 per eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="3c621-181">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="3c621-182">Visual Studio apre un browser e naviga all'indirizzo `https://localhost:<port>/api/values`, dove `<port>` è un numero di porta selezionato a caso.</span><span class="sxs-lookup"><span data-stu-id="3c621-182">Visual Studio launches a browser and navigates to `https://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="3c621-183">Se in una finestra di dialogo viene chiesto se considerare attendibile il certificato IIS Express, selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="3c621-183">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="3c621-184">Nella finestra di dialogo **Avviso di sicurezza** successiva selezionare **Sì**.</span><span class="sxs-lookup"><span data-stu-id="3c621-184">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="3c621-185">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="3c621-185">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="3c621-186">Premere CTRL+F5 per eseguire l'app.</span><span class="sxs-lookup"><span data-stu-id="3c621-186">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="3c621-187">In un browser passare all'URL seguente: [https://localhost:5001/api/values](https://localhost:5001/api/values).</span><span class="sxs-lookup"><span data-stu-id="3c621-187">In a browser, go to following URL: [https://localhost:5001/api/values](https://localhost:5001/api/values).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="3c621-188">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="3c621-188">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="3c621-189">Selezionare **Esegui** > **Avvia debug** per avviare l'app.</span><span class="sxs-lookup"><span data-stu-id="3c621-189">Select **Run** > **Start Debugging** to launch the app.</span></span> <span data-ttu-id="3c621-190">Visual Studio per Mac apre un browser e naviga all'indirizzo `https://localhost:<port>`, dove `<port>` è un numero di porta selezionato a caso.</span><span class="sxs-lookup"><span data-stu-id="3c621-190">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="3c621-191">Viene restituito un errore HTTP 404 (Non trovato).</span><span class="sxs-lookup"><span data-stu-id="3c621-191">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="3c621-192">Accodare `/api/values` all'URL (impostare l'URL su `https://localhost:<port>/api/values`).</span><span class="sxs-lookup"><span data-stu-id="3c621-192">Append `/api/values` to the URL (change the URL to `https://localhost:<port>/api/values`).</span></span>

---

<span data-ttu-id="3c621-193">Viene restituito il codice JSON seguente:</span><span class="sxs-lookup"><span data-stu-id="3c621-193">The following JSON is returned:</span></span>

```json
["value1","value2"]
```

## <a name="add-a-model-class"></a><span data-ttu-id="3c621-194">Aggiungere una classe del modello</span><span class="sxs-lookup"><span data-stu-id="3c621-194">Add a model class</span></span>

<span data-ttu-id="3c621-195">Un *modello* è un set di classi che rappresentano i dati gestiti dall'app.</span><span class="sxs-lookup"><span data-stu-id="3c621-195">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="3c621-196">Il modello per questa app è una classe `TodoItem` singola.</span><span class="sxs-lookup"><span data-stu-id="3c621-196">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3c621-197">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3c621-197">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="3c621-198">In **Esplora soluzioni** fare clic con il pulsante destro del mouse sul progetto.</span><span class="sxs-lookup"><span data-stu-id="3c621-198">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="3c621-199">Selezionare **Aggiungi** > **Nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="3c621-199">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="3c621-200">Assegnare il nome *Models* alla cartella.</span><span class="sxs-lookup"><span data-stu-id="3c621-200">Name the folder *Models*.</span></span>

* <span data-ttu-id="3c621-201">Fare clic con il pulsante destro del mouse sulla cartella *Models* e scegliere **Aggiungi** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="3c621-201">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="3c621-202">Assegnare alla classe il nome *TodoItem* e selezionare **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="3c621-202">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="3c621-203">Sostituire il codice del modello con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="3c621-203">Replace the template code with the following code:</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="3c621-204">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="3c621-204">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="3c621-205">Aggiungere una cartella denominata *Models*.</span><span class="sxs-lookup"><span data-stu-id="3c621-205">Add a folder named *Models*.</span></span>

* <span data-ttu-id="3c621-206">Aggiungere una classe `TodoItem` alla cartella *Models* con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="3c621-206">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="3c621-207">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="3c621-207">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="3c621-208">Fare clic con il pulsante destro del mouse sul progetto.</span><span class="sxs-lookup"><span data-stu-id="3c621-208">Right-click the project.</span></span> <span data-ttu-id="3c621-209">Selezionare **Aggiungi** > **Nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="3c621-209">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="3c621-210">Assegnare il nome *Models* alla cartella.</span><span class="sxs-lookup"><span data-stu-id="3c621-210">Name the folder *Models*.</span></span>

  ![Nuova cartella](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="3c621-212">Fare clic con il pulsante destro del mouse sulla cartella *Models* e selezionare **Aggiungi** > **Nuovo file** > **Generale** > **Classe vuota**.</span><span class="sxs-lookup"><span data-stu-id="3c621-212">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="3c621-213">Assegnare alla classe il nome *TodoItem* e fare clic su **Nuovo**.</span><span class="sxs-lookup"><span data-stu-id="3c621-213">Name the class *TodoItem*, and then click **New**.</span></span>

* <span data-ttu-id="3c621-214">Sostituire il codice del modello con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="3c621-214">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="3c621-215">La proprietà `Id` funziona come chiave univoca in un database relazionale.</span><span class="sxs-lookup"><span data-stu-id="3c621-215">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="3c621-216">È possibile inserire le classi del modello in qualsiasi punto del progetto, ma per convenzione viene usata la cartella *Models*.</span><span class="sxs-lookup"><span data-stu-id="3c621-216">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="3c621-217">Aggiungere un contesto di database</span><span class="sxs-lookup"><span data-stu-id="3c621-217">Add a database context</span></span>

<span data-ttu-id="3c621-218">Il *contesto di database* è la classe principale che coordina le funzionalità di Entity Framework per un modello di dati.</span><span class="sxs-lookup"><span data-stu-id="3c621-218">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="3c621-219">Questa classe viene creata mediante derivazione dalla classe `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="3c621-219">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3c621-220">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3c621-220">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="3c621-221">Fare clic con il pulsante destro del mouse sulla cartella *Models* e scegliere **Aggiungi** > **Classe**.</span><span class="sxs-lookup"><span data-stu-id="3c621-221">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="3c621-222">Assegnare alla classe il nome *TodoContext* e fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="3c621-222">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="3c621-223">Visual Studio Code/Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="3c621-223">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="3c621-224">Aggiungere una classe `TodoContext` alla cartella *Models*.</span><span class="sxs-lookup"><span data-stu-id="3c621-224">Add a `TodoContext` class to the *Models* folder.</span></span>

---

* <span data-ttu-id="3c621-225">Sostituire il codice del modello con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="3c621-225">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="3c621-226">Registrare il contesto del database</span><span class="sxs-lookup"><span data-stu-id="3c621-226">Register the database context</span></span>

<span data-ttu-id="3c621-227">In ASP.NET Core i servizi come il contesto del database devono essere registrati con il [contenitore di inserimento delle dipendenze](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="3c621-227">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="3c621-228">Il contenitore rende disponibile il servizio ai controller.</span><span class="sxs-lookup"><span data-stu-id="3c621-228">The container provides the service to controllers.</span></span>

<span data-ttu-id="3c621-229">Aggiornare *Startup.cs* con il codice evidenziato seguente:</span><span class="sxs-lookup"><span data-stu-id="3c621-229">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup1.cs?highlight=5,8,25-26&name=snippet_all)]

<span data-ttu-id="3c621-230">Il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="3c621-230">The preceding code:</span></span>

* <span data-ttu-id="3c621-231">Rimuove le dichiarazioni `using` inutilizzate.</span><span class="sxs-lookup"><span data-stu-id="3c621-231">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="3c621-232">Aggiunge il contesto del database al contenitore di inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="3c621-232">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="3c621-233">Specifica che il contesto del database userà un database in memoria.</span><span class="sxs-lookup"><span data-stu-id="3c621-233">Specifies that the database context will use an in-memory database.</span></span>

## <a name="add-a-controller"></a><span data-ttu-id="3c621-234">Aggiungere un controller</span><span class="sxs-lookup"><span data-stu-id="3c621-234">Add a controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3c621-235">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3c621-235">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="3c621-236">Fare clic con il pulsante destro del mouse sulla cartella *Controllers*.</span><span class="sxs-lookup"><span data-stu-id="3c621-236">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="3c621-237">Selezionare **Aggiungi** > **Nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="3c621-237">Select **Add** > **New Item**.</span></span>
* <span data-ttu-id="3c621-238">Nella finestra di dialogo **Aggiungi nuovo elemento** selezionare il modello **API Controller Class** (Classe controller API).</span><span class="sxs-lookup"><span data-stu-id="3c621-238">In the **Add New Item** dialog, select the **API Controller Class** template.</span></span>
* <span data-ttu-id="3c621-239">Assegnare alla classe il nome *TodoController* e selezionare **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="3c621-239">Name the class *TodoController*, and select **Add**.</span></span>

  ![Finestra di dialogo Aggiungi nuovo elemento con controller nella casella di ricerca e controller API Web selezionato](first-web-api/_static/new_controller.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="3c621-241">Visual Studio Code/Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="3c621-241">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="3c621-242">Nella cartella *Controllers* creare una classe denominata `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="3c621-242">In the *Controllers* folder, create a class named `TodoController`.</span></span>

---

* <span data-ttu-id="3c621-243">Sostituire il codice del modello con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="3c621-243">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="3c621-244">Il codice precedente:</span><span class="sxs-lookup"><span data-stu-id="3c621-244">The preceding code:</span></span>

* <span data-ttu-id="3c621-245">Definisce una classe controller API senza metodi.</span><span class="sxs-lookup"><span data-stu-id="3c621-245">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="3c621-246">Contrassegna la classe con l'attributo [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute).</span><span class="sxs-lookup"><span data-stu-id="3c621-246">Decorates the class with the [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute.</span></span> <span data-ttu-id="3c621-247">L'attributo indica che il controller risponde alle richieste di API Web.</span><span class="sxs-lookup"><span data-stu-id="3c621-247">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="3c621-248">Per informazioni sui comportamenti specifici consentiti dall'attributo, vedere <xref:web-api/index>.</span><span class="sxs-lookup"><span data-stu-id="3c621-248">For information about specific behaviors that the attribute enables, see <xref:web-api/index>.</span></span>
* <span data-ttu-id="3c621-249">Usa l'inserimento delle dipendenze per inserire il contesto del database (`TodoContext`) nel controller.</span><span class="sxs-lookup"><span data-stu-id="3c621-249">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="3c621-250">Il contesto di database viene usato in ognuno dei metodi [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) nel controller.</span><span class="sxs-lookup"><span data-stu-id="3c621-250">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>
* <span data-ttu-id="3c621-251">Aggiunge un elemento denominato `Item1` al database se il database è vuoto.</span><span class="sxs-lookup"><span data-stu-id="3c621-251">Adds an item named `Item1` to the database if the database is empty.</span></span> <span data-ttu-id="3c621-252">Questo codice si trova nel costruttore, quindi viene eseguito ogni volta che è presente una nuova richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="3c621-252">This code is in the constructor, so it runs every time there's a new HTTP request.</span></span> <span data-ttu-id="3c621-253">Se si eliminano tutti gli elementi, il costruttore crea di nuovo `Item1` quando viene nuovamente chiamato un metodo API.</span><span class="sxs-lookup"><span data-stu-id="3c621-253">If you delete all items, the constructor creates `Item1` again the next time an API method is called.</span></span> <span data-ttu-id="3c621-254">Potrebbe pertanto sembrare che l'eliminazione non abbia funzionato, mentre di fatto ha funzionato.</span><span class="sxs-lookup"><span data-stu-id="3c621-254">So it may look like the deletion didn't work when it actually did work.</span></span>

## <a name="add-get-methods"></a><span data-ttu-id="3c621-255">Aggiungere metodi Get</span><span class="sxs-lookup"><span data-stu-id="3c621-255">Add Get methods</span></span>

<span data-ttu-id="3c621-256">Per specificare un'API che recupera elementi attività, aggiungere i metodi seguenti alla classe `TodoController`:</span><span class="sxs-lookup"><span data-stu-id="3c621-256">To provide an API that retrieves to-do items, add the following methods to the `TodoController` class:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

<span data-ttu-id="3c621-257">Questi metodi implementano due metodi GET:</span><span class="sxs-lookup"><span data-stu-id="3c621-257">These methods implement two GET endpoints:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="3c621-258">Arrestare l'app se è ancora in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="3c621-258">Stop the app if it's still running.</span></span> <span data-ttu-id="3c621-259">Quindi eseguirla di nuovo per includere le modifiche più recenti.</span><span class="sxs-lookup"><span data-stu-id="3c621-259">Then run it again to include the latest changes.</span></span>

<span data-ttu-id="3c621-260">Testare l'app chiamando i due endpoint da un browser.</span><span class="sxs-lookup"><span data-stu-id="3c621-260">Test the app by calling the two endpoints from a browser.</span></span> <span data-ttu-id="3c621-261">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="3c621-261">For example:</span></span>

* `https://localhost:<port>/api/todo`
* `https://localhost:<port>/api/todo/1`

<span data-ttu-id="3c621-262">La chiamata a `GetTodoItems` genera la risposta HTTP seguente:</span><span class="sxs-lookup"><span data-stu-id="3c621-262">The following HTTP response is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

## <a name="routing-and-url-paths"></a><span data-ttu-id="3c621-263">Routing e percorsi di URL</span><span class="sxs-lookup"><span data-stu-id="3c621-263">Routing and URL paths</span></span>

<span data-ttu-id="3c621-264">L'attributo [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) indica un metodo che risponde a una richiesta HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="3c621-264">The [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="3c621-265">Il percorso dell'URL per ogni metodo viene costruito nel modo seguente:</span><span class="sxs-lookup"><span data-stu-id="3c621-265">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="3c621-266">Iniziare con la stringa di modello nell'attributo `Route` del controller:</span><span class="sxs-lookup"><span data-stu-id="3c621-266">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

* <span data-ttu-id="3c621-267">Sostituire `[controller]` con il nome del controller, ovvero, per convenzione, il nome della classe controller meno il suffisso "Controller".</span><span class="sxs-lookup"><span data-stu-id="3c621-267">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="3c621-268">In questo esempio il nome della classe controller è **Todo**Controller, pertanto il nome del controller è "todo".</span><span class="sxs-lookup"><span data-stu-id="3c621-268">For this sample, the controller class name is **Todo**Controller, so the controller name is "todo".</span></span> <span data-ttu-id="3c621-269">Il [routing](xref:mvc/controllers/routing) ASP.NET Core non fa distinzione tra maiuscole e minuscole.</span><span class="sxs-lookup"><span data-stu-id="3c621-269">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="3c621-270">Se l'attributo `[HttpGet]` ha un modello di route, ad esempio `[HttpGet("products")]`, aggiungerlo al percorso.</span><span class="sxs-lookup"><span data-stu-id="3c621-270">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="3c621-271">In questo esempio non si usa un modello.</span><span class="sxs-lookup"><span data-stu-id="3c621-271">This sample doesn't use a template.</span></span> <span data-ttu-id="3c621-272">Per altre informazioni, vedere [Routing con attributi Http[verb]](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span><span class="sxs-lookup"><span data-stu-id="3c621-272">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="3c621-273">Nel metodo `GetTodoItem` seguente, `"{id}"` è una variabile segnaposto per l'identificatore univoco dell'elemento attività.</span><span class="sxs-lookup"><span data-stu-id="3c621-273">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="3c621-274">Quando viene chiamato `GetTodoItem`, il valore di `"{id}"` viene specificato per il metodo nel rispettivo parametro `id`.</span><span class="sxs-lookup"><span data-stu-id="3c621-274">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its`id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a><span data-ttu-id="3c621-275">Valori restituiti</span><span class="sxs-lookup"><span data-stu-id="3c621-275">Return values</span></span>

<span data-ttu-id="3c621-276">Il tipo restituito dei metodi `GetTodoItems` e `GetTodoItem` è [ActionResult\<T>](xref:web-api/action-return-types#actionresultt-type).</span><span class="sxs-lookup"><span data-stu-id="3c621-276">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="3c621-277">ASP.NET Core serializza automaticamente l'oggetto su [JSON](https://www.json.org/) e scrive il codice JSON nel corpo del messaggio di risposta.</span><span class="sxs-lookup"><span data-stu-id="3c621-277">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="3c621-278">Il codice di risposta per questo tipo restituito è 200, presupponendo che non esistano eccezioni non gestite.</span><span class="sxs-lookup"><span data-stu-id="3c621-278">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="3c621-279">Le eccezioni non gestite vengono convertite in errori 5xx.</span><span class="sxs-lookup"><span data-stu-id="3c621-279">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="3c621-280">I tipi restituiti `ActionResult` possono rappresentare un ampio intervallo di codici di stato HTTP.</span><span class="sxs-lookup"><span data-stu-id="3c621-280">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="3c621-281">Ad esempio, `GetTodoItem` può restituire due valori di stato diversi:</span><span class="sxs-lookup"><span data-stu-id="3c621-281">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="3c621-282">Se nessun elemento corrisponde all'ID richiesto, il metodo restituisce un codice di errore 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound).</span><span class="sxs-lookup"><span data-stu-id="3c621-282">If no item matches the requested ID, the method returns a 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) error code.</span></span>
* <span data-ttu-id="3c621-283">In caso contrario, il metodo restituisce il codice 200 con un corpo della risposta JSON.</span><span class="sxs-lookup"><span data-stu-id="3c621-283">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="3c621-284">La restituzione di `item` risulta in una risposta HTTP 200.</span><span class="sxs-lookup"><span data-stu-id="3c621-284">Returning `item` results in an HTTP 200 response.</span></span>

## <a name="test-the-gettodoitems-method"></a><span data-ttu-id="3c621-285">Testare il metodo GetTodoItems</span><span class="sxs-lookup"><span data-stu-id="3c621-285">Test the GetTodoItems method</span></span>

<span data-ttu-id="3c621-286">Questa esercitazione usa Postman per testare l'API Web.</span><span class="sxs-lookup"><span data-stu-id="3c621-286">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="3c621-287">Installare [Postman](https://www.getpostman.com/downloads/)</span><span class="sxs-lookup"><span data-stu-id="3c621-287">Install [Postman](https://www.getpostman.com/downloads/)</span></span>
* <span data-ttu-id="3c621-288">Avviare l'app Web.</span><span class="sxs-lookup"><span data-stu-id="3c621-288">Start the web app.</span></span>
* <span data-ttu-id="3c621-289">Avviare Postman.</span><span class="sxs-lookup"><span data-stu-id="3c621-289">Start Postman.</span></span>
* <span data-ttu-id="3c621-290">Disattivare **SSL certificate verification** (Verifica certificato SSL)</span><span class="sxs-lookup"><span data-stu-id="3c621-290">Disable **SSL certificate verification**</span></span>
  
  * <span data-ttu-id="3c621-291">From  **File > Settings** (File > Impostazioni) (scheda \**General* (Generale)), disattivare **SSL certificate verification** (Verifica certificato SSL).</span><span class="sxs-lookup"><span data-stu-id="3c621-291">From  **File > Settings** (\**General* tab), disable **SSL certificate verification**.</span></span>
    > [!WARNING]
    > <span data-ttu-id="3c621-292">Riattivare la verifica dei certificati SSL al termine del test del controller.</span><span class="sxs-lookup"><span data-stu-id="3c621-292">Re-enable SSL certificate verification after testing the controller.</span></span>

* <span data-ttu-id="3c621-293">Creare una nuova richiesta.</span><span class="sxs-lookup"><span data-stu-id="3c621-293">Create a new request.</span></span>
  * <span data-ttu-id="3c621-294">Impostare il metodo HTTP su **GET**.</span><span class="sxs-lookup"><span data-stu-id="3c621-294">Set the HTTP method to **GET**.</span></span>
  * <span data-ttu-id="3c621-295">Impostare l'URL della richiesta su `https://localhost:<port>/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="3c621-295">Set the request URL to `https://localhost:<port>/api/todo`.</span></span> <span data-ttu-id="3c621-296">Ad esempio `https://localhost:5001/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="3c621-296">For example, `https://localhost:5001/api/todo`.</span></span>
* <span data-ttu-id="3c621-297">Impostare **Two pane view** (Visualizzazione in due riquadri) in Postman.</span><span class="sxs-lookup"><span data-stu-id="3c621-297">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="3c621-298">Selezionare **Send** (Invia).</span><span class="sxs-lookup"><span data-stu-id="3c621-298">Select **Send**.</span></span>

![Postman con richiesta GET](first-web-api/_static/2pv.png)

## <a name="add-a-create-method"></a><span data-ttu-id="3c621-300">Aggiungere un metodo Create</span><span class="sxs-lookup"><span data-stu-id="3c621-300">Add a Create method</span></span>

<span data-ttu-id="3c621-301">Aggiungere il metodo `PostTodoItem` seguente:</span><span class="sxs-lookup"><span data-stu-id="3c621-301">Add the following `PostTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="3c621-302">Il codice precedente è un metodo HTTP POST, come indicato dall'attributo [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute).</span><span class="sxs-lookup"><span data-stu-id="3c621-302">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="3c621-303">Il metodo ottiene il valore dell'elemento attività dal corpo della richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="3c621-303">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="3c621-304">Il metodo `CreatedAtAction`:</span><span class="sxs-lookup"><span data-stu-id="3c621-304">The `CreatedAtAction` method:</span></span>

* <span data-ttu-id="3c621-305">Restituisce un codice di stato HTTP 201 in caso di esito positivo.</span><span class="sxs-lookup"><span data-stu-id="3c621-305">Returns an HTTP 201 status code, if successful.</span></span> <span data-ttu-id="3c621-306">HTTP 201 è la risposta standard per un metodo HTTP POST che crea una nuova risorsa nel server.</span><span class="sxs-lookup"><span data-stu-id="3c621-306">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="3c621-307">Aggiunge un'intestazione `Location` alla risposta.</span><span class="sxs-lookup"><span data-stu-id="3c621-307">Adds a `Location` header to the response.</span></span> <span data-ttu-id="3c621-308">L'intestazione `Location` specifica l'URI dell'elemento attività appena creato.</span><span class="sxs-lookup"><span data-stu-id="3c621-308">The `Location` header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="3c621-309">Per altre informazioni, vedere [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="3c621-309">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="3c621-310">Fa riferimento all'azione `GetTodoItem` per creare l'URI dell'intestazione `Location`.</span><span class="sxs-lookup"><span data-stu-id="3c621-310">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="3c621-311">La parola chiave `nameof` C# viene usata per evitare di impostare il nome dell'azione come hardcoded nella chiamata a `CreatedAtAction`.</span><span class="sxs-lookup"><span data-stu-id="3c621-311">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

### <a name="test-the-posttodoitem-method"></a><span data-ttu-id="3c621-312">Testare il metodo PostTodoItem</span><span class="sxs-lookup"><span data-stu-id="3c621-312">Test the PostTodoItem method</span></span>

* <span data-ttu-id="3c621-313">Compilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="3c621-313">Build the project.</span></span>
* <span data-ttu-id="3c621-314">In Postman impostare il metodo HTTP su `POST`.</span><span class="sxs-lookup"><span data-stu-id="3c621-314">In Postman, set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="3c621-315">Fare clic sulla scheda **Body** (Corpo).</span><span class="sxs-lookup"><span data-stu-id="3c621-315">Select the **Body** tab.</span></span>
* <span data-ttu-id="3c621-316">Selezionare il pulsante di opzione **raw** (non elaborato).</span><span class="sxs-lookup"><span data-stu-id="3c621-316">Select the **raw** radio button.</span></span>
* <span data-ttu-id="3c621-317">Impostare il tipo su **JSON (application/json)** .</span><span class="sxs-lookup"><span data-stu-id="3c621-317">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="3c621-318">Nel corpo della richiesta immettere codice JSON per un elemento attività:</span><span class="sxs-lookup"><span data-stu-id="3c621-318">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="3c621-319">Selezionare **Send** (Invia).</span><span class="sxs-lookup"><span data-stu-id="3c621-319">Select **Send**.</span></span>

  ![Postman con richiesta di creazione](first-web-api/_static/create.png)

  <span data-ttu-id="3c621-321">Se viene visualizzato un errore HTTP 405 - Metodo non consentito, è probabile che il progetto non sia stato compilato dopo l'aggiunta del metodo `PostTodoItem`.</span><span class="sxs-lookup"><span data-stu-id="3c621-321">If you get a 405 Method Not Allowed error, it's probably the result of not compiling the project after adding the `PostTodoItem` method.</span></span>

### <a name="test-the-location-header-uri"></a><span data-ttu-id="3c621-322">Testare l'URI dell'intestazione della posizione</span><span class="sxs-lookup"><span data-stu-id="3c621-322">Test the location header URI</span></span>

* <span data-ttu-id="3c621-323">Selezionare la scheda **Headers** (Intestazioni) nel riquadro **Response** (Risposta).</span><span class="sxs-lookup"><span data-stu-id="3c621-323">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="3c621-324">Copiare il valore dell'intestazione **Location** (Posizione):</span><span class="sxs-lookup"><span data-stu-id="3c621-324">Copy the **Location** header value:</span></span>

  ![Scheda Headers (Intestazioni) della console Postman](first-web-api/_static/pmc2.png)

* <span data-ttu-id="3c621-326">Impostare il metodo su GET.</span><span class="sxs-lookup"><span data-stu-id="3c621-326">Set the method to GET.</span></span>
* <span data-ttu-id="3c621-327">Incollare l'URI (ad esempio `https://localhost:5001/api/Todo/2`)</span><span class="sxs-lookup"><span data-stu-id="3c621-327">Paste the URI (for example, `https://localhost:5001/api/Todo/2`)</span></span>
* <span data-ttu-id="3c621-328">Selezionare **Send** (Invia).</span><span class="sxs-lookup"><span data-stu-id="3c621-328">Select **Send**.</span></span>

## <a name="add-a-puttodoitem-method"></a><span data-ttu-id="3c621-329">Aggiungere un metodo PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="3c621-329">Add a PutTodoItem method</span></span>

<span data-ttu-id="3c621-330">Aggiungere il metodo `PutTodoItem` seguente:</span><span class="sxs-lookup"><span data-stu-id="3c621-330">Add the following `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

<span data-ttu-id="3c621-331">`PutTodoItem` è simile a `PostTodoItem` ma usa la richiesta HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="3c621-331">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="3c621-332">La risposta è [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="3c621-332">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="3c621-333">In base alla specifica HTTP, una richiesta PUT richiede che il client invii l'intera entità aggiornata e non solo le modifiche.</span><span class="sxs-lookup"><span data-stu-id="3c621-333">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="3c621-334">Per supportare gli aggiornamenti parziali, usare [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span><span class="sxs-lookup"><span data-stu-id="3c621-334">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="3c621-335">Se si riceve un errore chiamando `PutTodoItem`, chiamare `GET` per verificare che il database includa un elemento.</span><span class="sxs-lookup"><span data-stu-id="3c621-335">If you get an error calling `PutTodoItem`, call `GET` to ensure there's an item in the database.</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="3c621-336">Testare il metodo PutTodoItem</span><span class="sxs-lookup"><span data-stu-id="3c621-336">Test the PutTodoItem method</span></span>

<span data-ttu-id="3c621-337">Questo esempio usa un database in memoria che deve essere inizializzato ogni volta che l'app viene avviata.</span><span class="sxs-lookup"><span data-stu-id="3c621-337">This sample uses an in-memory database that must be initialed each time the app is started.</span></span> <span data-ttu-id="3c621-338">Deve esistere un elemento nel database prima di eseguire una chiamata PUT.</span><span class="sxs-lookup"><span data-stu-id="3c621-338">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="3c621-339">Chiamare GET per assicurarsi che sia presente un elemento nel database prima di eseguire una chiamata PUT.</span><span class="sxs-lookup"><span data-stu-id="3c621-339">Call GET to insure there's an item in the database before making a PUT call.</span></span>

<span data-ttu-id="3c621-340">Aggiornare l'elemento attività con id = 1 e impostarne il nome su "feed fish":</span><span class="sxs-lookup"><span data-stu-id="3c621-340">Update the to-do item that has id = 1 and set its name to "feed fish":</span></span>

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="3c621-341">L'immagine seguente visualizza l'aggiornamento di Postman:</span><span class="sxs-lookup"><span data-stu-id="3c621-341">The following image shows the Postman update:</span></span>

![Console Postman con risposta 204 (No Content)](first-web-api/_static/pmcput.png)

## <a name="add-a-deletetodoitem-method"></a><span data-ttu-id="3c621-343">Aggiungere un metodo DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="3c621-343">Add a DeleteTodoItem method</span></span>

<span data-ttu-id="3c621-344">Aggiungere il metodo `DeleteTodoItem` seguente:</span><span class="sxs-lookup"><span data-stu-id="3c621-344">Add the following `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="3c621-345">La risposta `DeleteTodoItem` è [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="3c621-345">The `DeleteTodoItem` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="3c621-346">Testare il metodo DeleteTodoItem</span><span class="sxs-lookup"><span data-stu-id="3c621-346">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="3c621-347">Usare Postman per eliminare un elemento attività:</span><span class="sxs-lookup"><span data-stu-id="3c621-347">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="3c621-348">Impostare il metodo su `DELETE`.</span><span class="sxs-lookup"><span data-stu-id="3c621-348">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="3c621-349">Impostare l'URI dell'oggetto da eliminare, ad esempio `https://localhost:5001/api/todo/1`</span><span class="sxs-lookup"><span data-stu-id="3c621-349">Set the URI of the object to delete, for example `https://localhost:5001/api/todo/1`</span></span>
* <span data-ttu-id="3c621-350">Selezionare **Send** (Invia).</span><span class="sxs-lookup"><span data-stu-id="3c621-350">Select **Send**</span></span>

<span data-ttu-id="3c621-351">L'app di esempio consente di eliminare tutti gli elementi.</span><span class="sxs-lookup"><span data-stu-id="3c621-351">The sample app allows you to delete all the items.</span></span> <span data-ttu-id="3c621-352">Quando viene eliminato l'ultimo elemento, tuttavia, ne viene creato uno nuovo dal costruttore della classe modello alla successiva chiamata dell'API.</span><span class="sxs-lookup"><span data-stu-id="3c621-352">However, when the last item is deleted, a new one is created by the model class constructor the next time the API is called.</span></span>

## <a name="call-the-api-with-jquery"></a><span data-ttu-id="3c621-353">Chiamare l'API con jQuery</span><span class="sxs-lookup"><span data-stu-id="3c621-353">Call the API with jQuery</span></span>

<span data-ttu-id="3c621-354">In questa sezione viene aggiunta una pagina HTML che usa jQuery per chiamare l'API Web.</span><span class="sxs-lookup"><span data-stu-id="3c621-354">In this section, an HTML page is added that uses jQuery to call the web api.</span></span> <span data-ttu-id="3c621-355">jQuery avvia la richiesta e aggiorna la pagina con i dettagli ottenuti dalla risposta dell'API.</span><span class="sxs-lookup"><span data-stu-id="3c621-355">jQuery initiates the request and updates the page with the details from the API's response.</span></span>

<span data-ttu-id="3c621-356">Configurare l'app per [servire file statici](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) e [abilitare il mapping del file predefinito](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) aggiornando *Startup.cs* con il codice evidenziato seguente:</span><span class="sxs-lookup"><span data-stu-id="3c621-356">Configure the app to [serve static files](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [enable default file mapping](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) by updating *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup.cs?highlight=14-15&name=snippet_configure)]

::: moniker range=">= aspnetcore-2.2"
<span data-ttu-id="3c621-357">Creare una cartella *wwwroot* nella directory del progetto.</span><span class="sxs-lookup"><span data-stu-id="3c621-357">Create a *wwwroot* folder in the project directory.</span></span>
::: moniker-end

<span data-ttu-id="3c621-358">Aggiungere un file HTML denominato *index.html* alla directory *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="3c621-358">Add an HTML file named *index.html* to the *wwwroot* directory.</span></span> <span data-ttu-id="3c621-359">Sostituire il contenuto con il markup seguente:</span><span class="sxs-lookup"><span data-stu-id="3c621-359">Replace its contents with the following markup:</span></span>

[!code-html[](first-web-api/samples/2.2/TodoApi/wwwroot/index.html)]

<span data-ttu-id="3c621-360">Aggiungere un file JavaScript con nome *site.js* alla directory *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="3c621-360">Add a JavaScript file named *site.js* to the *wwwroot* directory.</span></span> <span data-ttu-id="3c621-361">Sostituire il contenuto con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="3c621-361">Replace its contents with the following code:</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_SiteJs)]

<span data-ttu-id="3c621-362">Può essere necessario modificare le impostazioni di avvio del progetto ASP.NET Core per il test della pagina HTML in locale:</span><span class="sxs-lookup"><span data-stu-id="3c621-362">A change to the ASP.NET Core project's launch settings may be required to test the HTML page locally:</span></span>

* <span data-ttu-id="3c621-363">Aprire *Properties\launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="3c621-363">Open *Properties\launchSettings.json*.</span></span>
* <span data-ttu-id="3c621-364">Rimuovere la proprietà `launchUrl` per forzare l'app ad aprire *index.html*, il file predefinito del progetto.</span><span class="sxs-lookup"><span data-stu-id="3c621-364">Remove the `launchUrl` property to force the app to open at *index.html*&mdash;the project's default file.</span></span>

<span data-ttu-id="3c621-365">È possibile ottenere jQuery in vari modi.</span><span class="sxs-lookup"><span data-stu-id="3c621-365">There are several ways to get jQuery.</span></span> <span data-ttu-id="3c621-366">Nel frammento precedente la libreria viene caricata da una rete CDN.</span><span class="sxs-lookup"><span data-stu-id="3c621-366">In the preceding snippet, the library is loaded from a CDN.</span></span>

<span data-ttu-id="3c621-367">In questo esempio vengono chiamati tutti i metodi CRUD dell'API.</span><span class="sxs-lookup"><span data-stu-id="3c621-367">This sample calls all of the CRUD methods of the API.</span></span> <span data-ttu-id="3c621-368">Di seguito sono incluse le spiegazioni delle chiamate all'API.</span><span class="sxs-lookup"><span data-stu-id="3c621-368">Following are explanations of the calls to the API.</span></span>

### <a name="get-a-list-of-to-do-items"></a><span data-ttu-id="3c621-369">Ottenere un elenco di elementi attività</span><span class="sxs-lookup"><span data-stu-id="3c621-369">Get a list of to-do items</span></span>

<span data-ttu-id="3c621-370">La funzione [ajax](https://api.jquery.com/jquery.ajax/) di jQuery invia una richiesta `GET` all'API, la quale restituisce codice JSON che rappresenta una matrice di elementi attività.</span><span class="sxs-lookup"><span data-stu-id="3c621-370">The jQuery [ajax](https://api.jquery.com/jquery.ajax/) function sends a `GET` request to the API, which returns JSON representing an array of to-do items.</span></span> <span data-ttu-id="3c621-371">La funzione di callback `success` viene chiamata se la richiesta ha esito positivo.</span><span class="sxs-lookup"><span data-stu-id="3c621-371">The `success` callback function is invoked if the request succeeds.</span></span> <span data-ttu-id="3c621-372">Nel callback il modello DOM viene aggiornato con le informazioni dell'elemento attività.</span><span class="sxs-lookup"><span data-stu-id="3c621-372">In the callback, the DOM is updated with the to-do information.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_GetData)]

### <a name="add-a-to-do-item"></a><span data-ttu-id="3c621-373">Aggiungere un elemento attività</span><span class="sxs-lookup"><span data-stu-id="3c621-373">Add a to-do item</span></span>

<span data-ttu-id="3c621-374">La funzione [ajax](https://api.jquery.com/jquery.ajax/) invia una richiesta `POST` con l'elemento attività nel corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="3c621-374">The [ajax](https://api.jquery.com/jquery.ajax/) function sends a `POST` request with the to-do item in the request body.</span></span> <span data-ttu-id="3c621-375">Le opzioni `accepts` e `contentType` sono impostate su `application/json` per specificare il tipo di supporto ricevuto e inviato.</span><span class="sxs-lookup"><span data-stu-id="3c621-375">The `accepts` and `contentType` options are set to `application/json` to specify the media type being received and sent.</span></span> <span data-ttu-id="3c621-376">L'elemento attività viene convertito in JSON usando [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span><span class="sxs-lookup"><span data-stu-id="3c621-376">The to-do item is converted to JSON by using [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span></span> <span data-ttu-id="3c621-377">Quando l'API restituisce un codice di stato di esecuzione riuscita, viene chiamata la funzione `getData` per aggiornare la tabella HTML.</span><span class="sxs-lookup"><span data-stu-id="3c621-377">When the API returns a successful status code, the `getData` function is invoked to update the HTML table.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AddItem)]

### <a name="update-a-to-do-item"></a><span data-ttu-id="3c621-378">Aggiornare un elemento attività</span><span class="sxs-lookup"><span data-stu-id="3c621-378">Update a to-do item</span></span>

<span data-ttu-id="3c621-379">L'aggiornamento di un elemento attività è simile all'aggiunta di un elemento di questo tipo.</span><span class="sxs-lookup"><span data-stu-id="3c621-379">Updating a to-do item is similar to adding one.</span></span> <span data-ttu-id="3c621-380">L'`url` cambia per includere l'identificatore univoco dell'elemento e `type` è `PUT`.</span><span class="sxs-lookup"><span data-stu-id="3c621-380">The `url` changes to add the unique identifier of the item, and the `type` is `PUT`.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxPut)]

### <a name="delete-a-to-do-item"></a><span data-ttu-id="3c621-381">Eliminare un elemento attività</span><span class="sxs-lookup"><span data-stu-id="3c621-381">Delete a to-do item</span></span>

<span data-ttu-id="3c621-382">È possibile eliminare un elemento attività impostando `type` su `DELETE` nella chiamata AJAX e specificando l'identificatore univoco dell'elemento nell'URL.</span><span class="sxs-lookup"><span data-stu-id="3c621-382">Deleting a to-do item is accomplished by setting the `type` on the AJAX call to `DELETE` and specifying the item's unique identifier in the URL.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxDelete)]

## <a name="additional-resources"></a><span data-ttu-id="3c621-383">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="3c621-383">Additional resources</span></span>

<span data-ttu-id="3c621-384">[Visualizzare o scaricare il codice di esempio per questa esercitazione](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span><span class="sxs-lookup"><span data-stu-id="3c621-384">[View or download sample code for this tutorial](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span></span> <span data-ttu-id="3c621-385">Vedere [come scaricare un esempio](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="3c621-385">See [how to download](xref:index#how-to-download-a-sample).</span></span>

<span data-ttu-id="3c621-386">Per altre informazioni, vedere le seguenti risorse:</span><span class="sxs-lookup"><span data-stu-id="3c621-386">For more information, see the following resources:</span></span>

* <xref:web-api/index>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:data/ef-rp/index>
* <xref:mvc/controllers/routing>
* <xref:web-api/action-return-types>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/index>
* [<span data-ttu-id="3c621-387">Versione YouTube dell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="3c621-387">YouTube version of this tutorial</span></span>](https://www.youtube.com/watch?v=TTkhEyGBfAk)

## <a name="next-steps"></a><span data-ttu-id="3c621-388">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="3c621-388">Next steps</span></span>

<span data-ttu-id="3c621-389">In questa esercitazione si è appreso come:</span><span class="sxs-lookup"><span data-stu-id="3c621-389">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="3c621-390">Creare un progetto di API Web.</span><span class="sxs-lookup"><span data-stu-id="3c621-390">Create a web api project.</span></span>
> * <span data-ttu-id="3c621-391">Aggiungere una classe modello.</span><span class="sxs-lookup"><span data-stu-id="3c621-391">Add a model class.</span></span>
> * <span data-ttu-id="3c621-392">Creare il contesto del database.</span><span class="sxs-lookup"><span data-stu-id="3c621-392">Create the database context.</span></span>
> * <span data-ttu-id="3c621-393">Registrare il contesto del database.</span><span class="sxs-lookup"><span data-stu-id="3c621-393">Register the database context.</span></span>
> * <span data-ttu-id="3c621-394">Aggiungere un controller.</span><span class="sxs-lookup"><span data-stu-id="3c621-394">Add a controller.</span></span>
> * <span data-ttu-id="3c621-395">Aggiungere metodi CRUD.</span><span class="sxs-lookup"><span data-stu-id="3c621-395">Add CRUD methods.</span></span>
> * <span data-ttu-id="3c621-396">Configurare routing e percorsi URL.</span><span class="sxs-lookup"><span data-stu-id="3c621-396">Configure routing and URL paths.</span></span>
> * <span data-ttu-id="3c621-397">Specificare valori restituiti.</span><span class="sxs-lookup"><span data-stu-id="3c621-397">Specify return values.</span></span>
> * <span data-ttu-id="3c621-398">Chiamare l'API Web con Postman.</span><span class="sxs-lookup"><span data-stu-id="3c621-398">Call the web API with Postman.</span></span>
> * <span data-ttu-id="3c621-399">Chiamare l'API Web con jQuery.</span><span class="sxs-lookup"><span data-stu-id="3c621-399">Call the web api with jQuery.</span></span>

<span data-ttu-id="3c621-400">Passare all'esercitazione successiva per apprendere come generare pagine della Guida dell'API:</span><span class="sxs-lookup"><span data-stu-id="3c621-400">Advance to the next tutorial to learn how to generate API help pages:</span></span>

> [!div class="nextstepaction"]
> <xref:tutorials/get-started-with-swashbuckle>
