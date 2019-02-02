---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
title: Creare un Endpoint OData v4 tramite ASP.NET Web API 2.2 | Microsoft Docs
author: MikeWasson
description: Open Data Protocol (OData) è un protocollo di accesso di dati per il web. OData offre un metodo uniforme per eseguire query e modificare set di dati tramite operazioni CRUD...
ms.author: riande
ms.date: 01/23/2019
ms.assetid: 1e1927c0-ded1-4752-80fd-a146628d2f09
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
msc.type: authoredcontent
ms.openlocfilehash: c6a4aa4eb563fd77d5afd9248175d5f5b7984d19
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667570"
---
# <a name="create-an-odata-v4-endpoint-using-aspnet-web-api"></a><span data-ttu-id="20a6e-104">Creare un Endpoint OData v4 tramite l'API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="20a6e-104">Create an OData v4 Endpoint Using ASP.NET Web API</span></span> 

> <span data-ttu-id="20a6e-105">Open Data Protocol (OData) è un protocollo di accesso di dati per il web.</span><span class="sxs-lookup"><span data-stu-id="20a6e-105">The Open Data Protocol (OData) is a data access protocol for the web.</span></span> <span data-ttu-id="20a6e-106">OData offre un metodo uniforme per eseguire query e modificare set di dati tramite operazioni CRUD (creare, leggere, aggiornare ed eliminare).</span><span class="sxs-lookup"><span data-stu-id="20a6e-106">OData provides a uniform way to query and manipulate data sets through CRUD operations (create, read, update, and delete).</span></span>
>
> <span data-ttu-id="20a6e-107">API Web ASP.NET supporta v3 e v4 del protocollo.</span><span class="sxs-lookup"><span data-stu-id="20a6e-107">ASP.NET Web API supports both v3 and v4 of the protocol.</span></span> <span data-ttu-id="20a6e-108">È anche possibile avere un endpoint v4 che viene eseguito side-by-side con un endpoint di v3.</span><span class="sxs-lookup"><span data-stu-id="20a6e-108">You can even have a v4 endpoint that runs side-by-side with a v3 endpoint.</span></span>
>
> <span data-ttu-id="20a6e-109">Questa esercitazione illustra come creare un endpoint OData v4 che supporta operazioni CRUD.</span><span class="sxs-lookup"><span data-stu-id="20a6e-109">This tutorial shows how to create an OData v4 endpoint that supports CRUD operations.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="20a6e-110">Versioni del software utilizzate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="20a6e-110">Software versions used in the tutorial</span></span>
>
> - <span data-ttu-id="20a6e-111">Web API 5.2</span><span class="sxs-lookup"><span data-stu-id="20a6e-111">Web API 5.2</span></span>
> - <span data-ttu-id="20a6e-112">OData v4</span><span class="sxs-lookup"><span data-stu-id="20a6e-112">OData v4</span></span>
> - <span data-ttu-id="20a6e-113">Visual Studio 2017 (download di Visual Studio 2017 [qui](https://visualstudio.microsoft.com/downloads/))</span><span class="sxs-lookup"><span data-stu-id="20a6e-113">Visual Studio 2017 (download Visual Studio 2017 [here](https://visualstudio.microsoft.com/downloads/))</span></span>
> - <span data-ttu-id="20a6e-114">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="20a6e-114">Entity Framework 6</span></span>
> - <span data-ttu-id="20a6e-115">.NET 4.7.2</span><span class="sxs-lookup"><span data-stu-id="20a6e-115">.NET 4.7.2</span></span>
>
> ## <a name="tutorial-versions"></a><span data-ttu-id="20a6e-116">Versioni dell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="20a6e-116">Tutorial versions</span></span>
>
> <span data-ttu-id="20a6e-117">Per OData versione 3, vedere [creazione di un Endpoint OData v3](../odata-v3/creating-an-odata-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="20a6e-117">For the OData Version 3, see [Creating an OData v3 Endpoint](../odata-v3/creating-an-odata-endpoint.md).</span></span>

## <a name="create-the-visual-studio-project"></a><span data-ttu-id="20a6e-118">Creare il progetto di Visual Studio</span><span class="sxs-lookup"><span data-stu-id="20a6e-118">Create the Visual Studio Project</span></span>

<span data-ttu-id="20a6e-119">In Visual Studio, dai **File** dal menu **New** &gt; **progetto**.</span><span class="sxs-lookup"><span data-stu-id="20a6e-119">In Visual Studio, from the **File** menu, select **New** &gt; **Project**.</span></span>

<span data-ttu-id="20a6e-120">Espandere **Installed** &gt; **Visual C#**  &gt; **Web**e selezionare il **applicazione Web ASP.NET (.NET Framework)**  modello.</span><span class="sxs-lookup"><span data-stu-id="20a6e-120">Expand **Installed** &gt; **Visual C#** &gt; **Web**, and select the **ASP.NET Web Application (.NET Framework)** template.</span></span> <span data-ttu-id="20a6e-121">Denominare il progetto &quot;ProductService&quot;.</span><span class="sxs-lookup"><span data-stu-id="20a6e-121">Name the project &quot;ProductService&quot;.</span></span>

[![](create-an-odata-v4-endpoint/_static/image7.png)](create-an-odata-v4-endpoint/_static/image7.png)

<span data-ttu-id="20a6e-122">Scegliere **OK**.</span><span class="sxs-lookup"><span data-stu-id="20a6e-122">Select **OK**.</span></span>



[![](create-an-odata-v4-endpoint/_static/image8.png)](create-an-odata-v4-endpoint/_static/image8.png)

<span data-ttu-id="20a6e-123">Selezionare il **vuoto** modello.</span><span class="sxs-lookup"><span data-stu-id="20a6e-123">Select the **Empty** template.</span></span> <span data-ttu-id="20a6e-124">Sotto **aggiungere le cartelle e di base di riferimenti per:**, selezionare **API Web**.</span><span class="sxs-lookup"><span data-stu-id="20a6e-124">Under **Add folders and core references for:**, select **Web API**.</span></span> <span data-ttu-id="20a6e-125">Scegliere **OK**.</span><span class="sxs-lookup"><span data-stu-id="20a6e-125">Select **OK**.</span></span>

## <a name="install-the-odata-packages"></a><span data-ttu-id="20a6e-126">Installare i pacchetti di OData</span><span class="sxs-lookup"><span data-stu-id="20a6e-126">Install the OData packages</span></span>

<span data-ttu-id="20a6e-127">Dal menu **Strumenti** selezionare **Gestione pacchetti NuGet** &gt;  **Console di Gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="20a6e-127">From the **Tools** menu, select **NuGet Package Manager** &gt; **Package Manager Console**.</span></span> <span data-ttu-id="20a6e-128">Nella finestra della Console di gestione pacchetti, digitare:</span><span class="sxs-lookup"><span data-stu-id="20a6e-128">In the Package Manager Console window, type:</span></span>

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample1.cmd)]

<span data-ttu-id="20a6e-129">Questo comando consente di installare gli ultimi pacchetti NuGet di OData.</span><span class="sxs-lookup"><span data-stu-id="20a6e-129">This command installs the latest OData NuGet packages.</span></span>

## <a name="add-a-model-class"></a><span data-ttu-id="20a6e-130">Aggiungere una classe del modello</span><span class="sxs-lookup"><span data-stu-id="20a6e-130">Add a model class</span></span>

<span data-ttu-id="20a6e-131">Oggetto *modello* è un oggetto che rappresenta un'entità di dati nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="20a6e-131">A *model* is an object that represents a data entity in your application.</span></span>

<span data-ttu-id="20a6e-132">In Esplora soluzioni fare clic sulla cartella Models.</span><span class="sxs-lookup"><span data-stu-id="20a6e-132">In Solution Explorer, right-click the Models folder.</span></span> <span data-ttu-id="20a6e-133">Dal menu di scelta rapida, selezionare **Add** &gt; **classe**.</span><span class="sxs-lookup"><span data-stu-id="20a6e-133">From the context menu, select **Add** &gt; **Class**.</span></span>

[![](create-an-odata-v4-endpoint/_static/image6.png)](create-an-odata-v4-endpoint/_static/image5.png)

> [!NOTE]
> <span data-ttu-id="20a6e-134">Per convenzione, le classi del modello vengono inserite nella cartella Models, ma non è necessario seguono questa convenzione nei propri progetti.</span><span class="sxs-lookup"><span data-stu-id="20a6e-134">By convention, model classes are placed in the Models folder, but you don't have to follow this convention in your own projects.</span></span>


<span data-ttu-id="20a6e-135">Assegnare alla classe il nome `Product`.</span><span class="sxs-lookup"><span data-stu-id="20a6e-135">Name the class `Product`.</span></span> <span data-ttu-id="20a6e-136">Nel file Product.cs, sostituire il codice boilerplate con gli elementi seguenti:</span><span class="sxs-lookup"><span data-stu-id="20a6e-136">In the Product.cs file, replace the boilerplate code with the following:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample2.cs)]

