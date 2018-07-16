---
title: Creare un'API Web con ASP.NET Core e Visual Studio per Mac
author: rick-anderson
description: Creare un'API Web con ASP.NET Core MVC e Visual Studio per Mac
ms.author: riande
ms.custom: mvc
ms.date: 05/08/2018
uid: tutorials/first-web-api-mac
ms.openlocfilehash: 4caa6d9057de8d0e821c4abefe22985f43ff95ad
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/11/2018
ms.locfileid: "38156140"
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-mac"></a><span data-ttu-id="cd395-103">Creare un'API Web con ASP.NET Core e Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="cd395-103">Create a Web API with ASP.NET Core and Visual Studio for Mac</span></span>

<span data-ttu-id="cd395-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT) e [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="cd395-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="cd395-105">Questa esercitazione consente di creare un'API Web per la gestione di un elenco di elementi di tipo "attività".</span><span class="sxs-lookup"><span data-stu-id="cd395-105">In this tutorial, build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="cd395-106">Non è stata costruita l'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="cd395-106">The UI isn't constructed.</span></span>

<span data-ttu-id="cd395-107">Sono disponibili tre versioni di questa esercitazione:</span><span class="sxs-lookup"><span data-stu-id="cd395-107">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="cd395-108">macOS: API Web con Visual Studio per Mac (questa esercitazione)</span><span class="sxs-lookup"><span data-stu-id="cd395-108">macOS: Web API with Visual Studio for Mac (This tutorial)</span></span>
* <span data-ttu-id="cd395-109">Windows: [API Web con Visual Studio per Windows](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="cd395-109">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>
* <span data-ttu-id="cd395-110">macOS, Linux, Windows: [API Web con Visual Studio Code](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="cd395-110">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

<span data-ttu-id="cd395-111">Vedere [Introduction to ASP.NET Core MVC on macOS or Linux](xref:tutorials/first-mvc-app-xplat/index) (Introduzione ad ASP.NET Core MVC su macOS o Linux) per un esempio che usa un database persistente.</span><span class="sxs-lookup"><span data-stu-id="cd395-111">See [Introduction to ASP.NET Core MVC on macOS or Linux](xref:tutorials/first-mvc-app-xplat/index) for an example that uses a persistent database.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cd395-112">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="cd395-112">Prerequisites</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-macos.md)]

## <a name="create-the-project"></a><span data-ttu-id="cd395-113">Creare il progetto</span><span class="sxs-lookup"><span data-stu-id="cd395-113">Create the project</span></span>

<span data-ttu-id="cd395-114">In Visual Studio selezionare **File** > **Nuova soluzione**.</span><span class="sxs-lookup"><span data-stu-id="cd395-114">From Visual Studio, select **File** > **New Solution**.</span></span>

![Nuova soluzione macOS](first-web-api-mac/_static/sln.png)

<span data-ttu-id="cd395-116">Selezionare **.NET Core App** > **ASP.NET Core Web API** > **Next**.</span><span class="sxs-lookup"><span data-stu-id="cd395-116">Select **.NET Core App** > **ASP.NET Core Web API** > **Next**.</span></span>

![Finestra di dialogo Nuovo progetto di macOS](first-web-api-mac/_static/1.png)

<span data-ttu-id="cd395-118">Immettere *TodoApi* in **Nome progetto**, quindi fare clic su **Crea**.</span><span class="sxs-lookup"><span data-stu-id="cd395-118">Enter *TodoApi* for the **Project Name**, and then click **Create**.</span></span>

![Finestra di dialogo di configurazione](first-web-api-mac/_static/2.png)

### <a name="launch-the-app"></a><span data-ttu-id="cd395-120">Avviare l'app</span><span class="sxs-lookup"><span data-stu-id="cd395-120">Launch the app</span></span>

<span data-ttu-id="cd395-121">In Visual Studio selezionare **Esegui** > **Avvia eseguendo il debug** per avviare l'app.</span><span class="sxs-lookup"><span data-stu-id="cd395-121">In Visual Studio, select **Run** > **Start With Debugging** to launch the app.</span></span> <span data-ttu-id="cd395-122">Visual Studio avvia un browser e passa a `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="cd395-122">Visual Studio launches a browser and navigates to `http://localhost:5000`.</span></span> <span data-ttu-id="cd395-123">Si verifica un errore HTTP 404 (Non trovato).</span><span class="sxs-lookup"><span data-stu-id="cd395-123">You get an HTTP 404 (Not Found) error.</span></span> <span data-ttu-id="cd395-124">Modificare l'URL in `http://localhost:<port>/api/values`.</span><span class="sxs-lookup"><span data-stu-id="cd395-124">Change the URL to `http://localhost:<port>/api/values`.</span></span> <span data-ttu-id="cd395-125">Vengono visualizzati i dati `ValuesController`:</span><span class="sxs-lookup"><span data-stu-id="cd395-125">The `ValuesController` data is displayed:</span></span>

```json
["value1","value2"]
```

### <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="cd395-126">Aggiungere il supporto per Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="cd395-126">Add support for Entity Framework Core</span></span>

<span data-ttu-id="cd395-127">Installare il provider di database [Entity Framework Core InMemory](/ef/core/providers/in-memory/).</span><span class="sxs-lookup"><span data-stu-id="cd395-127">Install the [Entity Framework Core InMemory](/ef/core/providers/in-memory/) database provider.</span></span> <span data-ttu-id="cd395-128">Questo provider di database consente l'uso di Entity Framework Core con un database in memoria.</span><span class="sxs-lookup"><span data-stu-id="cd395-128">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

* <span data-ttu-id="cd395-129">Nel menu **Progetto** scegliere **Add NuGet Packages** (Aggiungi pacchetti NuGet al progetto).</span><span class="sxs-lookup"><span data-stu-id="cd395-129">From the **Project** menu, select **Add NuGet Packages**.</span></span>

  * <span data-ttu-id="cd395-130">In alternativa fare clic con il pulsante destro del mouse su **Dipendenze** e selezionare **Aggiungi pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="cd395-130">Alternatively, you can right-click **Dependencies**, and then select **Add Packages**.</span></span>

* <span data-ttu-id="cd395-131">Immettere `EntityFrameworkCore.InMemory` nella casella di ricerca.</span><span class="sxs-lookup"><span data-stu-id="cd395-131">Enter `EntityFrameworkCore.InMemory` in the search box.</span></span>
* <span data-ttu-id="cd395-132">Selezionare `Microsoft.EntityFrameworkCore.InMemory`, quindi selezionare **Aggiungi pacchetto**.</span><span class="sxs-lookup"><span data-stu-id="cd395-132">Select `Microsoft.EntityFrameworkCore.InMemory`, and then select **Add Package**.</span></span>

### <a name="add-a-model-class"></a><span data-ttu-id="cd395-133">Aggiungere una classe del modello</span><span class="sxs-lookup"><span data-stu-id="cd395-133">Add a model class</span></span>

<span data-ttu-id="cd395-134">Un modello è un oggetto che rappresenta i dati nell'app.</span><span class="sxs-lookup"><span data-stu-id="cd395-134">A model is an object representing the data in your app.</span></span> <span data-ttu-id="cd395-135">In questo caso l'unico modello è un elemento attività.</span><span class="sxs-lookup"><span data-stu-id="cd395-135">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="cd395-136">In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto.</span><span class="sxs-lookup"><span data-stu-id="cd395-136">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="cd395-137">Selezionare **Aggiungi** > **Nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="cd395-137">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="cd395-138">Assegnare il nome *Models* alla cartella.</span><span class="sxs-lookup"><span data-stu-id="cd395-138">Name the folder *Models*.</span></span>

![Nuova cartella](first-web-api-mac/_static/folder.png)

> [!NOTE]
> <span data-ttu-id="cd395-140">È possibile inserire le classi del modello in qualsiasi punto del progetto, ma per convenzione viene usata la cartella *Models*.</span><span class="sxs-lookup"><span data-stu-id="cd395-140">You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="cd395-141">Fare clic con il pulsante destro del mouse sulla cartella *Models* e selezionare **Aggiungi** > **Nuovo file** > **Generale** > **Classe vuota**.</span><span class="sxs-lookup"><span data-stu-id="cd395-141">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span> <span data-ttu-id="cd395-142">Assegnare alla classe il nome *TodoItem* e fare clic su **Nuovo**.</span><span class="sxs-lookup"><span data-stu-id="cd395-142">Name the class *TodoItem*, and then click **New**.</span></span>

<span data-ttu-id="cd395-143">Sostituire il codice generato con:</span><span class="sxs-lookup"><span data-stu-id="cd395-143">Replace the generated code with:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="cd395-144">Il database genera `Id` quando viene creato un `TodoItem`.</span><span class="sxs-lookup"><span data-stu-id="cd395-144">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="cd395-145">Creare il contesto di database</span><span class="sxs-lookup"><span data-stu-id="cd395-145">Create the database context</span></span>

<span data-ttu-id="cd395-146">Il *contesto di database* è la classe principale che coordina le funzionalità di Entity Framework per un determinato modello di dati.</span><span class="sxs-lookup"><span data-stu-id="cd395-146">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="cd395-147">Questa classe viene creata mediante derivazione dalla classe `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="cd395-147">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="cd395-148">Aggiungere una classe `TodoContext` alla cartella *Models*.</span><span class="sxs-lookup"><span data-stu-id="cd395-148">Add a `TodoContext` class to the *Models* folder.</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="cd395-149">Aggiungere un controller</span><span class="sxs-lookup"><span data-stu-id="cd395-149">Add a controller</span></span>

<span data-ttu-id="cd395-150">In Esplora soluzioni, nella cartella *Controllers* aggiungere la classe `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="cd395-150">In Solution Explorer, in the *Controllers* folder, add the class `TodoController`.</span></span>

<span data-ttu-id="cd395-151">Sostituire il codice generato con il seguente:</span><span class="sxs-lookup"><span data-stu-id="cd395-151">Replace the generated code with the following:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="cd395-152">Avviare l'app</span><span class="sxs-lookup"><span data-stu-id="cd395-152">Launch the app</span></span>

<span data-ttu-id="cd395-153">In Visual Studio selezionare **Esegui** > **Avvia eseguendo il debug** per avviare l'app.</span><span class="sxs-lookup"><span data-stu-id="cd395-153">In Visual Studio, select **Run** > **Start With Debugging** to launch the app.</span></span> <span data-ttu-id="cd395-154">Visual Studio apre un browser e naviga all'indirizzo `http://localhost:<port>`, dove `<port>` è un numero di porta selezionato a caso.</span><span class="sxs-lookup"><span data-stu-id="cd395-154">Visual Studio launches a browser and navigates to `http://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="cd395-155">Si verifica un errore HTTP 404 (Non trovato).</span><span class="sxs-lookup"><span data-stu-id="cd395-155">You get an HTTP 404 (Not Found) error.</span></span> <span data-ttu-id="cd395-156">Modificare l'URL in `http://localhost:<port>/api/values`.</span><span class="sxs-lookup"><span data-stu-id="cd395-156">Change the URL to `http://localhost:<port>/api/values`.</span></span> <span data-ttu-id="cd395-157">Vengono visualizzati i dati `ValuesController`:</span><span class="sxs-lookup"><span data-stu-id="cd395-157">The `ValuesController` data is displayed:</span></span>

```json
["value1","value2"]
```

<span data-ttu-id="cd395-158">Passare al controller `Todo` all'indirizzo `http://localhost:<port>/api/todo`.</span><span class="sxs-lookup"><span data-stu-id="cd395-158">Navigate to the `Todo` controller at `http://localhost:<port>/api/todo`.</span></span> <span data-ttu-id="cd395-159">Viene restituito il codice JSON seguente:</span><span class="sxs-lookup"><span data-stu-id="cd395-159">The following JSON is returned:</span></span>

```json
[{"key":1,"name":"Item1","isComplete":false}]
```

## <a name="implement-the-other-crud-operations"></a><span data-ttu-id="cd395-160">Implementare le altre operazioni CRUD</span><span class="sxs-lookup"><span data-stu-id="cd395-160">Implement the other CRUD operations</span></span>

<span data-ttu-id="cd395-161">Verranno aggiunti i metodi `Create`, `Update` e `Delete` al controller.</span><span class="sxs-lookup"><span data-stu-id="cd395-161">We'll add `Create`, `Update`, and `Delete` methods to the controller.</span></span> <span data-ttu-id="cd395-162">Dato che questi metodi sono variazioni su un tema, il codice visualizzato di seguito evidenzia le differenze principali.</span><span class="sxs-lookup"><span data-stu-id="cd395-162">These methods are variations on a theme, so I'll just show the code and highlight the main differences.</span></span> <span data-ttu-id="cd395-163">Compilare il progetto dopo l'aggiunta o la modifica del codice.</span><span class="sxs-lookup"><span data-stu-id="cd395-163">Build the project after adding or changing code.</span></span>

### <a name="create"></a><span data-ttu-id="cd395-164">Crea</span><span class="sxs-lookup"><span data-stu-id="cd395-164">Create</span></span>

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="cd395-165">Il metodo precedente risponde a un POST HTTP, come indicato dall'attributo [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute).</span><span class="sxs-lookup"><span data-stu-id="cd395-165">The preceding method responds to an HTTP POST, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="cd395-166">L'attributo [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) indica a MVC di ottenere il valore dell'elemento attività dal corpo della richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="cd395-166">The [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) attribute tells MVC to get the value of the to-do item from the body of the HTTP request.</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="cd395-167">Il metodo precedente risponde a un POST HTTP, come indicato dall'attributo [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute).</span><span class="sxs-lookup"><span data-stu-id="cd395-167">The preceding method responds to an HTTP POST, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="cd395-168">MVC ottiene il valore dell'elemento attività dal corpo della richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="cd395-168">MVC gets the value of the to-do item from the body of the HTTP request.</span></span>
::: moniker-end

<span data-ttu-id="cd395-169">Il metodo `CreatedAtRoute` restituisce una risposta 201.</span><span class="sxs-lookup"><span data-stu-id="cd395-169">The `CreatedAtRoute` method returns a 201 response.</span></span> <span data-ttu-id="cd395-170">Si tratta della risposta standard per un metodo HTTP POST che crea una nuova risorsa nel server.</span><span class="sxs-lookup"><span data-stu-id="cd395-170">It's the standard response for an HTTP POST method that creates a new resource on the server.</span></span> <span data-ttu-id="cd395-171">`CreatedAtRoute` aggiunge anche un'intestazione Location (Posizione) alla risposta.</span><span class="sxs-lookup"><span data-stu-id="cd395-171">`CreatedAtRoute` also adds a Location header to the response.</span></span> <span data-ttu-id="cd395-172">L'intestazione Location (Posizione) specifica l'URI dell'elemento attività appena creato.</span><span class="sxs-lookup"><span data-stu-id="cd395-172">The Location header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="cd395-173">Vedere [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="cd395-173">See [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>

### <a name="use-postman-to-send-a-create-request"></a><span data-ttu-id="cd395-174">Usare Postman per inviare una richiesta di creazione</span><span class="sxs-lookup"><span data-stu-id="cd395-174">Use Postman to send a Create request</span></span>

* <span data-ttu-id="cd395-175">Avviare l'app (**Esegui** > **Avvia eseguendo il debug**).</span><span class="sxs-lookup"><span data-stu-id="cd395-175">Start the app (**Run** > **Start With Debugging**).</span></span>
* <span data-ttu-id="cd395-176">Aprire Postman.</span><span class="sxs-lookup"><span data-stu-id="cd395-176">Open Postman.</span></span>

![Console di Postman](first-web-api/_static/pmc.png)

* <span data-ttu-id="cd395-178">Aggiornare il numero di porta nell'URL localhost.</span><span class="sxs-lookup"><span data-stu-id="cd395-178">Update the port number in the localhost URL.</span></span>
* <span data-ttu-id="cd395-179">Impostare il metodo HTTP su *POST*.</span><span class="sxs-lookup"><span data-stu-id="cd395-179">Set the HTTP method to *POST*.</span></span>
* <span data-ttu-id="cd395-180">Fare clic sulla scheda **Body** (Corpo).</span><span class="sxs-lookup"><span data-stu-id="cd395-180">Click the **Body** tab.</span></span>
* <span data-ttu-id="cd395-181">Selezionare il pulsante di opzione **raw** (non elaborato).</span><span class="sxs-lookup"><span data-stu-id="cd395-181">Select the **raw** radio button.</span></span>
* <span data-ttu-id="cd395-182">Impostare il tipo su *JSON (application/json)*.</span><span class="sxs-lookup"><span data-stu-id="cd395-182">Set the type to *JSON (application/json)*.</span></span>
* <span data-ttu-id="cd395-183">Immettere un corpo della richiesta con un elemento attività simile al codice JSON seguente:</span><span class="sxs-lookup"><span data-stu-id="cd395-183">Enter a request body with a to-do item resembling the following JSON:</span></span>

```json
{
  "name":"walk dog",
  "isComplete":true
}
```

* <span data-ttu-id="cd395-184">Fare clic sul pulsante **Invia**.</span><span class="sxs-lookup"><span data-stu-id="cd395-184">Click the **Send** button.</span></span>

::: moniker range=">= aspnetcore-2.1"
> [!TIP]
> <span data-ttu-id="cd395-185">Se dopo aver fatto clic su **Invia** non viene visualizzata nessuna risposta, disattivare l'opzione **SSL certification verification** (Verifica certificazione SSL).</span><span class="sxs-lookup"><span data-stu-id="cd395-185">If no response displays after clicking **Send**, disable the **SSL certification verification** option.</span></span> <span data-ttu-id="cd395-186">L'opzione è disponibile in **File** > **Impostazioni**.</span><span class="sxs-lookup"><span data-stu-id="cd395-186">This is found under **File** > **Settings**.</span></span> <span data-ttu-id="cd395-187">Fare di nuovo clic sul pulsante **Invia** dopo aver disabilitato l'impostazione.</span><span class="sxs-lookup"><span data-stu-id="cd395-187">Click the **Send** button again after disabling the setting.</span></span>
::: moniker-end

<span data-ttu-id="cd395-188">Fare clic sulla scheda **Headers** (Intestazioni) nel riquadro **Response** (Risposta) e copiare l'intestazione **Location** (Posizione):</span><span class="sxs-lookup"><span data-stu-id="cd395-188">Click the **Headers** tab in the **Response** pane and copy the **Location** header value:</span></span>

![Scheda Headers (Intestazioni) della console Postman](first-web-api/_static/pmc2.png)

<span data-ttu-id="cd395-190">È possibile usare l'URI dell'intestazione Location (Posizione) per accedere alla risorsa creata.</span><span class="sxs-lookup"><span data-stu-id="cd395-190">You can use the Location header URI to access the resource you created.</span></span> <span data-ttu-id="cd395-191">Il metodo `Create` restituisce [CreatedAtRoute](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdatroute#Microsoft_AspNetCore_Mvc_ControllerBase_CreatedAtRoute_System_String_System_Object_System_Object_).</span><span class="sxs-lookup"><span data-stu-id="cd395-191">The `Create` method returns [CreatedAtRoute](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdatroute#Microsoft_AspNetCore_Mvc_ControllerBase_CreatedAtRoute_System_String_System_Object_System_Object_).</span></span> <span data-ttu-id="cd395-192">Il primo parametro passato a `CreatedAtRoute` rappresenta la route denominata da usare per generare l'URL.</span><span class="sxs-lookup"><span data-stu-id="cd395-192">The first parameter passed to `CreatedAtRoute` represents the named route to use for generating the URL.</span></span> <span data-ttu-id="cd395-193">Chiamare il metodo `GetById` che ha creato la route denominata `"GetTodo"`:</span><span class="sxs-lookup"><span data-stu-id="cd395-193">Recall that the `GetById` method created the `"GetTodo"` named route:</span></span>

```csharp
[HttpGet("{id}", Name = "GetTodo")]
```

### <a name="update"></a><span data-ttu-id="cd395-194">Aggiorna</span><span class="sxs-lookup"><span data-stu-id="cd395-194">Update</span></span>

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]
::: moniker-end

<span data-ttu-id="cd395-195">`Update` è simile a `Create` ma usa la richiesta HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="cd395-195">`Update` is similar to `Create`, but uses HTTP PUT.</span></span> <span data-ttu-id="cd395-196">La risposta è [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="cd395-196">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="cd395-197">In base alla specifica HTTP una richiesta PUT richiede che il client invii l'intera entità aggiornata, non solo i delta.</span><span class="sxs-lookup"><span data-stu-id="cd395-197">According to the HTTP spec, a PUT request requires the client to send the entire updated entity, not just the deltas.</span></span> <span data-ttu-id="cd395-198">Per supportare gli aggiornamenti parziali usare HTTP PATCH.</span><span class="sxs-lookup"><span data-stu-id="cd395-198">To support partial updates, use HTTP PATCH.</span></span>

```json
{
  "key": 1,
  "name": "walk dog",
  "isComplete": true
}
```

![Console Postman con risposta 204 (No Content)](first-web-api/_static/pmcput.png)

### <a name="delete"></a><span data-ttu-id="cd395-200">Eliminare</span><span class="sxs-lookup"><span data-stu-id="cd395-200">Delete</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="cd395-201">La risposta è [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="cd395-201">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

![Console Postman con risposta 204 (No Content)](first-web-api/_static/pmd.png)

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]
