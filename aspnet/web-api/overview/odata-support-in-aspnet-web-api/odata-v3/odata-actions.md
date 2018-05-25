---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
title: Supporto di azioni OData in ASP.NET Web API 2 | Documenti Microsoft
author: MikeWasson
description: 'In OData, le azioni sono un modo per aggiungere comportamenti sul lato server che non possono essere definiti facilmente come operazioni CRUD su entità. Alcuni tipi di utilizzo per le azioni includono: implementare...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/25/2014
ms.topic: article
ms.assetid: 2d7b3aa2-aa47-4e6e-b0ce-3d65a1c6fe02
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
msc.type: authoredcontent
ms.openlocfilehash: acb369ca8f1bab8d7cad14c15f46cfd44beb9fdd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="supporting-odata-actions-in-aspnet-web-api-2"></a><span data-ttu-id="c91bb-104">Supporto di azioni OData in ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="c91bb-104">Supporting OData Actions in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="c91bb-105">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="c91bb-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="c91bb-106">Scaricare il progetto completato</span><span class="sxs-lookup"><span data-stu-id="c91bb-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="c91bb-107">In OData, *azioni* rappresentano un modo per aggiungere comportamenti sul lato server che non possono essere definiti facilmente come operazioni CRUD su entità.</span><span class="sxs-lookup"><span data-stu-id="c91bb-107">In OData, *actions* are a way to add server-side behaviors that are not easily defined as CRUD operations on entities.</span></span> <span data-ttu-id="c91bb-108">Alcuni tipi di utilizzo per le azioni includono:</span><span class="sxs-lookup"><span data-stu-id="c91bb-108">Some uses for actions include:</span></span>
> 
> - <span data-ttu-id="c91bb-109">Implementare le transazioni complesse.</span><span class="sxs-lookup"><span data-stu-id="c91bb-109">Implementing complex transactions.</span></span>
> - <span data-ttu-id="c91bb-110">Modifica di alcune entità in una sola volta.</span><span class="sxs-lookup"><span data-stu-id="c91bb-110">Manipulating several entities at once.</span></span>
> - <span data-ttu-id="c91bb-111">Consentire aggiornamenti solo a determinate proprietà di un'entità.</span><span class="sxs-lookup"><span data-stu-id="c91bb-111">Allowing updates only to certain properties of an entity.</span></span>
> - <span data-ttu-id="c91bb-112">L'invio di informazioni al server che non è definito in un'entità.</span><span class="sxs-lookup"><span data-stu-id="c91bb-112">Sending information to the server that is not defined in an entity.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="c91bb-113">Versioni del software utilizzate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="c91bb-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="c91bb-114">Web API 2</span><span class="sxs-lookup"><span data-stu-id="c91bb-114">Web API 2</span></span>
> - <span data-ttu-id="c91bb-115">OData versione 3</span><span class="sxs-lookup"><span data-stu-id="c91bb-115">OData Version 3</span></span>
> - <span data-ttu-id="c91bb-116">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="c91bb-116">Entity Framework 6</span></span>


## <a name="example-rating-a-product"></a><span data-ttu-id="c91bb-117">Esempio: Valutazione di un prodotto</span><span class="sxs-lookup"><span data-stu-id="c91bb-117">Example: Rating a Product</span></span>

<span data-ttu-id="c91bb-118">In questo esempio, si vuole consentire agli utenti di valutare i prodotti e quindi esporre le valutazioni medie per ogni prodotto.</span><span class="sxs-lookup"><span data-stu-id="c91bb-118">In this example, we want to let users rate products, and then expose the average ratings for each product.</span></span> <span data-ttu-id="c91bb-119">Il database, verrà archiviato un elenco di classificazioni, con chiave fornita per i prodotti.</span><span class="sxs-lookup"><span data-stu-id="c91bb-119">On the database, we will store a list of ratings, keyed to products.</span></span>

<span data-ttu-id="c91bb-120">Di seguito è riportato il modello è possibile che venga utilizzata per rappresentare le classificazioni di Entity Framework:</span><span class="sxs-lookup"><span data-stu-id="c91bb-120">Here is the model we might use to represent the ratings in Entity Framework:</span></span>

[!code-csharp[Main](odata-actions/samples/sample1.cs)]

