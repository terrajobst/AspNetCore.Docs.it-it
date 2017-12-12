---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
title: Azioni e funzioni in OData v4 tramite ASP.NET Web API 2.2 | Documenti Microsoft
author: MikeWasson
description: "In OData, funzioni e le azioni sono un modo per aggiungere comportamenti sul lato server che non possono essere definiti facilmente come operazioni CRUD su entità. In questa esercitazione viene illustrato come..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/27/2014
ms.topic: article
ms.assetid: 0e6fb03c-b16d-4bb0-ab0b-552bd2b6ece1
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
msc.type: authoredcontent
ms.openlocfilehash: 532362f0c0faaaf0cb0c04726856f0497e5261b5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="actions-and-functions-in-odata-v4-using-aspnet-web-api-22"></a><span data-ttu-id="73d89-104">Azioni e funzioni in OData v4 tramite ASP.NET Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="73d89-104">Actions and Functions in OData v4 Using ASP.NET Web API 2.2</span></span>
====================
<span data-ttu-id="73d89-105">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="73d89-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="73d89-106">In OData, funzioni e le azioni sono un modo per aggiungere comportamenti sul lato server che non possono essere definiti facilmente come operazioni CRUD su entità.</span><span class="sxs-lookup"><span data-stu-id="73d89-106">In OData, actions and functions are a way to add server-side behaviors that are not easily defined as CRUD operations on entities.</span></span> <span data-ttu-id="73d89-107">In questa esercitazione viene illustrato come aggiungere un endpoint OData v4 con Web API 2.2 funzioni e le azioni.</span><span class="sxs-lookup"><span data-stu-id="73d89-107">This tutorial shows how to add actions and functions to an OData v4 endpoint, using Web API 2.2.</span></span> <span data-ttu-id="73d89-108">L'esercitazione si basa sull'esercitazione [creare un OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md)</span><span class="sxs-lookup"><span data-stu-id="73d89-108">The tutorial builds on the tutorial [Create an OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md)</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="73d89-109">Versioni del software utilizzate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="73d89-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="73d89-110">2.2 API Web</span><span class="sxs-lookup"><span data-stu-id="73d89-110">Web API 2.2</span></span>
> - <span data-ttu-id="73d89-111">OData v4</span><span class="sxs-lookup"><span data-stu-id="73d89-111">OData v4</span></span>
> - [<span data-ttu-id="73d89-112">Visual Studio 2013 Update 2</span><span class="sxs-lookup"><span data-stu-id="73d89-112">Visual Studio 2013 Update 2</span></span>](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - <span data-ttu-id="73d89-113">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="73d89-113">.NET 4.5</span></span>
> 
> 
> ## <a name="tutorial-versions"></a><span data-ttu-id="73d89-114">Versioni dell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="73d89-114">Tutorial versions</span></span>
> 
> <span data-ttu-id="73d89-115">Per OData versione 3, vedere [le azioni in ASP.NET Web API 2 OData](../odata-v3/odata-actions.md).</span><span class="sxs-lookup"><span data-stu-id="73d89-115">For OData Version 3, see [OData Actions in ASP.NET Web API 2](../odata-v3/odata-actions.md).</span></span>


<span data-ttu-id="73d89-116">La differenza tra *azioni* e *funzioni* è che le azioni possono avere effetti collaterali e non funzioni.</span><span class="sxs-lookup"><span data-stu-id="73d89-116">The difference between *actions* and *functions* is that actions can have side effects, and functions do not.</span></span> <span data-ttu-id="73d89-117">Le azioni e funzioni possono restituire dati.</span><span class="sxs-lookup"><span data-stu-id="73d89-117">Both actions and functions can return data.</span></span> <span data-ttu-id="73d89-118">Alcuni tipi di utilizzo per le azioni includono:</span><span class="sxs-lookup"><span data-stu-id="73d89-118">Some uses for actions include:</span></span>

- <span data-ttu-id="73d89-119">Transazioni complesse.</span><span class="sxs-lookup"><span data-stu-id="73d89-119">Complex transactions.</span></span>
- <span data-ttu-id="73d89-120">Modifica di alcune entità in una sola volta.</span><span class="sxs-lookup"><span data-stu-id="73d89-120">Manipulating several entities at once.</span></span>
- <span data-ttu-id="73d89-121">Consentire aggiornamenti solo a determinate proprietà di un'entità.</span><span class="sxs-lookup"><span data-stu-id="73d89-121">Allowing updates only to certain properties of an entity.</span></span>
- <span data-ttu-id="73d89-122">L'invio dei dati che non è un'entità.</span><span class="sxs-lookup"><span data-stu-id="73d89-122">Sending data that is not an entity.</span></span>

<span data-ttu-id="73d89-123">Le funzioni sono utili per la restituzione di informazioni che non corrispondono direttamente a un'entità o una raccolta.</span><span class="sxs-lookup"><span data-stu-id="73d89-123">Functions are useful for returning information that does not correspond directly to an entity or collection.</span></span>

<span data-ttu-id="73d89-124">Un'azione (o una funzione) può far riferimento una singola entità o una raccolta.</span><span class="sxs-lookup"><span data-stu-id="73d89-124">An action (or function) can target a single entity or a collection.</span></span> <span data-ttu-id="73d89-125">Nella terminologia di OData, si tratta di *associazione*.</span><span class="sxs-lookup"><span data-stu-id="73d89-125">In OData terminology, this is the *binding*.</span></span> <span data-ttu-id="73d89-126">È anche possibile &quot;non associato&quot; azioni/funzioni, che vengono chiamate come statiche operazioni nel servizio.</span><span class="sxs-lookup"><span data-stu-id="73d89-126">You can also have &quot;unbound&quot; actions/functions, which are called as static operations on the service.</span></span>

## <a name="example-adding-an-action"></a><span data-ttu-id="73d89-127">Esempio: Aggiunta di un'azione</span><span class="sxs-lookup"><span data-stu-id="73d89-127">Example: Adding an Action</span></span>

<span data-ttu-id="73d89-128">È pertanto possibile definire un'azione per valutare un prodotto.</span><span class="sxs-lookup"><span data-stu-id="73d89-128">Let's define an action to rate a product.</span></span>

> [!NOTE]
> <span data-ttu-id="73d89-129">In questa esercitazione si basa sull'esercitazione [creare un OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md)</span><span class="sxs-lookup"><span data-stu-id="73d89-129">This tutorial builds on the tutorial [Create an OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md)</span></span>


<span data-ttu-id="73d89-130">Aggiungere innanzitutto un `ProductRating` modello per rappresentare le classificazioni.</span><span class="sxs-lookup"><span data-stu-id="73d89-130">First, add a `ProductRating` model to represent the ratings.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample1.cs)]

