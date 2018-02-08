---
title: Creare un'API Web con ASP.NET Core e Visual Studio per Mac
author: rick-anderson
description: Creare un'API Web con ASP.NET Core MVC e Visual Studio per Mac
helpviewer_heywords: ASP.NET Core, WebAPI, Web API, REST, mac, macOS, HTTP, Service, HTTP Service
manager: wpickett
ms.author: riande
ms.date: 09/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-web-api-mac
ms.openlocfilehash: b0e1a331fe3229119f4669fa336b6af4822785bf
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2018
---
# <a name="create-a-web-api-with-aspnet-core-mvc-and-visual-studio-for-mac"></a><span data-ttu-id="a0df2-103">Creare un'API Web con ASP.NET Core MVC e Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="a0df2-103">Create a Web API with ASP.NET Core MVC and Visual Studio for Mac</span></span>

<span data-ttu-id="a0df2-104">Di [Rick Anderson](https://twitter.com/RickAndMSFT) e [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="a0df2-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="a0df2-105">Questa esercitazione consente di creare un'API Web per la gestione di un elenco di elementi di tipo "attività".</span><span class="sxs-lookup"><span data-stu-id="a0df2-105">In this tutorial, build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="a0df2-106">Non è stata costruita l'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="a0df2-106">The UI isn't constructed.</span></span>

<span data-ttu-id="a0df2-107">Sono disponibili 3 versioni dell'esercitazione:</span><span class="sxs-lookup"><span data-stu-id="a0df2-107">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="a0df2-108">macOS: API Web con Visual Studio per Mac (questa esercitazione)</span><span class="sxs-lookup"><span data-stu-id="a0df2-108">macOS: Web API with Visual Studio for Mac (This tutorial)</span></span>
* <span data-ttu-id="a0df2-109">Windows: [API Web con Visual Studio per Windows](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="a0df2-109">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>
* <span data-ttu-id="a0df2-110">macOS, Linux, Windows: [API Web con Visual Studio Code](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="a0df2-110">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

* <span data-ttu-id="a0df2-111">Vedere [Introduction to ASP.NET Core MVC on Mac or Linux](xref:tutorials/first-mvc-app-xplat/index) (Introduzione ad ASP.NET Core MVC su Mac o Linux) per un esempio che usa un database persistente.</span><span class="sxs-lookup"><span data-stu-id="a0df2-111">See [Introduction to ASP.NET Core MVC on Mac or Linux](xref:tutorials/first-mvc-app-xplat/index) for an example that uses a persistent database.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a0df2-112">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="a0df2-112">Prerequisites</span></span>

<span data-ttu-id="a0df2-113">Installare gli elementi seguenti:</span><span class="sxs-lookup"><span data-stu-id="a0df2-113">Install the following:</span></span>

- <span data-ttu-id="a0df2-114">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) o versione successiva</span><span class="sxs-lookup"><span data-stu-id="a0df2-114">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later</span></span>
- [<span data-ttu-id="a0df2-115">Visual Studio per Mac</span><span class="sxs-lookup"><span data-stu-id="a0df2-115">Visual Studio for Mac</span></span>](https://www.visualstudio.com/vs/visual-studio-mac/)

## <a name="create-the-project"></a><span data-ttu-id="a0df2-116">Creare il progetto</span><span class="sxs-lookup"><span data-stu-id="a0df2-116">Create the project</span></span>

<span data-ttu-id="a0df2-117">In Visual Studio selezionare **File > Nuova soluzione**.</span><span class="sxs-lookup"><span data-stu-id="a0df2-117">From Visual Studio, select **File > New Solution**.</span></span>

![Nuova soluzione macOS](first-web-api-mac/_static/sln.png)

<span data-ttu-id="a0df2-119">Selezionare **.NET Core App (App .NET Core) > ASP.NET Core Web API (API Web ASP.NET Core) > Avanti**.</span><span class="sxs-lookup"><span data-stu-id="a0df2-119">Select **.NET Core App >  ASP.NET Core Web API > Next**.</span></span>

![Finestra di dialogo Nuovo progetto di macOS](first-web-api-mac/_static/1.png)

<span data-ttu-id="a0df2-121">Immettere **TodoApi** in **Nome progetto**, quindi selezionare Crea.</span><span class="sxs-lookup"><span data-stu-id="a0df2-121">Enter **TodoApi** for the **Project Name**, and then select Create.</span></span>

![Finestra di dialogo di configurazione](first-web-api-mac/_static/2.png)

### <a name="launch-the-app"></a><span data-ttu-id="a0df2-123">Avviare l'app</span><span class="sxs-lookup"><span data-stu-id="a0df2-123">Launch the app</span></span>

<span data-ttu-id="a0df2-124">In Visual Studio selezionare **Esegui > Avvia eseguendo il debug** per avviare l'app.</span><span class="sxs-lookup"><span data-stu-id="a0df2-124">In Visual Studio, select **Run > Start With Debugging** to launch the app.</span></span> <span data-ttu-id="a0df2-125">Visual Studio avvia un browser e passa a `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="a0df2-125">Visual Studio launches a browser and navigates to `http://localhost:5000`.</span></span> <span data-ttu-id="a0df2-126">Si verifica un errore HTTP 404 (Non trovato).</span><span class="sxs-lookup"><span data-stu-id="a0df2-126">You get an HTTP 404 (Not Found) error.</span></span>  <span data-ttu-id="a0df2-127">Modificare l'URL in `http://localhost:port/api/values`.</span><span class="sxs-lookup"><span data-stu-id="a0df2-127">Change the URL to `http://localhost:port/api/values`.</span></span> <span data-ttu-id="a0df2-128">Vengono visualizzati i dati `ValuesController`:</span><span class="sxs-lookup"><span data-stu-id="a0df2-128">The `ValuesController` data will be displayed:</span></span>

```
["value1","value2"]
```

### <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="a0df2-129">Aggiungere il supporto per Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="a0df2-129">Add support for Entity Framework Core</span></span>

<span data-ttu-id="a0df2-130">Installare il provider di database [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/).</span><span class="sxs-lookup"><span data-stu-id="a0df2-130">Install the [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) database provider.</span></span> <span data-ttu-id="a0df2-131">Questo provider di database consente l'uso di Entity Framework Core con un database in memoria.</span><span class="sxs-lookup"><span data-stu-id="a0df2-131">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

* <span data-ttu-id="a0df2-132">Nel menu **Progetto** scegliere **Add NuGet Packages** (Aggiungi pacchetti NuGet al progetto).</span><span class="sxs-lookup"><span data-stu-id="a0df2-132">From the **Project** menu, select **Add NuGet Packages**.</span></span> 

  *  <span data-ttu-id="a0df2-133">In alternativa fare clic con il pulsante destro del mouse su **Dipendenze** e selezionare **Aggiungi pacchetto**.</span><span class="sxs-lookup"><span data-stu-id="a0df2-133">Alternately, you can right-click **Dependencies**, and then select **Add Packages**.</span></span>

* <span data-ttu-id="a0df2-134">Immettere `EntityFrameworkCore.InMemory` nella casella di ricerca.</span><span class="sxs-lookup"><span data-stu-id="a0df2-134">Enter `EntityFrameworkCore.InMemory` in the search box.</span></span>
* <span data-ttu-id="a0df2-135">Selezionare `Microsoft.EntityFrameworkCore.InMemory`, quindi selezionare **Aggiungi pacchetto**.</span><span class="sxs-lookup"><span data-stu-id="a0df2-135">Select `Microsoft.EntityFrameworkCore.InMemory`, and then select **Add Package**.</span></span>

### <a name="add-a-model-class"></a><span data-ttu-id="a0df2-136">Aggiungere una classe modello</span><span class="sxs-lookup"><span data-stu-id="a0df2-136">Add a model class</span></span>

<span data-ttu-id="a0df2-137">Un modello è un oggetto che rappresenta i dati nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="a0df2-137">A model is an object that represents the data in your application.</span></span> <span data-ttu-id="a0df2-138">In questo caso l'unico modello è un elemento attività.</span><span class="sxs-lookup"><span data-stu-id="a0df2-138">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="a0df2-139">Aggiungere una cartella denominata *Models*.</span><span class="sxs-lookup"><span data-stu-id="a0df2-139">Add a folder named *Models*.</span></span> <span data-ttu-id="a0df2-140">In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto.</span><span class="sxs-lookup"><span data-stu-id="a0df2-140">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="a0df2-141">Selezionare **Aggiungi** > **Nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="a0df2-141">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="a0df2-142">Assegnare il nome *Models* alla cartella.</span><span class="sxs-lookup"><span data-stu-id="a0df2-142">Name the folder *Models*.</span></span>

![Nuova cartella](first-web-api-mac/_static/folder.png)

<span data-ttu-id="a0df2-144">Nota: è possibile inserire le classi del modello in qualsiasi punto del progetto, ma per convenzione viene usata la cartella *Models*.</span><span class="sxs-lookup"><span data-stu-id="a0df2-144">Note: You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="a0df2-145">Aggiungere una classe `TodoItem`.</span><span class="sxs-lookup"><span data-stu-id="a0df2-145">Add a `TodoItem` class.</span></span> <span data-ttu-id="a0df2-146">Fare clic con il pulsante destro del mouse sulla cartella *Models* e selezionare **Aggiungi > Nuovo file > Generale > Classe vuota**.</span><span class="sxs-lookup"><span data-stu-id="a0df2-146">Right-click the *Models* folder and select **Add > New File > General > Empty Class**.</span></span> <span data-ttu-id="a0df2-147">Assegnare il nome `TodoItem` alla classe, quindi selezionare **Nuovo**.</span><span class="sxs-lookup"><span data-stu-id="a0df2-147">Name the class `TodoItem`, and then select **New**.</span></span>

<span data-ttu-id="a0df2-148">Sostituire il codice generato con:</span><span class="sxs-lookup"><span data-stu-id="a0df2-148">Replace the generated code with:</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="a0df2-149">Il database genera `Id` quando viene creato un `TodoItem`.</span><span class="sxs-lookup"><span data-stu-id="a0df2-149">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="a0df2-150">Creare il contesto di database</span><span class="sxs-lookup"><span data-stu-id="a0df2-150">Create the database context</span></span>

<span data-ttu-id="a0df2-151">Il *contesto di database* è la classe principale che coordina le funzionalità di Entity Framework per un determinato modello di dati.</span><span class="sxs-lookup"><span data-stu-id="a0df2-151">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="a0df2-152">Questa classe viene creata mediante derivazione dalla classe `Microsoft.EntityFrameworkCore.DbContext`.</span><span class="sxs-lookup"><span data-stu-id="a0df2-152">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="a0df2-153">Aggiungere una classe `TodoContext` alla cartella *Models*.</span><span class="sxs-lookup"><span data-stu-id="a0df2-153">Add a `TodoContext` class to the *Models* folder.</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="a0df2-154">Aggiungere un controller</span><span class="sxs-lookup"><span data-stu-id="a0df2-154">Add a controller</span></span>

<span data-ttu-id="a0df2-155">In Esplora soluzioni, nella cartella *Controllers* aggiungere la classe `TodoController`.</span><span class="sxs-lookup"><span data-stu-id="a0df2-155">In Solution Explorer, in the *Controllers* folder, add the class `TodoController`.</span></span>

<span data-ttu-id="a0df2-156">Sostituire il codice generato con il seguente codice (e aggiungere le parentesi graffe di chiusura):</span><span class="sxs-lookup"><span data-stu-id="a0df2-156">Replace the generated code with the following (and add closing braces):</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="a0df2-157">Avviare l'app</span><span class="sxs-lookup"><span data-stu-id="a0df2-157">Launch the app</span></span>

<span data-ttu-id="a0df2-158">In Visual Studio selezionare **Esegui > Avvia eseguendo il debug** per avviare l'app.</span><span class="sxs-lookup"><span data-stu-id="a0df2-158">In Visual Studio, select **Run > Start With Debugging** to launch the app.</span></span> <span data-ttu-id="a0df2-159">Visual Studio viene avviato un browser e passa a `http://localhost:port`, dove *port* è un numero di porta selezionato a caso.</span><span class="sxs-lookup"><span data-stu-id="a0df2-159">Visual Studio launches a browser and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span> <span data-ttu-id="a0df2-160">Si verifica un errore HTTP 404 (Non trovato).</span><span class="sxs-lookup"><span data-stu-id="a0df2-160">You get an HTTP 404 (Not Found) error.</span></span>  <span data-ttu-id="a0df2-161">Modificare l'URL in `http://localhost:port/api/values`.</span><span class="sxs-lookup"><span data-stu-id="a0df2-161">Change the URL to `http://localhost:port/api/values`.</span></span> <span data-ttu-id="a0df2-162">Vengono visualizzati i dati `ValuesController`:</span><span class="sxs-lookup"><span data-stu-id="a0df2-162">The `ValuesController` data will be displayed:</span></span>

```
["value1","value2"]
```

<span data-ttu-id="a0df2-163">Passare al controller `Todo` all'indirizzo `http://localhost:port/api/todo`:</span><span class="sxs-lookup"><span data-stu-id="a0df2-163">Navigate to the `Todo` controller at`http://localhost:port/api/todo`:</span></span>

```
[{"key":1,"name":"Item1","isComplete":false}]
```

## <a name="implement-the-other-crud-operations"></a><span data-ttu-id="a0df2-164">Implementare le altre operazioni CRUD</span><span class="sxs-lookup"><span data-stu-id="a0df2-164">Implement the other CRUD operations</span></span>

<span data-ttu-id="a0df2-165">Verranno aggiunti i metodi `Create`, `Update` e `Delete` al controller.</span><span class="sxs-lookup"><span data-stu-id="a0df2-165">We'll add `Create`, `Update`, and `Delete` methods to the controller.</span></span> <span data-ttu-id="a0df2-166">Dato che si tratta di variazioni di un tema, di seguito viene mostrato il codice evidenziando le differenze principali.</span><span class="sxs-lookup"><span data-stu-id="a0df2-166">These are variations on a theme, so I'll just show the code and highlight the main differences.</span></span> <span data-ttu-id="a0df2-167">Compilare il progetto dopo l'aggiunta o la modifica del codice.</span><span class="sxs-lookup"><span data-stu-id="a0df2-167">Build the project after adding or changing code.</span></span>

### <a name="create"></a><span data-ttu-id="a0df2-168">Crea</span><span class="sxs-lookup"><span data-stu-id="a0df2-168">Create</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="a0df2-169">Questo è un metodo HTTP POST, indicato dall'attributo [`[HttpPost]`](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute).</span><span class="sxs-lookup"><span data-stu-id="a0df2-169">This is an HTTP POST method, indicated by the [`[HttpPost]`](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="a0df2-170">L'attributo [`[FromBody]`](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) indica a MVC di ottenere il valore dell'elemento attività dal corpo della richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="a0df2-170">The [`[FromBody]`](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) attribute tells MVC to get the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="a0df2-171">Il metodo `CreatedAtRoute` restituisce una risposta 201, la risposta standard per un metodo HTTP POST che crea una nuova risorsa nel server.</span><span class="sxs-lookup"><span data-stu-id="a0df2-171">The `CreatedAtRoute` method returns a 201 response, which is the standard response for an HTTP POST method that creates a new resource on the server.</span></span> <span data-ttu-id="a0df2-172">`CreatedAtRoute` aggiunge anche un'intestazione Location (Posizione) alla risposta.</span><span class="sxs-lookup"><span data-stu-id="a0df2-172">`CreatedAtRoute` also adds a Location header to the response.</span></span> <span data-ttu-id="a0df2-173">L'intestazione Location (Posizione) specifica l'URI dell'elemento attività appena creato.</span><span class="sxs-lookup"><span data-stu-id="a0df2-173">The Location header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="a0df2-174">Vedere [10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span><span class="sxs-lookup"><span data-stu-id="a0df2-174">See [10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>

### <a name="use-postman-to-send-a-create-request"></a><span data-ttu-id="a0df2-175">Usare Postman per inviare una richiesta di creazione</span><span class="sxs-lookup"><span data-stu-id="a0df2-175">Use Postman to send a Create request</span></span>

* <span data-ttu-id="a0df2-176">Avviare l'app (**Esegui > Avvia eseguendo il debug**).</span><span class="sxs-lookup"><span data-stu-id="a0df2-176">Start the app (**Run > Start With Debugging**).</span></span>
* <span data-ttu-id="a0df2-177">Avviare Postman.</span><span class="sxs-lookup"><span data-stu-id="a0df2-177">Start Postman.</span></span>

![Console di Postman](first-web-api/_static/pmc.png)

* <span data-ttu-id="a0df2-179">Impostare il metodo HTTP su `POST`.</span><span class="sxs-lookup"><span data-stu-id="a0df2-179">Set the HTTP method to `POST`</span></span>
* <span data-ttu-id="a0df2-180">Selezionare il pulsante di opzione **Body** (Corpo).</span><span class="sxs-lookup"><span data-stu-id="a0df2-180">Select the **Body** radio button</span></span>
* <span data-ttu-id="a0df2-181">Selezionare il pulsante di opzione **raw** (non elaborato).</span><span class="sxs-lookup"><span data-stu-id="a0df2-181">Select the **raw** radio button</span></span>
* <span data-ttu-id="a0df2-182">Impostare il tipo su JSON</span><span class="sxs-lookup"><span data-stu-id="a0df2-182">Set the type to JSON</span></span>
* <span data-ttu-id="a0df2-183">Nell'editor chiave-valore immettere un elemento attività, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="a0df2-183">In the key-value editor, enter a Todo item such as</span></span>

```json
{
    "name":"walk dog",
    "isComplete":true
}
```

* <span data-ttu-id="a0df2-184">Selezionare **Send** (Invia).</span><span class="sxs-lookup"><span data-stu-id="a0df2-184">Select **Send**</span></span>

* <span data-ttu-id="a0df2-185">Selezionare la scheda Headers (Intestazioni) nel riquadro inferiore e copiare l'intestazione **Location** (Posizione):</span><span class="sxs-lookup"><span data-stu-id="a0df2-185">Select the Headers tab in the lower pane and copy the **Location** header:</span></span>

![Scheda Headers (Intestazioni) della console Postman](first-web-api/_static/pmget.png)

<span data-ttu-id="a0df2-187">È possibile usare l'URI dell'intestazione Location (Posizione) per accedere alla risorsa appena creata.</span><span class="sxs-lookup"><span data-stu-id="a0df2-187">You can use the Location header URI to access the resource you just created.</span></span> <span data-ttu-id="a0df2-188">Richiamare il metodo `GetById` che ha creato la route denominata `"GetTodo"`:</span><span class="sxs-lookup"><span data-stu-id="a0df2-188">Recall the `GetById` method created the `"GetTodo"` named route:</span></span>

```csharp
[HttpGet("{id}", Name = "GetTodo")]
public IActionResult GetById(string id)
```

### <a name="update"></a><span data-ttu-id="a0df2-189">Aggiorna</span><span class="sxs-lookup"><span data-stu-id="a0df2-189">Update</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

<span data-ttu-id="a0df2-190">`Update` è simile a `Create` ma usa la richiesta HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="a0df2-190">`Update` is similar to `Create`, but uses HTTP PUT.</span></span> <span data-ttu-id="a0df2-191">La risposta è [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="a0df2-191">The response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="a0df2-192">In base alla specifica HTTP una richiesta PUT richiede che il client invii l'intera entità aggiornata, non solo i delta.</span><span class="sxs-lookup"><span data-stu-id="a0df2-192">According to the HTTP spec, a PUT request requires the client to send the entire updated entity, not just the deltas.</span></span> <span data-ttu-id="a0df2-193">Per supportare gli aggiornamenti parziali usare HTTP PATCH.</span><span class="sxs-lookup"><span data-stu-id="a0df2-193">To support partial updates, use HTTP PATCH.</span></span>

```json
{
  "key": 1,
  "name": "walk dog",
  "isComplete": true
}
```

![Console Postman con risposta 204 (No Content)](first-web-api/_static/pmcput.png)

### <a name="delete"></a><span data-ttu-id="a0df2-195">Eliminare</span><span class="sxs-lookup"><span data-stu-id="a0df2-195">Delete</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="a0df2-196">La risposta è [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span><span class="sxs-lookup"><span data-stu-id="a0df2-196">The response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

![Console Postman con risposta 204 (No Content)](first-web-api/_static/pmd.png)

## <a name="next-steps"></a><span data-ttu-id="a0df2-198">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="a0df2-198">Next steps</span></span>

* [<span data-ttu-id="a0df2-199">Routing alle azioni del controller</span><span class="sxs-lookup"><span data-stu-id="a0df2-199">Routing to Controller Actions</span></span>](xref:mvc/controllers/routing)
* <span data-ttu-id="a0df2-200">Per informazioni sulla distribuzione dell'API, vedere [Ospitare e distribuire](xref:host-and-deploy/index).</span><span class="sxs-lookup"><span data-stu-id="a0df2-200">For information about deploying your API, see [Host and deploy](xref:host-and-deploy/index).</span></span>
* <span data-ttu-id="a0df2-201">[Visualizzare o scaricare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample) ([procedura per il download](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a0df2-201">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>
* [<span data-ttu-id="a0df2-202">Postman</span><span class="sxs-lookup"><span data-stu-id="a0df2-202">Postman</span></span>](https://www.getpostman.com/)
* [<span data-ttu-id="a0df2-203">Fiddler</span><span class="sxs-lookup"><span data-stu-id="a0df2-203">Fiddler</span></span>](https://www.telerik.com/download/fiddler)