<span data-ttu-id="c91bb-121">Ma non si desidera che i client registra un `ProductRating` oggetto da una raccolta di "Restrizioni".</span><span class="sxs-lookup"><span data-stu-id="c91bb-121">But we don't want clients to POST a `ProductRating` object to a "Ratings" collection.</span></span> <span data-ttu-id="c91bb-122">In modo intuitivo, la classificazione è associata alla raccolta di prodotti e il client è necessario solo registrare il valore delle classificazioni.</span><span class="sxs-lookup"><span data-stu-id="c91bb-122">Intuitively, the rating is associated with the Products collection, and the client should only need to post the rating value.</span></span>

<span data-ttu-id="c91bb-123">Pertanto, anziché utilizzare le normali operazioni CRUD, è possibile definire un'azione che un client può essere richiamato su un prodotto.</span><span class="sxs-lookup"><span data-stu-id="c91bb-123">Therefore, instead of using the normal CRUD operations, we define an action that a client can invoke on a Product.</span></span> <span data-ttu-id="c91bb-124">Nella terminologia di OData, l'azione è *associato* alle entità del prodotto.</span><span class="sxs-lookup"><span data-stu-id="c91bb-124">In OData terminology, the action is *bound* to Product entities.</span></span>

><span data-ttu-id="c91bb-125">Azioni hanno effetti collaterali sul server.</span><span class="sxs-lookup"><span data-stu-id="c91bb-125">Actions have side-effects on the server.</span></span> <span data-ttu-id="c91bb-126">Per questo motivo, vengono richiamati tramite richieste HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="c91bb-126">For this reason, they are invoked using HTTP POST requests.</span></span> <span data-ttu-id="c91bb-127">Azioni possono avere parametri e tipi restituiti, che sono descritti nei metadati del servizio.</span><span class="sxs-lookup"><span data-stu-id="c91bb-127">Actions can have parameters and return types, which are described in the service metadata.</span></span> <span data-ttu-id="c91bb-128">Il client invia i parametri nel corpo della richiesta e il server invia il valore restituito nel corpo della risposta.</span><span class="sxs-lookup"><span data-stu-id="c91bb-128">The client sends the parameters in the request body, and the server sends the return value in the response body.</span></span> <span data-ttu-id="c91bb-129">Per richiamare l'azione "Frequenza Product", il client invia un POST a un URI simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="c91bb-129">To invoke the "Rate Product" action, the client sends a POST to a URI like the following:</span></span>

[!code-console[Main](odata-actions/samples/sample2.cmd)]

<span data-ttu-id="c91bb-130">I dati nella richiesta POST sono semplicemente la valutazione del prodotto:</span><span class="sxs-lookup"><span data-stu-id="c91bb-130">The data in the POST request is simply the product rating:</span></span>

[!code-console[Main](odata-actions/samples/sample3.cmd)]

## <a name="declare-the-action-in-the-entity-data-model"></a><span data-ttu-id="c91bb-131">Dichiarare l'azione in Entity Data Model</span><span class="sxs-lookup"><span data-stu-id="c91bb-131">Declare the Action in the Entity Data Model</span></span>

<span data-ttu-id="c91bb-132">Nella configurazione dell'API Web, aggiungere l'azione a entity data model (EDM):</span><span class="sxs-lookup"><span data-stu-id="c91bb-132">In your Web API configuration, add the action to the entity data model (EDM):</span></span>

[!code-csharp[Main](odata-actions/samples/sample4.cs)]

<span data-ttu-id="c91bb-133">Questo codice definisce "RateProduct" come un'azione che può essere eseguita su entità Product.</span><span class="sxs-lookup"><span data-stu-id="c91bb-133">This code defines "RateProduct" as an action that can be performed on Product entities.</span></span> <span data-ttu-id="c91bb-134">Dichiara che l'azione che utilizza un **int** parametro denominato "Livello" e restituisce un **int** valore.</span><span class="sxs-lookup"><span data-stu-id="c91bb-134">It also declares that the action takes an **int** parameter named "Rating", and returns an **int** value.</span></span>

## <a name="add-the-action-to-the-controller"></a><span data-ttu-id="c91bb-135">Aggiungere l'azione nel controller</span><span class="sxs-lookup"><span data-stu-id="c91bb-135">Add the Action to the Controller</span></span>

