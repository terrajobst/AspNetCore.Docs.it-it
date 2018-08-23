---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
title: Creare un Endpoint OData v4 tramite ASP.NET Web API 2.2 | Microsoft Docs
author: MikeWasson
description: Open Data Protocol (OData) è un protocollo di accesso di dati per il web. OData offre un metodo uniforme per eseguire query e modificare set di dati tramite operazioni CRUD...
ms.author: riande
ms.date: 06/24/2014
ms.assetid: 1e1927c0-ded1-4752-80fd-a146628d2f09
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
msc.type: authoredcontent
ms.openlocfilehash: 7f2d0b8fa8ac290e5018cb5237b1fedb5f40eeb0
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834849"
---
<a name="create-an-odata-v4-endpoint-using-aspnet-web-api-22"></a><span data-ttu-id="d31c5-104">Creare un Endpoint OData v4 tramite ASP.NET Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="d31c5-104">Create an OData v4 Endpoint Using ASP.NET Web API 2.2</span></span>
====================
<span data-ttu-id="d31c5-105">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="d31c5-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="d31c5-106">Open Data Protocol (OData) è un protocollo di accesso di dati per il web.</span><span class="sxs-lookup"><span data-stu-id="d31c5-106">The Open Data Protocol (OData) is a data access protocol for the web.</span></span> <span data-ttu-id="d31c5-107">OData offre un metodo uniforme per eseguire query e modificare set di dati tramite operazioni CRUD (creare, leggere, aggiornare ed eliminare).</span><span class="sxs-lookup"><span data-stu-id="d31c5-107">OData provides a uniform way to query and manipulate data sets through CRUD operations (create, read, update, and delete).</span></span>
> 
> <span data-ttu-id="d31c5-108">API Web ASP.NET supporta v3 e v4 del protocollo.</span><span class="sxs-lookup"><span data-stu-id="d31c5-108">ASP.NET Web API supports both v3 and v4 of the protocol.</span></span> <span data-ttu-id="d31c5-109">È anche possibile avere un endpoint v4 che viene eseguito side-by-side con un endpoint di v3.</span><span class="sxs-lookup"><span data-stu-id="d31c5-109">You can even have a v4 endpoint that runs side-by-side with a v3 endpoint.</span></span>
> 
> <span data-ttu-id="d31c5-110">Questa esercitazione illustra come creare un endpoint OData v4 che supporta operazioni CRUD.</span><span class="sxs-lookup"><span data-stu-id="d31c5-110">This tutorial shows how to create an OData v4 endpoint that supports CRUD operations.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="d31c5-111">Versioni del software utilizzate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="d31c5-111">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="d31c5-112">API Web 2.2</span><span class="sxs-lookup"><span data-stu-id="d31c5-112">Web API 2.2</span></span>
> - <span data-ttu-id="d31c5-113">OData v4</span><span class="sxs-lookup"><span data-stu-id="d31c5-113">OData v4</span></span>
> - [<span data-ttu-id="d31c5-114">Visual Studio 2013 Update 2</span><span class="sxs-lookup"><span data-stu-id="d31c5-114">Visual Studio 2013 Update 2</span></span>](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - <span data-ttu-id="d31c5-115">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="d31c5-115">Entity Framework 6</span></span>
> - <span data-ttu-id="d31c5-116">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="d31c5-116">.NET 4.5</span></span>
> 
> 
> ## <a name="tutorial-versions"></a><span data-ttu-id="d31c5-117">Versioni dell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="d31c5-117">Tutorial versions</span></span>
> 
> <span data-ttu-id="d31c5-118">Per OData versione 3, vedere [creazione di un Endpoint OData v3](../odata-v3/creating-an-odata-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="d31c5-118">For the OData Version 3, see [Creating an OData v3 Endpoint](../odata-v3/creating-an-odata-endpoint.md).</span></span>


## <a name="create-the-visual-studio-project"></a><span data-ttu-id="d31c5-119">Creare il progetto di Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d31c5-119">Create the Visual Studio Project</span></span>

<span data-ttu-id="d31c5-120">In Visual Studio, dai **File** dal menu **New** &gt; **progetto**.</span><span class="sxs-lookup"><span data-stu-id="d31c5-120">In Visual Studio, from the **File** menu, select **New** &gt; **Project**.</span></span>

<span data-ttu-id="d31c5-121">Espandere **Installed** &gt; **modelli** &gt; **Visual c#** &gt; **Web**e selezionare il  **Applicazione Web ASP.NET** modello.</span><span class="sxs-lookup"><span data-stu-id="d31c5-121">Expand **Installed** &gt; **Templates** &gt; **Visual C#** &gt; **Web**, and select the **ASP.NET Web Application** template.</span></span> <span data-ttu-id="d31c5-122">Denominare il progetto &quot;ProductService&quot;.</span><span class="sxs-lookup"><span data-stu-id="d31c5-122">Name the project &quot;ProductService&quot;.</span></span>

[![](create-an-odata-v4-endpoint/_static/image2.png)](create-an-odata-v4-endpoint/_static/image1.png)

<span data-ttu-id="d31c5-123">Nel **nuovo progetto** finestra di dialogo, seleziona la **vuota** modello.</span><span class="sxs-lookup"><span data-stu-id="d31c5-123">In the **New Project** dialog, select the **Empty** template.</span></span> <span data-ttu-id="d31c5-124">In &quot;aggiungere cartelle e riferimenti di base... &quot;, fare clic su **API Web**.</span><span class="sxs-lookup"><span data-stu-id="d31c5-124">Under &quot;Add folders and core references...&quot;, click **Web API**.</span></span> <span data-ttu-id="d31c5-125">Fare clic su **OK**.</span><span class="sxs-lookup"><span data-stu-id="d31c5-125">Click **OK**.</span></span>

[![](create-an-odata-v4-endpoint/_static/image4.png)](create-an-odata-v4-endpoint/_static/image3.png)

## <a name="install-the-odata-packages"></a><span data-ttu-id="d31c5-126">Installare i pacchetti di OData</span><span class="sxs-lookup"><span data-stu-id="d31c5-126">Install the OData Packages</span></span>

<span data-ttu-id="d31c5-127">Dal menu **Strumenti** selezionare **Gestione pacchetti NuGet** &gt;  **Console di Gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="d31c5-127">From the **Tools** menu, select **NuGet Package Manager** &gt; **Package Manager Console**.</span></span> <span data-ttu-id="d31c5-128">Nella finestra della Console di gestione pacchetti, digitare:</span><span class="sxs-lookup"><span data-stu-id="d31c5-128">In the Package Manager Console window, type:</span></span>

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample1.cmd)]