<span data-ttu-id="73d89-131">Aggiungere anche un **DbSet** per la `ProductsContext` classe, in modo che EF verrà creata una tabella di classificazioni nel database.</span><span class="sxs-lookup"><span data-stu-id="73d89-131">Also add a **DbSet** to the `ProductsContext` class, so that EF will create a Ratings table in the database.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample2.cs)]

### <a name="add-the-action-to-the-edm"></a><span data-ttu-id="73d89-132">Aggiungere l'azione di EDM</span><span class="sxs-lookup"><span data-stu-id="73d89-132">Add the Action to the EDM</span></span>

<span data-ttu-id="73d89-133">In WebApiConfig.cs, aggiungere il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="73d89-133">In WebApiConfig.cs, add the following code:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample3.cs)]

<span data-ttu-id="73d89-134">Il **EntityTypeConfiguration.Action** metodo aggiunge un'azione a entity data model (EDM).</span><span class="sxs-lookup"><span data-stu-id="73d89-134">The **EntityTypeConfiguration.Action** method adds an action to the entity data model (EDM).</span></span> <span data-ttu-id="73d89-135">Il **parametro** metodo specifica un parametro tipizzato per l'azione.</span><span class="sxs-lookup"><span data-stu-id="73d89-135">The **Parameter** method specifies a typed parameter for the action.</span></span>

<span data-ttu-id="73d89-136">Questo codice imposta anche lo spazio dei nomi per il modello EDM.</span><span class="sxs-lookup"><span data-stu-id="73d89-136">This code also sets the namespace for the EDM.</span></span> <span data-ttu-id="73d89-137">Lo spazio dei nomi è importante perché l'URI per l'azione include il nome dell'azione completo:</span><span class="sxs-lookup"><span data-stu-id="73d89-137">The namespace matters because the URI for the action includes the fully-qualified action name:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample4.cmd)]

> [!NOTE]
> <span data-ttu-id="73d89-138">In una configurazione tipica di IIS, il punto in questo URL causerà IIS restituire l'errore 404.</span><span class="sxs-lookup"><span data-stu-id="73d89-138">In a typical IIS configuration, the dot in this URL will cause IIS to return error 404.</span></span> <span data-ttu-id="73d89-139">È possibile risolvere il problema aggiungendo la seguente sezione al file Web. config:</span><span class="sxs-lookup"><span data-stu-id="73d89-139">You can resolve this by adding the following section to your Web.Config file:</span></span>