<span data-ttu-id="20a6e-137">Il `Id` proprietà è la chiave di entità.</span><span class="sxs-lookup"><span data-stu-id="20a6e-137">The `Id` property is the entity key.</span></span> <span data-ttu-id="20a6e-138">I client possono eseguire query sulle entità tramite chiave.</span><span class="sxs-lookup"><span data-stu-id="20a6e-138">Clients can query entities by key.</span></span> <span data-ttu-id="20a6e-139">Per ottenere il prodotto con ID 5, ad esempio, l'URI è `/Products(5)`.</span><span class="sxs-lookup"><span data-stu-id="20a6e-139">For example, to get the product with ID of 5, the URI is `/Products(5)`.</span></span> <span data-ttu-id="20a6e-140">Il `Id` proprietà sarà anche la chiave primaria nel database di back-end.</span><span class="sxs-lookup"><span data-stu-id="20a6e-140">The `Id` property will also be the primary key in the back-end database.</span></span>

## <a name="enable-entity-framework"></a><span data-ttu-id="20a6e-141">Abilitare Entity Framework</span><span class="sxs-lookup"><span data-stu-id="20a6e-141">Enable Entity Framework</span></span>

<span data-ttu-id="20a6e-142">Per questa esercitazione, useremo Code First di Entity Framework (EF) per creare il database back-end.</span><span class="sxs-lookup"><span data-stu-id="20a6e-142">For this tutorial, we'll use Entity Framework (EF) Code First to create the back-end database.</span></span>