<span data-ttu-id="d31c5-129">Questo comando consente di installare gli ultimi pacchetti NuGet di OData.</span><span class="sxs-lookup"><span data-stu-id="d31c5-129">This command installs the latest OData NuGet packages.</span></span>

## <a name="add-a-model-class"></a><span data-ttu-id="d31c5-130">Aggiungere una classe modello</span><span class="sxs-lookup"><span data-stu-id="d31c5-130">Add a Model Class</span></span>

<span data-ttu-id="d31c5-131">Oggetto *modello* è un oggetto che rappresenta un'entità di dati nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="d31c5-131">A *model* is an object that represents a data entity in your application.</span></span>

<span data-ttu-id="d31c5-132">In Esplora soluzioni fare clic sulla cartella Models.</span><span class="sxs-lookup"><span data-stu-id="d31c5-132">In Solution Explorer, right-click the Models folder.</span></span> <span data-ttu-id="d31c5-133">Dal menu di scelta rapida, selezionare **Add** &gt; **classe**.</span><span class="sxs-lookup"><span data-stu-id="d31c5-133">From the context menu, select **Add** &gt; **Class**.</span></span>

[![](create-an-odata-v4-endpoint/_static/image6.png)](create-an-odata-v4-endpoint/_static/image5.png)

> [!NOTE]
> <span data-ttu-id="d31c5-134">Per convenzione, le classi del modello vengono inserite nella cartella Models, ma non è necessario seguono questa convenzione nei propri progetti.</span><span class="sxs-lookup"><span data-stu-id="d31c5-134">By convention, model classes are placed in the Models folder, but you don't have to follow this convention in your own projects.</span></span>


