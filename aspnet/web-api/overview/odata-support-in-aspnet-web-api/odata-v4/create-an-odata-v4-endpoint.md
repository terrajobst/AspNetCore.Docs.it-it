---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
title: Creare un Endpoint di OData v4 con ASP.NET Web API 2.2 | Documenti Microsoft
author: MikeWasson
description: "Open Data Protocol (OData) è un protocollo di accesso ai dati per il web. OData fornisce un metodo uniforme per query e modificare i set di dati tramite operazioni CRUD..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/24/2014
ms.topic: article
ms.assetid: 1e1927c0-ded1-4752-80fd-a146628d2f09
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
msc.type: authoredcontent
ms.openlocfilehash: a3f94818f9674b0e1e9a45b2a6cc9455edc79726
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="create-an-odata-v4-endpoint-using-aspnet-web-api-22"></a><span data-ttu-id="b0746-104">Creare un Endpoint di OData v4 con ASP.NET Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="b0746-104">Create an OData v4 Endpoint Using ASP.NET Web API 2.2</span></span>
====================
<span data-ttu-id="b0746-105">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="b0746-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="b0746-106">Open Data Protocol (OData) è un protocollo di accesso ai dati per il web.</span><span class="sxs-lookup"><span data-stu-id="b0746-106">The Open Data Protocol (OData) is a data access protocol for the web.</span></span> <span data-ttu-id="b0746-107">OData fornisce un metodo uniforme per query e modificare i set di dati tramite operazioni CRUD (creare, leggere, aggiornare ed eliminare).</span><span class="sxs-lookup"><span data-stu-id="b0746-107">OData provides a uniform way to query and manipulate data sets through CRUD operations (create, read, update, and delete).</span></span>
> 
> <span data-ttu-id="b0746-108">API Web ASP.NET supporta v3 e v4 del protocollo.</span><span class="sxs-lookup"><span data-stu-id="b0746-108">ASP.NET Web API supports both v3 and v4 of the protocol.</span></span> <span data-ttu-id="b0746-109">È inoltre possibile impostare un endpoint v4 eseguito side-by-side con un endpoint v3.</span><span class="sxs-lookup"><span data-stu-id="b0746-109">You can even have a v4 endpoint that runs side-by-side with a v3 endpoint.</span></span>
> 
> <span data-ttu-id="b0746-110">In questa esercitazione viene illustrato come creare un endpoint di OData v4 che supporta operazioni CRUD.</span><span class="sxs-lookup"><span data-stu-id="b0746-110">This tutorial shows how to create an OData v4 endpoint that supports CRUD operations.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="b0746-111">Versioni del software utilizzate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="b0746-111">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="b0746-112">2.2 API Web</span><span class="sxs-lookup"><span data-stu-id="b0746-112">Web API 2.2</span></span>
> - <span data-ttu-id="b0746-113">OData v4</span><span class="sxs-lookup"><span data-stu-id="b0746-113">OData v4</span></span>
> - [<span data-ttu-id="b0746-114">Visual Studio 2013 Update 2</span><span class="sxs-lookup"><span data-stu-id="b0746-114">Visual Studio 2013 Update 2</span></span>](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - <span data-ttu-id="b0746-115">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="b0746-115">Entity Framework 6</span></span>
> - <span data-ttu-id="b0746-116">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="b0746-116">.NET 4.5</span></span>
> 
> 
> ## <a name="tutorial-versions"></a><span data-ttu-id="b0746-117">Versioni dell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="b0746-117">Tutorial versions</span></span>
> 
> <span data-ttu-id="b0746-118">Per OData versione 3, vedere [creazione di un Endpoint di OData v3](../odata-v3/creating-an-odata-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="b0746-118">For the OData Version 3, see [Creating an OData v3 Endpoint](../odata-v3/creating-an-odata-endpoint.md).</span></span>


## <a name="create-the-visual-studio-project"></a><span data-ttu-id="b0746-119">Creare il progetto di Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b0746-119">Create the Visual Studio Project</span></span>

<span data-ttu-id="b0746-120">In Visual Studio, dal **File** dal menu **New** &gt; **progetto**.</span><span class="sxs-lookup"><span data-stu-id="b0746-120">In Visual Studio, from the **File** menu, select **New** &gt; **Project**.</span></span>

<span data-ttu-id="b0746-121">Espandere **installato** &gt; **modelli** &gt; **Visual c#** &gt; **Web**e selezionare il  **Applicazione Web ASP.NET** modello.</span><span class="sxs-lookup"><span data-stu-id="b0746-121">Expand **Installed** &gt; **Templates** &gt; **Visual C#** &gt; **Web**, and select the **ASP.NET Web Application** template.</span></span> <span data-ttu-id="b0746-122">Denominare il progetto &quot;ProductService&quot;.</span><span class="sxs-lookup"><span data-stu-id="b0746-122">Name the project &quot;ProductService&quot;.</span></span>

[![](create-an-odata-v4-endpoint/_static/image2.png)](create-an-odata-v4-endpoint/_static/image1.png)

<span data-ttu-id="b0746-123">Nel **nuovo progetto** finestra di dialogo Seleziona il **vuoto** modello.</span><span class="sxs-lookup"><span data-stu-id="b0746-123">In the **New Project** dialog, select the **Empty** template.</span></span> <span data-ttu-id="b0746-124">In &quot;aggiungere cartelle e i riferimenti di base... &quot;, fare clic su **API Web**.</span><span class="sxs-lookup"><span data-stu-id="b0746-124">Under &quot;Add folders and core references...&quot;, click **Web API**.</span></span> <span data-ttu-id="b0746-125">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="b0746-125">Click **OK**.</span></span>

[![](create-an-odata-v4-endpoint/_static/image4.png)](create-an-odata-v4-endpoint/_static/image3.png)

## <a name="install-the-odata-packages"></a><span data-ttu-id="b0746-126">Installare i pacchetti di OData</span><span class="sxs-lookup"><span data-stu-id="b0746-126">Install the OData Packages</span></span>

<span data-ttu-id="b0746-127">Dal **strumenti** dal menu **Gestione pacchetti NuGet** &gt; **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="b0746-127">From the **Tools** menu, select **NuGet Package Manager** &gt; **Package Manager Console**.</span></span> <span data-ttu-id="b0746-128">Nella finestra della Console di gestione pacchetti, digitare:</span><span class="sxs-lookup"><span data-stu-id="b0746-128">In the Package Manager Console window, type:</span></span>

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample1.cmd)]