<span data-ttu-id="c91bb-136">L'azione "RateProduct" è associato a entità Product.</span><span class="sxs-lookup"><span data-stu-id="c91bb-136">The "RateProduct" action is bound to Product entities.</span></span> <span data-ttu-id="c91bb-137">Per implementare l'azione, aggiungere un metodo denominato `RateProduct` al controller di prodotti:</span><span class="sxs-lookup"><span data-stu-id="c91bb-137">To implement the action, add a method named `RateProduct` to the Products controller:</span></span>

[!code-csharp[Main](odata-actions/samples/sample5.cs)]

<span data-ttu-id="c91bb-138">Si noti che il nome del metodo corrisponde al nome dell'azione in EDM.</span><span class="sxs-lookup"><span data-stu-id="c91bb-138">Notice that the method name matches the name of the action in the EDM.</span></span> <span data-ttu-id="c91bb-139">Il metodo presenta due parametri:</span><span class="sxs-lookup"><span data-stu-id="c91bb-139">The method has two parameters:</span></span>

- <span data-ttu-id="c91bb-140">*chiave*: la chiave per il prodotto alla velocità.</span><span class="sxs-lookup"><span data-stu-id="c91bb-140">*key*: The key for the product to rate.</span></span>
- <span data-ttu-id="c91bb-141">*i parametri*: un dizionario dei valori di parametro di azione.</span><span class="sxs-lookup"><span data-stu-id="c91bb-141">*parameters*: A dictionary of action parameter values.</span></span>

<span data-ttu-id="c91bb-142">Se si utilizza le convenzioni di routing predefinita, il parametro della chiave deve essere denominato "chiave".</span><span class="sxs-lookup"><span data-stu-id="c91bb-142">If you are using the default routing conventions, the key parameter must be named "key".</span></span> <span data-ttu-id="c91bb-143">È anche importante includere il **[FromOdataUri]** attributo, come illustrato.</span><span class="sxs-lookup"><span data-stu-id="c91bb-143">It is also important to include the **[FromOdataUri]** attribute, as shown.</span></span> <span data-ttu-id="c91bb-144">Questo attributo indica l'API Web per utilizzare le regole di sintassi di OData quando analizza la chiave dall'URI della richiesta.</span><span class="sxs-lookup"><span data-stu-id="c91bb-144">This attribute tells Web API to use OData syntax rules when it parses the key from the request URI.</span></span>

<span data-ttu-id="c91bb-145">Utilizzare il *parametri* dizionario per recuperare i parametri di azione:</span><span class="sxs-lookup"><span data-stu-id="c91bb-145">Use the *parameters* dictionary to get the action parameters:</span></span>

[!code-csharp[Main](odata-actions/samples/sample6.cs)]

<span data-ttu-id="c91bb-146">Se il client invia i parametri dell'azione in corrette di formato, il valore di **ModelState.IsValid** è true.</span><span class="sxs-lookup"><span data-stu-id="c91bb-146">If the client sends the action parameters in the correct format, the value of **ModelState.IsValid** is true.</span></span> <span data-ttu-id="c91bb-147">In tal caso, è possibile utilizzare il **ODataActionParameters** dizionario per ottenere i valori dei parametri.</span><span class="sxs-lookup"><span data-stu-id="c91bb-147">In that case, you can use the **ODataActionParameters** dictionary to get the parameter values.</span></span> <span data-ttu-id="c91bb-148">In questo esempio, il `RateProduct` azione accetta un singolo parametro denominato "Livello".</span><span class="sxs-lookup"><span data-stu-id="c91bb-148">In this example, the `RateProduct` action takes a single parameter named "Rating".</span></span>

## <a name="action-metadata"></a><span data-ttu-id="c91bb-149">Metadati di azione</span><span class="sxs-lookup"><span data-stu-id="c91bb-149">Action Metadata</span></span>

<span data-ttu-id="c91bb-150">Per visualizzare i metadati del servizio, inviare una richiesta GET a /odata/$ metadata.</span><span class="sxs-lookup"><span data-stu-id="c91bb-150">To view the service metadata, send a GET request to /odata/$metadata.</span></span> <span data-ttu-id="c91bb-151">Di seguito è la parte dei metadati che dichiara il `RateProduct` azione:</span><span class="sxs-lookup"><span data-stu-id="c91bb-151">Here is the portion of the metadata that declares the `RateProduct` action:</span></span>

[!code-xml[Main](odata-actions/samples/sample7.xml)]

