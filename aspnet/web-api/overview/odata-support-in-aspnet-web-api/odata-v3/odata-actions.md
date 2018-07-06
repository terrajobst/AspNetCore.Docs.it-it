---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
title: Supportare le azioni OData nell'API Web ASP.NET 2 | Microsoft Docs
author: MikeWasson
description: 'In OData le azioni sono un modo per aggiungere comportamenti lato server che non sono definiti facilmente come operazioni CRUD su entità. Alcuni usi di azioni includono: implementare...'
ms.author: aspnetcontent
ms.date: 02/25/2014
ms.assetid: 2d7b3aa2-aa47-4e6e-b0ce-3d65a1c6fe02
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
msc.type: authoredcontent
ms.openlocfilehash: e02ab21b864e328fe6892a00e5d5aca3f88eb9a2
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37837772"
---
<a name="supporting-odata-actions-in-aspnet-web-api-2"></a><span data-ttu-id="b742b-104">Supportare le azioni OData nell'API Web ASP.NET 2</span><span class="sxs-lookup"><span data-stu-id="b742b-104">Supporting OData Actions in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="b742b-105">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="b742b-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="b742b-106">Download progetto completato</span><span class="sxs-lookup"><span data-stu-id="b742b-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="b742b-107">In OData *azioni* rappresentano un modo per aggiungere comportamenti lato server che non sono definiti facilmente come operazioni CRUD su entità.</span><span class="sxs-lookup"><span data-stu-id="b742b-107">In OData, *actions* are a way to add server-side behaviors that are not easily defined as CRUD operations on entities.</span></span> <span data-ttu-id="b742b-108">Alcuni usi della azioni includono:</span><span class="sxs-lookup"><span data-stu-id="b742b-108">Some uses for actions include:</span></span>
> 
> - <span data-ttu-id="b742b-109">Implementazione di transazioni complesse.</span><span class="sxs-lookup"><span data-stu-id="b742b-109">Implementing complex transactions.</span></span>
> - <span data-ttu-id="b742b-110">La modifica più entità in una sola volta.</span><span class="sxs-lookup"><span data-stu-id="b742b-110">Manipulating several entities at once.</span></span>
> - <span data-ttu-id="b742b-111">Consentire gli aggiornamenti solo a determinate proprietà di un'entità.</span><span class="sxs-lookup"><span data-stu-id="b742b-111">Allowing updates only to certain properties of an entity.</span></span>
> - <span data-ttu-id="b742b-112">L'invio di informazioni al server che non è definito in un'entità.</span><span class="sxs-lookup"><span data-stu-id="b742b-112">Sending information to the server that is not defined in an entity.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="b742b-113">Versioni del software utilizzate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="b742b-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="b742b-114">API Web 2</span><span class="sxs-lookup"><span data-stu-id="b742b-114">Web API 2</span></span>
> - <span data-ttu-id="b742b-115">OData versione 3</span><span class="sxs-lookup"><span data-stu-id="b742b-115">OData Version 3</span></span>
> - <span data-ttu-id="b742b-116">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="b742b-116">Entity Framework 6</span></span>


## <a name="example-rating-a-product"></a><span data-ttu-id="b742b-117">Esempio: Valutazione di un prodotto</span><span class="sxs-lookup"><span data-stu-id="b742b-117">Example: Rating a Product</span></span>

<span data-ttu-id="b742b-118">In questo esempio si vuole consentire agli utenti di classificare i prodotti ed esporre le valutazioni medie per ogni prodotto.</span><span class="sxs-lookup"><span data-stu-id="b742b-118">In this example, we want to let users rate products, and then expose the average ratings for each product.</span></span> <span data-ttu-id="b742b-119">Il database, Microsoft archivia un elenco di classificazioni, con chiave fornita per i prodotti.</span><span class="sxs-lookup"><span data-stu-id="b742b-119">On the database, we will store a list of ratings, keyed to products.</span></span>

<span data-ttu-id="b742b-120">Ecco il modello è possibile che venga utilizzata per rappresentare le classificazioni in Entity Framework:</span><span class="sxs-lookup"><span data-stu-id="b742b-120">Here is the model we might use to represent the ratings in Entity Framework:</span></span>

[!code-csharp[Main](odata-actions/samples/sample1.cs)]

