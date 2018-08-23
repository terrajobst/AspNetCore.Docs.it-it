---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
title: Azioni e funzioni in OData v4 tramite ASP.NET Web API 2.2 | Microsoft Docs
author: MikeWasson
description: In OData, funzioni e le azioni sono un modo per aggiungere comportamenti lato server che non sono definiti facilmente come operazioni CRUD su entità. Questa esercitazione viene illustrato come...
ms.author: riande
ms.date: 06/27/2014
ms.assetid: 0e6fb03c-b16d-4bb0-ab0b-552bd2b6ece1
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
msc.type: authoredcontent
ms.openlocfilehash: 4e61c6b4bf59792b6570e32e6d24635d4f5e5ac3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838017"
---
<a name="actions-and-functions-in-odata-v4-using-aspnet-web-api-22"></a><span data-ttu-id="44351-104">Azioni e funzioni in OData v4 tramite ASP.NET Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="44351-104">Actions and Functions in OData v4 Using ASP.NET Web API 2.2</span></span>
====================
<span data-ttu-id="44351-105">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="44351-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="44351-106">In OData, funzioni e le azioni sono un modo per aggiungere comportamenti lato server che non sono definiti facilmente come operazioni CRUD su entità.</span><span class="sxs-lookup"><span data-stu-id="44351-106">In OData, actions and functions are a way to add server-side behaviors that are not easily defined as CRUD operations on entities.</span></span> <span data-ttu-id="44351-107">Questa esercitazione illustra come aggiungere azioni e funzioni a un endpoint OData v4, usando l'API Web 2.2.</span><span class="sxs-lookup"><span data-stu-id="44351-107">This tutorial shows how to add actions and functions to an OData v4 endpoint, using Web API 2.2.</span></span> <span data-ttu-id="44351-108">L'esercitazione si basa sull'esercitazione [creare un OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md)</span><span class="sxs-lookup"><span data-stu-id="44351-108">The tutorial builds on the tutorial [Create an OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md)</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="44351-109">Versioni del software utilizzate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="44351-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="44351-110">API Web 2.2</span><span class="sxs-lookup"><span data-stu-id="44351-110">Web API 2.2</span></span>
> - <span data-ttu-id="44351-111">OData v4</span><span class="sxs-lookup"><span data-stu-id="44351-111">OData v4</span></span>
> - [<span data-ttu-id="44351-112">Visual Studio 2013 Update 2</span><span class="sxs-lookup"><span data-stu-id="44351-112">Visual Studio 2013 Update 2</span></span>](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - <span data-ttu-id="44351-113">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="44351-113">.NET 4.5</span></span>
> 
> 
> ## <a name="tutorial-versions"></a><span data-ttu-id="44351-114">Versioni dell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="44351-114">Tutorial versions</span></span>
> 
> <span data-ttu-id="44351-115">Per OData versione 3, vedere [azioni OData nell'API Web ASP.NET 2](../odata-v3/odata-actions.md).</span><span class="sxs-lookup"><span data-stu-id="44351-115">For OData Version 3, see [OData Actions in ASP.NET Web API 2](../odata-v3/odata-actions.md).</span></span>


<span data-ttu-id="44351-116">La differenza tra *azioni* e *funzioni* è che le azioni possono avere effetti collaterali e non funzioni.</span><span class="sxs-lookup"><span data-stu-id="44351-116">The difference between *actions* and *functions* is that actions can have side effects, and functions do not.</span></span> <span data-ttu-id="44351-117">Le azioni e funzioni possono restituire dati.</span><span class="sxs-lookup"><span data-stu-id="44351-117">Both actions and functions can return data.</span></span> <span data-ttu-id="44351-118">Alcuni usi della azioni includono:</span><span class="sxs-lookup"><span data-stu-id="44351-118">Some uses for actions include:</span></span>

- <span data-ttu-id="44351-119">Transazioni complesse.</span><span class="sxs-lookup"><span data-stu-id="44351-119">Complex transactions.</span></span>
- <span data-ttu-id="44351-120">La modifica più entità in una sola volta.</span><span class="sxs-lookup"><span data-stu-id="44351-120">Manipulating several entities at once.</span></span>
- <span data-ttu-id="44351-121">Consentire gli aggiornamenti solo a determinate proprietà di un'entità.</span><span class="sxs-lookup"><span data-stu-id="44351-121">Allowing updates only to certain properties of an entity.</span></span>
- <span data-ttu-id="44351-122">L'invio di dati che non è un'entità.</span><span class="sxs-lookup"><span data-stu-id="44351-122">Sending data that is not an entity.</span></span>

<span data-ttu-id="44351-123">Le funzioni sono utili per la restituzione di informazioni che non corrispondono direttamente a un'entità o una raccolta.</span><span class="sxs-lookup"><span data-stu-id="44351-123">Functions are useful for returning information that does not correspond directly to an entity or collection.</span></span>

<span data-ttu-id="44351-124">Un'azione (o (funzione)) è destinata a una singola entità o una raccolta.</span><span class="sxs-lookup"><span data-stu-id="44351-124">An action (or function) can target a single entity or a collection.</span></span> <span data-ttu-id="44351-125">Nella terminologia di OData, questo è il *associazione*.</span><span class="sxs-lookup"><span data-stu-id="44351-125">In OData terminology, this is the *binding*.</span></span> <span data-ttu-id="44351-126">È anche possibile avere &quot;non associato&quot; azioni/funzioni, che vengono chiamate come operazioni statiche nel servizio.</span><span class="sxs-lookup"><span data-stu-id="44351-126">You can also have &quot;unbound&quot; actions/functions, which are called as static operations on the service.</span></span>

## <a name="example-adding-an-action"></a><span data-ttu-id="44351-127">Esempio: Aggiunta di un'azione</span><span class="sxs-lookup"><span data-stu-id="44351-127">Example: Adding an Action</span></span>

<span data-ttu-id="44351-128">È importante definire un'azione per valutare un prodotto.</span><span class="sxs-lookup"><span data-stu-id="44351-128">Let's define an action to rate a product.</span></span>

> [!NOTE]
> <span data-ttu-id="44351-129">Questa esercitazione si basa sull'esercitazione [creare un OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md)</span><span class="sxs-lookup"><span data-stu-id="44351-129">This tutorial builds on the tutorial [Create an OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md)</span></span>


<span data-ttu-id="44351-130">In primo luogo, aggiungere un `ProductRating` modello per rappresentare le classificazioni.</span><span class="sxs-lookup"><span data-stu-id="44351-130">First, add a `ProductRating` model to represent the ratings.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample1.cs)]