<span data-ttu-id="b0746-129">Questo comando consente di installare i pacchetti di OData NuGet più recenti.</span><span class="sxs-lookup"><span data-stu-id="b0746-129">This command installs the latest OData NuGet packages.</span></span>

## <a name="add-a-model-class"></a><span data-ttu-id="b0746-130">Aggiungere una classe modello</span><span class="sxs-lookup"><span data-stu-id="b0746-130">Add a Model Class</span></span>

<span data-ttu-id="b0746-131">Oggetto *modello* è un oggetto che rappresenta un'entità di dati nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="b0746-131">A *model* is an object that represents a data entity in your application.</span></span>

<span data-ttu-id="b0746-132">In Esplora soluzioni, fare clic sulla cartella Models.</span><span class="sxs-lookup"><span data-stu-id="b0746-132">In Solution Explorer, right-click the Models folder.</span></span> <span data-ttu-id="b0746-133">Dal menu di scelta rapida, selezionare **Aggiungi** &gt; **classe**.</span><span class="sxs-lookup"><span data-stu-id="b0746-133">From the context menu, select **Add** &gt; **Class**.</span></span>

[![](create-an-odata-v4-endpoint/_static/image6.png)](create-an-odata-v4-endpoint/_static/image5.png)

> [!NOTE]
> <span data-ttu-id="b0746-134">Per convenzione, le classi del modello vengono inserite nella cartella Models, ma non è necessario seguire questa convenzione nei propri progetti.</span><span class="sxs-lookup"><span data-stu-id="b0746-134">By convention, model classes are placed in the Models folder, but you don't have to follow this convention in your own projects.</span></span>