<span data-ttu-id="b742b-121">Ma non vogliamo che i client a POST un `ProductRating` oggetto a una raccolta di "Restrizioni".</span><span class="sxs-lookup"><span data-stu-id="b742b-121">But we don't want clients to POST a `ProductRating` object to a "Ratings" collection.</span></span> <span data-ttu-id="b742b-122">Intuitivamente, la classificazione è associata all'insieme di prodotti e il client è necessario solo registrare il valore di classificazione.</span><span class="sxs-lookup"><span data-stu-id="b742b-122">Intuitively, the rating is associated with the Products collection, and the client should only need to post the rating value.</span></span>

<span data-ttu-id="b742b-123">Invece di usare le normali operazioni CRUD, definiamo pertanto un'azione che un client può essere richiamato su un prodotto.</span><span class="sxs-lookup"><span data-stu-id="b742b-123">Therefore, instead of using the normal CRUD operations, we define an action that a client can invoke on a Product.</span></span> <span data-ttu-id="b742b-124">Nella terminologia di OData, è l'azione *associata* alle entità Product.</span><span class="sxs-lookup"><span data-stu-id="b742b-124">In OData terminology, the action is *bound* to Product entities.</span></span>

><span data-ttu-id="b742b-125">Le azioni hanno effetti collaterali sul server.</span><span class="sxs-lookup"><span data-stu-id="b742b-125">Actions have side-effects on the server.</span></span> <span data-ttu-id="b742b-126">Per questo motivo, vengono richiamati usando richieste HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="b742b-126">For this reason, they are invoked using HTTP POST requests.</span></span> <span data-ttu-id="b742b-127">Le azioni possono includere i parametri e tipi restituiti, che sono descritti nei metadati del servizio.</span><span class="sxs-lookup"><span data-stu-id="b742b-127">Actions can have parameters and return types, which are described in the service metadata.</span></span> <span data-ttu-id="b742b-128">Il client invia i parametri nel corpo della richiesta e il server invia il valore restituito nel corpo della risposta.</span><span class="sxs-lookup"><span data-stu-id="b742b-128">The client sends the parameters in the request body, and the server sends the return value in the response body.</span></span> <span data-ttu-id="b742b-129">Per richiamare l'azione "Frequenza Product", il client invia una richiesta POST a un URI simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="b742b-129">To invoke the "Rate Product" action, the client sends a POST to a URI like the following:</span></span>

[!code-console[Main](odata-actions/samples/sample2.cmd)]

<span data-ttu-id="b742b-130">I dati nella richiesta POST sono semplicemente la valutazione del prodotto:</span><span class="sxs-lookup"><span data-stu-id="b742b-130">The data in the POST request is simply the product rating:</span></span>

[!code-console[Main](odata-actions/samples/sample3.cmd)]

## <a name="declare-the-action-in-the-entity-data-model"></a><span data-ttu-id="b742b-131">Dichiarare l'azione in Entity Data Model</span><span class="sxs-lookup"><span data-stu-id="b742b-131">Declare the Action in the Entity Data Model</span></span>

<span data-ttu-id="b742b-132">Nella configurazione dell'API Web, aggiungere l'azione di entity data model (EDM):</span><span class="sxs-lookup"><span data-stu-id="b742b-132">In your Web API configuration, add the action to the entity data model (EDM):</span></span>

[!code-csharp[Main](odata-actions/samples/sample4.cs)]

<span data-ttu-id="b742b-133">Questo codice definisce "RateProduct" come un'azione che può essere eseguita su entità Product.</span><span class="sxs-lookup"><span data-stu-id="b742b-133">This code defines "RateProduct" as an action that can be performed on Product entities.</span></span> <span data-ttu-id="b742b-134">Dichiara inoltre che l'azione richiede un **int** parametro denominato "Rating" e restituisce un' **int** valore.</span><span class="sxs-lookup"><span data-stu-id="b742b-134">It also declares that the action takes an **int** parameter named "Rating", and returns an **int** value.</span></span>

## <a name="add-the-action-to-the-controller"></a><span data-ttu-id="b742b-135">Aggiungere l'azione al Controller</span><span class="sxs-lookup"><span data-stu-id="b742b-135">Add the Action to the Controller</span></span>