[!code-xml[Main](odata-actions-and-functions/samples/sample5.xml)]

### <a name="add-a-controller-method-for-the-action"></a><span data-ttu-id="73d89-140">Aggiungere un metodo del Controller per l'azione</span><span class="sxs-lookup"><span data-stu-id="73d89-140">Add a Controller Method for the Action</span></span>

<span data-ttu-id="73d89-141">Per abilitare il &quot;frequenza&quot; azione, aggiungere il seguente metodo alla `ProductsController`:</span><span class="sxs-lookup"><span data-stu-id="73d89-141">To enable the &quot;Rate&quot; action, add the following method to `ProductsController`:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample6.cs)]

<span data-ttu-id="73d89-142">Si noti che il nome del metodo corrisponde al nome di azione.</span><span class="sxs-lookup"><span data-stu-id="73d89-142">Notice that the method name matches the action name.</span></span> <span data-ttu-id="73d89-143">Il **[HttpPost]** attributo specifica il metodo è un metodo HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="73d89-143">The **[HttpPost]** attribute specifies the method is an HTTP POST method.</span></span>

<span data-ttu-id="73d89-144">Per richiamare l'azione, il client invia una richiesta HTTP POST simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="73d89-144">To invoke the action, the client sends an HTTP POST request like the following:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample7.cmd)]

<span data-ttu-id="73d89-145">Il &quot;frequenza&quot; azione è associata a istanze del prodotto, pertanto l'URI per l'azione è il nome dell'azione completo aggiunto all'URI dell'entità.</span><span class="sxs-lookup"><span data-stu-id="73d89-145">The &quot;Rate&quot; action is bound to Product instances, so the URI for the action is the fully-qualified action name appended to the entity URI.</span></span> <span data-ttu-id="73d89-146">(Tenere presente che lo spazio dei nomi EDM è impostato su &quot;ProductService&quot;, pertanto è il nome dell'azione completo &quot;ProductService.Rate&quot;.)</span><span class="sxs-lookup"><span data-stu-id="73d89-146">(Recall that we set the EDM namespace to &quot;ProductService&quot;, so the fully-qualified action name is &quot;ProductService.Rate&quot;.)</span></span>

<span data-ttu-id="73d89-147">Il corpo della richiesta contiene i parametri dell'azione come un payload JSON.</span><span class="sxs-lookup"><span data-stu-id="73d89-147">The body of the request contains the action parameters as a JSON payload.</span></span> <span data-ttu-id="73d89-148">API Web converte automaticamente il payload JSON per un **ODataActionParameters** oggetto, che è semplicemente un dizionario di valori di parametro.</span><span class="sxs-lookup"><span data-stu-id="73d89-148">Web API automatically converts the JSON payload to an **ODataActionParameters** object, which is just a dictionary of parameter values.</span></span> <span data-ttu-id="73d89-149">Utilizzare questo dizionario per accedere ai parametri del metodo del controller.</span><span class="sxs-lookup"><span data-stu-id="73d89-149">Use this dictionary to access the parameters in your controller method.</span></span>

<span data-ttu-id="73d89-150">Se il client invia i parametri dell'azione in errato formattare, il valore di **ModelState.IsValid** è false.</span><span class="sxs-lookup"><span data-stu-id="73d89-150">If the client sends the action parameters in the wrong format, the value of **ModelState.IsValid** is false.</span></span> <span data-ttu-id="73d89-151">Controllare questo flag nel metodo di controller e viene restituito un errore se **IsValid** è false.</span><span class="sxs-lookup"><span data-stu-id="73d89-151">Check this flag in your controller method and return an error if **IsValid** is false.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample8.cs)]

## <a name="example-adding-a-function"></a><span data-ttu-id="73d89-152">Esempio: Aggiunta di una funzione</span><span class="sxs-lookup"><span data-stu-id="73d89-152">Example: Adding a Function</span></span>

<span data-ttu-id="73d89-153">Ora aggiungere una funzione OData che restituisce il prodotto più costoso.</span><span class="sxs-lookup"><span data-stu-id="73d89-153">Now let's add an OData function that returns the most expensive product.</span></span> <span data-ttu-id="73d89-154">Come prima, il primo passaggio consiste nell'aggiungere la funzione a EDM.</span><span class="sxs-lookup"><span data-stu-id="73d89-154">As before, the first step is adding the function to the EDM.</span></span> <span data-ttu-id="73d89-155">In WebApiConfig.cs, aggiungere il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="73d89-155">In WebApiConfig.cs, add the following code.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample9.cs)]