<span data-ttu-id="c91bb-152">Il **FunctionImport** elemento dichiara l'azione.</span><span class="sxs-lookup"><span data-stu-id="c91bb-152">The **FunctionImport** element declares the action.</span></span> <span data-ttu-id="c91bb-153">La maggior parte dei campi sono di chiara interpretazione, ma non degni di nota sono due:</span><span class="sxs-lookup"><span data-stu-id="c91bb-153">Most of the fields are self-explanatory, but two are worth noting:</span></span>

- <span data-ttu-id="c91bb-154">**È IsBindable** significa che l'azione può essere richiamato almeno su entità di destinazione, parte del tempo.</span><span class="sxs-lookup"><span data-stu-id="c91bb-154">**IsBindable** means the action can be invoked on the target entity, at least some of the time.</span></span>
- <span data-ttu-id="c91bb-155">**IsAlwaysBindable** significa che l'azione può sempre essere richiamato su entità di destinazione.</span><span class="sxs-lookup"><span data-stu-id="c91bb-155">**IsAlwaysBindable** means the action can always be invoked on the target entity.</span></span>

<span data-ttu-id="c91bb-156">La differenza è che alcune azioni sono sempre disponibili ai client, ma altre azioni potrebbero dipendono dallo stato dell'entità.</span><span class="sxs-lookup"><span data-stu-id="c91bb-156">The difference is that some actions are always available to clients, but other actions might depend on the state of the entity.</span></span> <span data-ttu-id="c91bb-157">Ad esempio, si supponga di che definisce un'azione "Acquisto".</span><span class="sxs-lookup"><span data-stu-id="c91bb-157">For example, suppose you define a "Purchase" action.</span></span> <span data-ttu-id="c91bb-158">È possibile acquistare solo un elemento che è disponibile in magazzino.</span><span class="sxs-lookup"><span data-stu-id="c91bb-158">You can only purchase an item that is in stock.</span></span> <span data-ttu-id="c91bb-159">Se l'elemento è esaurito, un client non può richiamare tale azione.</span><span class="sxs-lookup"><span data-stu-id="c91bb-159">If the item is out of stock, a client cannot invoke that action.</span></span>

<span data-ttu-id="c91bb-160">Quando si definisce EDM, il **azione** metodo crea un'azione sempre associabile:</span><span class="sxs-lookup"><span data-stu-id="c91bb-160">When you define the EDM, the **Action** method creates an always-bindable action:</span></span>

[!code-csharp[Main](odata-actions/samples/sample8.cs?highlight=1)]

<span data-ttu-id="c91bb-161">Parlerò azioni non sempre-associabile (detto anche *temporanei* azioni) più avanti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="c91bb-161">I'll talk about not-always-bindable actions (also called *transient* actions) later in this topic.</span></span>

## <a name="invoking-the-action"></a><span data-ttu-id="c91bb-162">Richiamare l'azione</span><span class="sxs-lookup"><span data-stu-id="c91bb-162">Invoking the Action</span></span>

<span data-ttu-id="c91bb-163">Ora di seguito viene illustrato come un client richiama l'azione.</span><span class="sxs-lookup"><span data-stu-id="c91bb-163">Now let's see how a client would invoke this action.</span></span> <span data-ttu-id="c91bb-164">Si supponga che il client desidera assegnare una classificazione pari a 2 per il prodotto con ID = 4.</span><span class="sxs-lookup"><span data-stu-id="c91bb-164">Suppose the client wants to give a rating of 2 to the product with ID = 4.</span></span> <span data-ttu-id="c91bb-165">Di seguito è riportato un messaggio di richiesta di esempio, nel formato JSON per il corpo della richiesta:</span><span class="sxs-lookup"><span data-stu-id="c91bb-165">Here is an example request message, using JSON format for the request body:</span></span>

[!code-console[Main](odata-actions/samples/sample9.cmd)]

<span data-ttu-id="c91bb-166">Di seguito è riportato il messaggio di risposta:</span><span class="sxs-lookup"><span data-stu-id="c91bb-166">Here is the response message:</span></span>

[!code-console[Main](odata-actions/samples/sample10.cmd)]

## <a name="binding-an-action-to-an-entity-set"></a><span data-ttu-id="c91bb-167">Associazione di un'azione a un Set di entità</span><span class="sxs-lookup"><span data-stu-id="c91bb-167">Binding an Action to an Entity Set</span></span>