<span data-ttu-id="b742b-136">L'azione "RateProduct" è associato a entità Product.</span><span class="sxs-lookup"><span data-stu-id="b742b-136">The "RateProduct" action is bound to Product entities.</span></span> <span data-ttu-id="b742b-137">Per implementare l'azione, aggiungere un metodo denominato `RateProduct` al controller di prodotti:</span><span class="sxs-lookup"><span data-stu-id="b742b-137">To implement the action, add a method named `RateProduct` to the Products controller:</span></span>

[!code-csharp[Main](odata-actions/samples/sample5.cs)]

<span data-ttu-id="b742b-138">Si noti che il nome del metodo corrisponde al nome dell'azione nel modello EDM.</span><span class="sxs-lookup"><span data-stu-id="b742b-138">Notice that the method name matches the name of the action in the EDM.</span></span> <span data-ttu-id="b742b-139">Il metodo ha due parametri:</span><span class="sxs-lookup"><span data-stu-id="b742b-139">The method has two parameters:</span></span>

- <span data-ttu-id="b742b-140">*chiave*: la chiave per il prodotto alla velocità.</span><span class="sxs-lookup"><span data-stu-id="b742b-140">*key*: The key for the product to rate.</span></span>
- <span data-ttu-id="b742b-141">*parametri*: un dizionario di valori di parametro di azione.</span><span class="sxs-lookup"><span data-stu-id="b742b-141">*parameters*: A dictionary of action parameter values.</span></span>

<span data-ttu-id="b742b-142">Se si usa le convenzioni di routing predefinita, il parametro della chiave deve essere denominato "chiavi".</span><span class="sxs-lookup"><span data-stu-id="b742b-142">If you are using the default routing conventions, the key parameter must be named "key".</span></span> <span data-ttu-id="b742b-143">È anche importante includere la **[FromOdataUri]** dell'attributo, come illustrato.</span><span class="sxs-lookup"><span data-stu-id="b742b-143">It is also important to include the **[FromOdataUri]** attribute, as shown.</span></span> <span data-ttu-id="b742b-144">Questo attributo indica a Web API da usare le regole di sintassi di OData quando analizza la chiave dall'URI della richiesta.</span><span class="sxs-lookup"><span data-stu-id="b742b-144">This attribute tells Web API to use OData syntax rules when it parses the key from the request URI.</span></span>

<span data-ttu-id="b742b-145">Usare la *parametri* dizionario utilizzato per recuperare i parametri di azione:</span><span class="sxs-lookup"><span data-stu-id="b742b-145">Use the *parameters* dictionary to get the action parameters:</span></span>

[!code-csharp[Main](odata-actions/samples/sample6.cs)]

<span data-ttu-id="b742b-146">Se il client invia parametri di azione il corretto di formato, il valore di **ModelState** è true.</span><span class="sxs-lookup"><span data-stu-id="b742b-146">If the client sends the action parameters in the correct format, the value of **ModelState.IsValid** is true.</span></span> <span data-ttu-id="b742b-147">In tal caso, è possibile usare la **ODataActionParameters** dizionario utilizzato per ottenere i valori dei parametri.</span><span class="sxs-lookup"><span data-stu-id="b742b-147">In that case, you can use the **ODataActionParameters** dictionary to get the parameter values.</span></span> <span data-ttu-id="b742b-148">In questo esempio, il `RateProduct` azione accetta un singolo parametro denominato "Rating".</span><span class="sxs-lookup"><span data-stu-id="b742b-148">In this example, the `RateProduct` action takes a single parameter named "Rating".</span></span>

## <a name="action-metadata"></a><span data-ttu-id="b742b-149">Metadati di azione</span><span class="sxs-lookup"><span data-stu-id="b742b-149">Action Metadata</span></span>

<span data-ttu-id="b742b-150">Per visualizzare i metadati del servizio, inviare una richiesta GET a /odata/$ metadata.</span><span class="sxs-lookup"><span data-stu-id="b742b-150">To view the service metadata, send a GET request to /odata/$metadata.</span></span> <span data-ttu-id="b742b-151">Questa è la parte dei metadati che dichiara il `RateProduct` azione:</span><span class="sxs-lookup"><span data-stu-id="b742b-151">Here is the portion of the metadata that declares the `RateProduct` action:</span></span>

[!code-xml[Main](odata-actions/samples/sample7.xml)]