> [!NOTE]
> <span data-ttu-id="20a6e-143">API Web OData non richiede Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="20a6e-143">Web API OData does not require EF.</span></span> <span data-ttu-id="20a6e-144">Usare qualsiasi livello di accesso ai dati che può tradurre le entità di database in modelli.</span><span class="sxs-lookup"><span data-stu-id="20a6e-144">Use any data-access layer that can translate database entities into models.</span></span>


<span data-ttu-id="20a6e-145">Prima di tutto installare il pacchetto NuGet per Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="20a6e-145">First, install the NuGet package for EF.</span></span> <span data-ttu-id="20a6e-146">Dal menu **Strumenti** selezionare **Gestione pacchetti NuGet** &gt;  **Console di Gestione pacchetti**.</span><span class="sxs-lookup"><span data-stu-id="20a6e-146">From the **Tools** menu, select **NuGet Package Manager** &gt; **Package Manager Console**.</span></span> <span data-ttu-id="20a6e-147">Nella finestra della Console di gestione pacchetti, digitare:</span><span class="sxs-lookup"><span data-stu-id="20a6e-147">In the Package Manager Console window, type:</span></span>

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample3.cmd)]

<span data-ttu-id="20a6e-148">Aprire il file Web. config e aggiungere la sezione seguente all'interno di **configuration** elemento, dopo il **configSections** elemento.</span><span class="sxs-lookup"><span data-stu-id="20a6e-148">Open the Web.config file, and add the following section inside the **configuration** element, after the **configSections** element.</span></span>

[!code-xml[Main](create-an-odata-v4-endpoint/samples/sample4.xml?highlight=6)]

<span data-ttu-id="20a6e-149">Questa impostazione consente di aggiungere una stringa di connessione per un database LocalDB.</span><span class="sxs-lookup"><span data-stu-id="20a6e-149">This setting adds a connection string for a LocalDB database.</span></span> <span data-ttu-id="20a6e-150">Questo database verrà utilizzato quando si esegue l'app in locale.</span><span class="sxs-lookup"><span data-stu-id="20a6e-150">This database will be used when you run the app locally.</span></span>

<span data-ttu-id="20a6e-151">Successivamente, aggiungere una classe denominata `ProductsContext` nella cartella Models:</span><span class="sxs-lookup"><span data-stu-id="20a6e-151">Next, add a class named `ProductsContext` to the Models folder:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample5.cs)]