<span data-ttu-id="44351-131">Aggiungere anche un **DbSet** per il `ProductsContext` classe, in modo che Entity Framework creerà una tabella di classificazione nel database.</span><span class="sxs-lookup"><span data-stu-id="44351-131">Also add a **DbSet** to the `ProductsContext` class, so that EF will create a Ratings table in the database.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample2.cs)]

### <a name="add-the-action-to-the-edm"></a><span data-ttu-id="44351-132">Aggiungere l'azione per il modello EDM</span><span class="sxs-lookup"><span data-stu-id="44351-132">Add the Action to the EDM</span></span>

<span data-ttu-id="44351-133">In WebApiConfig.cs, aggiungere il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="44351-133">In WebApiConfig.cs, add the following code:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample3.cs)]

<span data-ttu-id="44351-134">Il **EntityTypeConfiguration.Action** metodo aggiunge un'azione a entity data model (EDM).</span><span class="sxs-lookup"><span data-stu-id="44351-134">The **EntityTypeConfiguration.Action** method adds an action to the entity data model (EDM).</span></span> <span data-ttu-id="44351-135">Il **parametro** metodo specifica un parametro tipizzato per l'azione.</span><span class="sxs-lookup"><span data-stu-id="44351-135">The **Parameter** method specifies a typed parameter for the action.</span></span>

<span data-ttu-id="44351-136">Questo codice imposta inoltre lo spazio dei nomi per il modello EDM.</span><span class="sxs-lookup"><span data-stu-id="44351-136">This code also sets the namespace for the EDM.</span></span> <span data-ttu-id="44351-137">Lo spazio dei nomi è importante perché l'URI per l'azione include il nome dell'azione completo:</span><span class="sxs-lookup"><span data-stu-id="44351-137">The namespace matters because the URI for the action includes the fully-qualified action name:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample4.cmd)]

> [!NOTE]
> <span data-ttu-id="44351-138">In una tipica configurazione di IIS, il punto in questo URL causerà IIS restituire l'errore 404.</span><span class="sxs-lookup"><span data-stu-id="44351-138">In a typical IIS configuration, the dot in this URL will cause IIS to return error 404.</span></span> <span data-ttu-id="44351-139">È possibile risolvere il problema aggiungendo la sezione seguente al file Web. config:</span><span class="sxs-lookup"><span data-stu-id="44351-139">You can resolve this by adding the following section to your Web.Config file:</span></span>