<span data-ttu-id="b742b-152">Il **FunctionImport** elemento dichiara l'azione.</span><span class="sxs-lookup"><span data-stu-id="b742b-152">The **FunctionImport** element declares the action.</span></span> <span data-ttu-id="b742b-153">La maggior parte dei campi sono di chiara interpretazione, ma sono la pena notare due:</span><span class="sxs-lookup"><span data-stu-id="b742b-153">Most of the fields are self-explanatory, but two are worth noting:</span></span>

- <span data-ttu-id="b742b-154">**È IsBindable** significa che l'azione può essere richiamato almeno su entità di destinazione, alcuni dei casi.</span><span class="sxs-lookup"><span data-stu-id="b742b-154">**IsBindable** means the action can be invoked on the target entity, at least some of the time.</span></span>
- <span data-ttu-id="b742b-155">**IsAlwaysBindable** significa che l'azione può sempre essere richiamato su entità di destinazione.</span><span class="sxs-lookup"><span data-stu-id="b742b-155">**IsAlwaysBindable** means the action can always be invoked on the target entity.</span></span>

<span data-ttu-id="b742b-156">La differenza è che alcune azioni sono sempre disponibili ai client, ma altre azioni potrebbero dipendere dallo stato dell'entità.</span><span class="sxs-lookup"><span data-stu-id="b742b-156">The difference is that some actions are always available to clients, but other actions might depend on the state of the entity.</span></span> <span data-ttu-id="b742b-157">Ad esempio, si supponga di definita un'azione "Acquisto".</span><span class="sxs-lookup"><span data-stu-id="b742b-157">For example, suppose you define a "Purchase" action.</span></span> <span data-ttu-id="b742b-158">È possibile acquistare solo un elemento che è in magazzino.</span><span class="sxs-lookup"><span data-stu-id="b742b-158">You can only purchase an item that is in stock.</span></span> <span data-ttu-id="b742b-159">Se l'elemento è esaurito a distanza, un client non è possibile richiamare tale azione.</span><span class="sxs-lookup"><span data-stu-id="b742b-159">If the item is out of stock, a client cannot invoke that action.</span></span>

<span data-ttu-id="b742b-160">Quando si definisce il modello EDM, il **azione** metodo crea un'azione sempre associabile:</span><span class="sxs-lookup"><span data-stu-id="b742b-160">When you define the EDM, the **Action** method creates an always-bindable action:</span></span>

[!code-csharp[Main](odata-actions/samples/sample8.cs?highlight=1)]

<span data-ttu-id="b742b-161">Verranno descritte le azioni non sempre-associabile (chiamato anche *temporanei* azioni) più avanti in questo argomento.</span><span class="sxs-lookup"><span data-stu-id="b742b-161">I'll talk about not-always-bindable actions (also called *transient* actions) later in this topic.</span></span>

## <a name="invoking-the-action"></a><span data-ttu-id="b742b-162">Chiamata dell'azione</span><span class="sxs-lookup"><span data-stu-id="b742b-162">Invoking the Action</span></span>

<span data-ttu-id="b742b-163">Ora vediamo come un client potrebbe richiamare questa azione.</span><span class="sxs-lookup"><span data-stu-id="b742b-163">Now let's see how a client would invoke this action.</span></span> <span data-ttu-id="b742b-164">Si supponga che il client vuole assegnare una classificazione compresa tra 2 al prodotto con ID = 4.</span><span class="sxs-lookup"><span data-stu-id="b742b-164">Suppose the client wants to give a rating of 2 to the product with ID = 4.</span></span> <span data-ttu-id="b742b-165">Ecco un messaggio di richiesta di esempio, usando il formato JSON per il corpo della richiesta:</span><span class="sxs-lookup"><span data-stu-id="b742b-165">Here is an example request message, using JSON format for the request body:</span></span>

[!code-console[Main](odata-actions/samples/sample9.cmd)]

<span data-ttu-id="b742b-166">Ecco il messaggio di risposta:</span><span class="sxs-lookup"><span data-stu-id="b742b-166">Here is the response message:</span></span>

[!code-console[Main](odata-actions/samples/sample10.cmd)]

## <a name="binding-an-action-to-an-entity-set"></a><span data-ttu-id="b742b-167">Associazione di un'azione a un Set di entità</span><span class="sxs-lookup"><span data-stu-id="b742b-167">Binding an Action to an Entity Set</span></span>