<span data-ttu-id="20a6e-152">Nel costruttore, `"name=ProductsContext"` fornisce il nome della stringa di connessione.</span><span class="sxs-lookup"><span data-stu-id="20a6e-152">In the constructor, `"name=ProductsContext"` gives the name of the connection string.</span></span>

## <a name="configure-the-odata-endpoint"></a><span data-ttu-id="20a6e-153">Configurare l'endpoint OData</span><span class="sxs-lookup"><span data-stu-id="20a6e-153">Configure the OData endpoint</span></span>

<span data-ttu-id="20a6e-154">Aprire il file App\_Start/WebApiConfig.cs.</span><span class="sxs-lookup"><span data-stu-id="20a6e-154">Open the file App\_Start/WebApiConfig.cs.</span></span> <span data-ttu-id="20a6e-155">Aggiungere il codice seguente **usando** istruzioni:</span><span class="sxs-lookup"><span data-stu-id="20a6e-155">Add the following **using** statements:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample6.cs)]

<span data-ttu-id="20a6e-156">Quindi aggiungere il codice seguente per il **registrare** metodo:</span><span class="sxs-lookup"><span data-stu-id="20a6e-156">Then add the following code to the **Register** method:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample7.cs)]

<span data-ttu-id="20a6e-157">Questo codice esegue due operazioni:</span><span class="sxs-lookup"><span data-stu-id="20a6e-157">This code does two things:</span></span>

- <span data-ttu-id="20a6e-158">Crea un Entity Data Model (EDM).</span><span class="sxs-lookup"><span data-stu-id="20a6e-158">Creates an Entity Data Model (EDM).</span></span>
- <span data-ttu-id="20a6e-159">Aggiunge una route.</span><span class="sxs-lookup"><span data-stu-id="20a6e-159">Adds a route.</span></span>

<span data-ttu-id="20a6e-160">Un modello EDM è un modello di dati astratto.</span><span class="sxs-lookup"><span data-stu-id="20a6e-160">An EDM is an abstract model of the data.</span></span> <span data-ttu-id="20a6e-161">EDM viene utilizzato per creare il documento di metadati di servizio.</span><span class="sxs-lookup"><span data-stu-id="20a6e-161">The EDM is used to create the service metadata document.</span></span> <span data-ttu-id="20a6e-162">Il **ODataConventionModelBuilder** classe crea un modello EDM utilizzando le convenzioni di denominazione predefinito.</span><span class="sxs-lookup"><span data-stu-id="20a6e-162">The **ODataConventionModelBuilder** class creates an EDM by using default naming conventions.</span></span> <span data-ttu-id="20a6e-163">Questo approccio richiede il minor quantità di codice.</span><span class="sxs-lookup"><span data-stu-id="20a6e-163">This approach requires the least code.</span></span> <span data-ttu-id="20a6e-164">Se si desidera maggiore controllo sul modello EDM, è possibile usare la **ODataModelBuilder** classe per creare il modello EDM mediante l'aggiunta di proprietà, le chiavi e le proprietà di navigazione in modo esplicito.</span><span class="sxs-lookup"><span data-stu-id="20a6e-164">If you want more control over the EDM, you can use the **ODataModelBuilder** class to create the EDM by adding properties, keys, and navigation properties explicitly.</span></span>

<span data-ttu-id="20a6e-165">Oggetto *route* indica come indirizzare le richieste HTTP all'endpoint API Web.</span><span class="sxs-lookup"><span data-stu-id="20a6e-165">A *route* tells Web API how to route HTTP requests to the endpoint.</span></span> <span data-ttu-id="20a6e-166">Per creare una route di OData v4, chiamare il **MapODataServiceRoute** metodo di estensione.</span><span class="sxs-lookup"><span data-stu-id="20a6e-166">To create an OData v4 route, call the **MapODataServiceRoute** extension method.</span></span>

<span data-ttu-id="20a6e-167">Se l'applicazione dispone di più endpoint OData, creare una route separata per ognuna.</span><span class="sxs-lookup"><span data-stu-id="20a6e-167">If your application has multiple OData endpoints, create a separate route for each.</span></span> <span data-ttu-id="20a6e-168">Assegnare ogni route un nome di route univoco e un prefisso.</span><span class="sxs-lookup"><span data-stu-id="20a6e-168">Give each route a unique route name and prefix.</span></span>