<span data-ttu-id="73d89-156">In questo caso, la funzione è associata la raccolta di prodotti, anziché singole istanze di prodotto.</span><span class="sxs-lookup"><span data-stu-id="73d89-156">In this case, the function is bound to the Products collection, rather than individual Product instances.</span></span> <span data-ttu-id="73d89-157">I client di richiamare la funzione inviando una richiesta GET:</span><span class="sxs-lookup"><span data-stu-id="73d89-157">Clients invoke the function by sending a GET request:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample10.cmd)]

<span data-ttu-id="73d89-158">Ecco il metodo del controller per questa funzione:</span><span class="sxs-lookup"><span data-stu-id="73d89-158">Here is the controller method for this function:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample11.cs)]

<span data-ttu-id="73d89-159">Si noti che il nome del metodo corrisponde al nome di funzione.</span><span class="sxs-lookup"><span data-stu-id="73d89-159">Notice that the method name matches the function name.</span></span> <span data-ttu-id="73d89-160">Il **[HttpGet]** attributo specifica il metodo è un metodo HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="73d89-160">The **[HttpGet]** attribute specifies the method is an HTTP GET method.</span></span>

<span data-ttu-id="73d89-161">Di seguito è riportata la risposta HTTP:</span><span class="sxs-lookup"><span data-stu-id="73d89-161">Here is the HTTP response:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample12.cmd)]

## <a name="example-adding-an-unbound-function"></a><span data-ttu-id="73d89-162">Esempio: Aggiunta di una funzione non associata</span><span class="sxs-lookup"><span data-stu-id="73d89-162">Example: Adding an Unbound Function</span></span>

<span data-ttu-id="73d89-163">Nell'esempio precedente è una funzione associata a una raccolta.</span><span class="sxs-lookup"><span data-stu-id="73d89-163">The previous example was a function bound to a collection.</span></span> <span data-ttu-id="73d89-164">Nell'esempio seguente, si creerà un *non associato* (funzione).</span><span class="sxs-lookup"><span data-stu-id="73d89-164">In this next example, we'll create an *unbound* function.</span></span> <span data-ttu-id="73d89-165">Funzioni non associate vengono chiamate come statiche operazioni nel servizio.</span><span class="sxs-lookup"><span data-stu-id="73d89-165">Unbound functions are called as static operations on the service.</span></span> <span data-ttu-id="73d89-166">In questo esempio la funzione restituirà l'IVA per un determinato codice postale.</span><span class="sxs-lookup"><span data-stu-id="73d89-166">The function in this example will return the sales tax for a given postal code.</span></span>

<span data-ttu-id="73d89-167">Nel file WebApiConfig, aggiungere la funzione EDM:</span><span class="sxs-lookup"><span data-stu-id="73d89-167">In the WebApiConfig file, add the function to the EDM:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample13.cs)]

<span data-ttu-id="73d89-168">Si noti che stiamo chiamando il **funzione** direttamente sul **ODataModelBuilder**, anziché il tipo di entità o la raccolta.</span><span class="sxs-lookup"><span data-stu-id="73d89-168">Notice that we are calling **Function** directly on the **ODataModelBuilder**, instead of the entity type or collection.</span></span> <span data-ttu-id="73d89-169">Ciò indica che la funzione è associata il generatore di modelli.</span><span class="sxs-lookup"><span data-stu-id="73d89-169">This tells the model builder that the function is unbound.</span></span>

<span data-ttu-id="73d89-170">Ecco il metodo del controller che implementa la funzione:</span><span class="sxs-lookup"><span data-stu-id="73d89-170">Here is the controller method that implements the function:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample14.cs)]

<span data-ttu-id="73d89-171">Non è importante inserire questo metodo in quale controller di Web API.</span><span class="sxs-lookup"><span data-stu-id="73d89-171">It does not matter which Web API controller you place this method in.</span></span> <span data-ttu-id="73d89-172">È stato possibile inserirla `ProductsController`, o definire controller separato.</span><span class="sxs-lookup"><span data-stu-id="73d89-172">You could put it in `ProductsController`, or define a separate controller.</span></span> <span data-ttu-id="73d89-173">Il **[ODataRoute]** attributo definisce il modello URI per la funzione.</span><span class="sxs-lookup"><span data-stu-id="73d89-173">The **[ODataRoute]** attribute defines the URI template for the function.</span></span>

<span data-ttu-id="73d89-174">Di seguito è riportato un esempio di richiesta client:</span><span class="sxs-lookup"><span data-stu-id="73d89-174">Here is an example client request:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample15.cmd)]

<span data-ttu-id="73d89-175">La risposta HTTP:</span><span class="sxs-lookup"><span data-stu-id="73d89-175">The HTTP response:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample16.cmd)]