<span data-ttu-id="b742b-168">Nell'esempio precedente, l'azione è associata a una singola entità: il client classifica un singolo prodotto.</span><span class="sxs-lookup"><span data-stu-id="b742b-168">In the previous example, the action is bound to a single entity: The client rates a single product.</span></span> <span data-ttu-id="b742b-169">È anche possibile associare un'azione per una raccolta di entità.</span><span class="sxs-lookup"><span data-stu-id="b742b-169">You can also bind an action to a collection of entities.</span></span> <span data-ttu-id="b742b-170">Solo apportare le modifiche seguenti:</span><span class="sxs-lookup"><span data-stu-id="b742b-170">Just make the following changes:</span></span>

<span data-ttu-id="b742b-171">In EDM, aggiungere l'azione per l'entità **raccolta** proprietà.</span><span class="sxs-lookup"><span data-stu-id="b742b-171">In the EDM, add the action to the entity's **Collection** property.</span></span>

[!code-csharp[Main](odata-actions/samples/sample11.cs?highlight=1)]

<span data-ttu-id="b742b-172">Nel metodo del controller, omettere il *chiave* parametro.</span><span class="sxs-lookup"><span data-stu-id="b742b-172">In the controller method, omit the *key* parameter.</span></span>

[!code-csharp[Main](odata-actions/samples/sample12.cs)]

<span data-ttu-id="b742b-173">A questo punto il client richiama l'azione sul set di entità Products:</span><span class="sxs-lookup"><span data-stu-id="b742b-173">Now the client invokes the action on the Products entity set:</span></span>

[!code-console[Main](odata-actions/samples/sample13.cmd)]

## <a name="actions-with-collection-parameters"></a><span data-ttu-id="b742b-174">Azioni con i parametri della raccolta</span><span class="sxs-lookup"><span data-stu-id="b742b-174">Actions with Collection Parameters</span></span>

<span data-ttu-id="b742b-175">Le azioni possono includere i parametri che accettano una raccolta di valori.</span><span class="sxs-lookup"><span data-stu-id="b742b-175">Actions can have parameters that take a collection of values.</span></span> <span data-ttu-id="b742b-176">In EDM, utilizzare **CollectionParameter&lt;T&gt;**  per dichiarare il parametro.</span><span class="sxs-lookup"><span data-stu-id="b742b-176">In the EDM, use **CollectionParameter&lt;T&gt;** to declare the parameter.</span></span>

[!code-csharp[Main](odata-actions/samples/sample14.cs)]

<span data-ttu-id="b742b-177">Questo codice dichiara un parametro denominato "Simboli valutazione" che accetta una raccolta di **int** valori.</span><span class="sxs-lookup"><span data-stu-id="b742b-177">This declares a parameter named "Ratings" that takes a collection of **int** values.</span></span> <span data-ttu-id="b742b-178">Nel metodo del controller, è comunque ottenere il valore del parametro dal **ODataActionParameters** oggetti, ma ora il valore è un **ICollection&lt;int&gt;**  valore:</span><span class="sxs-lookup"><span data-stu-id="b742b-178">In the controller method, you still get the parameter value from the **ODataActionParameters** object, but now the value is an **ICollection&lt;int&gt;** value:</span></span>

[!code-csharp[Main](odata-actions/samples/sample15.cs)]

## <a name="transient-actions"></a><span data-ttu-id="b742b-179">Azioni temporanee</span><span class="sxs-lookup"><span data-stu-id="b742b-179">Transient Actions</span></span>

<span data-ttu-id="b742b-180">Nell'esempio "RateProduct", gli utenti possono sempre frequenza un prodotto, in modo che l'azione è sempre disponibile.</span><span class="sxs-lookup"><span data-stu-id="b742b-180">In the "RateProduct" example, users can always rate a product, so the action is always available.</span></span> <span data-ttu-id="b742b-181">Tuttavia, alcune azioni dipendono dallo stato dell'entità.</span><span class="sxs-lookup"><span data-stu-id="b742b-181">But some actions depend on the state of the entity.</span></span> <span data-ttu-id="b742b-182">Ad esempio, in un servizio di noleggio di video, l'azione "Checkpoint" non è sempre disponibile.</span><span class="sxs-lookup"><span data-stu-id="b742b-182">For example, in a video rental service, the "CheckOut" action is not always available.</span></span> <span data-ttu-id="b742b-183">(Seconda se una copia di tale video è disponibile.) Questo tipo di azione viene chiamato una *temporanei* azione.</span><span class="sxs-lookup"><span data-stu-id="b742b-183">(It depends whether a copy of that video is available.) This type of action is called a *transient* action.</span></span>