<span data-ttu-id="c91bb-168">Nell'esempio precedente, l'azione è associata a una singola entità: velocità di trasferimento dei client di un singolo prodotto.</span><span class="sxs-lookup"><span data-stu-id="c91bb-168">In the previous example, the action is bound to a single entity: The client rates a single product.</span></span> <span data-ttu-id="c91bb-169">È anche possibile associare un'azione a una raccolta di entità.</span><span class="sxs-lookup"><span data-stu-id="c91bb-169">You can also bind an action to a collection of entities.</span></span> <span data-ttu-id="c91bb-170">Consente di eseguire solo le modifiche seguenti:</span><span class="sxs-lookup"><span data-stu-id="c91bb-170">Just make the following changes:</span></span>

<span data-ttu-id="c91bb-171">In EDM, aggiungere l'azione per l'entità **raccolta** proprietà.</span><span class="sxs-lookup"><span data-stu-id="c91bb-171">In the EDM, add the action to the entity's **Collection** property.</span></span>

[!code-csharp[Main](odata-actions/samples/sample11.cs?highlight=1)]

<span data-ttu-id="c91bb-172">Nel metodo del controller, omettere il *chiave* parametro.</span><span class="sxs-lookup"><span data-stu-id="c91bb-172">In the controller method, omit the *key* parameter.</span></span>

[!code-csharp[Main](odata-actions/samples/sample12.cs)]

<span data-ttu-id="c91bb-173">A questo punto il client richiama l'azione sul set di entità Products:</span><span class="sxs-lookup"><span data-stu-id="c91bb-173">Now the client invokes the action on the Products entity set:</span></span>

[!code-console[Main](odata-actions/samples/sample13.cmd)]

## <a name="actions-with-collection-parameters"></a><span data-ttu-id="c91bb-174">Operazioni con i parametri della raccolta</span><span class="sxs-lookup"><span data-stu-id="c91bb-174">Actions with Collection Parameters</span></span>

<span data-ttu-id="c91bb-175">Azioni possono avere parametri che accettano una raccolta di valori.</span><span class="sxs-lookup"><span data-stu-id="c91bb-175">Actions can have parameters that take a collection of values.</span></span> <span data-ttu-id="c91bb-176">In EDM, utilizzare **CollectionParameter&lt;T&gt;**  per dichiarare il parametro.</span><span class="sxs-lookup"><span data-stu-id="c91bb-176">In the EDM, use **CollectionParameter&lt;T&gt;** to declare the parameter.</span></span>

[!code-csharp[Main](odata-actions/samples/sample14.cs)]

<span data-ttu-id="c91bb-177">Si dichiara un parametro denominato "Restrizioni" che accetta una raccolta di **int** valori.</span><span class="sxs-lookup"><span data-stu-id="c91bb-177">This declares a parameter named "Ratings" that takes a collection of **int** values.</span></span> <span data-ttu-id="c91bb-178">Nel metodo del controller, è comunque ottenere il valore di parametro di **ODataActionParameters** oggetto, ma ora il valore è un **ICollection&lt;int&gt;**  valore:</span><span class="sxs-lookup"><span data-stu-id="c91bb-178">In the controller method, you still get the parameter value from the **ODataActionParameters** object, but now the value is an **ICollection&lt;int&gt;** value:</span></span>

[!code-csharp[Main](odata-actions/samples/sample15.cs)]

## <a name="transient-actions"></a><span data-ttu-id="c91bb-179">Azioni temporanee</span><span class="sxs-lookup"><span data-stu-id="c91bb-179">Transient Actions</span></span>

<span data-ttu-id="c91bb-180">Nell'esempio "RateProduct", gli utenti possono sempre frequenza un prodotto, pertanto l'azione è sempre disponibile.</span><span class="sxs-lookup"><span data-stu-id="c91bb-180">In the "RateProduct" example, users can always rate a product, so the action is always available.</span></span> <span data-ttu-id="c91bb-181">Tuttavia, alcune azioni dipendono dallo stato dell'entità.</span><span class="sxs-lookup"><span data-stu-id="c91bb-181">But some actions depend on the state of the entity.</span></span> <span data-ttu-id="c91bb-182">Ad esempio, in un servizio di noleggio di video, l'azione "Checkpoint" non è sempre disponibile.</span><span class="sxs-lookup"><span data-stu-id="c91bb-182">For example, in a video rental service, the "CheckOut" action is not always available.</span></span> <span data-ttu-id="c91bb-183">(A seconda se è disponibile una copia di tale video.) Questo tipo di azione è definito un *temporanei* azione.</span><span class="sxs-lookup"><span data-stu-id="c91bb-183">(It depends whether a copy of that video is available.) This type of action is called a *transient* action.</span></span>