[!code-xml[Main](odata-actions-and-functions/samples/sample5.xml)]

### <a name="add-a-controller-method-for-the-action"></a><span data-ttu-id="44351-140">Aggiungere un metodo del Controller per l'azione</span><span class="sxs-lookup"><span data-stu-id="44351-140">Add a Controller Method for the Action</span></span>

<span data-ttu-id="44351-141">Per abilitare la &quot;tasso&quot; azione, aggiungere il metodo seguente alla `ProductsController`:</span><span class="sxs-lookup"><span data-stu-id="44351-141">To enable the &quot;Rate&quot; action, add the following method to `ProductsController`:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample6.cs)]

<span data-ttu-id="44351-142">Si noti che il nome del metodo corrisponda al nome dell'azione.</span><span class="sxs-lookup"><span data-stu-id="44351-142">Notice that the method name matches the action name.</span></span> <span data-ttu-id="44351-143">Il **[HttpPost]** attributo specifica il metodo è un metodo HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="44351-143">The **[HttpPost]** attribute specifies the method is an HTTP POST method.</span></span>

<span data-ttu-id="44351-144">Per richiamare l'azione, il client invia una richiesta POST HTTP simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="44351-144">To invoke the action, the client sends an HTTP POST request like the following:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample7.cmd)]

<span data-ttu-id="44351-145">Il &quot;frequenza&quot; azione è associata a istanze del prodotto, in modo che l'URI per l'azione è il nome dell'azione completo aggiunto all'URI dell'entità.</span><span class="sxs-lookup"><span data-stu-id="44351-145">The &quot;Rate&quot; action is bound to Product instances, so the URI for the action is the fully-qualified action name appended to the entity URI.</span></span> <span data-ttu-id="44351-146">(Tenere presente che lo spazio dei nomi EDM è impostato su &quot;ProductService&quot;, quindi è il nome dell'azione completo &quot;ProductService.Rate&quot;.)</span><span class="sxs-lookup"><span data-stu-id="44351-146">(Recall that we set the EDM namespace to &quot;ProductService&quot;, so the fully-qualified action name is &quot;ProductService.Rate&quot;.)</span></span>

<span data-ttu-id="44351-147">Il corpo della richiesta contiene i parametri dell'azione come payload JSON.</span><span class="sxs-lookup"><span data-stu-id="44351-147">The body of the request contains the action parameters as a JSON payload.</span></span> <span data-ttu-id="44351-148">API Web converte automaticamente il payload JSON per un **ODataActionParameters** oggetto, che è semplicemente un dizionario di valori di parametro.</span><span class="sxs-lookup"><span data-stu-id="44351-148">Web API automatically converts the JSON payload to an **ODataActionParameters** object, which is just a dictionary of parameter values.</span></span> <span data-ttu-id="44351-149">Usare questo dizionario per accedere ai parametri nel metodo controller.</span><span class="sxs-lookup"><span data-stu-id="44351-149">Use this dictionary to access the parameters in your controller method.</span></span>

<span data-ttu-id="44351-150">Se il client invia i parametri dell'azione in errate formattare, il valore di **ModelState** è false.</span><span class="sxs-lookup"><span data-stu-id="44351-150">If the client sends the action parameters in the wrong format, the value of **ModelState.IsValid** is false.</span></span> <span data-ttu-id="44351-151">Selezionare questo flag nel metodo di controller e restituire un errore se **IsValid** è false.</span><span class="sxs-lookup"><span data-stu-id="44351-151">Check this flag in your controller method and return an error if **IsValid** is false.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample8.cs)]

## <a name="example-adding-a-function"></a><span data-ttu-id="44351-152">Esempio: Aggiunta di una funzione</span><span class="sxs-lookup"><span data-stu-id="44351-152">Example: Adding a Function</span></span>

<span data-ttu-id="44351-153">A questo punto è possibile aggiungere una funzione OData che restituisce il prodotto più costoso.</span><span class="sxs-lookup"><span data-stu-id="44351-153">Now let's add an OData function that returns the most expensive product.</span></span> <span data-ttu-id="44351-154">Come prima, il primo passaggio viene aggiunta la funzione al modello EDM.</span><span class="sxs-lookup"><span data-stu-id="44351-154">As before, the first step is adding the function to the EDM.</span></span> <span data-ttu-id="44351-155">In WebApiConfig.cs, aggiungere il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="44351-155">In WebApiConfig.cs, add the following code.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample9.cs)]