<span data-ttu-id="d31c5-135">Assegnare alla classe il nome `Product`.</span><span class="sxs-lookup"><span data-stu-id="d31c5-135">Name the class `Product`.</span></span> <span data-ttu-id="d31c5-136">Nel file Product.cs, sostituire il codice boilerplate con gli elementi seguenti:</span><span class="sxs-lookup"><span data-stu-id="d31c5-136">In the Product.cs file, replace the boilerplate code with the following:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample2.cs)]

<span data-ttu-id="d31c5-137">Il `Id` proprietà è la chiave di entità.</span><span class="sxs-lookup"><span data-stu-id="d31c5-137">The `Id` property is the entity key.</span></span> <span data-ttu-id="d31c5-138">I client possono eseguire query sulle entità tramite chiave.</span><span class="sxs-lookup"><span data-stu-id="d31c5-138">Clients can query entities by key.</span></span> <span data-ttu-id="d31c5-139">Per ottenere il prodotto con ID 5, ad esempio, l'URI è `/Products(5)`.</span><span class="sxs-lookup"><span data-stu-id="d31c5-139">For example, to get the product with ID of 5, the URI is `/Products(5)`.</span></span> <span data-ttu-id="d31c5-140">Il `Id` proprietà sarà anche la chiave primaria nel database di back-end.</span><span class="sxs-lookup"><span data-stu-id="d31c5-140">The `Id` property will also be the primary key in the back-end database.</span></span>

## <a name="enable-entity-framework"></a><span data-ttu-id="d31c5-141">Abilitare Entity Framework</span><span class="sxs-lookup"><span data-stu-id="d31c5-141">Enable Entity Framework</span></span>

<span data-ttu-id="d31c5-142">Per questa esercitazione, useremo Code First di Entity Framework (EF) per creare il database back-end.</span><span class="sxs-lookup"><span data-stu-id="d31c5-142">For this tutorial, we'll use Entity Framework (EF) Code First to create the back-end database.</span></span>

> [!NOTE]
> <span data-ttu-id="d31c5-143">API Web OData non richiede Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="d31c5-143">Web API OData does not require EF.</span></span> <span data-ttu-id="d31c5-144">Usare qualsiasi livello di accesso ai dati che può tradurre le entità di database in modelli.</span><span class="sxs-lookup"><span data-stu-id="d31c5-144">Use any data-access layer that can translate database entities into models.</span></span>


<span data-ttu-id="d31c5-145">Prima di tutto installare il pacchetto NuGet per Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="d31c5-145">First, install the NuGet package for EF.</span></span> <span data-ttu-id="d31c5-146">Dal menu **Strumenti** selezionare **Gestione pacchetti NuGet** &gt;  **Console di Gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="d31c5-146">From the **Tools** menu, select **NuGet Package Manager** &gt; **Package Manager Console**.</span></span> <span data-ttu-id="d31c5-147">Nella finestra della Console di gestione pacchetti, digitare:</span><span class="sxs-lookup"><span data-stu-id="d31c5-147">In the Package Manager Console window, type:</span></span>

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample3.cmd)]

<span data-ttu-id="d31c5-148">Aprire il file Web. config e aggiungere la sezione seguente all'interno di **configuration** elemento, dopo il **configSections** elemento.</span><span class="sxs-lookup"><span data-stu-id="d31c5-148">Open the Web.config file, and add the following section inside the **configuration** element, after the **configSections** element.</span></span>

[!code-xml[Main](create-an-odata-v4-endpoint/samples/sample4.xml?highlight=6)]

<span data-ttu-id="d31c5-149">Questa impostazione consente di aggiungere una stringa di connessione per un database LocalDB.</span><span class="sxs-lookup"><span data-stu-id="d31c5-149">This setting adds a connection string for a LocalDB database.</span></span> <span data-ttu-id="d31c5-150">Questo database verrà utilizzato quando si esegue l'app in locale.</span><span class="sxs-lookup"><span data-stu-id="d31c5-150">This database will be used when you run the app locally.</span></span>

<span data-ttu-id="d31c5-151">Successivamente, aggiungere una classe denominata `ProductsContext` nella cartella Models:</span><span class="sxs-lookup"><span data-stu-id="d31c5-151">Next, add a class named `ProductsContext` to the Models folder:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample5.cs)]

