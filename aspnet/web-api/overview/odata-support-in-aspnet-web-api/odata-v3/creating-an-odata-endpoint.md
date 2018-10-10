---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
title: Creazione di un Endpoint OData v3 con API Web 2 | Microsoft Docs
author: MikeWasson
description: Open Data Protocol (OData) è un protocollo di accesso di dati per il web. OData offre un metodo uniforme per strutturare i dati, eseguire query sui dati e modificare i dati...
ms.author: riande
ms.date: 02/25/2014
ms.assetid: 262843d6-43a2-4f1c-82d9-0b90ae6df0cf
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
msc.type: authoredcontent
ms.openlocfilehash: 2e0d3b45fd51192d227d852dc2f05b45ca42944c
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/10/2018
ms.locfileid: "48910915"
---
<a name="creating-an-odata-v3-endpoint-with-web-api-2"></a><span data-ttu-id="b6ef9-104">Creazione di un Endpoint OData v3 con API Web 2</span><span class="sxs-lookup"><span data-stu-id="b6ef9-104">Creating an OData v3 Endpoint with Web API 2</span></span>
====================
<span data-ttu-id="b6ef9-105">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="b6ef9-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="b6ef9-106">Download progetto completato</span><span class="sxs-lookup"><span data-stu-id="b6ef9-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="b6ef9-107">Il [Open Data Protocol](http://www.odata.org/) (OData) è un protocollo di accesso di dati per il web.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-107">The [Open Data Protocol](http://www.odata.org/) (OData) is a data access protocol for the web.</span></span> <span data-ttu-id="b6ef9-108">OData offre un metodo uniforme per strutturare i dati, eseguire query sui dati e modificare il set di dati tramite operazioni CRUD (creare, leggere, aggiornare ed eliminare).</span><span class="sxs-lookup"><span data-stu-id="b6ef9-108">OData provides a uniform way to structure data, query the data, and manipulate the data set through CRUD operations (create, read, update, and delete).</span></span> <span data-ttu-id="b6ef9-109">OData supporta AtomPub (XML) e formati JSON.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-109">OData supports both AtomPub (XML) and JSON formats.</span></span> <span data-ttu-id="b6ef9-110">OData definisce anche un modo per esporre i metadati relativi a dati.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-110">OData also defines a way to expose metadata about the data.</span></span> <span data-ttu-id="b6ef9-111">I client possono usare i metadati per individuare le informazioni sul tipo e le relazioni per il set di dati.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-111">Clients can use the metadata to discover the type information and relationships for the data set.</span></span>
>
> <span data-ttu-id="b6ef9-112">API Web ASP.NET rende più semplice creare un endpoint OData per un set di dati.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-112">ASP.NET Web API makes it easy to create an OData endpoint for a data set.</span></span> <span data-ttu-id="b6ef9-113">È possibile controllare esattamente quali operazioni OData supportati dall'endpoint.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-113">You can control exactly which OData operations the endpoint supports.</span></span> <span data-ttu-id="b6ef9-114">È possibile ospitare più endpoint OData, insieme a endpoint non OData.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-114">You can host multiple OData endpoints, alongside non-OData endpoints.</span></span> <span data-ttu-id="b6ef9-115">Si hanno pieno controllo sul modello di dati, la logica di business back-end e livello dati.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-115">You have full control over your data model, back-end business logic, and data layer.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="b6ef9-116">Versioni del software utilizzate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="b6ef9-116">Software versions used in the tutorial</span></span>
>
>
> - [<span data-ttu-id="b6ef9-117">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="b6ef9-117">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="b6ef9-118">API Web 2</span><span class="sxs-lookup"><span data-stu-id="b6ef9-118">Web API 2</span></span>
> - <span data-ttu-id="b6ef9-119">OData versione 3</span><span class="sxs-lookup"><span data-stu-id="b6ef9-119">OData Version 3</span></span>
> - <span data-ttu-id="b6ef9-120">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="b6ef9-120">Entity Framework 6</span></span>
> - [<span data-ttu-id="b6ef9-121">Fiddler Web Debugging Proxy (facoltativo)</span><span class="sxs-lookup"><span data-stu-id="b6ef9-121">Fiddler Web Debugging Proxy (Optional)</span></span>](http://www.fiddler2.com)
>
> <span data-ttu-id="b6ef9-122">È stato aggiunto il supporto di Web API OData [ASP.NET e Web Tools 2012.2 Update](https://go.microsoft.com/fwlink/?LinkId=282650).</span><span class="sxs-lookup"><span data-stu-id="b6ef9-122">Web API OData support was added in [ASP.NET and Web Tools 2012.2 Update](https://go.microsoft.com/fwlink/?LinkId=282650).</span></span> <span data-ttu-id="b6ef9-123">Tuttavia, questa esercitazione Usa lo scaffolding che è stato aggiunto in Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-123">However, this tutorial uses scaffolding that was added in Visual Studio 2013.</span></span>


<span data-ttu-id="b6ef9-124">In questa esercitazione si creerà un semplice endpoint OData che i client possono eseguire query.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-124">In this tutorial, you will create a simple OData endpoint that clients can query.</span></span> <span data-ttu-id="b6ef9-125">Si creerà anche un client c# per l'endpoint.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-125">You will also create a C# client for the endpoint.</span></span> <span data-ttu-id="b6ef9-126">Dopo aver completato questa esercitazione, il set successivo di esercitazioni illustrano come aggiungere ulteriori funzionalità, tra cui relazioni tra entità, le azioni, e selezionare $espandere / $.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-126">After you complete this tutorial, the next set of tutorials show how to add more functionality, including entity relations, actions, and $expand/$select.</span></span>

- [<span data-ttu-id="b6ef9-127">Creare il progetto di Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b6ef9-127">Create the Visual Studio Project</span></span>](#create-project)
- [<span data-ttu-id="b6ef9-128">Aggiungere un modello di entità</span><span class="sxs-lookup"><span data-stu-id="b6ef9-128">Add an Entity Model</span></span>](#add-model)
- [<span data-ttu-id="b6ef9-129">Aggiungere un Controller OData</span><span class="sxs-lookup"><span data-stu-id="b6ef9-129">Add an OData Controller</span></span>](#add-controller)
- [<span data-ttu-id="b6ef9-130">Aggiungere il modello EDM e Route</span><span class="sxs-lookup"><span data-stu-id="b6ef9-130">Add the EDM and Route</span></span>](#edm)
- [<span data-ttu-id="b6ef9-131">Seeding del Database (facoltativo)</span><span class="sxs-lookup"><span data-stu-id="b6ef9-131">Seed the Database (Optional)</span></span>](#seed-db)
- [<span data-ttu-id="b6ef9-132">Esplorare l'OData Endpoint</span><span class="sxs-lookup"><span data-stu-id="b6ef9-132">Exploring the OData Endpoint</span></span>](#explore)
- [<span data-ttu-id="b6ef9-133">Formati di serializzazione OData</span><span class="sxs-lookup"><span data-stu-id="b6ef9-133">OData Serialization Formats</span></span>](#formats)

<a id="create-project"></a>
## <a name="create-the-visual-studio-project"></a><span data-ttu-id="b6ef9-134">Creare il progetto di Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b6ef9-134">Create the Visual Studio Project</span></span>

<span data-ttu-id="b6ef9-135">In questa esercitazione si creerà un endpoint OData che supporta operazioni CRUD di base.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-135">In this tutorial, you will create an OData endpoint that supports basic CRUD operations.</span></span> <span data-ttu-id="b6ef9-136">L'endpoint espone una sola risorsa, un elenco di prodotti.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-136">The endpoint will expose a single resource, a list of products.</span></span> <span data-ttu-id="b6ef9-137">Le esercitazioni successive verranno aggiunti ulteriori funzionalità.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-137">Later tutorials will add more features.</span></span>

<span data-ttu-id="b6ef9-138">Avviare Visual Studio e selezionare **nuovo progetto** dalla pagina iniziale.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-138">Start Visual Studio and select **New Project** from the Start page.</span></span> <span data-ttu-id="b6ef9-139">E viceversa, il **File** dal menu **New** e quindi **progetto**.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-139">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="b6ef9-140">Nel **modelli** riquadro, selezionare **modelli installati** ed espandere il nodo Visual c#.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-140">In the **Templates** pane, select **Installed Templates** and expand the Visual C# node.</span></span> <span data-ttu-id="b6ef9-141">Sotto **Visual c#**, selezionare **Web**.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-141">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="b6ef9-142">Selezionare **l'applicazione Web ASP.NET** modello.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-142">Select **the ASP.NET Web Application** template.</span></span>

![](creating-an-odata-endpoint/_static/image1.png)

<span data-ttu-id="b6ef9-143">Nel **nuovo progetto ASP.NET** finestra di dialogo, seleziona la **vuota** modello.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-143">In the **New ASP.NET Project** dialog, select the **Empty** template.</span></span> <span data-ttu-id="b6ef9-144">In &quot;aggiungere cartelle e di base di riferimenti per... &quot;, controllare **API Web**.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-144">Under &quot;Add folders and core references for...&quot;, check **Web API**.</span></span> <span data-ttu-id="b6ef9-145">Scegliere **OK**.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-145">Click **OK**.</span></span>

![](creating-an-odata-endpoint/_static/image2.png)

<a id="add-model"></a>
## <a name="add-an-entity-model"></a><span data-ttu-id="b6ef9-146">Aggiungere un modello di entità</span><span class="sxs-lookup"><span data-stu-id="b6ef9-146">Add an Entity Model</span></span>

<span data-ttu-id="b6ef9-147">Un *modello* è un oggetto che rappresenta i dati nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-147">A *model* is an object that represents the data in your application.</span></span> <span data-ttu-id="b6ef9-148">Per questa esercitazione, è necessario un modello che rappresenta un prodotto.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-148">For this tutorial, we need a model that represents a product.</span></span> <span data-ttu-id="b6ef9-149">Il modello corrisponde al nostro tipo di entità OData.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-149">The model corresponds to our OData entity type.</span></span>

<span data-ttu-id="b6ef9-150">In Esplora soluzioni fare clic sulla cartella Models.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-150">In Solution Explorer, right-click the Models folder.</span></span> <span data-ttu-id="b6ef9-151">Dal menu di scelta rapida, selezionare **Add** quindi selezionare **classe**.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-151">From the context menu, select **Add** then select **Class**.</span></span>

![](creating-an-odata-endpoint/_static/image3.png)

<span data-ttu-id="b6ef9-152">Nel **Aggiungi nuovo** finestra di dialogo di elemento, denominare la classe &quot;prodotto&quot;.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-152">In the **Add New** Item dialog, name the class &quot;Product&quot;.</span></span>

![](creating-an-odata-endpoint/_static/image4.png)

> [!NOTE]
> <span data-ttu-id="b6ef9-153">Per convenzione, le classi del modello vengono inserite nella cartella Models.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-153">By convention, model classes are placed in the Models folder.</span></span> <span data-ttu-id="b6ef9-154">Non devi seguono questa convenzione nei propri progetti, ma è possibile usarlo per questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-154">You don't have to follow this convention in your own projects, but we'll use it for this tutorial.</span></span>


<span data-ttu-id="b6ef9-155">Nel file Product.cs, aggiungere la definizione di classe seguente:</span><span class="sxs-lookup"><span data-stu-id="b6ef9-155">In the Product.cs file, add the following class definition:</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample1.cs)]

<span data-ttu-id="b6ef9-156">La proprietà ID è la chiave di entità.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-156">The ID property will be the entity key.</span></span> <span data-ttu-id="b6ef9-157">I client possono eseguire query sui prodotti in base all'ID.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-157">Clients can query products by ID.</span></span> <span data-ttu-id="b6ef9-158">Questo campo è anche la chiave primaria nel database di back-end.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-158">This field would also be the primary key in the back-end database.</span></span>

<span data-ttu-id="b6ef9-159">A questo punto, compilare il progetto.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-159">Build the project now.</span></span> <span data-ttu-id="b6ef9-160">Nel passaggio successivo, si userà alcune scaffolding di Visual Studio che usa la reflection per trovare il tipo di prodotto.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-160">In the next step, we'll use some Visual Studio scaffolding that uses reflection to find the Product type.</span></span>

<a id="add-controller"></a>
## <a name="add-an-odata-controller"></a><span data-ttu-id="b6ef9-161">Aggiungere un Controller OData</span><span class="sxs-lookup"><span data-stu-id="b6ef9-161">Add an OData Controller</span></span>

<span data-ttu-id="b6ef9-162">Oggetto *controller* è una classe che gestisce le richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-162">A *controller* is a class that handles HTTP requests.</span></span> <span data-ttu-id="b6ef9-163">È possibile definire un controller separato per ogni set di entità in è un servizio OData.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-163">You define a separate controller for each entity set in you OData service.</span></span> <span data-ttu-id="b6ef9-164">In questa esercitazione si creerà un singolo controller.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-164">In this tutorial, we'll create a single controller.</span></span>

<span data-ttu-id="b6ef9-165">In Esplora soluzioni fare clic sulla cartella Controllers.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-165">In Solution Explorer, right-click the Controllers folder.</span></span> <span data-ttu-id="b6ef9-166">Selezionare **Add** e quindi selezionare **Controller**.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-166">Select **Add** and then select **Controller**.</span></span>

![](creating-an-odata-endpoint/_static/image5.png)

<span data-ttu-id="b6ef9-167">Nel **Add Scaffold** finestra di dialogo, seleziona &quot;Controller Web API 2 OData con azioni, che usa Entity Framework&quot;.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-167">In the **Add Scaffold** dialog, select &quot;Web API 2 OData Controller with actions, using Entity Framework&quot;.</span></span>

![](creating-an-odata-endpoint/_static/image6.png)

<span data-ttu-id="b6ef9-168">Nel **Aggiungi Controller** finestra di dialogo nome al controller "ProductsController".</span><span class="sxs-lookup"><span data-stu-id="b6ef9-168">In the **Add Controller** dialog, name the controller "ProductsController".</span></span> <span data-ttu-id="b6ef9-169">Selezionare il &quot;usare le azioni del controller asincrono&quot; casella di controllo.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-169">Select the &quot;Use async controller actions&quot; checkbox.</span></span> <span data-ttu-id="b6ef9-170">Nel **modello** elenco a discesa selezionare la classe di prodotto.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-170">In the **Model** drop-down list, select the Product class.</span></span>

![](creating-an-odata-endpoint/_static/image7.png)

<span data-ttu-id="b6ef9-171">Fare clic su di **nuovo contesto dati...**  pulsante.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-171">Click the **New data context...** button.</span></span> <span data-ttu-id="b6ef9-172">Lasciare il nome predefinito per il tipo di contesto dei dati e fare clic su **Add**.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-172">Leave the default name for the data context type, and click **Add**.</span></span>

![](creating-an-odata-endpoint/_static/image8.png)

<span data-ttu-id="b6ef9-173">Fare clic su Aggiungi nella finestra di dialogo Aggiungi Controller per aggiungere il controller.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-173">Click Add in the Add Controller dialog to add the controller.</span></span>

![](creating-an-odata-endpoint/_static/image9.png)

<span data-ttu-id="b6ef9-174">Nota: Se viene visualizzato un messaggio di errore che dice &quot;si è verificato un errore di recupero del tipo... &quot;, assicurarsi che è stato compilato il progetto di Visual Studio dopo aver aggiunto la classe di prodotto.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-174">Note: If you get an error message that says &quot;There was an error getting the type...&quot;, make sure that you built the Visual Studio project after you added the Product class.</span></span> <span data-ttu-id="b6ef9-175">Lo scaffolding Usa la reflection per trovare la classe.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-175">The scaffolding uses reflection to find the class.</span></span>

![](creating-an-odata-endpoint/_static/image10.png)

<span data-ttu-id="b6ef9-176">Lo scaffolding aggiunge due file di codice al progetto:</span><span class="sxs-lookup"><span data-stu-id="b6ef9-176">The scaffolding adds two code files to the project:</span></span>

- <span data-ttu-id="b6ef9-177">Sarà Products.cs definisce il controller API Web che implementa l'endpoint OData.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-177">Products.cs defines the Web API controller that implements the OData endpoint.</span></span>
- <span data-ttu-id="b6ef9-178">ProductServiceContext.cs fornisce metodi per eseguire query nel database sottostante, tramite Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-178">ProductServiceContext.cs provides methods to query the underlying database, using Entity Framework.</span></span>

![](creating-an-odata-endpoint/_static/image11.png)

<a id="edm"></a>
## <a name="add-the-edm-and-route"></a><span data-ttu-id="b6ef9-179">Aggiungere il modello EDM e Route</span><span class="sxs-lookup"><span data-stu-id="b6ef9-179">Add the EDM and Route</span></span>

<span data-ttu-id="b6ef9-180">In Esplora soluzioni espandere l'App\_avviare cartella e aprire il file WebApiConfig.cs.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-180">In Solution Explorer, expand the App\_Start folder and open the file named WebApiConfig.cs.</span></span> <span data-ttu-id="b6ef9-181">Questa classe contiene codice di configurazione per l'API Web.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-181">This class holds configuration code for Web API.</span></span> <span data-ttu-id="b6ef9-182">Sostituire il codice con quanto segue:</span><span class="sxs-lookup"><span data-stu-id="b6ef9-182">Replace this code with the following:</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample2.cs)]

<span data-ttu-id="b6ef9-183">Questo codice esegue due operazioni:</span><span class="sxs-lookup"><span data-stu-id="b6ef9-183">This code does two things:</span></span>

- <span data-ttu-id="b6ef9-184">Crea un modello EDM (Entity dati Model) per l'endpoint OData.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-184">Creates an Entity Data Model (EDM) for the OData endpoint.</span></span>
- <span data-ttu-id="b6ef9-185">Aggiunge una route per l'endpoint.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-185">Adds a route for the endpoint.</span></span>

<span data-ttu-id="b6ef9-186">Un modello EDM è un modello di dati astratto.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-186">An EDM is an abstract model of the data.</span></span> <span data-ttu-id="b6ef9-187">Il modello EDM viene utilizzato per creare il documento di metadati e definire gli URI per il servizio.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-187">The EDM is used to create the metadata document and define the URIs for the service.</span></span> <span data-ttu-id="b6ef9-188">Il **ODataConventionModelBuilder** crea un modello EDM, con un set di convenzioni di denominazione predefinito EDM.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-188">The **ODataConventionModelBuilder** creates an EDM by using a set of default naming conventions EDM.</span></span> <span data-ttu-id="b6ef9-189">Questo approccio richiede il minor quantità di codice.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-189">This approach requires the least code.</span></span> <span data-ttu-id="b6ef9-190">Se si desidera maggiore controllo sul modello EDM, è possibile usare la **ODataModelBuilder** classe per creare il modello EDM mediante l'aggiunta di proprietà, le chiavi e le proprietà di navigazione in modo esplicito.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-190">If you want more control over the EDM, you can use the **ODataModelBuilder** class to create the EDM by adding properties, keys, and navigation properties explicitly.</span></span>

<span data-ttu-id="b6ef9-191">Il **EntitySet** metodo aggiunge una set di entità al modello EDM:</span><span class="sxs-lookup"><span data-stu-id="b6ef9-191">The **EntitySet** method adds an entity set to the EDM:</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample3.cs)]

<span data-ttu-id="b6ef9-192">La stringa "Prodotti" definisce il nome del set di entità.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-192">The string "Products" defines the name of the entity set.</span></span> <span data-ttu-id="b6ef9-193">Il nome del controller deve corrispondere al nome del set di entità.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-193">The name of the controller must match the name of the entity set.</span></span> <span data-ttu-id="b6ef9-194">In questa esercitazione, il set di entità viene denominato "Prodotti" e il controller `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-194">In this tutorial, the entity set is named "Products" and the controller is named `ProductsController`.</span></span> <span data-ttu-id="b6ef9-195">Se è stata denominata "ProductSet" del set di entità, si potrebbe denominare il controller `ProductSetController`.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-195">If you named the entity set "ProductSet", you would name the controller `ProductSetController`.</span></span> <span data-ttu-id="b6ef9-196">Si noti che un endpoint può avere più set di entità.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-196">Note that an endpoint can have multiple entity sets.</span></span> <span data-ttu-id="b6ef9-197">Chiamare **EntitySet&lt;T&gt;**  per ogni entità impostata e quindi definire un controller corrispondente.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-197">Call **EntitySet&lt;T&gt;** for each entity set, and then define a corresponding controller.</span></span>

<span data-ttu-id="b6ef9-198">Il **MapODataRoute** metodo aggiunge una route per l'endpoint OData.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-198">The **MapODataRoute** method adds a route for the OData endpoint.</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample4.cs)]

<span data-ttu-id="b6ef9-199">Il primo parametro è un nome descrittivo per la route.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-199">The first parameter is a friendly name for the route.</span></span> <span data-ttu-id="b6ef9-200">Client del servizio non viene visualizzato il nome specificato.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-200">Clients of your service do not see this name.</span></span> <span data-ttu-id="b6ef9-201">Il secondo parametro è il prefisso URI per l'endpoint.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-201">The second parameter is the URI prefix for the endpoint.</span></span> <span data-ttu-id="b6ef9-202">Questo codice, l'URI per il set di entità prodotti è http://<em>hostname</em>  /odata/prodotti.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-202">Given this code, the URI for the Products entity set is http://<em>hostname</em>/odata/Products.</span></span> <span data-ttu-id="b6ef9-203">L'applicazione può avere più di un endpoint OData.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-203">Your application can have more than one OData endpoint.</span></span> <span data-ttu-id="b6ef9-204">Per ogni endpoint, chiamare <strong>MapODataRoute</strong> e fornire un nome di route univoco e un prefisso URI univoco.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-204">For each endpoint, call <strong>MapODataRoute</strong> and provide a unique route name and a unique URI prefix.</span></span>

<a id="seed-db"></a>
## <a name="seed-the-database-optional"></a><span data-ttu-id="b6ef9-205">Seeding del Database (facoltativo)</span><span class="sxs-lookup"><span data-stu-id="b6ef9-205">Seed the Database (Optional)</span></span>

<span data-ttu-id="b6ef9-206">In questo passaggio si userà Entity Framework per effettuare il seeding del database con alcuni dati di test.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-206">In this step, you will use Entity Framework to seed the database with some test data.</span></span> <span data-ttu-id="b6ef9-207">Questo passaggio è facoltativo, ma consente di testare l'endpoint OData sin da subito.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-207">This step is optional, but it lets you test out your OData endpoint right away.</span></span>

<span data-ttu-id="b6ef9-208">Dal **degli strumenti** dal menu **Gestione pacchetti NuGet**, quindi selezionare **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-208">From the **Tools** menu, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="b6ef9-209">Nella finestra della Console di gestione pacchetti immettere il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="b6ef9-209">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample5.cmd)]

<span data-ttu-id="b6ef9-210">Aggiunge una cartella denominata le migrazioni e il file di codice Configuration.cs.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-210">This adds a folder named Migrations and a code file named Configuration.cs.</span></span>

![](creating-an-odata-endpoint/_static/image12.png)

<span data-ttu-id="b6ef9-211">Aprire il file e aggiungere il codice seguente per il `Configuration.Seed` (metodo).</span><span class="sxs-lookup"><span data-stu-id="b6ef9-211">Open this file and add the following code to the `Configuration.Seed` method.</span></span>

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample6.cs)]

<span data-ttu-id="b6ef9-212">Nella finestra della Console di gestione pacchetti immettere i comandi seguenti:</span><span class="sxs-lookup"><span data-stu-id="b6ef9-212">In the Package Manager Console Window, enter the following commands:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample7.cmd)]

<span data-ttu-id="b6ef9-213">Questi comandi generano codice che crea il database e quindi esegue tale codice.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-213">These commands generate code that creates the database, and then executes that code.</span></span>

<a id="explore"></a>
## <a name="exploring-the-odata-endpoint"></a><span data-ttu-id="b6ef9-214">Esplorare l'OData Endpoint</span><span class="sxs-lookup"><span data-stu-id="b6ef9-214">Exploring the OData Endpoint</span></span>

<span data-ttu-id="b6ef9-215">In questa sezione si userà il [Fiddler Web Debugging Proxy](http://www.fiddler2.com) per inviare richieste all'endpoint ed esaminare i messaggi di risposta.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-215">In this section, we'll use the [Fiddler Web Debugging Proxy](http://www.fiddler2.com) to send requests to the endpoint and examine the response messages.</span></span> <span data-ttu-id="b6ef9-216">Ciò consentirà di comprendere le capacità di un endpoint OData.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-216">This will help you to understand the capabilities of an OData endpoint.</span></span>

<span data-ttu-id="b6ef9-217">In Visual Studio, premere F5 per avviare il debug.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-217">In Visual Studio, press F5 to start debugging.</span></span> <span data-ttu-id="b6ef9-218">Per impostazione predefinita, Visual Studio apre nel browser l'indirizzo `http://localhost:*port*`, dove *porta* è il numero di porta configurato nelle impostazioni del progetto.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-218">By default, Visual Studio opens your browser to `http://localhost:*port*`, where *port* is the port number configured in the project settings.</span></span>

<span data-ttu-id="b6ef9-219">È possibile modificare il numero di porta nelle impostazioni del progetto.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-219">You can change the port number in the project settings.</span></span> <span data-ttu-id="b6ef9-220">In Esplora soluzioni fare clic sul progetto e selezionare **proprietà**.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-220">In Solution Explorer, right-click the project and select **Properties**.</span></span> <span data-ttu-id="b6ef9-221">Nella finestra Proprietà, selezionare **Web**.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-221">In the properties window, select **Web**.</span></span> <span data-ttu-id="b6ef9-222">Immettere il numero di porta sotto **Url del progetto**.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-222">Enter the port number under **Project Url**.</span></span>

### <a name="service-document"></a><span data-ttu-id="b6ef9-223">Documento di servizio</span><span class="sxs-lookup"><span data-stu-id="b6ef9-223">Service Document</span></span>

<span data-ttu-id="b6ef9-224">Il *documento di servizio* contiene un elenco dei set di entità per l'endpoint OData.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-224">The *service document* contains a list of the entity sets for the OData endpoint.</span></span> <span data-ttu-id="b6ef9-225">Per ottenere il documento di servizio, inviare una richiesta GET all'URI del servizio di radice.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-225">To get the service document, send a GET request to the root URI of the service.</span></span>

<span data-ttu-id="b6ef9-226">Uso di Fiddler, immettere l'URI seguente nel **Composer** scheda: `http://localhost:port/odata/`, dove *porta* è il numero di porta.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-226">Using Fiddler, enter the following URI in the **Composer** tab: `http://localhost:port/odata/`, where *port* is the port number.</span></span>

![](creating-an-odata-endpoint/_static/image13.png)

<span data-ttu-id="b6ef9-227">Scegliere il **Execute** pulsante.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-227">Click the **Execute** button.</span></span> <span data-ttu-id="b6ef9-228">Fiddler invia una richiesta HTTP GET all'applicazione.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-228">Fiddler sends an HTTP GET request to your application.</span></span> <span data-ttu-id="b6ef9-229">Verrà visualizzata la risposta nell'elenco di sessioni di Web.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-229">You should see the response in the Web Sessions list.</span></span> <span data-ttu-id="b6ef9-230">Se tutto funziona, il codice di stato sarà 200.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-230">If everything is working, the status code will be 200.</span></span>

![](creating-an-odata-endpoint/_static/image14.png)

<span data-ttu-id="b6ef9-231">Fare doppio clic la risposta dell'elenco di sessioni Web per visualizzare i dettagli del messaggio di risposta nella scheda Inspectors.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-231">Double-click the response in the Web Sessions list to see the details of the response message in the Inspectors tab.</span></span>

![](creating-an-odata-endpoint/_static/image15.png)

<span data-ttu-id="b6ef9-232">Il messaggio di risposta HTTP non elaborato dovrebbe essere simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="b6ef9-232">The raw HTTP response message should look similar to the following:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample8.cmd)]

<span data-ttu-id="b6ef9-233">Per impostazione predefinita, API Web restituisce il documento di servizio in formato AtomPub.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-233">By default, Web API returns the service document in AtomPub format.</span></span> <span data-ttu-id="b6ef9-234">Per richiedere JSON, aggiungere la seguente intestazione alla richiesta HTTP:</span><span class="sxs-lookup"><span data-stu-id="b6ef9-234">To request JSON, add the following header to the HTTP request:</span></span>

`Accept: application/json`

![](creating-an-odata-endpoint/_static/image16.png)

<span data-ttu-id="b6ef9-235">Ora la risposta HTTP contiene un payload JSON:</span><span class="sxs-lookup"><span data-stu-id="b6ef9-235">Now the HTTP response contains a JSON payload:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample9.cmd)]

### <a name="service-metadata-document"></a><span data-ttu-id="b6ef9-236">Documento di metadati di servizio</span><span class="sxs-lookup"><span data-stu-id="b6ef9-236">Service Metadata Document</span></span>

<span data-ttu-id="b6ef9-237">Il *documento di metadati di servizio* descrive il modello di dati del servizio, usando un linguaggio XML denominato Conceptual Schema Definition Language (CSDL).</span><span class="sxs-lookup"><span data-stu-id="b6ef9-237">The *service metadata document* describes the data model of the service, using an XML language called the Conceptual Schema Definition Language (CSDL).</span></span> <span data-ttu-id="b6ef9-238">Il documento di metadati viene illustrata la struttura dei dati nel servizio e può essere utilizzato per generare codice client.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-238">The metadata document shows the structure of the data in the service, and can be used to generate client code.</span></span>

<span data-ttu-id="b6ef9-239">Per ottenere il documento di metadati, inviare una richiesta GET a `http://localhost:port/odata/$metadata`.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-239">To get the metadata document, send a GET request to `http://localhost:port/odata/$metadata`.</span></span> <span data-ttu-id="b6ef9-240">Ecco i metadati per l'endpoint mostrato in questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-240">Here is the metadata for the endpoint shown in this tutorial.</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample10.cmd)]

### <a name="entity-set"></a><span data-ttu-id="b6ef9-241">Set di entità</span><span class="sxs-lookup"><span data-stu-id="b6ef9-241">Entity Set</span></span>

<span data-ttu-id="b6ef9-242">Per ottenere il set di entità prodotti, inviare una richiesta GET a `http://localhost:port/odata/Products`.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-242">To get the Products entity set, send a GET request to `http://localhost:port/odata/Products`.</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample11.cmd)]

### <a name="entity"></a><span data-ttu-id="b6ef9-243">Entità</span><span class="sxs-lookup"><span data-stu-id="b6ef9-243">Entity</span></span>

<span data-ttu-id="b6ef9-244">Per ottenere un singolo prodotto, inviare una richiesta GET a `http://localhost:port/odata/Products(1)`, dove "1" è l'ID prodotto.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-244">To get an individual product, send a GET request to `http://localhost:port/odata/Products(1)`, where "1" is the product ID.</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample12.cmd)]

<a id="formats"></a>
## <a name="odata-serialization-formats"></a><span data-ttu-id="b6ef9-245">Formati di serializzazione OData</span><span class="sxs-lookup"><span data-stu-id="b6ef9-245">OData Serialization Formats</span></span>

<span data-ttu-id="b6ef9-246">OData supporta diversi formati di serializzazione:</span><span class="sxs-lookup"><span data-stu-id="b6ef9-246">OData supports several serialization formats:</span></span>

- <span data-ttu-id="b6ef9-247">Atom Pub (XML)</span><span class="sxs-lookup"><span data-stu-id="b6ef9-247">Atom Pub (XML)</span></span>
- <span data-ttu-id="b6ef9-248">JSON "light" (introdotta in OData v3)</span><span class="sxs-lookup"><span data-stu-id="b6ef9-248">JSON "light" (introduced in OData v3)</span></span>
- <span data-ttu-id="b6ef9-249">JSON "verbose" (OData v2)</span><span class="sxs-lookup"><span data-stu-id="b6ef9-249">JSON "verbose" (OData v2)</span></span>

<span data-ttu-id="b6ef9-250">Per impostazione predefinita, API Web Usa AtomPubJSON "light" formato.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-250">By default, Web API uses AtomPubJSON "light" format.</span></span>

<span data-ttu-id="b6ef9-251">Per ottenere il formato AtomPub, impostare l'intestazione Accept su "application/atom + xml".</span><span class="sxs-lookup"><span data-stu-id="b6ef9-251">To get AtomPub format, set the Accept header to "application/atom+xml".</span></span> <span data-ttu-id="b6ef9-252">Di seguito è riportato un corpo della risposta di esempio:</span><span class="sxs-lookup"><span data-stu-id="b6ef9-252">Here is an example response body:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample13.cmd)]

<span data-ttu-id="b6ef9-253">È possibile visualizzare uno svantaggio ovvio del formato Atom: è molto più dettagliato rispetto a JSON light.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-253">You can see one obvious disadvantage of the Atom format: It's a lot more verbose than the JSON light.</span></span> <span data-ttu-id="b6ef9-254">Tuttavia, se si dispone di un client in grado di interpretare AtomPub, il client potrebbe preferire tale formato JSON.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-254">However, if you have a client that understands AtomPub, the client might prefer that format over JSON.</span></span>

<span data-ttu-id="b6ef9-255">Ecco la versione light di JSON della stessa entità:</span><span class="sxs-lookup"><span data-stu-id="b6ef9-255">Here is the JSON light version of the same entity:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample14.cmd)]

