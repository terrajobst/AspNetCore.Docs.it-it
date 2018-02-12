---
uid: web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
title: Abilitazione CRUD Operations in ASP.NET Web API 1 | Documenti Microsoft
author: MikeWasson
description: In questa esercitazione viene illustrato come supportare le operazioni CRUD in un servizio HTTP mediante ASP.NET Web API. Versioni del software utilizzate nell'esercitazione Visual Studio 2012 Web punto di accesso...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/28/2012
ms.topic: article
ms.assetid: c125ca47-606a-4d6f-a1fc-1fc62928af93
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
msc.type: authoredcontent
ms.openlocfilehash: 69b7d5453b6ff36d6e28a69428b016cb8cfd06e9
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/12/2018
---
<a name="enabling-crud-operations-in-aspnet-web-api-1"></a><span data-ttu-id="59395-104">Abilitazione CRUD Operations in ASP.NET Web API 1</span><span class="sxs-lookup"><span data-stu-id="59395-104">Enabling CRUD Operations in ASP.NET Web API 1</span></span>
====================
<span data-ttu-id="59395-105">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="59395-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="59395-106">Scaricare il progetto completato</span><span class="sxs-lookup"><span data-stu-id="59395-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-c4761894)

> <span data-ttu-id="59395-107">In questa esercitazione viene illustrato come supportare le operazioni CRUD in un servizio HTTP mediante ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="59395-107">This tutorial shows how to support CRUD operations in an HTTP service using ASP.NET Web API.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="59395-108">Versioni del software utilizzate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="59395-108">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="59395-109">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="59395-109">Visual Studio 2012</span></span>
> - <span data-ttu-id="59395-110">API Web 1 (funziona anche con l'API Web 2)</span><span class="sxs-lookup"><span data-stu-id="59395-110">Web API 1 (also works with Web API 2)</span></span>


<span data-ttu-id="59395-111">È l'acronimo di CRUD &quot;Create, Read, Update e Delete,&quot; quali sono le quattro operazioni di database di base.</span><span class="sxs-lookup"><span data-stu-id="59395-111">CRUD stands for &quot;Create, Read, Update, and Delete,&quot; which are the four basic database operations.</span></span> <span data-ttu-id="59395-112">Molti servizi HTTP modello anche le operazioni CRUD tramite REST o le API REST simile.</span><span class="sxs-lookup"><span data-stu-id="59395-112">Many HTTP services also model CRUD operations through REST or REST-like APIs.</span></span>

<span data-ttu-id="59395-113">In questa esercitazione si creerà un'API per gestire un elenco di prodotti di web molto semplice.</span><span class="sxs-lookup"><span data-stu-id="59395-113">In this tutorial, you will build a very simple web API to manage a list of products.</span></span> <span data-ttu-id="59395-114">Ogni prodotto conterrà un nome, prezzo e la categoria (ad esempio &quot;toys&quot; o &quot;hardware&quot;), nonché un ID prodotto.</span><span class="sxs-lookup"><span data-stu-id="59395-114">Each product will contain a name, price, and category (such as &quot;toys&quot; or &quot;hardware&quot;), plus a product ID.</span></span>

<span data-ttu-id="59395-115">Espongono le API di prodotti seguenti metodi.</span><span class="sxs-lookup"><span data-stu-id="59395-115">The products API will expose following methods.</span></span>

| <span data-ttu-id="59395-116">Operazione</span><span class="sxs-lookup"><span data-stu-id="59395-116">Action</span></span> | <span data-ttu-id="59395-117">Metodo HTTP</span><span class="sxs-lookup"><span data-stu-id="59395-117">HTTP method</span></span> | <span data-ttu-id="59395-118">URI relativo</span><span class="sxs-lookup"><span data-stu-id="59395-118">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="59395-119">Ottenere un elenco di tutti i prodotti</span><span class="sxs-lookup"><span data-stu-id="59395-119">Get a list of all products</span></span> | <span data-ttu-id="59395-120">GET</span><span class="sxs-lookup"><span data-stu-id="59395-120">GET</span></span> | <span data-ttu-id="59395-121">prodotti/api /</span><span class="sxs-lookup"><span data-stu-id="59395-121">/api/products</span></span> |
| <span data-ttu-id="59395-122">Ottenere un prodotto in base all'ID</span><span class="sxs-lookup"><span data-stu-id="59395-122">Get a product by ID</span></span> | <span data-ttu-id="59395-123">GET</span><span class="sxs-lookup"><span data-stu-id="59395-123">GET</span></span> | <span data-ttu-id="59395-124">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="59395-124">/api/products/*id*</span></span> |
| <span data-ttu-id="59395-125">Ottenere un prodotto per categoria</span><span class="sxs-lookup"><span data-stu-id="59395-125">Get a product by category</span></span> | <span data-ttu-id="59395-126">GET</span><span class="sxs-lookup"><span data-stu-id="59395-126">GET</span></span> | <span data-ttu-id="59395-127">/api/products?category=*category*</span><span class="sxs-lookup"><span data-stu-id="59395-127">/api/products?category=*category*</span></span> |
| <span data-ttu-id="59395-128">Creare un nuovo prodotto</span><span class="sxs-lookup"><span data-stu-id="59395-128">Create a new product</span></span> | <span data-ttu-id="59395-129">INSERISCI</span><span class="sxs-lookup"><span data-stu-id="59395-129">POST</span></span> | <span data-ttu-id="59395-130">prodotti/api /</span><span class="sxs-lookup"><span data-stu-id="59395-130">/api/products</span></span> |
| <span data-ttu-id="59395-131">Aggiornamento di un prodotto</span><span class="sxs-lookup"><span data-stu-id="59395-131">Update a product</span></span> | <span data-ttu-id="59395-132">PUT</span><span class="sxs-lookup"><span data-stu-id="59395-132">PUT</span></span> | <span data-ttu-id="59395-133">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="59395-133">/api/products/*id*</span></span> |
| <span data-ttu-id="59395-134">Eliminare un prodotto</span><span class="sxs-lookup"><span data-stu-id="59395-134">Delete a product</span></span> | <span data-ttu-id="59395-135">DELETE</span><span class="sxs-lookup"><span data-stu-id="59395-135">DELETE</span></span> | <span data-ttu-id="59395-136">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="59395-136">/api/products/*id*</span></span> |

<span data-ttu-id="59395-137">Si noti che alcuni degli URI includono l'ID prodotto nel percorso.</span><span class="sxs-lookup"><span data-stu-id="59395-137">Notice that some of the URIs include the product ID in path.</span></span> <span data-ttu-id="59395-138">Ad esempio, per ottenere il prodotto il cui ID è 28, il client invia una richiesta GET `http://hostname/api/products/28`.</span><span class="sxs-lookup"><span data-stu-id="59395-138">For example, to get the product whose ID is 28, the client sends a GET request for `http://hostname/api/products/28`.</span></span>

### <a name="resources"></a><span data-ttu-id="59395-139">Risorse</span><span class="sxs-lookup"><span data-stu-id="59395-139">Resources</span></span>

<span data-ttu-id="59395-140">I prodotti API definisce l'URI per due tipi di risorse:</span><span class="sxs-lookup"><span data-stu-id="59395-140">The products API defines URIs for two resource types:</span></span>

| <span data-ttu-id="59395-141">Risorsa</span><span class="sxs-lookup"><span data-stu-id="59395-141">Resource</span></span> | <span data-ttu-id="59395-142">URI</span><span class="sxs-lookup"><span data-stu-id="59395-142">URI</span></span> |
| --- | --- |
| <span data-ttu-id="59395-143">L'elenco di tutti i prodotti.</span><span class="sxs-lookup"><span data-stu-id="59395-143">The list of all the products.</span></span> | <span data-ttu-id="59395-144">prodotti/api /</span><span class="sxs-lookup"><span data-stu-id="59395-144">/api/products</span></span> |
| <span data-ttu-id="59395-145">Un singolo prodotto.</span><span class="sxs-lookup"><span data-stu-id="59395-145">An individual product.</span></span> | <span data-ttu-id="59395-146">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="59395-146">/api/products/*id*</span></span> |

### <a name="methods"></a><span data-ttu-id="59395-147">Metodi</span><span class="sxs-lookup"><span data-stu-id="59395-147">Methods</span></span>

<span data-ttu-id="59395-148">I quattro principali metodi HTTP (GET, PUT, POST e DELETE) possono essere mappati a operazioni CRUD, come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="59395-148">The four main HTTP methods (GET, PUT, POST, and DELETE) can be mapped to CRUD operations as follows:</span></span>

- <span data-ttu-id="59395-149">GET recupera la rappresentazione della risorsa a un URI specificato.</span><span class="sxs-lookup"><span data-stu-id="59395-149">GET retrieves the representation of the resource at a specified URI.</span></span> <span data-ttu-id="59395-150">GET deve non hanno effetti collaterali sul server.</span><span class="sxs-lookup"><span data-stu-id="59395-150">GET should have no side effects on the server.</span></span>
- <span data-ttu-id="59395-151">PUT consente di aggiornare una risorsa a un URI specificato.</span><span class="sxs-lookup"><span data-stu-id="59395-151">PUT updates a resource at a specified URI.</span></span> <span data-ttu-id="59395-152">PUT anche utilizzabile per creare una nuova risorsa in un URI specificato, se il server consente ai client di specificare di nuovo URI.</span><span class="sxs-lookup"><span data-stu-id="59395-152">PUT can also be used to create a new resource at a specified URI, if the server allows clients to specify new URIs.</span></span> <span data-ttu-id="59395-153">Per questa esercitazione, l'API non supporta la creazione tramite PUT.</span><span class="sxs-lookup"><span data-stu-id="59395-153">For this tutorial, the API will not support creation through PUT.</span></span>
- <span data-ttu-id="59395-154">POST crea una nuova risorsa.</span><span class="sxs-lookup"><span data-stu-id="59395-154">POST creates a new resource.</span></span> <span data-ttu-id="59395-155">Il server assegna l'URI per il nuovo oggetto e restituisce l'URI come parte del messaggio di risposta.</span><span class="sxs-lookup"><span data-stu-id="59395-155">The server assigns the URI for the new object and returns this URI as part of the response message.</span></span>
- <span data-ttu-id="59395-156">DELETE elimina una risorsa a un URI specificato.</span><span class="sxs-lookup"><span data-stu-id="59395-156">DELETE deletes a resource at a specified URI.</span></span>

<span data-ttu-id="59395-157">Nota: Il metodo PUT sostituisce l'entità product intero.</span><span class="sxs-lookup"><span data-stu-id="59395-157">Note: The PUT method replaces the entire product entity.</span></span> <span data-ttu-id="59395-158">Ovvero, il client è previsto per l'invio di una rappresentazione completa del prodotto aggiornato.</span><span class="sxs-lookup"><span data-stu-id="59395-158">That is, the client is expected to send a complete representation of the updated product.</span></span> <span data-ttu-id="59395-159">Se si desidera supportare gli aggiornamenti parziali, il metodo PATCH è preferibile.</span><span class="sxs-lookup"><span data-stu-id="59395-159">If you want to support partial updates, the PATCH method is preferred.</span></span> <span data-ttu-id="59395-160">In questa esercitazione non implementa PATCH.</span><span class="sxs-lookup"><span data-stu-id="59395-160">This tutorial does not implement PATCH.</span></span>

## <a name="create-a-new-web-api-project"></a><span data-ttu-id="59395-161">Creare un nuovo progetto di API Web</span><span class="sxs-lookup"><span data-stu-id="59395-161">Create a New Web API Project</span></span>

<span data-ttu-id="59395-162">Iniziare eseguendo Visual Studio e selezionare **nuovo progetto** dal **avviare** pagina.</span><span class="sxs-lookup"><span data-stu-id="59395-162">Start by running Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="59395-163">O dal **File** dal menu **New** e quindi **progetto**.</span><span class="sxs-lookup"><span data-stu-id="59395-163">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="59395-164">Nel **modelli** riquadro, selezionare **modelli installati** ed espandere il **Visual c#** nodo.</span><span class="sxs-lookup"><span data-stu-id="59395-164">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="59395-165">In **Visual c#**selezionare **Web**.</span><span class="sxs-lookup"><span data-stu-id="59395-165">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="59395-166">Nell'elenco dei modelli di progetto, selezionare **applicazione Web ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="59395-166">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="59395-167">Denominare il progetto &quot;ProductStore&quot; e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="59395-167">Name the project &quot;ProductStore&quot; and click **OK**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image1.png)

<span data-ttu-id="59395-168">Nel **nuovo progetto ASP.NET MVC 4** finestra di dialogo Seleziona **API Web** e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="59395-168">In the **New ASP.NET MVC 4 Project** dialog, select **Web API** and click **OK**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image2.png)

## <a name="adding-a-model"></a><span data-ttu-id="59395-169">Aggiunta di un modello</span><span class="sxs-lookup"><span data-stu-id="59395-169">Adding a Model</span></span>

<span data-ttu-id="59395-170">Un *modello* è un oggetto che rappresenta i dati nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="59395-170">A *model* is an object that represents the data in your application.</span></span> <span data-ttu-id="59395-171">In ASP.NET Web API, è possibile utilizzare oggetti CLR fortemente tipizzati come modelli e vengono automaticamente serializzati in XML o JSON per il client.</span><span class="sxs-lookup"><span data-stu-id="59395-171">In ASP.NET Web API, you can use strongly typed CLR objects as models, and they will automatically be serialized to XML or JSON for the client.</span></span>

<span data-ttu-id="59395-172">Per l'API ProductStore, costituito da dati di prodotti, verrà quindi creata una nuova classe denominata `Product`.</span><span class="sxs-lookup"><span data-stu-id="59395-172">For the ProductStore API, our data consists of products, so we'll create a new class named `Product`.</span></span>

<span data-ttu-id="59395-173">Se Esplora soluzioni non è visibile, fare clic su di **vista** dal menu **Esplora**.</span><span class="sxs-lookup"><span data-stu-id="59395-173">If Solution Explorer is not already visible, click the **View** menu and select **Solution Explorer**.</span></span> <span data-ttu-id="59395-174">In Esplora soluzioni fare doppio clic su di **modelli** cartella.</span><span class="sxs-lookup"><span data-stu-id="59395-174">In Solution Explorer, right-click the **Models** folder.</span></span> <span data-ttu-id="59395-175">Selezionare il contesto meny, **Aggiungi**, quindi selezionare **classe**.</span><span class="sxs-lookup"><span data-stu-id="59395-175">From the context meny, select **Add**, then select **Class**.</span></span> <span data-ttu-id="59395-176">Nome della classe &quot;prodotto&quot;.</span><span class="sxs-lookup"><span data-stu-id="59395-176">Name the class &quot;Product&quot;.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image3.png)

<span data-ttu-id="59395-177">Aggiungere le seguenti proprietà per il `Product` classe.</span><span class="sxs-lookup"><span data-stu-id="59395-177">Add the following properties to the `Product` class.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample1.cs)]

## <a name="adding-a-repository"></a><span data-ttu-id="59395-178">Aggiunta di un Repository</span><span class="sxs-lookup"><span data-stu-id="59395-178">Adding a Repository</span></span>

<span data-ttu-id="59395-179">È necessario archiviare una raccolta di prodotti.</span><span class="sxs-lookup"><span data-stu-id="59395-179">We need to store a collection of products.</span></span> <span data-ttu-id="59395-180">È consigliabile separare la raccolta dall'implementazione del servizio.</span><span class="sxs-lookup"><span data-stu-id="59395-180">It's a good idea to separate the collection from our service implementation.</span></span> <span data-ttu-id="59395-181">In questo modo, è possibile modificare l'archivio di backup senza riscrivere la classe del servizio.</span><span class="sxs-lookup"><span data-stu-id="59395-181">That way, we can change the backing store without rewriting the service class.</span></span> <span data-ttu-id="59395-182">Questo tipo di progettazione viene chiamato il *repository* modello.</span><span class="sxs-lookup"><span data-stu-id="59395-182">This type of design is called the *repository* pattern.</span></span> <span data-ttu-id="59395-183">Per iniziare, definire un'interfaccia generica per il repository.</span><span class="sxs-lookup"><span data-stu-id="59395-183">Start by defining a generic interface for the repository.</span></span>

<span data-ttu-id="59395-184">In Esplora soluzioni fare doppio clic su di **modelli** cartella.</span><span class="sxs-lookup"><span data-stu-id="59395-184">In Solution Explorer, right-click the **Models** folder.</span></span> <span data-ttu-id="59395-185">Selezionare **Aggiungi**, quindi selezionare **nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="59395-185">Select **Add**, then select **New Item**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image4.png)

<span data-ttu-id="59395-186">Nel **modelli** riquadro, selezionare **modelli installati** ed espandere il nodo di c#.</span><span class="sxs-lookup"><span data-stu-id="59395-186">In the **Templates** pane, select **Installed Templates** and expand the C# node.</span></span> <span data-ttu-id="59395-187">In c#, selezionare **codice**.</span><span class="sxs-lookup"><span data-stu-id="59395-187">Under C#, select **Code**.</span></span> <span data-ttu-id="59395-188">Nell'elenco dei modelli di codice, selezionare **interfaccia**.</span><span class="sxs-lookup"><span data-stu-id="59395-188">In the list of code templates, select **Interface**.</span></span> <span data-ttu-id="59395-189">Nome dell'interfaccia &quot;IProductRepository&quot;.</span><span class="sxs-lookup"><span data-stu-id="59395-189">Name the interface &quot;IProductRepository&quot;.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image5.png)

<span data-ttu-id="59395-190">Aggiungere l'implementazione seguente:</span><span class="sxs-lookup"><span data-stu-id="59395-190">Add the following implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample2.cs)]

<span data-ttu-id="59395-191">A questo punto aggiungere un'altra classe nella cartella Models, denominato &quot;ProductRepository.&quot; Questa classe implementerà l'interfaccia `IProductRespository`.</span><span class="sxs-lookup"><span data-stu-id="59395-191">Now add another class to the Models folder, named &quot;ProductRepository.&quot; This class will implement the `IProductRespository` interface.</span></span> <span data-ttu-id="59395-192">Aggiungere l'implementazione seguente:</span><span class="sxs-lookup"><span data-stu-id="59395-192">Add the following implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample3.cs)]

<span data-ttu-id="59395-193">Il repository mantiene l'elenco nella memoria locale.</span><span class="sxs-lookup"><span data-stu-id="59395-193">The repository keeps the list in local memory.</span></span> <span data-ttu-id="59395-194">Questo stato è OK per un'esercitazione, ma in un'applicazione reale, è necessario archiviare esternamente, i dati di un database o nell'archiviazione cloud.</span><span class="sxs-lookup"><span data-stu-id="59395-194">This is OK for a tutorial, but in a real application, you would store the data externally, either a database or in cloud storage.</span></span> <span data-ttu-id="59395-195">Il modello di repository rende più semplice modificare l'implementazione in un secondo momento.</span><span class="sxs-lookup"><span data-stu-id="59395-195">The repository pattern will make it easier to change the implementation later.</span></span>

## <a name="adding-a-web-api-controller"></a><span data-ttu-id="59395-196">Aggiunta di un Controller API Web</span><span class="sxs-lookup"><span data-stu-id="59395-196">Adding a Web API Controller</span></span>

<span data-ttu-id="59395-197">Ha familiarità con MVC ASP.NET, quindi si ha già familiarità con i controller di.</span><span class="sxs-lookup"><span data-stu-id="59395-197">If you have worked with ASP.NET MVC, then you are already familiar with controllers.</span></span> <span data-ttu-id="59395-198">In ASP.NET Web API, un *controller* è una classe che gestisce le richieste HTTP dal client.</span><span class="sxs-lookup"><span data-stu-id="59395-198">In ASP.NET Web API, a *controller* is a class that handles HTTP requests from the client.</span></span> <span data-ttu-id="59395-199">La creazione guidata nuovo progetto creato due controller quando creato il progetto.</span><span class="sxs-lookup"><span data-stu-id="59395-199">The New Project wizard created two controllers for you when it created the project.</span></span> <span data-ttu-id="59395-200">Per visualizzarle, espandere la cartella controller in Esplora soluzioni.</span><span class="sxs-lookup"><span data-stu-id="59395-200">To see them, expand the Controllers folder in Solution Explorer.</span></span>

- <span data-ttu-id="59395-201">HomeController è un controller MVC ASP.NET tradizionale.</span><span class="sxs-lookup"><span data-stu-id="59395-201">HomeController is a traditional ASP.NET MVC controller.</span></span> <span data-ttu-id="59395-202">È responsabile per servire le pagine HTML per il sito e non è correlato direttamente l'API Web.</span><span class="sxs-lookup"><span data-stu-id="59395-202">It is responsible for serving HTML pages for the site, and is not directly related to our web API.</span></span>
- <span data-ttu-id="59395-203">ValuesController è un controller WebAPI di esempio.</span><span class="sxs-lookup"><span data-stu-id="59395-203">ValuesController is an example WebAPI controller.</span></span>

<span data-ttu-id="59395-204">Vado Avanti ed eliminare ValuesController, facendo clic sul file in Esplora soluzioni e selezionando **eliminare.**</span><span class="sxs-lookup"><span data-stu-id="59395-204">Go ahead and delete ValuesController, by right-clicking the file in Solution Explorer and selecting **Delete.**</span></span> <span data-ttu-id="59395-205">Aggiungere ora un nuovo controller, come segue:</span><span class="sxs-lookup"><span data-stu-id="59395-205">Now add a new controller, as follows:</span></span>

<span data-ttu-id="59395-206">In **Esplora**, fare clic sulla cartella controller.</span><span class="sxs-lookup"><span data-stu-id="59395-206">In **Solution Explorer**, right-click the Controllers folder.</span></span> <span data-ttu-id="59395-207">Selezionare **Aggiungi** e quindi selezionare **Controller**.</span><span class="sxs-lookup"><span data-stu-id="59395-207">Select **Add** and then select **Controller**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image6.png)

<span data-ttu-id="59395-208">Nel **Aggiungi Controller** procedura guidata, il nome del controller &quot;ProductsController&quot;.</span><span class="sxs-lookup"><span data-stu-id="59395-208">In the **Add Controller** wizard, name the controller &quot;ProductsController&quot;.</span></span> <span data-ttu-id="59395-209">Nel **modello** elenco a discesa, seleziona **Controller API vuoto**.</span><span class="sxs-lookup"><span data-stu-id="59395-209">In the **Template** drop-down list, select **Empty API Controller**.</span></span> <span data-ttu-id="59395-210">Fare quindi clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="59395-210">Then click **Add**.</span></span>

![](creating-a-web-api-that-supports-crud-operations/_static/image7.png)

> [!NOTE]
> <span data-ttu-id="59395-211">Non è necessario inserire il contollers in una cartella denominata controller.</span><span class="sxs-lookup"><span data-stu-id="59395-211">It is not necessary to put your contollers into a folder named Controllers.</span></span> <span data-ttu-id="59395-212">Il nome della cartella non è rilevante. è semplicemente un modo pratico per organizzare i file di origine.</span><span class="sxs-lookup"><span data-stu-id="59395-212">The folder name is not important; it is simply a convenient way to organize your source files.</span></span>


<span data-ttu-id="59395-213">Il **Aggiungi Controller** procedura guidata creerà un file denominato ProductsController.cs nella cartella controller.</span><span class="sxs-lookup"><span data-stu-id="59395-213">The **Add Controller** wizard will create a file named ProductsController.cs in the Controllers folder.</span></span> <span data-ttu-id="59395-214">Se questo file non è già aperto, fare doppio clic sul file per aprirlo.</span><span class="sxs-lookup"><span data-stu-id="59395-214">If this file is not open already, double-click the file to open it.</span></span> <span data-ttu-id="59395-215">Aggiungere il seguente **utilizzando** istruzione:</span><span class="sxs-lookup"><span data-stu-id="59395-215">Add the following **using** statement:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample4.cs)]

<span data-ttu-id="59395-216">Aggiungere un campo che contiene un **IProductRepository** istanza.</span><span class="sxs-lookup"><span data-stu-id="59395-216">Add a field that holds an **IProductRepository** instance.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample5.cs)]

> [!NOTE]
> <span data-ttu-id="59395-217">La chiamata `new ProductRepository()` nel controller non è l'approccio migliore, perché collega il controller di una particolare implementazione di `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="59395-217">Calling `new ProductRepository()` in the controller is not the best design, because it ties the controller to a particular implementation of `IProductRepository`.</span></span> <span data-ttu-id="59395-218">Per un approccio migliore, vedere [mediante il Resolver di dipendenza di API Web](../advanced/dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="59395-218">For a better approach, see [Using the Web API Dependency Resolver](../advanced/dependency-injection.md).</span></span>


## <a name="getting-a-resource"></a><span data-ttu-id="59395-219">Recupero di una risorsa</span><span class="sxs-lookup"><span data-stu-id="59395-219">Getting a Resource</span></span>

<span data-ttu-id="59395-220">L'API ProductStore esporrà diversi &quot;leggere&quot; azioni sotto forma di metodi HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="59395-220">The ProductStore API will expose several &quot;read&quot; actions as HTTP GET methods.</span></span> <span data-ttu-id="59395-221">Ogni azione corrisponderà a un metodo di `ProductsController` classe.</span><span class="sxs-lookup"><span data-stu-id="59395-221">Each action will correspond to a method in the `ProductsController` class.</span></span>

| <span data-ttu-id="59395-222">Operazione</span><span class="sxs-lookup"><span data-stu-id="59395-222">Action</span></span> | <span data-ttu-id="59395-223">Metodo HTTP</span><span class="sxs-lookup"><span data-stu-id="59395-223">HTTP method</span></span> | <span data-ttu-id="59395-224">URI relativo</span><span class="sxs-lookup"><span data-stu-id="59395-224">Relative URI</span></span> |
| --- | --- | --- |
| <span data-ttu-id="59395-225">Ottenere un elenco di tutti i prodotti</span><span class="sxs-lookup"><span data-stu-id="59395-225">Get a list of all products</span></span> | <span data-ttu-id="59395-226">GET</span><span class="sxs-lookup"><span data-stu-id="59395-226">GET</span></span> | <span data-ttu-id="59395-227">prodotti/api /</span><span class="sxs-lookup"><span data-stu-id="59395-227">/api/products</span></span> |
| <span data-ttu-id="59395-228">Ottenere un prodotto in base all'ID</span><span class="sxs-lookup"><span data-stu-id="59395-228">Get a product by ID</span></span> | <span data-ttu-id="59395-229">GET</span><span class="sxs-lookup"><span data-stu-id="59395-229">GET</span></span> | <span data-ttu-id="59395-230">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="59395-230">/api/products/*id*</span></span> |
| <span data-ttu-id="59395-231">Ottenere un prodotto per categoria</span><span class="sxs-lookup"><span data-stu-id="59395-231">Get a product by category</span></span> | <span data-ttu-id="59395-232">GET</span><span class="sxs-lookup"><span data-stu-id="59395-232">GET</span></span> | <span data-ttu-id="59395-233">/api/products?category=*category*</span><span class="sxs-lookup"><span data-stu-id="59395-233">/api/products?category=*category*</span></span> |

<span data-ttu-id="59395-234">Per ottenere l'elenco di tutti i prodotti, aggiungere questo metodo per la `ProductsController` classe:</span><span class="sxs-lookup"><span data-stu-id="59395-234">To get the list of all products, add this method to the `ProductsController` class:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample6.cs)]

<span data-ttu-id="59395-235">Il nome del metodo inizia con &quot;ottenere&quot;, pertanto per convenzione viene eseguito il mapping alle richieste GET.</span><span class="sxs-lookup"><span data-stu-id="59395-235">The method name starts with &quot;Get&quot;, so by convention it maps to GET requests.</span></span> <span data-ttu-id="59395-236">Inoltre, poiché il metodo non ha parametri, viene eseguito il mapping a un URI che non contiene un  *&quot;id&quot;*  segmento del percorso.</span><span class="sxs-lookup"><span data-stu-id="59395-236">Also, because the method has no parameters, it maps to a URI that does not contain an *&quot;id&quot;* segment in the path.</span></span>

<span data-ttu-id="59395-237">Per ottenere un prodotto dall'ID, aggiungere questo metodo per la `ProductsController` classe:</span><span class="sxs-lookup"><span data-stu-id="59395-237">To get a product by ID, add this method to the `ProductsController` class:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample7.cs)]

<span data-ttu-id="59395-238">Il nome del metodo inizia anche con &quot;ottenere&quot;, ma il metodo ha un parametro denominato *id*. Questo parametro viene eseguito il mapping per il &quot;id&quot; segmento del percorso URI.</span><span class="sxs-lookup"><span data-stu-id="59395-238">This method name also starts with &quot;Get&quot;, but the method has a parameter named *id*. This parameter is mapped to the &quot;id&quot; segment of the URI path.</span></span> <span data-ttu-id="59395-239">Il framework di ASP.NET Web API converte automaticamente l'ID per il tipo di dati corretto (**int**) per il parametro.</span><span class="sxs-lookup"><span data-stu-id="59395-239">The ASP.NET Web API framework automatically converts the ID to the correct data type (**int**) for the parameter.</span></span>

<span data-ttu-id="59395-240">Il metodo GetProduct genera un'eccezione di tipo **HttpResponseException** se *id* non è valido.</span><span class="sxs-lookup"><span data-stu-id="59395-240">The GetProduct method throws an exception of type **HttpResponseException** if *id* is not valid.</span></span> <span data-ttu-id="59395-241">Questa eccezione viene convertita dal framework in un errore 404 (non trovato).</span><span class="sxs-lookup"><span data-stu-id="59395-241">This exception will be translated by the framework into a 404 (Not Found) error.</span></span>

<span data-ttu-id="59395-242">Infine, aggiungere un metodo per trovare i prodotti per categoria:</span><span class="sxs-lookup"><span data-stu-id="59395-242">Finally, add a method to find products by category:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample8.cs)]

<span data-ttu-id="59395-243">Se l'URI della richiesta è una stringa di query, API Web tenta di associare i parametri di query ai parametri del metodo del controller.</span><span class="sxs-lookup"><span data-stu-id="59395-243">If the request URI has a query string, Web API tries to match the query parameters to parameters on the controller method.</span></span> <span data-ttu-id="59395-244">Pertanto, un URI nel formato "/ prodotti api? categoria =*categoria*" verrà eseguito il mapping a questo metodo.</span><span class="sxs-lookup"><span data-stu-id="59395-244">Therefore, a URI of the form "api/products?category=*category*" will map to this method.</span></span>

## <a name="creating-a-resource"></a><span data-ttu-id="59395-245">Creazione di una risorsa</span><span class="sxs-lookup"><span data-stu-id="59395-245">Creating a Resource</span></span>

<span data-ttu-id="59395-246">Successivamente, verrà aggiunto un metodo per la `ProductsController` classe per creare un nuovo prodotto.</span><span class="sxs-lookup"><span data-stu-id="59395-246">Next, we'll add a method to the `ProductsController` class to create a new product.</span></span> <span data-ttu-id="59395-247">Ecco una semplice implementazione del metodo:</span><span class="sxs-lookup"><span data-stu-id="59395-247">Here is a simple implementation of the method:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample9.cs)]

<span data-ttu-id="59395-248">Notare due cose su questo metodo:</span><span class="sxs-lookup"><span data-stu-id="59395-248">Note two things about this method:</span></span>

- <span data-ttu-id="59395-249">Il nome del metodo inizia con &quot;Post... &quot;.</span><span class="sxs-lookup"><span data-stu-id="59395-249">The method name starts with &quot;Post...&quot;.</span></span> <span data-ttu-id="59395-250">Per creare un nuovo prodotto, il client invia una richiesta HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="59395-250">To create a new product, the client sends an HTTP POST request.</span></span>
- <span data-ttu-id="59395-251">Il metodo accetta un parametro di tipo Product.</span><span class="sxs-lookup"><span data-stu-id="59395-251">The method takes a parameter of type Product.</span></span> <span data-ttu-id="59395-252">Nell'API Web, i parametri con tipi complessi vengono deserializzati dal corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="59395-252">In Web API, parameters with complex types are deserialized from the request body.</span></span> <span data-ttu-id="59395-253">Pertanto, è probabile che il client invia una rappresentazione serializzata di un oggetto di prodotto, in formato XML o JSON.</span><span class="sxs-lookup"><span data-stu-id="59395-253">Therefore, we expect the client to send a serialized representation of a product object, in either XML or JSON format.</span></span>

<span data-ttu-id="59395-254">Questa implementazione funzionerà, ma non è ancora completo.</span><span class="sxs-lookup"><span data-stu-id="59395-254">This implementation will work, but it is not quite complete.</span></span> <span data-ttu-id="59395-255">Idealmente, si sarebbe la risposta HTTP per includere i seguenti:</span><span class="sxs-lookup"><span data-stu-id="59395-255">Ideally, we would like the HTTP response to include the following:</span></span>

- <span data-ttu-id="59395-256">**Codice di risposta:** per impostazione predefinita, il framework Web API impostato il codice di stato di risposta 200 (OK).</span><span class="sxs-lookup"><span data-stu-id="59395-256">**Response code:** By default, the Web API framework sets the response status code to 200 (OK).</span></span> <span data-ttu-id="59395-257">Tuttavia, in base al protocollo HTTP/1.1, quando una richiesta POST comporta la creazione di una risorsa, il server deve rispondere con stato 201 (creato).</span><span class="sxs-lookup"><span data-stu-id="59395-257">But according to the HTTP/1.1 protocol, when a POST request results in the creation of a resource, the server should reply with status 201 (Created).</span></span>
- <span data-ttu-id="59395-258">**Percorso:** quando il server crea una risorsa, nell'intestazione Location della risposta deve includere l'URI della nuova risorsa.</span><span class="sxs-lookup"><span data-stu-id="59395-258">**Location:** When the server creates a resource, it should include the URI of the new resource in the Location header of the response.</span></span>

<span data-ttu-id="59395-259">ASP.NET Web API semplifica modificare il messaggio di risposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="59395-259">ASP.NET Web API makes it easy to manipulate the HTTP response message.</span></span> <span data-ttu-id="59395-260">Di seguito è riportata l'implementazione migliorata:</span><span class="sxs-lookup"><span data-stu-id="59395-260">Here is the improved implementation:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample10.cs)]

<span data-ttu-id="59395-261">Si noti che il tipo restituito del metodo è ora **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="59395-261">Notice that the method return type is now **HttpResponseMessage**.</span></span> <span data-ttu-id="59395-262">Restituendo un **HttpResponseMessage** anziché un prodotto, possiamo controllare i dettagli del messaggio di risposta HTTP, incluso il codice di stato e l'intestazione di posizione.</span><span class="sxs-lookup"><span data-stu-id="59395-262">By returning an **HttpResponseMessage** instead of a Product, we can control the details of the HTTP response message, including the status code and the Location header.</span></span>

<span data-ttu-id="59395-263">Il **CreateResponse** metodo crea un **HttpResponseMessage** e scrive automaticamente una rappresentazione serializzata dell'oggetto prodotto nel corpo fo il messaggio di risposta.</span><span class="sxs-lookup"><span data-stu-id="59395-263">The **CreateResponse** method creates an **HttpResponseMessage** and automatically writes a serialized representation of the Product object into the body fo the response message.</span></span>

> [!NOTE]
> <span data-ttu-id="59395-264">In questo esempio non convalida il `Product`.</span><span class="sxs-lookup"><span data-stu-id="59395-264">This example does not validate the `Product`.</span></span> <span data-ttu-id="59395-265">Per informazioni sulla convalida del modello, vedere [convalida del modello in ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="59395-265">For information about model validation, see [Model Validation in ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).</span></span>


## <a name="updating-a-resource"></a><span data-ttu-id="59395-266">Aggiornamento di una risorsa</span><span class="sxs-lookup"><span data-stu-id="59395-266">Updating a Resource</span></span>

<span data-ttu-id="59395-267">Aggiornamento di un prodotto con PUT è semplice:</span><span class="sxs-lookup"><span data-stu-id="59395-267">Updating a product with PUT is straightforward:</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample11.cs)]

<span data-ttu-id="59395-268">Il nome del metodo inizia con &quot;Put... &quot;, pertanto l'API Web a esso corrispondente per le richieste PUT.</span><span class="sxs-lookup"><span data-stu-id="59395-268">The method name starts with &quot;Put...&quot;, so Web API matches it to PUT requests.</span></span> <span data-ttu-id="59395-269">Il metodo accetta due parametri, l'ID prodotto e il prodotto aggiornato.</span><span class="sxs-lookup"><span data-stu-id="59395-269">The method takes two parameters, the product ID and the updated product.</span></span> <span data-ttu-id="59395-270">Il *id* parametro da cui proviene il percorso URI e *prodotto* parametro viene deserializzato dal corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="59395-270">The *id* parameter is taken from the URI path, and the *product* parameter is deserialized from the request body.</span></span> <span data-ttu-id="59395-271">Per impostazione predefinita, il framework di ASP.NET Web API accetta i tipi di parametro semplici dalla route e i tipi complessi dal corpo della richiesta.</span><span class="sxs-lookup"><span data-stu-id="59395-271">By default, the ASP.NET Web API framework takes simple parameter types from the route and complex types from the request body.</span></span>

## <a name="deleting-a-resource"></a><span data-ttu-id="59395-272">Eliminazione di una risorsa</span><span class="sxs-lookup"><span data-stu-id="59395-272">Deleting a Resource</span></span>

<span data-ttu-id="59395-273">Per eliminare una risorsa, definire un metodo di "Eliminare …".</span><span class="sxs-lookup"><span data-stu-id="59395-273">To delete a resourse, define a "Delete..." method.</span></span>

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample12.cs)]

<span data-ttu-id="59395-274">Se una richiesta di eliminazione ha esito positivo, restituisce lo stato 200 (OK) con un corpo dell'entità che descrive lo stato; lo stato 202 (accettato) se l'eliminazione non è ancora in sospeso; o di stato 204 (nessun contenuto) senza corpo entità.</span><span class="sxs-lookup"><span data-stu-id="59395-274">If a DELETE request succeeds, it can return status 200 (OK) with an entity-body that describes the status; status 202 (Accepted) if the deletion is still pending; or status 204 (No Content) with no entity body.</span></span> <span data-ttu-id="59395-275">In questo caso, il `DeleteProduct` metodo ha un `void` il tipo restituito, in modo da ASP.NET Web API converte automaticamente questo in stato codice 204 (nessun contenuto).</span><span class="sxs-lookup"><span data-stu-id="59395-275">In this case, the `DeleteProduct` method has a `void` return type, so ASP.NET Web API automatically translates this into status code 204 (No Content).</span></span>