<span data-ttu-id="d31c5-152">Nel costruttore, `"name=ProductsContext"` fornisce il nome della stringa di connessione.</span><span class="sxs-lookup"><span data-stu-id="d31c5-152">In the constructor, `"name=ProductsContext"` gives the name of the connection string.</span></span>

## <a name="configure-the-odata-endpoint"></a><span data-ttu-id="d31c5-153">Configurare l'OData Endpoint</span><span class="sxs-lookup"><span data-stu-id="d31c5-153">Configure the OData Endpoint</span></span>

<span data-ttu-id="d31c5-154">Aprire il file App\_Start/WebApiConfig.cs.</span><span class="sxs-lookup"><span data-stu-id="d31c5-154">Open the file App\_Start/WebApiConfig.cs.</span></span> <span data-ttu-id="d31c5-155">Aggiungere il codice seguente **usando** istruzioni:</span><span class="sxs-lookup"><span data-stu-id="d31c5-155">Add the following **using** statements:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample6.cs)]

<span data-ttu-id="d31c5-156">Quindi aggiungere il codice seguente per il **registrare** metodo:</span><span class="sxs-lookup"><span data-stu-id="d31c5-156">Then add the following code to the **Register** method:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample7.cs)]

<span data-ttu-id="d31c5-157">Questo codice esegue due operazioni:</span><span class="sxs-lookup"><span data-stu-id="d31c5-157">This code does two things:</span></span>

- <span data-ttu-id="d31c5-158">Crea un Entity Data Model (EDM).</span><span class="sxs-lookup"><span data-stu-id="d31c5-158">Creates an Entity Data Model (EDM).</span></span>
- <span data-ttu-id="d31c5-159">Aggiunge una route.</span><span class="sxs-lookup"><span data-stu-id="d31c5-159">Adds a route.</span></span>

<span data-ttu-id="d31c5-160">Un modello EDM è un modello di dati astratto.</span><span class="sxs-lookup"><span data-stu-id="d31c5-160">An EDM is an abstract model of the data.</span></span> <span data-ttu-id="d31c5-161">EDM viene utilizzato per creare il documento di metadati di servizio.</span><span class="sxs-lookup"><span data-stu-id="d31c5-161">The EDM is used to create the service metadata document.</span></span> <span data-ttu-id="d31c5-162">Il **ODataConventionModelBuilder** classe crea un modello EDM utilizzando le convenzioni di denominazione predefinito.</span><span class="sxs-lookup"><span data-stu-id="d31c5-162">The **ODataConventionModelBuilder** class creates an EDM by using default naming conventions.</span></span> <span data-ttu-id="d31c5-163">Questo approccio richiede il minor quantità di codice.</span><span class="sxs-lookup"><span data-stu-id="d31c5-163">This approach requires the least code.</span></span> <span data-ttu-id="d31c5-164">Se si desidera maggiore controllo sul modello EDM, è possibile usare la **ODataModelBuilder** classe per creare il modello EDM mediante l'aggiunta di proprietà, le chiavi e le proprietà di navigazione in modo esplicito.</span><span class="sxs-lookup"><span data-stu-id="d31c5-164">If you want more control over the EDM, you can use the **ODataModelBuilder** class to create the EDM by adding properties, keys, and navigation properties explicitly.</span></span>

<span data-ttu-id="d31c5-165">Oggetto *route* indica come indirizzare le richieste HTTP all'endpoint API Web.</span><span class="sxs-lookup"><span data-stu-id="d31c5-165">A *route* tells Web API how to route HTTP requests to the endpoint.</span></span> <span data-ttu-id="d31c5-166">Per creare una route di OData v4, chiamare il **MapODataServiceRoute** metodo di estensione.</span><span class="sxs-lookup"><span data-stu-id="d31c5-166">To create an OData v4 route, call the **MapODataServiceRoute** extension method.</span></span>

<span data-ttu-id="d31c5-167">Se l'applicazione dispone di più endpoint OData, creare una route separata per ognuna.</span><span class="sxs-lookup"><span data-stu-id="d31c5-167">If your application has multiple OData endpoints, create a separate route for each.</span></span> <span data-ttu-id="d31c5-168">Assegnare ogni route un nome di route univoco e un prefisso.</span><span class="sxs-lookup"><span data-stu-id="d31c5-168">Give each route a unique route name and prefix.</span></span>

