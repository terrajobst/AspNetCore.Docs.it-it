---
uid: web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
title: Introduzione a ASP.NET Web API 2 (c#)
author: MikeWasson
description: "HTTP non è disponibile solo per mettere a disposizione le pagine web. È anche una potente piattaforma per la compilazione di API che espongono servizi e dei dati. HTTP è semplice e flessibile e ubiq..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/28/2017
ms.topic: article
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
msc.type: authoredcontent
ms.openlocfilehash: 6ff9fd279a03197f761454bba3f180d7428b1b1f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="getting-started-with-aspnet-web-api-2-c"></a><span data-ttu-id="726a3-105">Introduzione a ASP.NET Web API 2 (c#)</span><span class="sxs-lookup"><span data-stu-id="726a3-105">Getting Started with ASP.NET Web API 2 (C#)</span></span>
====================
<span data-ttu-id="726a3-106">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="726a3-106">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="726a3-107">Scaricare il progetto completato</span><span class="sxs-lookup"><span data-stu-id="726a3-107">Download Completed Project</span></span>](https://code.msdn.microsoft.com/Sample-code-of-Getting-c56ccb28)

<span data-ttu-id="726a3-108">HTTP non è disponibile solo per mettere a disposizione le pagine web.</span><span class="sxs-lookup"><span data-stu-id="726a3-108">HTTP is not just for serving up web pages.</span></span> <span data-ttu-id="726a3-109">HTTP è una potente piattaforma per la compilazione di API che espongono servizi e dei dati.</span><span class="sxs-lookup"><span data-stu-id="726a3-109">HTTP is also a powerful platform for building APIs that expose services and data.</span></span> <span data-ttu-id="726a3-110">HTTP è semplice, flessibile e universale.</span><span class="sxs-lookup"><span data-stu-id="726a3-110">HTTP is simple, flexible, and ubiquitous.</span></span> <span data-ttu-id="726a3-111">Quasi tutte le piattaforme che è possibile considerare dispone di una libreria HTTP, pertanto i servizi HTTP possono raggiungere una vasta gamma di client, inclusi browser e i dispositivi mobili, applicazioni desktop tradizionali.</span><span class="sxs-lookup"><span data-stu-id="726a3-111">Almost any platform that you can think of has an HTTP library, so HTTP services can reach a broad range of clients, including browsers, mobile devices, and traditional desktop applications.</span></span>
 
<span data-ttu-id="726a3-112">API Web ASP.NET è un framework per la creazione di web API .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="726a3-112">ASP.NET Web API is a framework for building web APIs on top of the .NET Framework.</span></span> <span data-ttu-id="726a3-113">In questa esercitazione si utilizzerà ASP.NET Web API per creare una web API che restituisce un elenco di prodotti.</span><span class="sxs-lookup"><span data-stu-id="726a3-113">In this tutorial, you will use ASP.NET Web API to create a web API that returns a list of products.</span></span>

 
 ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="726a3-114">Versioni del software utilizzate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="726a3-114">Software versions used in the tutorial</span></span>
  
 - [<span data-ttu-id="726a3-115">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="726a3-115">Visual Studio 2017</span></span>](https://www.visualstudio.com/downloads/)
 - <span data-ttu-id="726a3-116">Web API 2</span><span class="sxs-lookup"><span data-stu-id="726a3-116">Web API 2</span></span>

<span data-ttu-id="726a3-117">Vedere [creare un'API web con ASP.NET Core e Visual Studio per Windows](https://docs.microsoft.com/aspnet/core/tutorials/first-web-api) per una versione più recente di questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="726a3-117">See [Create a web API with ASP.NET Core and Visual Studio for Windows](https://docs.microsoft.com/aspnet/core/tutorials/first-web-api) for a newer version of this tutorial.</span></span>

## <a name="create-a-web-api-project"></a><span data-ttu-id="726a3-118">Creare un progetto di API Web</span><span class="sxs-lookup"><span data-stu-id="726a3-118">Create a Web API Project</span></span>

<span data-ttu-id="726a3-119">In questa esercitazione si utilizzerà ASP.NET Web API per creare una web API che restituisce un elenco di prodotti.</span><span class="sxs-lookup"><span data-stu-id="726a3-119">In this tutorial, you will use ASP.NET Web API to create a web API that returns a list of products.</span></span> <span data-ttu-id="726a3-120">La pagina web front-end utilizza jQuery per visualizzare i risultati.</span><span class="sxs-lookup"><span data-stu-id="726a3-120">The front-end web page uses jQuery to display the results.</span></span>

![](tutorial-your-first-web-api/_static/image1.png)

<span data-ttu-id="726a3-121">Avviare Visual Studio e selezionare **nuovo progetto** dal **avviare** pagina.</span><span class="sxs-lookup"><span data-stu-id="726a3-121">Start Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="726a3-122">O dal **File** dal menu **New** e quindi **progetto**.</span><span class="sxs-lookup"><span data-stu-id="726a3-122">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="726a3-123">Nel **modelli** riquadro, selezionare **modelli installati** ed espandere il **Visual c#** nodo.</span><span class="sxs-lookup"><span data-stu-id="726a3-123">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="726a3-124">In **Visual c#**selezionare **Web**.</span><span class="sxs-lookup"><span data-stu-id="726a3-124">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="726a3-125">Nell'elenco dei modelli di progetto, selezionare **applicazione Web ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="726a3-125">In the list of project templates, select **ASP.NET Web Application**.</span></span> <span data-ttu-id="726a3-126">Denominare il progetto "ProductsApp" e fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="726a3-126">Name the project "ProductsApp" and click **OK**.</span></span>

![](tutorial-your-first-web-api/_static/image2.png)

<span data-ttu-id="726a3-127">Nel **nuovo progetto ASP.NET** finestra di dialogo Seleziona il **vuoto** modello.</span><span class="sxs-lookup"><span data-stu-id="726a3-127">In the **New ASP.NET Project** dialog, select the **Empty** template.</span></span> <span data-ttu-id="726a3-128">In &quot;aggiungere cartelle e i riferimenti per core&quot;, controllare **API Web**.</span><span class="sxs-lookup"><span data-stu-id="726a3-128">Under &quot;Add folders and core references for&quot;, check **Web API**.</span></span> <span data-ttu-id="726a3-129">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="726a3-129">Click **OK**.</span></span>

![](tutorial-your-first-web-api/_static/image3.png)

> [!NOTE]
> <span data-ttu-id="726a3-130">È inoltre possibile creare un progetto di API Web utilizzando il &quot;API Web&quot; modello.</span><span class="sxs-lookup"><span data-stu-id="726a3-130">You can also create a Web API project using the &quot;Web API&quot; template.</span></span> <span data-ttu-id="726a3-131">Il modello API Web utilizza ASP.NET MVC per fornire le pagine della Guida di API.</span><span class="sxs-lookup"><span data-stu-id="726a3-131">The Web API template uses ASP.NET MVC to provide API help pages.</span></span> <span data-ttu-id="726a3-132">Si utilizza il modello vuoto per questa esercitazione, poiché si desidera mostrare API Web senza MVC.</span><span class="sxs-lookup"><span data-stu-id="726a3-132">I'm using the Empty template for this tutorial because I want to show Web API without MVC.</span></span> <span data-ttu-id="726a3-133">In generale, non è necessario conoscere ASP.NET MVC per utilizzare l'API Web.</span><span class="sxs-lookup"><span data-stu-id="726a3-133">In general, you don't need to know ASP.NET MVC to use Web API.</span></span>


## <a name="adding-a-model"></a><span data-ttu-id="726a3-134">Aggiunta di un modello</span><span class="sxs-lookup"><span data-stu-id="726a3-134">Adding a Model</span></span>

<span data-ttu-id="726a3-135">Un *modello* è un oggetto che rappresenta i dati nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="726a3-135">A *model* is an object that represents the data in your application.</span></span> <span data-ttu-id="726a3-136">ASP.NET Web API può serializzare automaticamente il modello a JSON, XML o un altro formato e quindi scrivere i dati serializzati nel corpo del messaggio di risposta HTTP.</span><span class="sxs-lookup"><span data-stu-id="726a3-136">ASP.NET Web API can automatically serialize your model to JSON, XML, or some other format, and then write the serialized data into the body of the HTTP response message.</span></span> <span data-ttu-id="726a3-137">Fino a quando un client può leggere il formato di serializzazione, è possibile deserializzare l'oggetto.</span><span class="sxs-lookup"><span data-stu-id="726a3-137">As long as a client can read the serialization format, it can deserialize the object.</span></span> <span data-ttu-id="726a3-138">La maggior parte dei client consente di analizzare XML o JSON.</span><span class="sxs-lookup"><span data-stu-id="726a3-138">Most clients can parse either XML or JSON.</span></span> <span data-ttu-id="726a3-139">Inoltre, il client può indicare il formato deve impostando l'intestazione Accept nel messaggio di richiesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="726a3-139">Moreover, the client can indicate which format it wants by setting the Accept header in the HTTP request message.</span></span>

<span data-ttu-id="726a3-140">Iniziamo creando un modello semplice che rappresenta un prodotto.</span><span class="sxs-lookup"><span data-stu-id="726a3-140">Let's start by creating a simple model that represents a product.</span></span>

<span data-ttu-id="726a3-141">Se Esplora soluzioni non è visibile, fare clic su di **vista** dal menu **Esplora**.</span><span class="sxs-lookup"><span data-stu-id="726a3-141">If Solution Explorer is not already visible, click the **View** menu and select **Solution Explorer**.</span></span> <span data-ttu-id="726a3-142">In Esplora soluzioni, fare clic sulla cartella Models.</span><span class="sxs-lookup"><span data-stu-id="726a3-142">In Solution Explorer, right-click the Models folder.</span></span> <span data-ttu-id="726a3-143">Dal menu di scelta rapida, selezionare **Aggiungi** selezionare **classe**.</span><span class="sxs-lookup"><span data-stu-id="726a3-143">From the context menu, select **Add** then select **Class**.</span></span>

![](tutorial-your-first-web-api/_static/image4.png)

<span data-ttu-id="726a3-144">Nome della classe &quot;prodotto&quot;.</span><span class="sxs-lookup"><span data-stu-id="726a3-144">Name the class &quot;Product&quot;.</span></span> <span data-ttu-id="726a3-145">Aggiungere le seguenti proprietà per il `Product` classe.</span><span class="sxs-lookup"><span data-stu-id="726a3-145">Add the following properties to the `Product` class.</span></span>

[!code-csharp[Main](tutorial-your-first-web-api/samples/sample1.cs)]

## <a name="adding-a-controller"></a><span data-ttu-id="726a3-146">Aggiunta di un controller</span><span class="sxs-lookup"><span data-stu-id="726a3-146">Adding a Controller</span></span>

<span data-ttu-id="726a3-147">Nell'API Web, un *controller* è un oggetto che gestisce le richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="726a3-147">In Web API, a *controller* is an object that handles HTTP requests.</span></span> <span data-ttu-id="726a3-148">Verrà aggiunto un controller che può restituire un elenco di prodotti o un singolo prodotto specificato dall'ID.</span><span class="sxs-lookup"><span data-stu-id="726a3-148">We'll add a controller that can return either a list of products or a single product specified by ID.</span></span>

> [!NOTE]
> <span data-ttu-id="726a3-149">Se si utilizza ASP.NET MVC, si ha già familiarità con controller.</span><span class="sxs-lookup"><span data-stu-id="726a3-149">If you have used ASP.NET MVC, you are already familiar with controllers.</span></span> <span data-ttu-id="726a3-150">Controller Web API sono simili ai controller MVC, ma ereditano la **ApiController** classe anziché il **Controller** classe.</span><span class="sxs-lookup"><span data-stu-id="726a3-150">Web API controllers are similar to MVC controllers, but inherit the **ApiController** class instead of the **Controller** class.</span></span>

<span data-ttu-id="726a3-151">In **Esplora**, fare clic sulla cartella controller.</span><span class="sxs-lookup"><span data-stu-id="726a3-151">In **Solution Explorer**, right-click the Controllers folder.</span></span> <span data-ttu-id="726a3-152">Selezionare **Aggiungi** e quindi selezionare **Controller**.</span><span class="sxs-lookup"><span data-stu-id="726a3-152">Select **Add** and then select **Controller**.</span></span>

![](tutorial-your-first-web-api/_static/image5.png)

<span data-ttu-id="726a3-153">Nel **aggiungere lo scaffolding** finestra di dialogo Seleziona **Controller API Web - vuoto**.</span><span class="sxs-lookup"><span data-stu-id="726a3-153">In the **Add Scaffold** dialog, select **Web API Controller - Empty**.</span></span> <span data-ttu-id="726a3-154">Fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="726a3-154">Click **Add**.</span></span>

![](tutorial-your-first-web-api/_static/image6.png)

<span data-ttu-id="726a3-155">Nel **Aggiungi Controller** finestra di dialogo, nome del controller &quot;ProductsController&quot;.</span><span class="sxs-lookup"><span data-stu-id="726a3-155">In the **Add Controller** dialog, name the controller &quot;ProductsController&quot;.</span></span> <span data-ttu-id="726a3-156">Fare clic su **Aggiungi**.</span><span class="sxs-lookup"><span data-stu-id="726a3-156">Click **Add**.</span></span>

![](tutorial-your-first-web-api/_static/image7.png)

<span data-ttu-id="726a3-157">Lo scaffolding consente di creare un file denominato ProductsController.cs nella cartella controller.</span><span class="sxs-lookup"><span data-stu-id="726a3-157">The scaffolding creates a file named ProductsController.cs in the Controllers folder.</span></span>

![](tutorial-your-first-web-api/_static/image8.png)

> [!NOTE]
> <span data-ttu-id="726a3-158">Non è necessario inserire i controller in una cartella denominata controller.</span><span class="sxs-lookup"><span data-stu-id="726a3-158">You don't need to put your controllers into a folder named Controllers.</span></span> <span data-ttu-id="726a3-159">Il nome della cartella è semplicemente un modo pratico per organizzare i file di origine.</span><span class="sxs-lookup"><span data-stu-id="726a3-159">The folder name is just a convenient way to organize your source files.</span></span>


<span data-ttu-id="726a3-160">Se questo file non è già aperto, fare doppio clic sul file per aprirlo.</span><span class="sxs-lookup"><span data-stu-id="726a3-160">If this file is not open already, double-click the file to open it.</span></span> <span data-ttu-id="726a3-161">Sostituire il codice in questo file con le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="726a3-161">Replace the code in this file with the following:</span></span>

[!code-csharp[Main](tutorial-your-first-web-api/samples/sample2.cs)]

<span data-ttu-id="726a3-162">Per semplificare l'esempio, i prodotti vengono archiviati in una matrice fissa all'interno della classe controller.</span><span class="sxs-lookup"><span data-stu-id="726a3-162">To keep the example simple, products are stored in a fixed array inside the controller class.</span></span> <span data-ttu-id="726a3-163">In un'applicazione reale, naturalmente, sarebbe query a un database o utilizzare un'altra origine dati esterna.</span><span class="sxs-lookup"><span data-stu-id="726a3-163">Of course, in a real application, you would query a database or use some other external data source.</span></span>

<span data-ttu-id="726a3-164">Il controller definisce due metodi che restituiscono i prodotti:</span><span class="sxs-lookup"><span data-stu-id="726a3-164">The controller defines two methods that return products:</span></span>

- <span data-ttu-id="726a3-165">Il `GetAllProducts` il metodo restituisce l'intero elenco dei prodotti come un **IEnumerable&lt;prodotto&gt;**  tipo.</span><span class="sxs-lookup"><span data-stu-id="726a3-165">The `GetAllProducts` method returns the entire list of products as an **IEnumerable&lt;Product&gt;** type.</span></span>
- <span data-ttu-id="726a3-166">Il `GetProduct` metodo cerca un singolo prodotto dal relativo ID.</span><span class="sxs-lookup"><span data-stu-id="726a3-166">The `GetProduct` method looks up a single product by its ID.</span></span>

<span data-ttu-id="726a3-167">La procedura è terminata.</span><span class="sxs-lookup"><span data-stu-id="726a3-167">That's it!</span></span> <span data-ttu-id="726a3-168">È un'API web di lavoro.</span><span class="sxs-lookup"><span data-stu-id="726a3-168">You have a working web API.</span></span> <span data-ttu-id="726a3-169">Ogni metodo sul controller corrisponde a uno o più URI:</span><span class="sxs-lookup"><span data-stu-id="726a3-169">Each method on the controller corresponds to one or more URIs:</span></span>

| <span data-ttu-id="726a3-170">Metodo del controller</span><span class="sxs-lookup"><span data-stu-id="726a3-170">Controller Method</span></span> | <span data-ttu-id="726a3-171">URI</span><span class="sxs-lookup"><span data-stu-id="726a3-171">URI</span></span> |
| --- | --- |
| <span data-ttu-id="726a3-172">GetAllProducts</span><span class="sxs-lookup"><span data-stu-id="726a3-172">GetAllProducts</span></span> | <span data-ttu-id="726a3-173">prodotti/api /</span><span class="sxs-lookup"><span data-stu-id="726a3-173">/api/products</span></span> |
| <span data-ttu-id="726a3-174">GetProduct</span><span class="sxs-lookup"><span data-stu-id="726a3-174">GetProduct</span></span> | <span data-ttu-id="726a3-175">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="726a3-175">/api/products/*id*</span></span> |

<span data-ttu-id="726a3-176">Per il `GetProduct` (metodo), il *id* nell'URI è un segnaposto.</span><span class="sxs-lookup"><span data-stu-id="726a3-176">For the `GetProduct` method, the *id* in the URI is a placeholder.</span></span> <span data-ttu-id="726a3-177">Per ottenere il prodotto con ID 5, ad esempio, l'URI è `api/products/5`.</span><span class="sxs-lookup"><span data-stu-id="726a3-177">For example, to get the product with ID of 5, the URI is `api/products/5`.</span></span>

<span data-ttu-id="726a3-178">Per ulteriori informazioni su come API Web indirizza le richieste HTTP per i metodi del controller, vedere [Routing in ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="726a3-178">For more information about how Web API routes HTTP requests to controller methods, see [Routing in ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span></span>

## <a name="calling-the-web-api-with-javascript-and-jquery"></a><span data-ttu-id="726a3-179">Chiama l'API Web con Javascript e jQuery</span><span class="sxs-lookup"><span data-stu-id="726a3-179">Calling the Web API with Javascript and jQuery</span></span>

<span data-ttu-id="726a3-180">In questa sezione verrà aggiunto una pagina HTML che viene utilizzato AJAX per chiamare l'API web.</span><span class="sxs-lookup"><span data-stu-id="726a3-180">In this section, we'll add an HTML page that uses AJAX to call the web API.</span></span> <span data-ttu-id="726a3-181">Per eseguire le chiamate AJAX e aggiornare la pagina dei risultati, si userà jQuery.</span><span class="sxs-lookup"><span data-stu-id="726a3-181">We'll use jQuery to make the AJAX calls and also to update the page with the results.</span></span>

<span data-ttu-id="726a3-182">In Esplora soluzioni, fare clic sul progetto e selezionare **Aggiungi**, quindi selezionare **nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="726a3-182">In Solution Explorer, right-click the project and select **Add**, then select **New Item**.</span></span>

![](tutorial-your-first-web-api/_static/image9.png)

<span data-ttu-id="726a3-183">Nel **Aggiungi nuovo elemento** finestra di dialogo Seleziona il **Web** nodo **Visual c#**e quindi selezionare il **pagina HTML** elemento.</span><span class="sxs-lookup"><span data-stu-id="726a3-183">In the **Add New Item** dialog, select the **Web** node under **Visual C#**, and then select the **HTML Page** item.</span></span> <span data-ttu-id="726a3-184">Denominare la pagina &quot;index.html&quot;.</span><span class="sxs-lookup"><span data-stu-id="726a3-184">Name the page &quot;index.html&quot;.</span></span>

![](tutorial-your-first-web-api/_static/image10.png)

<span data-ttu-id="726a3-185">Sostituire tutto il contenuto in questo file con le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="726a3-185">Replace everything in this file with the following:</span></span>

[!code-html[Main](tutorial-your-first-web-api/samples/sample3.html)]

<span data-ttu-id="726a3-186">Esistono diversi modi per ottenere jQuery.</span><span class="sxs-lookup"><span data-stu-id="726a3-186">There are several ways to get jQuery.</span></span> <span data-ttu-id="726a3-187">In questo esempio utilizzato il [Microsoft Ajax CDN](../../../ajax/cdn/overview.md).</span><span class="sxs-lookup"><span data-stu-id="726a3-187">In this example, I used the [Microsoft Ajax CDN](../../../ajax/cdn/overview.md).</span></span> <span data-ttu-id="726a3-188">È anche possibile scaricarlo da [http://jquery.com/](http://jquery.com/), ASP.NET "API Web" e il modello di progetto include anche jQuery.</span><span class="sxs-lookup"><span data-stu-id="726a3-188">You can also download it from [http://jquery.com/](http://jquery.com/), and the ASP.NET "Web API" project template includes jQuery as well.</span></span>

### <a name="getting-a-list-of-products"></a><span data-ttu-id="726a3-189">Come ottenere un elenco di prodotti</span><span class="sxs-lookup"><span data-stu-id="726a3-189">Getting a List of Products</span></span>

<span data-ttu-id="726a3-190">Per ottenere un elenco di prodotti, inviare una richiesta HTTP GET per &quot;/api/prodotti&quot;.</span><span class="sxs-lookup"><span data-stu-id="726a3-190">To get a list of products, send an HTTP GET request to &quot;/api/products&quot;.</span></span>

<span data-ttu-id="726a3-191">Il jQuery [getJSON](http://api.jquery.com/jQuery.getJSON/) funzione invia una richiesta AJAX.</span><span class="sxs-lookup"><span data-stu-id="726a3-191">The jQuery [getJSON](http://api.jquery.com/jQuery.getJSON/) function sends an AJAX request.</span></span> <span data-ttu-id="726a3-192">Per la risposta contiene una matrice di oggetti JSON.</span><span class="sxs-lookup"><span data-stu-id="726a3-192">For response contains array of JSON objects.</span></span> <span data-ttu-id="726a3-193">Il `done` funzione specifica un callback che viene chiamato se la richiesta ha esito positivo.</span><span class="sxs-lookup"><span data-stu-id="726a3-193">The `done` function specifies a callback that is called if the request succeeds.</span></span> <span data-ttu-id="726a3-194">Nella richiamata, DOM viene aggiornato con le informazioni sul prodotto.</span><span class="sxs-lookup"><span data-stu-id="726a3-194">In the callback, we update the DOM with the product information.</span></span>

[!code-html[Main](tutorial-your-first-web-api/samples/sample4.html)]

### <a name="getting-a-product-by-id"></a><span data-ttu-id="726a3-195">Recupero di un prodotto in base all'ID</span><span class="sxs-lookup"><span data-stu-id="726a3-195">Getting a Product By ID</span></span>

<span data-ttu-id="726a3-196">Per ottenere un prodotto dall'ID, inviare una richiesta HTTP GET per &quot;/api/prodotti/*id*&quot;, dove *id* è l'ID prodotto.</span><span class="sxs-lookup"><span data-stu-id="726a3-196">To get a product by ID, send an HTTP GET request to &quot;/api/products/*id*&quot;, where *id* is the product ID.</span></span>

[!code-javascript[Main](tutorial-your-first-web-api/samples/sample5.js)]

<span data-ttu-id="726a3-197">È comunque possibile chiamare `getJSON` per inviare la richiesta AJAX, ma questa volta l'ID è stato inserito in URI della richiesta.</span><span class="sxs-lookup"><span data-stu-id="726a3-197">We still call `getJSON` to send the AJAX request, but this time we put the ID in the request URI.</span></span> <span data-ttu-id="726a3-198">La risposta da questa richiesta è una rappresentazione JSON di un singolo prodotto.</span><span class="sxs-lookup"><span data-stu-id="726a3-198">The response from this request is a JSON representation of a single product.</span></span>

## <a name="running-the-application"></a><span data-ttu-id="726a3-199">Esecuzione dell'applicazione</span><span class="sxs-lookup"><span data-stu-id="726a3-199">Running the Application</span></span>

<span data-ttu-id="726a3-200">Premere F5 per avviare il debug dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="726a3-200">Press F5 to start debugging the application.</span></span> <span data-ttu-id="726a3-201">La pagina web dovrebbe essere simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="726a3-201">The web page should look like the following:</span></span>

![](tutorial-your-first-web-api/_static/image11.png)

<span data-ttu-id="726a3-202">Per ottenere un prodotto dall'ID, immettere l'ID e fare clic su Cerca:</span><span class="sxs-lookup"><span data-stu-id="726a3-202">To get a product by ID, enter the ID and click Search:</span></span>

![](tutorial-your-first-web-api/_static/image12.png)

<span data-ttu-id="726a3-203">Se si immette un ID non valido, il server restituisce un errore HTTP:</span><span class="sxs-lookup"><span data-stu-id="726a3-203">If you enter an invalid ID, the server returns an HTTP error:</span></span>

![](tutorial-your-first-web-api/_static/image13.png)

## <a name="using-f12-to-view-the-http-request-and-response"></a><span data-ttu-id="726a3-204">Utilizzo di F12 per visualizzare la richiesta e risposta HTTP</span><span class="sxs-lookup"><span data-stu-id="726a3-204">Using F12 to View the HTTP Request and Response</span></span>

<span data-ttu-id="726a3-205">Quando si lavora con un servizio HTTP, può essere molto utile per visualizzare la richiesta HTTP e messaggi di richiesta.</span><span class="sxs-lookup"><span data-stu-id="726a3-205">When you are working with an HTTP service, it can be very useful to see the HTTP request and request messages.</span></span> <span data-ttu-id="726a3-206">È possibile farlo tramite gli strumenti di sviluppo F12 in Internet Explorer 9.</span><span class="sxs-lookup"><span data-stu-id="726a3-206">You can do this by using the F12 developer tools in Internet Explorer 9.</span></span> <span data-ttu-id="726a3-207">In Internet Explorer 9, premere **F12** per aprire gli strumenti.</span><span class="sxs-lookup"><span data-stu-id="726a3-207">From Internet Explorer 9, press **F12** to open the tools.</span></span> <span data-ttu-id="726a3-208">Fare clic su di **rete** scheda e premere **Avvia acquisizione**.</span><span class="sxs-lookup"><span data-stu-id="726a3-208">Click the **Network** tab and press **Start Capturing**.</span></span> <span data-ttu-id="726a3-209">Passare alla pagina web e premere **F5** a ricaricare la pagina web.</span><span class="sxs-lookup"><span data-stu-id="726a3-209">Now go back to the web page and press **F5** to reload the web page.</span></span> <span data-ttu-id="726a3-210">Internet Explorer consente di acquisire il traffico HTTP tra il browser e il server web.</span><span class="sxs-lookup"><span data-stu-id="726a3-210">Internet Explorer will capture the HTTP traffic between the browser and the web server.</span></span> <span data-ttu-id="726a3-211">La visualizzazione di riepilogo Mostra tutto il traffico di rete per una pagina:</span><span class="sxs-lookup"><span data-stu-id="726a3-211">The summary view shows all the network traffic for a page:</span></span>

![](tutorial-your-first-web-api/_static/image14.png)

<span data-ttu-id="726a3-212">Individuare la voce per l'URI relativo "api/prodotti /".</span><span class="sxs-lookup"><span data-stu-id="726a3-212">Locate the entry for the relative URI "api/products/".</span></span> <span data-ttu-id="726a3-213">Selezionare questa voce e fare clic su **passare alla visualizzazione dettagliata**.</span><span class="sxs-lookup"><span data-stu-id="726a3-213">Select this entry and click **Go to detailed view**.</span></span> <span data-ttu-id="726a3-214">Nella vista di dettaglio, sono disponibili schede per visualizzare le intestazioni di richiesta e risposta e corpi.</span><span class="sxs-lookup"><span data-stu-id="726a3-214">In the detail view, there are tabs to view the request and response headers and bodies.</span></span> <span data-ttu-id="726a3-215">Ad esempio, se si sceglie il **le intestazioni di richiesta** scheda, è possibile vedere che il client ha richiesto &quot;application/json&quot; nell'intestazione Accept.</span><span class="sxs-lookup"><span data-stu-id="726a3-215">For example, if you click the **Request headers** tab, you can see that the client requested &quot;application/json&quot; in the Accept header.</span></span>

![](tutorial-your-first-web-api/_static/image15.png)

<span data-ttu-id="726a3-216">Se si fa clic su scheda corpo della risposta, è possibile visualizzare come l'elenco dei prodotti è stato serializzato in JSON.</span><span class="sxs-lookup"><span data-stu-id="726a3-216">If you click the Response body tab, you can see how the product list was serialized to JSON.</span></span> <span data-ttu-id="726a3-217">Altri browser hanno una funzionalità simile.</span><span class="sxs-lookup"><span data-stu-id="726a3-217">Other browsers have similar functionality.</span></span> <span data-ttu-id="726a3-218">Un altro strumento utile è [Fiddler](http://www.fiddler2.com/fiddler2/), un web proxy per debug.</span><span class="sxs-lookup"><span data-stu-id="726a3-218">Another useful tool is [Fiddler](http://www.fiddler2.com/fiddler2/), a web debugging proxy.</span></span> <span data-ttu-id="726a3-219">È possibile utilizzare Fiddler per visualizzare il traffico HTTP e per comporre le richieste HTTP, che consente il controllo completo di intestazioni HTTP nella richiesta.</span><span class="sxs-lookup"><span data-stu-id="726a3-219">You can use Fiddler to view your HTTP traffic, and also to compose HTTP requests, which gives you full control over the HTTP headers in the request.</span></span>

## <a name="see-this-app-running-on-azure"></a><span data-ttu-id="726a3-220">Vedere l'App in esecuzione in Azure</span><span class="sxs-lookup"><span data-stu-id="726a3-220">See this App Running on Azure</span></span>

<span data-ttu-id="726a3-221">Si desidera vedere il sito completo in esecuzione come un'app web in tempo reale?</span><span class="sxs-lookup"><span data-stu-id="726a3-221">Would you like to see the finished site running as a live web app?</span></span> <span data-ttu-id="726a3-222">È possibile distribuire una versione completa dell'app al proprio account Azure, facendo semplicemente clic sul pulsante seguente.</span><span class="sxs-lookup"><span data-stu-id="726a3-222">You can deploy a complete version of the app to your Azure account by simply clicking the following button.</span></span>

[![](http://azuredeploy.net/deploybutton.png)](https://deploy.azure.com/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/WebAPI-ProductsApp#/form/setup)

<span data-ttu-id="726a3-223">È necessario un account Azure per distribuire questa soluzione in Azure.</span><span class="sxs-lookup"><span data-stu-id="726a3-223">You need an Azure account to deploy this solution to Azure.</span></span> <span data-ttu-id="726a3-224">Se si dispone già di un account, sono disponibili le opzioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="726a3-224">If you do not already have an account, you have the following options:</span></span>

- <span data-ttu-id="726a3-225">[Aprire un account Azure, gratuitamente](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -si ottiene crediti è possibile utilizzare per provare i servizi di Azure a pagamento e anche dopo l'uso massimo è possibile mantenere l'account e utilizzare senza servizi di Azure.</span><span class="sxs-lookup"><span data-stu-id="726a3-225">[Open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
- <span data-ttu-id="726a3-226">[Attivare i benefici per sottoscrittori MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -Your sottoscrizione MSDN offre crediti ogni mese in cui è possibile utilizzare per i servizi di Azure a pagamento.</span><span class="sxs-lookup"><span data-stu-id="726a3-226">[Activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

## <a name="next-steps"></a><span data-ttu-id="726a3-227">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="726a3-227">Next Steps</span></span>

- <span data-ttu-id="726a3-228">Per un esempio più completo di un servizio HTTP che supporta azioni di POST, PUT e DELETE e scrive in un database, vedere [mediante API Web 2 con Entity Framework 6](../data/using-web-api-with-entity-framework/part-1.md).</span><span class="sxs-lookup"><span data-stu-id="726a3-228">For a more complete example of an HTTP service that supports POST, PUT, and DELETE actions and writes to a database, see [Using Web API 2 with Entity Framework 6](../data/using-web-api-with-entity-framework/part-1.md).</span></span>
- <span data-ttu-id="726a3-229">Per ulteriori informazioni sulla creazione di applicazioni web flessibile ed efficiente all'inizio di un servizio HTTP, vedere [applicazione a pagina singola ASP.NET](../../../single-page-application/index.md).</span><span class="sxs-lookup"><span data-stu-id="726a3-229">For more about creating fluid and responsive web applications on top of an HTTP service, see [ASP.NET Single Page Application](../../../single-page-application/index.md).</span></span>
- <span data-ttu-id="726a3-230">Per informazioni su come distribuire un progetto web di Visual Studio in Azure App Service, vedere [creare un'app web ASP.NET in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).</span><span class="sxs-lookup"><span data-stu-id="726a3-230">For information about how to deploy a Visual Studio web project to Azure App Service, see [Create an ASP.NET web app in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).</span></span>