<span data-ttu-id="c91bb-184">Nei metadati del servizio, è un'azione temporanea **IsAlwaysBindable** uguale a false.</span><span class="sxs-lookup"><span data-stu-id="c91bb-184">In the service metadata, a transient action has **IsAlwaysBindable** equal to false.</span></span> <span data-ttu-id="c91bb-185">Che rappresenta il valore predefinito, i metadati avrà un aspetto simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="c91bb-185">That's actually the default value, so the metadata will look like this:</span></span>

[!code-xml[Main](odata-actions/samples/sample16.xml)]

<span data-ttu-id="c91bb-186">Ecco perché questo è importante: se un'azione è temporanea, il server è necessario indicare al client quando l'azione è disponibile.</span><span class="sxs-lookup"><span data-stu-id="c91bb-186">Here's why this matters: If an action is transient, the server needs to tell the client when the action is available.</span></span> <span data-ttu-id="c91bb-187">Ciò avviene inserendo un collegamento all'azione nell'entità.</span><span class="sxs-lookup"><span data-stu-id="c91bb-187">It does this by including a link to the action in the entity.</span></span> <span data-ttu-id="c91bb-188">Di seguito è riportato un esempio per un'entità filmato:</span><span class="sxs-lookup"><span data-stu-id="c91bb-188">Here is an example for a Movie entity:</span></span>

[!code-console[Main](odata-actions/samples/sample17.cmd)]

<span data-ttu-id="c91bb-189">La proprietà "#CheckOut" contiene un collegamento all'azione di estrazione.</span><span class="sxs-lookup"><span data-stu-id="c91bb-189">The "#CheckOut" property contains a link to the CheckOut action.</span></span> <span data-ttu-id="c91bb-190">Se l'azione non è disponibile, il server viene omesso il collegamento.</span><span class="sxs-lookup"><span data-stu-id="c91bb-190">If the action is not available, the server omits the link.</span></span>

<span data-ttu-id="c91bb-191">Per dichiarare un'azione temporanea in EDM, chiamare il **TransientAction** metodo:</span><span class="sxs-lookup"><span data-stu-id="c91bb-191">To declare a transient action in the EDM, call the **TransientAction** method:</span></span>

[!code-csharp[Main](odata-actions/samples/sample18.cs)]

<span data-ttu-id="c91bb-192">Inoltre, è necessario fornire una funzione che restituisce un collegamento all'azione per una determinata entità.</span><span class="sxs-lookup"><span data-stu-id="c91bb-192">Also, you must provide a function that returns an action link for a given entity.</span></span> <span data-ttu-id="c91bb-193">Impostare questa funzione chiamando **HasActionLink**.</span><span class="sxs-lookup"><span data-stu-id="c91bb-193">Set this function by calling **HasActionLink**.</span></span> <span data-ttu-id="c91bb-194">È possibile scrivere la funzione come un'espressione lambda:</span><span class="sxs-lookup"><span data-stu-id="c91bb-194">You can write the function as a lambda expression:</span></span>

[!code-csharp[Main](odata-actions/samples/sample19.cs)]

<span data-ttu-id="c91bb-195">Se l'azione è disponibile, l'espressione lambda restituisce un collegamento all'azione.</span><span class="sxs-lookup"><span data-stu-id="c91bb-195">If the action is available, the lambda expression returns a link to the action.</span></span> <span data-ttu-id="c91bb-196">Il serializzatore OData include questo collegamento durante la serializzazione dell'entità.</span><span class="sxs-lookup"><span data-stu-id="c91bb-196">The OData serializer includes this link when it serializes the entity.</span></span> <span data-ttu-id="c91bb-197">Quando l'azione non è disponibile, la funzione restituisce `null`.</span><span class="sxs-lookup"><span data-stu-id="c91bb-197">When the action is not available, the function returns `null`.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c91bb-198">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="c91bb-198">Additional Resources</span></span>

[<span data-ttu-id="c91bb-199">Esempio di azioni OData</span><span class="sxs-lookup"><span data-stu-id="c91bb-199">OData Actions Sample</span></span>](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v3/ODataActionsSample/)