<span data-ttu-id="b0746-135">Assegnare alla classe il nome `Product`.</span><span class="sxs-lookup"><span data-stu-id="b0746-135">Name the class `Product`.</span></span> <span data-ttu-id="b0746-136">Nel file Product.cs, sostituire il codice di boilerplate con gli elementi seguenti:</span><span class="sxs-lookup"><span data-stu-id="b0746-136">In the Product.cs file, replace the boilerplate code with the following:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample2.cs)]

<span data-ttu-id="b0746-137">Il `Id` proprietà è la chiave di entità.</span><span class="sxs-lookup"><span data-stu-id="b0746-137">The `Id` property is the entity key.</span></span> <span data-ttu-id="b0746-138">I client possono eseguire query entità tramite chiave.</span><span class="sxs-lookup"><span data-stu-id="b0746-138">Clients can query entities by key.</span></span> <span data-ttu-id="b0746-139">Per ottenere il prodotto con ID 5, ad esempio, l'URI è `/Products(5)`.</span><span class="sxs-lookup"><span data-stu-id="b0746-139">For example, to get the product with ID of 5, the URI is `/Products(5)`.</span></span> <span data-ttu-id="b0746-140">Il `Id` proprietà sarà anche la chiave primaria nel database back-end.</span><span class="sxs-lookup"><span data-stu-id="b0746-140">The `Id` property will also be the primary key in the back-end database.</span></span>

## <a name="enable-entity-framework"></a><span data-ttu-id="b0746-141">Abilitare Entity Framework</span><span class="sxs-lookup"><span data-stu-id="b0746-141">Enable Entity Framework</span></span>

<span data-ttu-id="b0746-142">Per questa esercitazione, si userà Code First di Entity Framework (EF) per creare il database back-end.</span><span class="sxs-lookup"><span data-stu-id="b0746-142">For this tutorial, we'll use Entity Framework (EF) Code First to create the back-end database.</span></span>

> [!NOTE]
> <span data-ttu-id="b0746-143">Web API OData non richiede Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="b0746-143">Web API OData does not require EF.</span></span> <span data-ttu-id="b0746-144">Utilizzare qualsiasi livello di accesso ai dati che si traduce le entità di database in modelli.</span><span class="sxs-lookup"><span data-stu-id="b0746-144">Use any data-access layer that can translate database entities into models.</span></span>


<span data-ttu-id="b0746-145">In primo luogo, è possibile installare il pacchetto NuGet per Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="b0746-145">First, install the NuGet package for EF.</span></span> <span data-ttu-id="b0746-146">Dal **strumenti** dal menu **Gestione pacchetti NuGet** &gt; **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="b0746-146">From the **Tools** menu, select **NuGet Package Manager** &gt; **Package Manager Console**.</span></span> <span data-ttu-id="b0746-147">Nella finestra della Console di gestione pacchetti, digitare:</span><span class="sxs-lookup"><span data-stu-id="b0746-147">In the Package Manager Console window, type:</span></span>

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample3.cmd)]

<span data-ttu-id="b0746-148">Aprire il file Web. config e aggiungere la seguente sezione all'interno di **configurazione** elemento, dopo il **configSections** elemento.</span><span class="sxs-lookup"><span data-stu-id="b0746-148">Open the Web.config file, and add the following section inside the **configuration** element, after the **configSections** element.</span></span>

[!code-xml[Main](create-an-odata-v4-endpoint/samples/sample4.xml?highlight=6)]

<span data-ttu-id="b0746-149">Questa impostazione viene aggiunta una stringa di connessione per un database LocalDB.</span><span class="sxs-lookup"><span data-stu-id="b0746-149">This setting adds a connection string for a LocalDB database.</span></span> <span data-ttu-id="b0746-150">Quando si esegue l'app localmente, verrà utilizzato il database.</span><span class="sxs-lookup"><span data-stu-id="b0746-150">This database will be used when you run the app locally.</span></span>

<span data-ttu-id="b0746-151">Successivamente, aggiungere una classe denominata `ProductsContext` nella cartella Models:</span><span class="sxs-lookup"><span data-stu-id="b0746-151">Next, add a class named `ProductsContext` to the Models folder:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample5.cs)]