<span data-ttu-id="44351-156">In questo caso, la funzione è associata la raccolta di prodotti, anziché singole istanze di Product.</span><span class="sxs-lookup"><span data-stu-id="44351-156">In this case, the function is bound to the Products collection, rather than individual Product instances.</span></span> <span data-ttu-id="44351-157">I client di richiamano la funzione inviando una richiesta GET:</span><span class="sxs-lookup"><span data-stu-id="44351-157">Clients invoke the function by sending a GET request:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample10.cmd)]

<span data-ttu-id="44351-158">Ecco il metodo di controller per questa funzione:</span><span class="sxs-lookup"><span data-stu-id="44351-158">Here is the controller method for this function:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample11.cs)]

<span data-ttu-id="44351-159">Si noti che il nome del metodo corrisponda al nome della funzione.</span><span class="sxs-lookup"><span data-stu-id="44351-159">Notice that the method name matches the function name.</span></span> <span data-ttu-id="44351-160">Il **[HttpGet]** attributo specifica il metodo è un metodo HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="44351-160">The **[HttpGet]** attribute specifies the method is an HTTP GET method.</span></span>

<span data-ttu-id="44351-161">Ecco la risposta HTTP:</span><span class="sxs-lookup"><span data-stu-id="44351-161">Here is the HTTP response:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample12.cmd)]

## <a name="example-adding-an-unbound-function"></a><span data-ttu-id="44351-162">Esempio: Aggiunta di una funzione non associata</span><span class="sxs-lookup"><span data-stu-id="44351-162">Example: Adding an Unbound Function</span></span>

<span data-ttu-id="44351-163">Nell'esempio precedente è stato una funzione associata a una raccolta.</span><span class="sxs-lookup"><span data-stu-id="44351-163">The previous example was a function bound to a collection.</span></span> <span data-ttu-id="44351-164">In questo esempio, si creerà un' *non associato* (funzione).</span><span class="sxs-lookup"><span data-stu-id="44351-164">In this next example, we'll create an *unbound* function.</span></span> <span data-ttu-id="44351-165">Le funzioni non associate vengono chiamate come operazioni statiche nel servizio.</span><span class="sxs-lookup"><span data-stu-id="44351-165">Unbound functions are called as static operations on the service.</span></span> <span data-ttu-id="44351-166">In questo esempio la funzione restituirà l'imposta sulle vendite per un determinato codice postale.</span><span class="sxs-lookup"><span data-stu-id="44351-166">The function in this example will return the sales tax for a given postal code.</span></span>

<span data-ttu-id="44351-167">Nel file WebApiConfig, aggiungere la funzione al modello EDM:</span><span class="sxs-lookup"><span data-stu-id="44351-167">In the WebApiConfig file, add the function to the EDM:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample13.cs)]

<span data-ttu-id="44351-168">Si noti che qui viene chiamato **funzione** direttamente sulle **ODataModelBuilder**, anziché il tipo di entità o la raccolta.</span><span class="sxs-lookup"><span data-stu-id="44351-168">Notice that we are calling **Function** directly on the **ODataModelBuilder**, instead of the entity type or collection.</span></span> <span data-ttu-id="44351-169">In questo modo il generatore di modelli che la funzione è non associata.</span><span class="sxs-lookup"><span data-stu-id="44351-169">This tells the model builder that the function is unbound.</span></span>

<span data-ttu-id="44351-170">Ecco il metodo del controller che implementa la funzione:</span><span class="sxs-lookup"><span data-stu-id="44351-170">Here is the controller method that implements the function:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample14.cs)]

<span data-ttu-id="44351-171">Non importa quale controller API Web è inserire in questo metodo.</span><span class="sxs-lookup"><span data-stu-id="44351-171">It does not matter which Web API controller you place this method in.</span></span> <span data-ttu-id="44351-172">È possibile inserirlo `ProductsController`, o definire un controller distinto.</span><span class="sxs-lookup"><span data-stu-id="44351-172">You could put it in `ProductsController`, or define a separate controller.</span></span> <span data-ttu-id="44351-173">Il **[ODataRoute]** attributo definisce il modello URI per la funzione.</span><span class="sxs-lookup"><span data-stu-id="44351-173">The **[ODataRoute]** attribute defines the URI template for the function.</span></span>

<span data-ttu-id="44351-174">Di seguito è riportato un esempio di richiesta client:</span><span class="sxs-lookup"><span data-stu-id="44351-174">Here is an example client request:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample15.cmd)]

<span data-ttu-id="44351-175">La risposta HTTP:</span><span class="sxs-lookup"><span data-stu-id="44351-175">The HTTP response:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample16.cmd)]
