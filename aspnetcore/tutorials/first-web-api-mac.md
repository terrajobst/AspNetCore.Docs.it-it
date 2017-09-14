---
title: Creare un'API Web con ASP.NET Core e Visual Studio per Mac
author: rick-anderson
description: Creare un'API Web con ASP.NET Core MVC e Visual Studio per Mac
keywords: ASP.NET Core, WebAPI, Web API, REST, mac, macOS, HTTP, Service, HTTP Service
ms.author: riande
manager: wpickett
ms.date: 5/24/2017
ms.topic: get-started-article
ms.assetid: 830b4af5-ed14-1638-7734-764a6f13a8f6
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-web-api-mac
ms.openlocfilehash: 08619d3b4ab2d6fdb04794dcbafac0b696dd8504
ms.sourcegitcommit: 3273675dad5ac3e1dc1c589938b73db3f7d6660a
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/28/2017
---
# <a name="create-a-web-api-with-aspnet-core-mvc-and-visual-studio-for-mac"></a><span data-ttu-id="645f9-104">Creare un'API Web con ASP.NET Core MVC e Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="645f9-104">Create a Web API with ASP.NET Core MVC and Visual Studio for Mac</span></span>

<span data-ttu-id="645f9-105">Di [Rick Anderson](https://twitter.com/RickAndMSFT) e [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="645f9-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="645f9-106">In questa esercitazione si crea un'API Web per la gestione di un elenco di elementi di tipo "attività".</span><span class="sxs-lookup"><span data-stu-id="645f9-106">In this tutorial, you’ll build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="645f9-107">Non si crea un'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="645f9-107">You won’t build a UI.</span></span>

<span data-ttu-id="645f9-108">Sono disponibili 3 versioni dell'esercitazione:</span><span class="sxs-lookup"><span data-stu-id="645f9-108">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="645f9-109">macOS: API Web con Visual Studio per Mac (questa esercitazione)</span><span class="sxs-lookup"><span data-stu-id="645f9-109">macOS: Web API with Visual Studio for Mac (This tutorial)</span></span>
* <span data-ttu-id="645f9-110">Windows: [API Web con Visual Studio per Windows](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="645f9-110">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>
* <span data-ttu-id="645f9-111">macOS, Linux, Windows: [API Web con Visual Studio Code](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="645f9-111">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

* <span data-ttu-id="645f9-112">Vedere [Introduction to ASP.NET Core MVC on Mac or Linux](xref:tutorials/first-mvc-app-xplat/index) (Introduzione ad ASP.NET Core MVC su Mac o Linux) per un esempio che usa un database persistente.</span><span class="sxs-lookup"><span data-stu-id="645f9-112">See [Introduction to ASP.NET Core MVC on Mac or Linux](xref:tutorials/first-mvc-app-xplat/index) for an example that uses a persistent database.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="645f9-113">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="645f9-113">Prerequisites</span></span>

<span data-ttu-id="645f9-114">Installare gli elementi seguenti:</span><span class="sxs-lookup"><span data-stu-id="645f9-114">Install the following:</span></span>

- [<span data-ttu-id="645f9-115">.NET Core SDK</span><span class="sxs-lookup"><span data-stu-id="645f9-115">.NET Core SDK</span></span>](https://www.microsoft.com/net/core#macos)  
- [<span data-ttu-id="645f9-116">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="645f9-116">Visual Studio for Mac</span></span>](https://www.visualstudio.com/vs/visual-studio-mac/)

## <a name="create-the-project"></a><span data-ttu-id="645f9-117">Creare il progetto</span><span class="sxs-lookup"><span data-stu-id="645f9-117">Create the project</span></span>

<span data-ttu-id="645f9-118">In Visual Studio, selezionare **File > Nuova soluzione**.</span><span class="sxs-lookup"><span data-stu-id="645f9-118">From Visual Studio, select **File > New Solution**.</span></span>

![Nuova soluzione macOS](first-web-api-mac/_static/sln.png)

<span data-ttu-id="645f9-120">Selezionare **.NET Core App (App .NET Core) > ASP.NET Core Web API (API Web ASP.NET Core) > Avanti**.</span><span class="sxs-lookup"><span data-stu-id="645f9-120">Select **.NET Core App >  ASP.NET Core Web API > Next**.</span></span>

![Finestra di dialogo Nuovo progetto di macOS](first-web-api-mac/_static/1.png)

<span data-ttu-id="645f9-122">Immettere **TodoApi** in **Nome progetto**, quindi selezionare Crea.</span><span class="sxs-lookup"><span data-stu-id="645f9-122">Enter **TodoApi** for the **Project Name**, and then select Create.</span></span>

![Finestra di dialogo di configurazione](first-web-api-mac/_static/2.png)

### <a name="launch-the-app"></a><span data-ttu-id="645f9-124">Avviare l'app</span><span class="sxs-lookup"><span data-stu-id="645f9-124">Launch the app</span></span>

<span data-ttu-id="645f9-125">In Visual Studio selezionare **Esegui > Avvia eseguendo il debug** per avviare l'app.</span><span class="sxs-lookup"><span data-stu-id="645f9-125">In Visual Studio, select **Run > Start With Debugging** to launch the app.</span></span> <span data-ttu-id="645f9-126">Visual Studio avvia un browser e passa a `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="645f9-126">Visual Studio launches a browser and navigates to `http://localhost:5000`.</span></span> <span data-ttu-id="645f9-127">Si verifica un errore HTTP 404 (Non trovato).</span><span class="sxs-lookup"><span data-stu-id="645f9-127">You get an HTTP 404 (Not Found) error.</span></span>  <span data-ttu-id="645f9-128">Modificare l'URL in `http://localhost:port/api/values`.</span><span class="sxs-lookup"><span data-stu-id="645f9-128">Change the URL to `http://localhost:port/api/values`.</span></span> <span data-ttu-id="645f9-129">Vengono visualizzati i dati `ValuesController`:</span><span class="sxs-lookup"><span data-stu-id="645f9-129">The `ValuesController` data will be displayed:</span></span>

```
["value1","value2"]
```

### <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="645f9-130">Aggiungere il supporto per Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="645f9-130">Add support for Entity Framework Core</span></span>

<span data-ttu-id="645f9-131">Installare il provider di database [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/).</span><span class="sxs-lookup"><span data-stu-id="645f9-131">Install the [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) database provider.</span></span> <span data-ttu-id="645f9-132">Questo provider di database consente l'uso di Entity Framework Core con un database in memoria.</span><span class="sxs-lookup"><span data-stu-id="645f9-132">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

* <span data-ttu-id="645f9-133">Nel menu **Progetto** scegliere **Add NuGet Packages** (Aggiungi pacchetti NuGet al progetto).</span><span class="sxs-lookup"><span data-stu-id="645f9-133">From the **Project** menu, select **Add NuGet Packages**.</span></span> 

  *  <span data-ttu-id="645f9-134">In alternativa fare clic con il pulsante destro del mouse su **Dipendenze** e selezionare **Aggiungi pacchetto**.</span><span class="sxs-lookup"><span data-stu-id="645f9-134">Alternately, you can right-click **Dependencies**, and then select **Add Packages**.</span></span>

* <span data-ttu-id="645f9-135">Immettere `EntityFrameworkCore.InMemory` nella casella di ricerca.</span><span class="sxs-lookup"><span data-stu-id="645f9-135">Enter `EntityFrameworkCore.InMemory` in the search box.</span></span>
* <span data-ttu-id="645f9-136">Selezionare `Microsoft.EntityFrameworkCore.InMemory`, quindi selezionare **Aggiungi pacchetto**.</span><span class="sxs-lookup"><span data-stu-id="645f9-136">Select `Microsoft.EntityFrameworkCore.InMemory`, and then select **Add Package**.</span></span>

### <a name="add-a-model-class"></a><span data-ttu-id="645f9-137">Aggiungere una classe modello</span><span class="sxs-lookup"><span data-stu-id="645f9-137">Add a model class</span></span>

<span data-ttu-id="645f9-138">Un modello è un oggetto che rappresenta i dati nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="645f9-138">A model is an object that represents the data in your application.</span></span> <span data-ttu-id="645f9-139">In questo caso l'unico modello è un elemento attività da eseguire.</span><span class="sxs-lookup"><span data-stu-id="645f9-139">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="645f9-140">Aggiungere una cartella denominata *Models*.</span><span class="sxs-lookup"><span data-stu-id="645f9-140">Add a folder named *Models*.</span></span> <span data-ttu-id="645f9-141">In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto.</span><span class="sxs-lookup"><span data-stu-id="645f9-141">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="645f9-142">Selezionare **Aggiungi** > **Nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="645f9-142">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="645f9-143">Assegnare il nome *Models* alla cartella.</span><span class="sxs-lookup"><span data-stu-id="645f9-143">Name the folder *Models*.</span></span>

![Nuova cartella](first-web-api-mac/_static/folder.png)

<span data-ttu-id="645f9-145">Nota: è possibile inserire le classi del modello in qualsiasi punto del progetto, ma per convenzione viene usata la cartella *Models*.</span><span class="sxs-lookup"><span data-stu-id="645f9-145">Note: You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="645f9-146">Aggiungere una classe `TodoItem`.</span><span class="sxs-lookup"><span data-stu-id="645f9-146">Add a `TodoItem` class.</span></span> <span data-ttu-id="645f9-147">Fare clic con il pulsante destro del mouse sulla cartella *Models* e selezionare **Aggiungi > Nuovo file > Generale > Classe vuota**.</span><span class="sxs-lookup"><span data-stu-id="645f9-147">Right-click the *Models* folder and select **Add > New File > General > Empty Class**.</span></span> <span data-ttu-id="645f9-148">Assegnare il nome `TodoItem` alla classe, quindi selezionare **Nuovo**.</span><span class="sxs-lookup"><span data-stu-id="645f9-148">Name the class `TodoItem`, and then select **New**.</span></span>

<span data-ttu-id="645f9-149">Sostituire il codice generato con:</span><span class="sxs-lookup"><span data-stu-id="645f9-149">Replace the generated code with:</span></span>

<span data-ttu-id="645f9-150">[!code-csharp[Principale](first-web-api/sample/TodoApi/Models/TodoItem.cs)]</span><span class="sxs-lookup"><span data-stu-id="645f9-150">[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]</span></span>

<span data-ttu-id="645f9-151">Il database genera il `Id` quando viene creato un `TodoItem`.</span><span class="sxs-lookup"><span data-stu-id="645f9-151">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="645f9-152">Creare il contesto di database</span><span class="sxs-lookup"><span data-stu-id="645f9-152">Create the database context</span></span>

<span data-ttu-id="645f9-153">Il *contesto di database* è la classe principale che coordina le funzionalità di Entity Framework per un determinato modello di dati.</span><span class="sxs-lookup"><span data-stu-id="645f9-153">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="645f9-154">Questa classe viene creata mediante derivazione dalla classe `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="645f9-154">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="645f9-155">Aggiungere una classe `TodoContext` alla cartella *Models*.</span><span class="sxs-lookup"><span data-stu-id="645f9-155">Add a `TodoContext` class to the *Models* folder.</span></span>

<span data-ttu-id="645f9-156">[!code-csharp[Principale](first-web-api/sample/TodoApi/Models/TodoContext.cs)]</span><span class="sxs-lookup"><span data-stu-id="645f9-156">[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]</span></span>

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="645f9-157">Aggiungere un controller</span><span class="sxs-lookup"><span data-stu-id="645f9-157">Add a controller</span></span>

<span data-ttu-id="645f9-158">In Esplora soluzioni, nella cartella *Controllers* aggiungere la classe `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="645f9-158">In Solution Explorer, in the *Controllers* folder, add the class `TodoController`.</span></span>

<span data-ttu-id="645f9-159">Sostituire il codice generato con il seguente codice (e aggiungere le parentesi graffe di chiusura):</span><span class="sxs-lookup"><span data-stu-id="645f9-159">Replace the generated code with the following (and add closing braces):</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="645f9-160">Avviare l'app</span><span class="sxs-lookup"><span data-stu-id="645f9-160">Launch the app</span></span>

<span data-ttu-id="645f9-161">In Visual Studio selezionare **Esegui > Avvia eseguendo il debug** per avviare l'app.</span><span class="sxs-lookup"><span data-stu-id="645f9-161">In Visual Studio, select **Run > Start With Debugging** to launch the app.</span></span> <span data-ttu-id="645f9-162">Visual Studio viene avviato un browser e passa a `http://localhost:port`, dove *port* è un numero di porta selezionato a caso.</span><span class="sxs-lookup"><span data-stu-id="645f9-162">Visual Studio launches a browser and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span> <span data-ttu-id="645f9-163">Si verifica un errore HTTP 404 (Non trovato).</span><span class="sxs-lookup"><span data-stu-id="645f9-163">You get an HTTP 404 (Not Found) error.</span></span>  <span data-ttu-id="645f9-164">Modificare l'URL in `http://localhost:port/api/values`.</span><span class="sxs-lookup"><span data-stu-id="645f9-164">Change the URL to `http://localhost:port/api/values`.</span></span> <span data-ttu-id="645f9-165">Vengono visualizzati i dati `ValuesController`:</span><span class="sxs-lookup"><span data-stu-id="645f9-165">The `ValuesController` data will be displayed:</span></span>

```
["value1","value2"]
```

<span data-ttu-id="645f9-166">Passare al controller `Todo` all'indirizzo `http://localhost:port/api/todo`:</span><span class="sxs-lookup"><span data-stu-id="645f9-166">Navigate to the `Todo` controller at`http://localhost:port/api/todo`:</span></span>

```
[{"key":1,"name":"Item1","isComplete":false}]
```

## <a name="implement-the-other-crud-operations"></a><span data-ttu-id="645f9-167">Implementare le altre operazioni CRUD</span><span class="sxs-lookup"><span data-stu-id="645f9-167">Implement the other CRUD operations</span></span>

<span data-ttu-id="645f9-168">È prevista l'aggiunta dei metodi `Create`, `Update` e `Delete` al controller.</span><span class="sxs-lookup"><span data-stu-id="645f9-168">We'll add `Create`, `Update`, and `Delete` methods to the controller.</span></span> <span data-ttu-id="645f9-169">Dato che si tratta di variazioni basate su un tema principale, di seguito viene visualizzato il codice e vengono evidenziate le differenze principali.</span><span class="sxs-lookup"><span data-stu-id="645f9-169">These are variations on a theme, so I'll just show the code and highlight the main differences.</span></span> <span data-ttu-id="645f9-170">Compilare il progetto dopo l'aggiunta o la modifica del codice.</span><span class="sxs-lookup"><span data-stu-id="645f9-170">Build the project after adding or changing code.</span></span>

### <a name="create"></a><span data-ttu-id="645f9-171">Crea</span><span class="sxs-lookup"><span data-stu-id="645f9-171">Create</span></span>

<span data-ttu-id="645f9-172">[!code-csharp[Principale](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]</span><span class="sxs-lookup"><span data-stu-id="645f9-172">[!code-csharp[Main](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]</span></span>

<span data-ttu-id="645f9-173">Questo è un metodo HTTP POST, indicato dall'attributo [`[HttpPost]`](https://docs.asp.net/projects/api/en/latest/autoapi/Microsoft/AspNetCore/Mvc/HttpPostAttribute/index.html).</span><span class="sxs-lookup"><span data-stu-id="645f9-173">This is an HTTP POST method, indicated by the [`[HttpPost]`](https://docs.asp.net/projects/api/en/latest/autoapi/Microsoft/AspNetCore/Mvc/HttpPostAttribute/index.html) attribute.</span></span> <span data-ttu-id="645f9-174">L'attributo [`[FromBody]`](https://docs.asp.net/projects/api/en/latest/autoapi/Microsoft/AspNetCore/Mvc/FromBodyAttribute/index.html) indica a MVC di ottenere il valore dell'elemento attività dal corpo della richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="645f9-174">The [`[FromBody]`](https://docs.asp.net/projects/api/en/latest/autoapi/Microsoft/AspNetCore/Mvc/FromBodyAttribute/index.html) attribute tells MVC to get the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="645f9-175">Il metodo `CreatedAtRoute` restituisce una risposta 201, la risposta standard per un metodo HTTP POST che crea una nuova risorsa nel server.</span><span class="sxs-lookup"><span data-stu-id="645f9-175">The `CreatedAtRoute` method returns a 201 response, which is the standard response for an HTTP POST method that creates a new resource on the server.</span></span> <span data-ttu-id="645f9-176">`CreatedAtRoute` aggiunge anche un'intestazione Location (Posizione) alla risposta.</span><span class="sxs-lookup"><span data-stu-id="645f9-176">`CreatedAtRoute` also adds a Location header to the response.</span></span> <span data-ttu-id="645f9-177">L'intestazione Location (Posizione) specifica l'URI dell'elemento attività appena creato.</span><span class="sxs-lookup"><span data-stu-id="645f9-177">The Location header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="645f9-178">Vedere [10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="645f9-178">See [10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>

### <a name="use-postman-to-send-a-create-request"></a><span data-ttu-id="645f9-179">Usare Postman per inviare una richiesta di creazione</span><span class="sxs-lookup"><span data-stu-id="645f9-179">Use Postman to send a Create request</span></span>

* <span data-ttu-id="645f9-180">Avviare l'app (**Esegui > Avvia eseguendo il debug**).</span><span class="sxs-lookup"><span data-stu-id="645f9-180">Start the app (**Run > Start With Debugging**).</span></span>
* <span data-ttu-id="645f9-181">Avviare Postman.</span><span class="sxs-lookup"><span data-stu-id="645f9-181">Start Postman.</span></span>

![Console di Postman](first-web-api/_static/pmc.png)

* <span data-ttu-id="645f9-183">Impostare il metodo HTTP su `POST`.</span><span class="sxs-lookup"><span data-stu-id="645f9-183">Set the HTTP method to `POST`</span></span>
* <span data-ttu-id="645f9-184">Selezionare il pulsante di opzione **Body** (Corpo).</span><span class="sxs-lookup"><span data-stu-id="645f9-184">Select the **Body** radio button</span></span>
* <span data-ttu-id="645f9-185">Selezionare il pulsante di opzione **raw**.</span><span class="sxs-lookup"><span data-stu-id="645f9-185">Select the **raw** radio button</span></span>
* <span data-ttu-id="645f9-186">Impostare il tipo su JSON</span><span class="sxs-lookup"><span data-stu-id="645f9-186">Set the type to JSON</span></span>
* <span data-ttu-id="645f9-187">Nell'editor chiave-valore immettere un elemento attività, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="645f9-187">In the key-value editor, enter a Todo item such as</span></span>

```json
{
    "name":"walk dog",
    "isComplete":true
}
```

* <span data-ttu-id="645f9-188">Selezionare **Send** (Invia).</span><span class="sxs-lookup"><span data-stu-id="645f9-188">Select **Send**</span></span>

* <span data-ttu-id="645f9-189">Selezionare la scheda Headers (Intestazioni) nel riquadro inferiore e copiare l'intestazione **Location** (Posizione):</span><span class="sxs-lookup"><span data-stu-id="645f9-189">Select the Headers tab in the lower pane and copy the **Location** header:</span></span>

![Scheda Headers (Intestazioni) della console Postman](first-web-api/_static/pmget.png)

<span data-ttu-id="645f9-191">È possibile usare l'URI dell'intestazione Location (Posizione) per accedere alla risorsa appena creata.</span><span class="sxs-lookup"><span data-stu-id="645f9-191">You can use the Location header URI to access the resource you just created.</span></span> <span data-ttu-id="645f9-192">Richiamare il metodo `GetById` per creare la route denominata `"GetTodo"`:</span><span class="sxs-lookup"><span data-stu-id="645f9-192">Recall the `GetById` method created the `"GetTodo"` named route:</span></span>

```csharp
[HttpGet("{id}", Name = "GetTodo")]
public IActionResult GetById(string id)
```

### <a name="update"></a><span data-ttu-id="645f9-193">Aggiorna</span><span class="sxs-lookup"><span data-stu-id="645f9-193">Update</span></span>

<span data-ttu-id="645f9-194">[!code-csharp[Principale](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]</span><span class="sxs-lookup"><span data-stu-id="645f9-194">[!code-csharp[Main](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]</span></span>

<span data-ttu-id="645f9-195">`Update` è simile a `Create` ma usa HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="645f9-195">`Update` is similar to `Create`, but uses HTTP PUT.</span></span> <span data-ttu-id="645f9-196">La risposta è [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="645f9-196">The response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="645f9-197">In base alla specifica HTTP una richiesta PUT richiede che il client invii l'intera entità aggiornata, non solo i delta.</span><span class="sxs-lookup"><span data-stu-id="645f9-197">According to the HTTP spec, a PUT request requires the client to send the entire updated entity, not just the deltas.</span></span> <span data-ttu-id="645f9-198">Per supportare gli aggiornamenti parziali usare HTTP PATCH.</span><span class="sxs-lookup"><span data-stu-id="645f9-198">To support partial updates, use HTTP PATCH.</span></span>

```json
{
  "key": 1,
  "name": "walk dog",
  "isComplete": true
}
```

![Console Postman con risposta 204 (No Content)](first-web-api/_static/pmcput.png)

### <a name="delete"></a><span data-ttu-id="645f9-200">Eliminare</span><span class="sxs-lookup"><span data-stu-id="645f9-200">Delete</span></span>

<span data-ttu-id="645f9-201">[!code-csharp[Principale](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]</span><span class="sxs-lookup"><span data-stu-id="645f9-201">[!code-csharp[Main](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]</span></span>

<span data-ttu-id="645f9-202">La risposta è [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="645f9-202">The response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

![Console Postman con risposta 204 (No Content)](first-web-api/_static/pmd.png)

## <a name="next-steps"></a><span data-ttu-id="645f9-204">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="645f9-204">Next steps</span></span>

* [<span data-ttu-id="645f9-205">Routing alle azioni del controller</span><span class="sxs-lookup"><span data-stu-id="645f9-205">Routing to Controller Actions</span></span>](xref:mvc/controllers/routing)
* <span data-ttu-id="645f9-206">Per informazioni sulla distribuzione dell'API, vedere [Pubblicazione e distribuzione](../publishing/index.md).</span><span class="sxs-lookup"><span data-stu-id="645f9-206">For information about deploying your API, see [Publishing and Deployment](../publishing/index.md).</span></span>
* [<span data-ttu-id="645f9-207">Visualizzare o scaricare codice di esempio</span><span class="sxs-lookup"><span data-stu-id="645f9-207">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample)
* [<span data-ttu-id="645f9-208">Postman</span><span class="sxs-lookup"><span data-stu-id="645f9-208">Postman</span></span>](https://www.getpostman.com/)
* [<span data-ttu-id="645f9-209">Fiddler</span><span class="sxs-lookup"><span data-stu-id="645f9-209">Fiddler</span></span>](http://www.fiddler2.com/fiddler2/)