<span data-ttu-id="b0746-152">Nel costruttore, `"name=ProductsContext"` fornisce il nome della stringa di connessione.</span><span class="sxs-lookup"><span data-stu-id="b0746-152">In the constructor, `"name=ProductsContext"` gives the name of the connection string.</span></span>

## <a name="configure-the-odata-endpoint"></a><span data-ttu-id="b0746-153">Configurare l'OData Endpoint</span><span class="sxs-lookup"><span data-stu-id="b0746-153">Configure the OData Endpoint</span></span>

<span data-ttu-id="b0746-154">Aprire il file App\_Start/WebApiConfig.cs.</span><span class="sxs-lookup"><span data-stu-id="b0746-154">Open the file App\_Start/WebApiConfig.cs.</span></span> <span data-ttu-id="b0746-155">Aggiungere il seguente **utilizzando** istruzioni:</span><span class="sxs-lookup"><span data-stu-id="b0746-155">Add the following **using** statements:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample6.cs)]

<span data-ttu-id="b0746-156">Quindi aggiungere il codice seguente per il **registrare** metodo:</span><span class="sxs-lookup"><span data-stu-id="b0746-156">Then add the following code to the **Register** method:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample7.cs)]

<span data-ttu-id="b0746-157">Questo codice eseguite due operazioni:</span><span class="sxs-lookup"><span data-stu-id="b0746-157">This code does two things:</span></span>

- <span data-ttu-id="b0746-158">Crea un Entity Data Model (EDM).</span><span class="sxs-lookup"><span data-stu-id="b0746-158">Creates an Entity Data Model (EDM).</span></span>
- <span data-ttu-id="b0746-159">Aggiunge una route.</span><span class="sxs-lookup"><span data-stu-id="b0746-159">Adds a route.</span></span>

<span data-ttu-id="b0746-160">Un modello EDM è un modello di dati astratto.</span><span class="sxs-lookup"><span data-stu-id="b0746-160">An EDM is an abstract model of the data.</span></span> <span data-ttu-id="b0746-161">EDM è utilizzato per creare il documento di metadati del servizio.</span><span class="sxs-lookup"><span data-stu-id="b0746-161">The EDM is used to create the service metadata document.</span></span> <span data-ttu-id="b0746-162">Il **ODataConventionModelBuilder** classe crea un modello EDM utilizzando le convenzioni di denominazione predefinito.</span><span class="sxs-lookup"><span data-stu-id="b0746-162">The **ODataConventionModelBuilder** class creates an EDM by using default naming conventions.</span></span> <span data-ttu-id="b0746-163">Questo approccio richiede il minor quantità di codice.</span><span class="sxs-lookup"><span data-stu-id="b0746-163">This approach requires the least code.</span></span> <span data-ttu-id="b0746-164">Se si desidera controllare più EDM, è possibile utilizzare il **ODataModelBuilder** classe per creare il modello EDM aggiungendo in modo esplicito le proprietà, le chiavi e le proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="b0746-164">If you want more control over the EDM, you can use the **ODataModelBuilder** class to create the EDM by adding properties, keys, and navigation properties explicitly.</span></span>

<span data-ttu-id="b0746-165">Oggetto *route* spiega come indirizzare le richieste HTTP all'endpoint API Web.</span><span class="sxs-lookup"><span data-stu-id="b0746-165">A *route* tells Web API how to route HTTP requests to the endpoint.</span></span> <span data-ttu-id="b0746-166">Per creare una route di OData v4, chiamare il **MapODataServiceRoute** metodo di estensione.</span><span class="sxs-lookup"><span data-stu-id="b0746-166">To create an OData v4 route, call the **MapODataServiceRoute** extension method.</span></span>

<span data-ttu-id="b0746-167">Se l'applicazione presenta più endpoint OData, creare una route separata per ogni.</span><span class="sxs-lookup"><span data-stu-id="b0746-167">If your application has multiple OData endpoints, create a separate route for each.</span></span> <span data-ttu-id="b0746-168">Assegnare ogni route di un nome univoco della route e un prefisso.</span><span class="sxs-lookup"><span data-stu-id="b0746-168">Give each route a unique route name and prefix.</span></span>