<span data-ttu-id="b6ef9-256">Il formato JSON light è stata introdotta nella versione 3 del protocollo OData.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-256">The JSON light format was introduced in version 3 of the OData protocol.</span></span> <span data-ttu-id="b6ef9-257">Per garantire la compatibilità con le versioni precedenti, un client può richiedere il formato JSON "dettagliato" precedente.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-257">For backward compatibility, a client can request the older "verbose" JSON format.</span></span> <span data-ttu-id="b6ef9-258">Per richiedere JSON dettagliato, impostare l'intestazione Accept `application/json;odata=verbose`.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-258">To request verbose JSON, set the Accept header to `application/json;odata=verbose`.</span></span> <span data-ttu-id="b6ef9-259">Ecco la versione dettagliata:</span><span class="sxs-lookup"><span data-stu-id="b6ef9-259">Here is the verbose version:</span></span>

[!code-console[Main](creating-an-odata-endpoint/samples/sample15.cmd)]

<span data-ttu-id="b6ef9-260">Questo formato fornisce più metadati nel corpo della risposta, che è possibile aggiungere un notevole sovraccarico su un'intera sessione.</span><span class="sxs-lookup"><span data-stu-id="b6ef9-260">This format conveys more metadata in the response body, which can add considerable overhead over an entire session.</span></span> <span data-ttu-id="b6ef9-261">Inoltre, aggiunge un livello di riferimento indiretto eseguendo il wrapping dell'oggetto in una proprietà denominata "d".</span><span class="sxs-lookup"><span data-stu-id="b6ef9-261">Also, it adds a level of indirection by wrapping the object in a property named "d".</span></span>

## <a name="next-steps"></a><span data-ttu-id="b6ef9-262">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="b6ef9-262">Next Steps</span></span>

- [<span data-ttu-id="b6ef9-263">Aggiungere relazioni tra entità</span><span class="sxs-lookup"><span data-stu-id="b6ef9-263">Add Entity Relations</span></span>](working-with-entity-relations.md)
- [<span data-ttu-id="b6ef9-264">Aggiungere le azioni OData</span><span class="sxs-lookup"><span data-stu-id="b6ef9-264">Add OData Actions</span></span>](odata-actions.md)
- [<span data-ttu-id="b6ef9-265">Chiamare il servizio OData da un Client .NET</span><span class="sxs-lookup"><span data-stu-id="b6ef9-265">Call the OData Service From a .NET Client</span></span>](calling-an-odata-service-from-a-net-client.md)