## <a name="add-the-odata-controller"></a><span data-ttu-id="d31c5-169">Aggiungere il Controller OData</span><span class="sxs-lookup"><span data-stu-id="d31c5-169">Add the OData Controller</span></span>

<span data-ttu-id="d31c5-170">Oggetto *controller* è una classe che gestisce le richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="d31c5-170">A *controller* is a class that handles HTTP requests.</span></span> <span data-ttu-id="d31c5-171">Creare un controller separato per ogni set di entità nel servizio OData.</span><span class="sxs-lookup"><span data-stu-id="d31c5-171">You create a separate controller for each entity set in your OData service.</span></span> <span data-ttu-id="d31c5-172">In questa esercitazione si creerà un controller, per il `Product` entità.</span><span class="sxs-lookup"><span data-stu-id="d31c5-172">In this tutorial, you will create one controller, for the `Product` entity.</span></span>

<span data-ttu-id="d31c5-173">In Esplora soluzioni fare doppio clic su cartella controller e selezionare **Add** &gt; **classe**.</span><span class="sxs-lookup"><span data-stu-id="d31c5-173">In Solution Explorer, right-click the Controllers folder and select **Add** &gt; **Class**.</span></span> <span data-ttu-id="d31c5-174">Assegnare alla classe il nome `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="d31c5-174">Name the class `ProductsController`.</span></span>

> [!NOTE]
> <span data-ttu-id="d31c5-175">La versione di questa esercitazione per OData v3 usi il **Aggiungi Controller** scaffolding.</span><span class="sxs-lookup"><span data-stu-id="d31c5-175">The version of this tutorial for OData v3 uses the **Add Controller** scaffolding.</span></span> <span data-ttu-id="d31c5-176">Attualmente, non è nessuna scaffolding per OData v4.</span><span class="sxs-lookup"><span data-stu-id="d31c5-176">Currently, there is no scaffolding for OData v4.</span></span>


<span data-ttu-id="d31c5-177">Sostituire il codice boilerplate in ProductsController.cs con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="d31c5-177">Replace the boilerplate code in ProductsController.cs with the following.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample8.cs)]

<span data-ttu-id="d31c5-178">Il controller viene utilizzato il `ProductsContext` classe per accedere al database tramite Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="d31c5-178">The controller uses the `ProductsContext` class to access the database using EF.</span></span> <span data-ttu-id="d31c5-179">Si noti che il controller esegue l'override di **Dispose** metodo per eliminare le **ProductsContext**.</span><span class="sxs-lookup"><span data-stu-id="d31c5-179">Notice that the controller overrides the **Dispose** method to dispose of the **ProductsContext**.</span></span>

<span data-ttu-id="d31c5-180">Questo è il punto di partenza per il controller.</span><span class="sxs-lookup"><span data-stu-id="d31c5-180">This is the starting point for the controller.</span></span> <span data-ttu-id="d31c5-181">Successivamente, si aggiungerà metodi per tutte le operazioni CRUD.</span><span class="sxs-lookup"><span data-stu-id="d31c5-181">Next, we'll add methods for all of the CRUD operations.</span></span>

## <a name="querying-the-entity-set"></a><span data-ttu-id="d31c5-182">L'esecuzione di query del Set di entità</span><span class="sxs-lookup"><span data-stu-id="d31c5-182">Querying the Entity Set</span></span>

<span data-ttu-id="d31c5-183">Aggiungere i metodi seguenti alla `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="d31c5-183">Add the following methods to `ProductsController`.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample9.cs)]

<span data-ttu-id="d31c5-184">La versione senza parametri del `Get` metodo restituisce l'intera raccolta di prodotti.</span><span class="sxs-lookup"><span data-stu-id="d31c5-184">The parameterless version of the `Get` method returns the entire Products collection.</span></span> <span data-ttu-id="d31c5-185">Il `Get` metodo con un *chiave* parametro Cerca un prodotto in base alla chiave (in questo caso, il `Id` proprietà).</span><span class="sxs-lookup"><span data-stu-id="d31c5-185">The `Get` method with a *key* parameter looks up a product by its key (in this case, the `Id` property).</span></span>