## <a name="add-the-odata-controller"></a><span data-ttu-id="b0746-169">Aggiungere il Controller OData</span><span class="sxs-lookup"><span data-stu-id="b0746-169">Add the OData Controller</span></span>

<span data-ttu-id="b0746-170">Oggetto *controller* è una classe che gestisce le richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="b0746-170">A *controller* is a class that handles HTTP requests.</span></span> <span data-ttu-id="b0746-171">Creerai un controller separato per ogni set di entità nel servizio OData.</span><span class="sxs-lookup"><span data-stu-id="b0746-171">You create a separate controller for each entity set in your OData service.</span></span> <span data-ttu-id="b0746-172">In questa esercitazione si creerà un controller, per il `Product` entità.</span><span class="sxs-lookup"><span data-stu-id="b0746-172">In this tutorial, you will create one controller, for the `Product` entity.</span></span>

<span data-ttu-id="b0746-173">In Esplora soluzioni, fare clic sulla cartella controller e selezionare **Aggiungi** &gt; **classe**.</span><span class="sxs-lookup"><span data-stu-id="b0746-173">In Solution Explorer, right-click the Controllers folder and select **Add** &gt; **Class**.</span></span> <span data-ttu-id="b0746-174">Assegnare alla classe il nome `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="b0746-174">Name the class `ProductsController`.</span></span>

> [!NOTE]
> <span data-ttu-id="b0746-175">La versione di questa esercitazione per OData v3 utilizzi il **Aggiungi Controller** lo scaffolding.</span><span class="sxs-lookup"><span data-stu-id="b0746-175">The version of this tutorial for OData v3 uses the **Add Controller** scaffolding.</span></span> <span data-ttu-id="b0746-176">Attualmente non è disponibile alcun lo scaffolding per OData v4.</span><span class="sxs-lookup"><span data-stu-id="b0746-176">Currently, there is no scaffolding for OData v4.</span></span>


<span data-ttu-id="b0746-177">Sostituire il codice boilerplate in ProductsController.cs con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="b0746-177">Replace the boilerplate code in ProductsController.cs with the following.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample8.cs)]

<span data-ttu-id="b0746-178">Il controller viene utilizzato il `ProductsContext` classe per accedere al database tramite Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="b0746-178">The controller uses the `ProductsContext` class to access the database using EF.</span></span> <span data-ttu-id="b0746-179">Si noti che il controller esegue l'override di **Dispose** metodo per eliminare il **ProductsContext**.</span><span class="sxs-lookup"><span data-stu-id="b0746-179">Notice that the controller overrides the **Dispose** method to dispose of the **ProductsContext**.</span></span>

<span data-ttu-id="b0746-180">Questo è il punto inizio per il controller.</span><span class="sxs-lookup"><span data-stu-id="b0746-180">This is the starting point for the controller.</span></span> <span data-ttu-id="b0746-181">Successivamente, aggiungeremo metodi per tutte le operazioni CRUD.</span><span class="sxs-lookup"><span data-stu-id="b0746-181">Next, we'll add methods for all of the CRUD operations.</span></span>

## <a name="querying-the-entity-set"></a><span data-ttu-id="b0746-182">L'esecuzione di query del Set di entità</span><span class="sxs-lookup"><span data-stu-id="b0746-182">Querying the Entity Set</span></span>

<span data-ttu-id="b0746-183">Aggiungere i metodi seguenti per `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="b0746-183">Add the following methods to `ProductsController`.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample9.cs)]

<span data-ttu-id="b0746-184">La versione senza parametri di `Get` metodo restituisce l'intera raccolta di prodotti.</span><span class="sxs-lookup"><span data-stu-id="b0746-184">The parameterless version of the `Get` method returns the entire Products collection.</span></span> <span data-ttu-id="b0746-185">Il `Get` metodo con un *chiave* parametro Cerca un prodotto in base alla chiave (in questo caso, il `Id` proprietà).</span><span class="sxs-lookup"><span data-stu-id="b0746-185">The `Get` method with a *key* parameter looks up a product by its key (in this case, the `Id` property).</span></span>