## <a name="add-the-odata-controller"></a><span data-ttu-id="20a6e-169">Aggiungere il controller OData</span><span class="sxs-lookup"><span data-stu-id="20a6e-169">Add the OData controller</span></span>

<span data-ttu-id="20a6e-170">Oggetto *controller* è una classe che gestisce le richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="20a6e-170">A *controller* is a class that handles HTTP requests.</span></span> <span data-ttu-id="20a6e-171">Creare un controller separato per ogni set di entità nel servizio OData.</span><span class="sxs-lookup"><span data-stu-id="20a6e-171">You create a separate controller for each entity set in your OData service.</span></span> <span data-ttu-id="20a6e-172">In questa esercitazione si creerà un controller, per il `Product` entità.</span><span class="sxs-lookup"><span data-stu-id="20a6e-172">In this tutorial, you will create one controller, for the `Product` entity.</span></span>

<span data-ttu-id="20a6e-173">In Esplora soluzioni fare doppio clic su cartella controller e selezionare **Add** &gt; **classe**.</span><span class="sxs-lookup"><span data-stu-id="20a6e-173">In Solution Explorer, right-click the Controllers folder and select **Add** &gt; **Class**.</span></span> <span data-ttu-id="20a6e-174">Assegnare alla classe il nome `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="20a6e-174">Name the class `ProductsController`.</span></span>

> [!NOTE]
> <span data-ttu-id="20a6e-175">La versione di questa esercitazione per OData v3 usi il **Aggiungi Controller** scaffolding.</span><span class="sxs-lookup"><span data-stu-id="20a6e-175">The version of this tutorial for OData v3 uses the **Add Controller** scaffolding.</span></span> <span data-ttu-id="20a6e-176">Attualmente, non è nessuna scaffolding per OData v4.</span><span class="sxs-lookup"><span data-stu-id="20a6e-176">Currently, there is no scaffolding for OData v4.</span></span>


<span data-ttu-id="20a6e-177">Sostituire il codice boilerplate in ProductsController.cs con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="20a6e-177">Replace the boilerplate code in ProductsController.cs with the following.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample8.cs)]

<span data-ttu-id="20a6e-178">Il controller viene utilizzato il `ProductsContext` classe per accedere al database tramite Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="20a6e-178">The controller uses the `ProductsContext` class to access the database using EF.</span></span> <span data-ttu-id="20a6e-179">Si noti che il controller esegue l'override di **Dispose** metodo per eliminare le **ProductsContext**.</span><span class="sxs-lookup"><span data-stu-id="20a6e-179">Notice that the controller overrides the **Dispose** method to dispose of the **ProductsContext**.</span></span>

<span data-ttu-id="20a6e-180">Questo è il punto di partenza per il controller.</span><span class="sxs-lookup"><span data-stu-id="20a6e-180">This is the starting point for the controller.</span></span> <span data-ttu-id="20a6e-181">Successivamente, si aggiungerà metodi per tutte le operazioni CRUD.</span><span class="sxs-lookup"><span data-stu-id="20a6e-181">Next, we'll add methods for all of the CRUD operations.</span></span>

## <a name="query-the-entity-set"></a><span data-ttu-id="20a6e-182">Eseguire una query del set di entità</span><span class="sxs-lookup"><span data-stu-id="20a6e-182">Query the entity set</span></span>

<span data-ttu-id="20a6e-183">Aggiungere i metodi seguenti alla `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="20a6e-183">Add the following methods to `ProductsController`.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample9.cs)]

<span data-ttu-id="20a6e-184">La versione senza parametri del `Get` metodo restituisce l'intera raccolta di prodotti.</span><span class="sxs-lookup"><span data-stu-id="20a6e-184">The parameterless version of the `Get` method returns the entire Products collection.</span></span> <span data-ttu-id="20a6e-185">Il `Get` metodo con un *chiave* parametro Cerca un prodotto in base alla chiave (in questo caso, il `Id` proprietà).</span><span class="sxs-lookup"><span data-stu-id="20a6e-185">The `Get` method with a *key* parameter looks up a product by its key (in this case, the `Id` property).</span></span>