<span data-ttu-id="b742b-184">Nei metadati del servizio, è un'azione temporanea **IsAlwaysBindable** uguale a false.</span><span class="sxs-lookup"><span data-stu-id="b742b-184">In the service metadata, a transient action has **IsAlwaysBindable** equal to false.</span></span> <span data-ttu-id="b742b-185">Che è effettivamente il valore predefinito, in modo che i metadati avrà un aspetto simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="b742b-185">That's actually the default value, so the metadata will look like this:</span></span>

[!code-xml[Main](odata-actions/samples/sample16.xml)]

<span data-ttu-id="b742b-186">Ecco perché questo è importante: se un'azione è temporanea, il server deve indicare al client quando l'azione è disponibile.</span><span class="sxs-lookup"><span data-stu-id="b742b-186">Here's why this matters: If an action is transient, the server needs to tell the client when the action is available.</span></span> <span data-ttu-id="b742b-187">Ciò avviene inserendo un collegamento all'azione nell'entità.</span><span class="sxs-lookup"><span data-stu-id="b742b-187">It does this by including a link to the action in the entity.</span></span> <span data-ttu-id="b742b-188">Di seguito è riportato un esempio per un'entità film:</span><span class="sxs-lookup"><span data-stu-id="b742b-188">Here is an example for a Movie entity:</span></span>

[!code-console[Main](odata-actions/samples/sample17.cmd)]

<span data-ttu-id="b742b-189">La proprietà "#CheckOut" contiene un collegamento all'azione di estrazione.</span><span class="sxs-lookup"><span data-stu-id="b742b-189">The "#CheckOut" property contains a link to the CheckOut action.</span></span> <span data-ttu-id="b742b-190">Se l'azione non è disponibile, il server viene omesso il collegamento.</span><span class="sxs-lookup"><span data-stu-id="b742b-190">If the action is not available, the server omits the link.</span></span>

<span data-ttu-id="b742b-191">Per dichiarare un'azione temporanea in EDM, chiamare il **TransientAction** metodo:</span><span class="sxs-lookup"><span data-stu-id="b742b-191">To declare a transient action in the EDM, call the **TransientAction** method:</span></span>

[!code-csharp[Main](odata-actions/samples/sample18.cs)]

<span data-ttu-id="b742b-192">Inoltre, è necessario fornire una funzione che restituisce un collegamento all'azione per una determinata entità.</span><span class="sxs-lookup"><span data-stu-id="b742b-192">Also, you must provide a function that returns an action link for a given entity.</span></span> <span data-ttu-id="b742b-193">Impostare questa funzione chiamando **HasActionLink**.</span><span class="sxs-lookup"><span data-stu-id="b742b-193">Set this function by calling **HasActionLink**.</span></span> <span data-ttu-id="b742b-194">È possibile scrivere la funzione come un'espressione lambda:</span><span class="sxs-lookup"><span data-stu-id="b742b-194">You can write the function as a lambda expression:</span></span>

[!code-csharp[Main](odata-actions/samples/sample19.cs)]

<span data-ttu-id="b742b-195">Se l'azione è disponibile, l'espressione lambda restituisce un collegamento all'azione.</span><span class="sxs-lookup"><span data-stu-id="b742b-195">If the action is available, the lambda expression returns a link to the action.</span></span> <span data-ttu-id="b742b-196">Il serializzatore OData include questo collegamento durante la serializzazione di entità.</span><span class="sxs-lookup"><span data-stu-id="b742b-196">The OData serializer includes this link when it serializes the entity.</span></span> <span data-ttu-id="b742b-197">Quando l'azione non è disponibile, la funzione restituisce `null`.</span><span class="sxs-lookup"><span data-stu-id="b742b-197">When the action is not available, the function returns `null`.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b742b-198">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="b742b-198">Additional Resources</span></span>

[<span data-ttu-id="b742b-199">Esempio di azioni OData</span><span class="sxs-lookup"><span data-stu-id="b742b-199">OData Actions Sample</span></span>](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v3/ODataActionsSample/)