<span data-ttu-id="b0746-186">Il **[EnableQuery]** attributo consente ai client di modificare la query, utilizzando le opzioni di query, ad esempio $filter $sort e $page.</span><span class="sxs-lookup"><span data-stu-id="b0746-186">The **[EnableQuery]** attribute enables clients to modify the query, by using query options such as $filter, $sort, and $page.</span></span> <span data-ttu-id="b0746-187">Per ulteriori informazioni, vedere [che supporta le opzioni di Query OData](../supporting-odata-query-options.md).</span><span class="sxs-lookup"><span data-stu-id="b0746-187">For more information, see [Supporting OData Query Options](../supporting-odata-query-options.md).</span></span>

## <a name="adding-an-entity-to-the-entity-set"></a><span data-ttu-id="b0746-188">Aggiunta di un'entità per il Set di entità</span><span class="sxs-lookup"><span data-stu-id="b0746-188">Adding an Entity to the Entity Set</span></span>

<span data-ttu-id="b0746-189">Per consentire ai client di aggiungere un nuovo prodotto per il database, aggiungere il metodo seguente alla `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="b0746-189">To enable clients to add a new product to the database, add the following method to `ProductsController`.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample10.cs)]

## <a name="updating-an-entity"></a><span data-ttu-id="b0746-190">Aggiornamento di un'entità</span><span class="sxs-lookup"><span data-stu-id="b0746-190">Updating an Entity</span></span>

<span data-ttu-id="b0746-191">OData supporta due diverse semantiche per l'aggiornamento di un'entità, PATCH e PUT.</span><span class="sxs-lookup"><span data-stu-id="b0746-191">OData supports two different semantics for updating an entity, PATCH and PUT.</span></span>

- <span data-ttu-id="b0746-192">PATCH esegue un aggiornamento parziale.</span><span class="sxs-lookup"><span data-stu-id="b0746-192">PATCH performs a partial update.</span></span> <span data-ttu-id="b0746-193">Il client specifica solo le proprietà da aggiornare.</span><span class="sxs-lookup"><span data-stu-id="b0746-193">The client specifies just the properties to update.</span></span>
- <span data-ttu-id="b0746-194">PUT sostituisce l'intera entità.</span><span class="sxs-lookup"><span data-stu-id="b0746-194">PUT replaces the entire entity.</span></span>

<span data-ttu-id="b0746-195">Lo svantaggio di PUT è che il client deve inviare i valori per tutte le proprietà dell'entità, inclusi i valori che non vengano modificate.</span><span class="sxs-lookup"><span data-stu-id="b0746-195">The disadvantage of PUT is that the client must send values for all of the properties in the entity, including values that are not changing.</span></span> <span data-ttu-id="b0746-196">Il [specifica OData](http://docs.oasis-open.org/odata/odata/v4.0/os/part1-protocol/odata-v4.0-os-part1-protocol.html#_Toc372793719) si afferma che PATCH è preferito.</span><span class="sxs-lookup"><span data-stu-id="b0746-196">The [OData spec](http://docs.oasis-open.org/odata/odata/v4.0/os/part1-protocol/odata-v4.0-os-part1-protocol.html#_Toc372793719) states that PATCH is preferred.</span></span>

<span data-ttu-id="b0746-197">In ogni caso, ecco il codice per i metodi di PATCH e PUT:</span><span class="sxs-lookup"><span data-stu-id="b0746-197">In any case, here is the code for both PATCH and PUT methods:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample11.cs)]

<span data-ttu-id="b0746-198">Nel caso di applicazione della PATCH si utilizza il controller di **Delta&lt;T&gt;**  tipo per tenere traccia delle modifiche.</span><span class="sxs-lookup"><span data-stu-id="b0746-198">In the case of PATCH, the controller uses the **Delta&lt;T&gt;** type to track the changes.</span></span>

## <a name="deleting-an-entity"></a><span data-ttu-id="b0746-199">Eliminazione di un'entità</span><span class="sxs-lookup"><span data-stu-id="b0746-199">Deleting an Entity</span></span>

<span data-ttu-id="b0746-200">Per consentire ai client di eliminare un prodotto dal database, aggiungere il metodo seguente alla `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="b0746-200">To enable clients to delete a product from the database, add the following method to `ProductsController`.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample12.cs)]