<span data-ttu-id="d31c5-186">Il **[EnableQuery]** attributo consente ai client di modificare la query, usando le opzioni di query, ad esempio $filter $sort e $page.</span><span class="sxs-lookup"><span data-stu-id="d31c5-186">The **[EnableQuery]** attribute enables clients to modify the query, by using query options such as $filter, $sort, and $page.</span></span> <span data-ttu-id="d31c5-187">Per altre informazioni, vedere [che supportano le opzioni di Query OData](../supporting-odata-query-options.md).</span><span class="sxs-lookup"><span data-stu-id="d31c5-187">For more information, see [Supporting OData Query Options](../supporting-odata-query-options.md).</span></span>

## <a name="adding-an-entity-to-the-entity-set"></a><span data-ttu-id="d31c5-188">Aggiunta di un'entità per il Set di entità</span><span class="sxs-lookup"><span data-stu-id="d31c5-188">Adding an Entity to the Entity Set</span></span>

<span data-ttu-id="d31c5-189">Per consentire ai client di aggiungere un nuovo prodotto per il database, aggiungere il metodo seguente alla `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="d31c5-189">To enable clients to add a new product to the database, add the following method to `ProductsController`.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample10.cs)]

## <a name="updating-an-entity"></a><span data-ttu-id="d31c5-190">Aggiornamento di un'entità</span><span class="sxs-lookup"><span data-stu-id="d31c5-190">Updating an Entity</span></span>

<span data-ttu-id="d31c5-191">OData supporta due diverse semantiche per aggiornare un'entità, PATCH e PUT.</span><span class="sxs-lookup"><span data-stu-id="d31c5-191">OData supports two different semantics for updating an entity, PATCH and PUT.</span></span>

- <span data-ttu-id="d31c5-192">PATCH esegue un aggiornamento parziale.</span><span class="sxs-lookup"><span data-stu-id="d31c5-192">PATCH performs a partial update.</span></span> <span data-ttu-id="d31c5-193">Il client specifica solo le proprietà da aggiornare.</span><span class="sxs-lookup"><span data-stu-id="d31c5-193">The client specifies just the properties to update.</span></span>
- <span data-ttu-id="d31c5-194">PUT sostituisce l'intera entità.</span><span class="sxs-lookup"><span data-stu-id="d31c5-194">PUT replaces the entire entity.</span></span>

<span data-ttu-id="d31c5-195">Lo svantaggio di PUT è che il client deve inviare i valori per tutte le proprietà dell'entità, inclusi i valori che non vengano modificate.</span><span class="sxs-lookup"><span data-stu-id="d31c5-195">The disadvantage of PUT is that the client must send values for all of the properties in the entity, including values that are not changing.</span></span> <span data-ttu-id="d31c5-196">Il [specifica OData](http://docs.oasis-open.org/odata/odata/v4.0/os/part1-protocol/odata-v4.0-os-part1-protocol.html#_Toc372793719) dichiara che PATCH è preferita.</span><span class="sxs-lookup"><span data-stu-id="d31c5-196">The [OData spec](http://docs.oasis-open.org/odata/odata/v4.0/os/part1-protocol/odata-v4.0-os-part1-protocol.html#_Toc372793719) states that PATCH is preferred.</span></span>

<span data-ttu-id="d31c5-197">In ogni caso, ecco il codice per i metodi sia PATCH e PUT:</span><span class="sxs-lookup"><span data-stu-id="d31c5-197">In any case, here is the code for both PATCH and PUT methods:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample11.cs)]

<span data-ttu-id="d31c5-198">Nel caso di PATCH, il controller Usa il **Delta&lt;T&gt;**  tipo per tenere traccia delle modifiche.</span><span class="sxs-lookup"><span data-stu-id="d31c5-198">In the case of PATCH, the controller uses the **Delta&lt;T&gt;** type to track the changes.</span></span>

## <a name="deleting-an-entity"></a><span data-ttu-id="d31c5-199">Eliminazione di un'entità</span><span class="sxs-lookup"><span data-stu-id="d31c5-199">Deleting an Entity</span></span>

<span data-ttu-id="d31c5-200">Per abilitare i client eliminare un prodotto dal database, aggiungere il metodo seguente alla `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="d31c5-200">To enable clients to delete a product from the database, add the following method to `ProductsController`.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample12.cs)]