<span data-ttu-id="20a6e-186">Il **[EnableQuery]** attributo consente ai client di modificare la query, usando le opzioni di query, ad esempio $filter $sort e $page.</span><span class="sxs-lookup"><span data-stu-id="20a6e-186">The **[EnableQuery]** attribute enables clients to modify the query, by using query options such as $filter, $sort, and $page.</span></span> <span data-ttu-id="20a6e-187">Per altre informazioni, vedere [che supportano le opzioni di Query OData](../supporting-odata-query-options.md).</span><span class="sxs-lookup"><span data-stu-id="20a6e-187">For more information, see [Supporting OData Query Options](../supporting-odata-query-options.md).</span></span>

## <a name="add-an-entity-to-the-entity-set"></a><span data-ttu-id="20a6e-188">Aggiungere un'entità per il set di entità</span><span class="sxs-lookup"><span data-stu-id="20a6e-188">Add an entity to the entity set</span></span>

<span data-ttu-id="20a6e-189">Per consentire ai client di aggiungere un nuovo prodotto per il database, aggiungere il metodo seguente alla `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="20a6e-189">To enable clients to add a new product to the database, add the following method to `ProductsController`.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample10.cs)]

## <a name="update-an-entity"></a><span data-ttu-id="20a6e-190">Aggiornare un'entità</span><span class="sxs-lookup"><span data-stu-id="20a6e-190">Update an entity</span></span>

<span data-ttu-id="20a6e-191">OData supporta due diverse semantiche per aggiornare un'entità, PATCH e PUT.</span><span class="sxs-lookup"><span data-stu-id="20a6e-191">OData supports two different semantics for updating an entity, PATCH and PUT.</span></span>

- <span data-ttu-id="20a6e-192">PATCH esegue un aggiornamento parziale.</span><span class="sxs-lookup"><span data-stu-id="20a6e-192">PATCH performs a partial update.</span></span> <span data-ttu-id="20a6e-193">Il client specifica solo le proprietà da aggiornare.</span><span class="sxs-lookup"><span data-stu-id="20a6e-193">The client specifies just the properties to update.</span></span>
- <span data-ttu-id="20a6e-194">PUT sostituisce l'intera entità.</span><span class="sxs-lookup"><span data-stu-id="20a6e-194">PUT replaces the entire entity.</span></span>

<span data-ttu-id="20a6e-195">Lo svantaggio di PUT è che il client deve inviare i valori per tutte le proprietà dell'entità, inclusi i valori che non vengano modificate.</span><span class="sxs-lookup"><span data-stu-id="20a6e-195">The disadvantage of PUT is that the client must send values for all of the properties in the entity, including values that are not changing.</span></span> <span data-ttu-id="20a6e-196">Il [specifica OData](http://docs.oasis-open.org/odata/odata/v4.0/os/part1-protocol/odata-v4.0-os-part1-protocol.html#_Toc372793719) dichiara che PATCH è preferita.</span><span class="sxs-lookup"><span data-stu-id="20a6e-196">The [OData spec](http://docs.oasis-open.org/odata/odata/v4.0/os/part1-protocol/odata-v4.0-os-part1-protocol.html#_Toc372793719) states that PATCH is preferred.</span></span>

<span data-ttu-id="20a6e-197">In ogni caso, ecco il codice per i metodi sia PATCH e PUT:</span><span class="sxs-lookup"><span data-stu-id="20a6e-197">In any case, here is the code for both PATCH and PUT methods:</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample11.cs)]

<span data-ttu-id="20a6e-198">Nel caso di PATCH, il controller Usa il **Delta&lt;T&gt;**  tipo per tenere traccia delle modifiche.</span><span class="sxs-lookup"><span data-stu-id="20a6e-198">In the case of PATCH, the controller uses the **Delta&lt;T&gt;** type to track the changes.</span></span>

## <a name="delete-an-entity"></a><span data-ttu-id="20a6e-199">Eliminare un'entità</span><span class="sxs-lookup"><span data-stu-id="20a6e-199">Delete an entity</span></span>

<span data-ttu-id="20a6e-200">Per abilitare i client eliminare un prodotto dal database, aggiungere il metodo seguente alla `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="20a6e-200">To enable clients to delete a product from the database, add the following method to `ProductsController`.</span></span>

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample12.cs)]
