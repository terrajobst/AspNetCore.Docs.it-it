---
uid: web-api/overview/odata-support-in-aspnet-web-api/supporting-odata-query-options
title: Che supportano le opzioni di Query OData nell'API Web ASP.NET 2 | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 02/04/2013
ms.assetid: 50e6e62b-e72e-4a29-8293-4b67377bd21f
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/supporting-odata-query-options
msc.type: authoredcontent
ms.openlocfilehash: 029a791074a5ab88f1c6d83311e88fc9f726fe6f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37814941"
---
<a name="supporting-odata-query-options-in-aspnet-web-api-2"></a><span data-ttu-id="2c134-102">Supportare opzioni di Query OData nell'API Web ASP.NET 2</span><span class="sxs-lookup"><span data-stu-id="2c134-102">Supporting OData Query Options in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="2c134-103">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="2c134-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="2c134-104">OData definisce parametri che possono essere usati per modificare una query OData.</span><span class="sxs-lookup"><span data-stu-id="2c134-104">OData defines parameters that can be used to modify an OData query.</span></span> <span data-ttu-id="2c134-105">Il client invia questi parametri nella stringa di query dell'URI della richiesta.</span><span class="sxs-lookup"><span data-stu-id="2c134-105">The client sends these parameters in the query string of the request URI.</span></span> <span data-ttu-id="2c134-106">Per ordinare i risultati, ad esempio, un client usa il parametro $orderby:</span><span class="sxs-lookup"><span data-stu-id="2c134-106">For example, to sort the results, a client uses the $orderby parameter:</span></span>

`http://localhost/Products?$orderby=Name`

<span data-ttu-id="2c134-107">La specifica OData chiama questi parametri *opzioni query*.</span><span class="sxs-lookup"><span data-stu-id="2c134-107">The OData specification calls these parameters *query options*.</span></span> <span data-ttu-id="2c134-108">È possibile abilitare le opzioni di query OData per i controller API Web nel progetto &#8212; il controller non è necessario essere un endpoint OData.</span><span class="sxs-lookup"><span data-stu-id="2c134-108">You can enable OData query options for any Web API controller in your project &#8212; the controller does not need to be an OData endpoint.</span></span> <span data-ttu-id="2c134-109">Ciò offre un modo pratico per aggiungere funzionalità come filtro e ordinamento a qualsiasi applicazione API Web.</span><span class="sxs-lookup"><span data-stu-id="2c134-109">This gives you a convenient way to add features such as filtering and sorting to any Web API application.</span></span>

<span data-ttu-id="2c134-110">Prima di abilitare le opzioni di query, leggere l'argomento [OData Security Guidance](odata-security-guidance.md).</span><span class="sxs-lookup"><span data-stu-id="2c134-110">Before enabling query options, please read the topic [OData Security Guidance](odata-security-guidance.md).</span></span>

- [<span data-ttu-id="2c134-111">Abilitazione delle opzioni di Query OData</span><span class="sxs-lookup"><span data-stu-id="2c134-111">Enabling OData Query Options</span></span>](#enable)
- [<span data-ttu-id="2c134-112">Query di esempio</span><span class="sxs-lookup"><span data-stu-id="2c134-112">Example Queries</span></span>](#examples)
- [<span data-ttu-id="2c134-113">Paging guidato da server</span><span class="sxs-lookup"><span data-stu-id="2c134-113">Server-Driven Paging</span></span>](#server-paging)
- [<span data-ttu-id="2c134-114">Limitando le opzioni di Query</span><span class="sxs-lookup"><span data-stu-id="2c134-114">Limiting the Query Options</span></span>](#limiting_query_options)
- [<span data-ttu-id="2c134-115">Richiamare direttamente le opzioni di Query</span><span class="sxs-lookup"><span data-stu-id="2c134-115">Invoking Query Options Directly</span></span>](#ODataQueryOptions)
- [<span data-ttu-id="2c134-116">Convalida query</span><span class="sxs-lookup"><span data-stu-id="2c134-116">Query Validation</span></span>](#query-validation)

<a id="enable"></a>
## <a name="enabling-odata-query-options"></a><span data-ttu-id="2c134-117">Abilitazione delle opzioni di Query OData</span><span class="sxs-lookup"><span data-stu-id="2c134-117">Enabling OData Query Options</span></span>

<span data-ttu-id="2c134-118">API Web supporta le opzioni di query OData seguenti:</span><span class="sxs-lookup"><span data-stu-id="2c134-118">Web API supports the following OData query options:</span></span>

| <span data-ttu-id="2c134-119">Opzione</span><span class="sxs-lookup"><span data-stu-id="2c134-119">Option</span></span> | <span data-ttu-id="2c134-120">Descrizione</span><span class="sxs-lookup"><span data-stu-id="2c134-120">Description</span></span> |
| --- | --- |
| <span data-ttu-id="2c134-121">$expand</span><span class="sxs-lookup"><span data-stu-id="2c134-121">$expand</span></span> | <span data-ttu-id="2c134-122">Espande le entità correlate inline.</span><span class="sxs-lookup"><span data-stu-id="2c134-122">Expands related entities inline.</span></span> |
| <span data-ttu-id="2c134-123">$filter</span><span class="sxs-lookup"><span data-stu-id="2c134-123">$filter</span></span> | <span data-ttu-id="2c134-124">Filtra i risultati, in base a una condizione booleana.</span><span class="sxs-lookup"><span data-stu-id="2c134-124">Filters the results, based on a Boolean condition.</span></span> |
| <span data-ttu-id="2c134-125">$inlinecount</span><span class="sxs-lookup"><span data-stu-id="2c134-125">$inlinecount</span></span> | <span data-ttu-id="2c134-126">Indica al server di includere il conteggio totale di entità corrispondenti nella risposta.</span><span class="sxs-lookup"><span data-stu-id="2c134-126">Tells the server to include the total count of matching entities in the response.</span></span> <span data-ttu-id="2c134-127">(Utile per il paging del lato server).</span><span class="sxs-lookup"><span data-stu-id="2c134-127">(Useful for server-side paging.)</span></span> |
| <span data-ttu-id="2c134-128">$orderby</span><span class="sxs-lookup"><span data-stu-id="2c134-128">$orderby</span></span> | <span data-ttu-id="2c134-129">Ordina i risultati.</span><span class="sxs-lookup"><span data-stu-id="2c134-129">Sorts the results.</span></span> |
| <span data-ttu-id="2c134-130">$select</span><span class="sxs-lookup"><span data-stu-id="2c134-130">$select</span></span> | <span data-ttu-id="2c134-131">Seleziona le proprietà da includere nella risposta.</span><span class="sxs-lookup"><span data-stu-id="2c134-131">Selects which properties to include in the response.</span></span> |
| <span data-ttu-id="2c134-132">$skip</span><span class="sxs-lookup"><span data-stu-id="2c134-132">$skip</span></span> | <span data-ttu-id="2c134-133">Ignora i primi n risultati.</span><span class="sxs-lookup"><span data-stu-id="2c134-133">Skips the first n results.</span></span> |
| <span data-ttu-id="2c134-134">$top</span><span class="sxs-lookup"><span data-stu-id="2c134-134">$top</span></span> | <span data-ttu-id="2c134-135">Restituisce solo i primi n risultati.</span><span class="sxs-lookup"><span data-stu-id="2c134-135">Returns only the first n the results.</span></span> |

<span data-ttu-id="2c134-136">Per usare le opzioni di query OData, è necessario consentire in modo esplicito.</span><span class="sxs-lookup"><span data-stu-id="2c134-136">To use OData query options, you must enable them explicitly.</span></span> <span data-ttu-id="2c134-137">È possibile abilitarli a livello globale per l'intera applicazione o abilitarli per controller o azioni specifiche specifiche.</span><span class="sxs-lookup"><span data-stu-id="2c134-137">You can enable them globally for the entire application, or enable them for specific controllers or specific actions.</span></span>

<span data-ttu-id="2c134-138">Per abilitare le opzioni di query OData a livello globale, chiamare **EnableQuerySupport** nel **HttpConfiguration** classe all'avvio:</span><span class="sxs-lookup"><span data-stu-id="2c134-138">To enable OData query options globally, call **EnableQuerySupport** on the **HttpConfiguration** class at startup:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample1.cs)]

<span data-ttu-id="2c134-139">Il **EnableQuerySupport** metodo abilita le opzioni di query a livello globale per qualsiasi azione del controller che restituisce un' **IQueryable** tipo.</span><span class="sxs-lookup"><span data-stu-id="2c134-139">The **EnableQuerySupport** method enables query options globally for any controller action that returns an **IQueryable** type.</span></span> <span data-ttu-id="2c134-140">Se non si vuole abilitate per l'intera applicazione opzioni di query, è possibile abilitarli per le azioni del controller specifico mediante l'aggiunta del **[Queryable]** attributo al metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="2c134-140">If you don't want query options enabled for the entire application, you can enable them for specific controller actions by adding the **[Queryable]** attribute to the action method.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample2.cs)]

<a id="examples"></a>
## <a name="example-queries"></a><span data-ttu-id="2c134-141">Query di esempio</span><span class="sxs-lookup"><span data-stu-id="2c134-141">Example Queries</span></span>

<span data-ttu-id="2c134-142">Questa sezione illustra i tipi di query che sono possibili utilizzando le opzioni di query OData.</span><span class="sxs-lookup"><span data-stu-id="2c134-142">This section shows the types of queries that are possible using the OData query options.</span></span> <span data-ttu-id="2c134-143">Per informazioni dettagliate sulle opzioni di query, fare riferimento alla documentazione di OData [www.odata.org](http://www.odata.org/).</span><span class="sxs-lookup"><span data-stu-id="2c134-143">For specific details about the query options, refer to the OData documentation at [www.odata.org](http://www.odata.org/).</span></span>

<span data-ttu-id="2c134-144">Per informazioni su $espandere e $select, vedere [Usa $select, $expand, $value in ASP.NET Web API OData e](using-select-expand-and-value.md).</span><span class="sxs-lookup"><span data-stu-id="2c134-144">For information about $expand and $select, see [Using $select, $expand, and $value in ASP.NET Web API OData](using-select-expand-and-value.md).</span></span>

<span data-ttu-id="2c134-145">**Paging basato su client**</span><span class="sxs-lookup"><span data-stu-id="2c134-145">**Client-Driven Paging**</span></span>

<span data-ttu-id="2c134-146">Per i set di entità di grandi dimensioni, il client potrebbe voler limitare il numero di risultati.</span><span class="sxs-lookup"><span data-stu-id="2c134-146">For large entity sets, the client might want to limit the number of results.</span></span> <span data-ttu-id="2c134-147">Ad esempio, un client potrebbe mostrare 10 voci alla volta, con collegamenti "Avanti" per accedere alla pagina successiva di risultati.</span><span class="sxs-lookup"><span data-stu-id="2c134-147">For example, a client might show 10 entries at a time, with "next" links to get the next page of results.</span></span> <span data-ttu-id="2c134-148">A tale scopo, il client usa le opzioni $top e $skip.</span><span class="sxs-lookup"><span data-stu-id="2c134-148">To do this, the client uses the $top and $skip options.</span></span>

`http://localhost/Products?$top=10&$skip=20`

<span data-ttu-id="2c134-149">L'opzione $top fornisce il numero massimo di voci da restituire, mentre l'opzione $skip fornisce il numero di voci da ignorare.</span><span class="sxs-lookup"><span data-stu-id="2c134-149">The $top option gives the maximum number of entries to return, and the $skip option gives the number of entries to skip.</span></span> <span data-ttu-id="2c134-150">Nell'esempio precedente recupera le voci 21 e 30.</span><span class="sxs-lookup"><span data-stu-id="2c134-150">The previous example fetches entries 21 through 30.</span></span>

<span data-ttu-id="2c134-151">**Applicazione di filtri**</span><span class="sxs-lookup"><span data-stu-id="2c134-151">**Filtering**</span></span>

<span data-ttu-id="2c134-152">L'opzione $filter consente a un client di filtrare i risultati applicando un'espressione booleana.</span><span class="sxs-lookup"><span data-stu-id="2c134-152">The $filter option lets a client filter the results by applying a Boolean expression.</span></span> <span data-ttu-id="2c134-153">Le espressioni di filtro sono molto potenti; includono gli operatori logici e aritmetici, funzioni stringa e funzioni di Data.</span><span class="sxs-lookup"><span data-stu-id="2c134-153">The filter expressions are quite powerful; they include logical and arithmetic operators, string functions, and date functions.</span></span>

| <span data-ttu-id="2c134-154">Restituire tutti i prodotti con la categoria è uguale a "Toys".</span><span class="sxs-lookup"><span data-stu-id="2c134-154">Return all products with category equal to "Toys".</span></span> | <span data-ttu-id="2c134-155">`http://localhost/Products?$filter=Category` EQ 'Toys'</span><span class="sxs-lookup"><span data-stu-id="2c134-155">`http://localhost/Products?$filter=Category` eq 'Toys'</span></span> |
| --- | --- |
| <span data-ttu-id="2c134-156">Restituire tutti i prodotti con prezzo minore di 10.</span><span class="sxs-lookup"><span data-stu-id="2c134-156">Return all products with price less than 10.</span></span> | <span data-ttu-id="2c134-157">`http://localhost/Products?$filter=Price` lt 10</span><span class="sxs-lookup"><span data-stu-id="2c134-157">`http://localhost/Products?$filter=Price` lt 10</span></span> |
| <span data-ttu-id="2c134-158">Gli operatori logici: restituire tutti i prodotti in cui price > = 5 e il prezzo < = 15.</span><span class="sxs-lookup"><span data-stu-id="2c134-158">Logical operators: Return all products where price >= 5 and price <= 15.</span></span> | <span data-ttu-id="2c134-159">`http://localhost/Products?$filter=Price` Ge 5 e prezzo le 15</span><span class="sxs-lookup"><span data-stu-id="2c134-159">`http://localhost/Products?$filter=Price` ge 5 and Price le 15</span></span> |
| <span data-ttu-id="2c134-160">Funzioni stringa: restituisca tutti i prodotti con "zz" nel nome.</span><span class="sxs-lookup"><span data-stu-id="2c134-160">String functions: Return all products with "zz" in the name.</span></span> | `http://localhost/Products?$filter=substringof('zz',Name)` |
| <span data-ttu-id="2c134-161">Le funzioni di data: restituisce tutti i prodotti con ReleaseDate dopo 2005.</span><span class="sxs-lookup"><span data-stu-id="2c134-161">Date functions: Return all products with ReleaseDate after 2005.</span></span> | <span data-ttu-id="2c134-162">`http://localhost/Products?$filter=year(ReleaseDate)` gt 2005</span><span class="sxs-lookup"><span data-stu-id="2c134-162">`http://localhost/Products?$filter=year(ReleaseDate)` gt 2005</span></span> |

<span data-ttu-id="2c134-163">**L'ordinamento**</span><span class="sxs-lookup"><span data-stu-id="2c134-163">**Sorting**</span></span>

<span data-ttu-id="2c134-164">Per ordinare i risultati, utilizzare i filtro $orderby.</span><span class="sxs-lookup"><span data-stu-id="2c134-164">To sort the results, use the $orderby filter.</span></span>

| <span data-ttu-id="2c134-165">Ordinamento in base al prezzo.</span><span class="sxs-lookup"><span data-stu-id="2c134-165">Sort by price.</span></span> | `http://localhost/Products?$orderby=Price` |
| --- | --- |
| <span data-ttu-id="2c134-166">Ordinamento in base al prezzo in senso decrescente (più alto al più basso).</span><span class="sxs-lookup"><span data-stu-id="2c134-166">Sort by price in descending order (highest to lowest).</span></span> | `http://localhost/Products?$orderby=Price desc` |
| <span data-ttu-id="2c134-167">Ordinare per categoria, quindi ordinare in base al prezzo in senso decrescente all'interno delle categorie.</span><span class="sxs-lookup"><span data-stu-id="2c134-167">Sort by category, then sort by price in descending order within categories.</span></span> | `http://localhost/odata/Products?$orderby=Category,Price desc` |

<a id="server-paging"></a>
## <a name="server-driven-paging"></a><span data-ttu-id="2c134-168">Paging guidato da server</span><span class="sxs-lookup"><span data-stu-id="2c134-168">Server-Driven Paging</span></span>

<span data-ttu-id="2c134-169">Se il database contiene milioni di record, non si desidera inviare loro in un payload.</span><span class="sxs-lookup"><span data-stu-id="2c134-169">If your database contains millions of records, you don't want to send them all in one payload.</span></span> <span data-ttu-id="2c134-170">Per evitare questo problema, il server può limitare il numero di voci che invia in un'unica risposta.</span><span class="sxs-lookup"><span data-stu-id="2c134-170">To prevent this, the server can limit the number of entries that it sends in a single response.</span></span> <span data-ttu-id="2c134-171">Per abilitare il paging del server, impostare il **PageSize** proprietà di **Queryable** attributo.</span><span class="sxs-lookup"><span data-stu-id="2c134-171">To enable server paging, set the **PageSize** property in the **Queryable** attribute.</span></span> <span data-ttu-id="2c134-172">Il valore è il numero massimo di voci da restituire.</span><span class="sxs-lookup"><span data-stu-id="2c134-172">The value is the maximum number of entries to return.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample3.cs)]

<span data-ttu-id="2c134-173">Se il controller restituisce formato OData, il corpo della risposta conterrà un collegamento alla pagina successiva di dati:</span><span class="sxs-lookup"><span data-stu-id="2c134-173">If your controller returns OData format, the response body will contain a link to the next page of data:</span></span>

[!code-json[Main](supporting-odata-query-options/samples/sample4.json?highlight=8)]

<span data-ttu-id="2c134-174">Il client può utilizzare questo collegamento per recuperare la pagina successiva.</span><span class="sxs-lookup"><span data-stu-id="2c134-174">The client can use this link to fetch the next page.</span></span> <span data-ttu-id="2c134-175">Per individuare il numero totale di voci del set di risultati, il client può impostare l'opzione di query $inlinecount con il valore "allpages".</span><span class="sxs-lookup"><span data-stu-id="2c134-175">To learn the total number of entries in the result set, the client can set the $inlinecount query option with the value "allpages".</span></span>

`http://localhost/Products?$inlinecount=allpages`

<span data-ttu-id="2c134-176">Il valore "allpages" indica al server di includere il conteggio totale nella risposta:</span><span class="sxs-lookup"><span data-stu-id="2c134-176">The value "allpages" tells the server to include the total count in the response:</span></span>

[!code-json[Main](supporting-odata-query-options/samples/sample5.json?highlight=3)]

> [!NOTE]
> <span data-ttu-id="2c134-177">I collegamenti alla pagina successiva e conteggio inline richiedono entrambi il formato OData.</span><span class="sxs-lookup"><span data-stu-id="2c134-177">Next-page links and inline count both require OData format.</span></span> <span data-ttu-id="2c134-178">Il motivo è che OData definisce i campi speciali nel corpo della risposta per contenere il collegamento e il conteggio.</span><span class="sxs-lookup"><span data-stu-id="2c134-178">The reason is that OData defines special fields in the response body to hold the link and count.</span></span>


<span data-ttu-id="2c134-179">Per i formati non OData, è comunque possibile supportare conteggio inline e i collegamenti di pagina successiva, includendo i risultati della query in una **PageResult&lt;T&gt;**  oggetto.</span><span class="sxs-lookup"><span data-stu-id="2c134-179">For non-OData formats, it is still possible to support next-page links and inline count, by wrapping the query results in a **PageResult&lt;T&gt;** object.</span></span> <span data-ttu-id="2c134-180">Tuttavia, è necessario un po' più codice.</span><span class="sxs-lookup"><span data-stu-id="2c134-180">However, it requires a bit more code.</span></span> <span data-ttu-id="2c134-181">Ecco un esempio:</span><span class="sxs-lookup"><span data-stu-id="2c134-181">Here is an example:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample6.cs)]

<span data-ttu-id="2c134-182">Di seguito è riportato un esempio di risposta JSON:</span><span class="sxs-lookup"><span data-stu-id="2c134-182">Here is an example JSON response:</span></span>

[!code-json[Main](supporting-odata-query-options/samples/sample7.json)]

<a id="limiting_query_options"></a>
## <a name="limiting-the-query-options"></a><span data-ttu-id="2c134-183">Limitando le opzioni di Query</span><span class="sxs-lookup"><span data-stu-id="2c134-183">Limiting the Query Options</span></span>

<span data-ttu-id="2c134-184">Le opzioni di query restituito al client un ampio controllo sulla query che viene eseguita nel server.</span><span class="sxs-lookup"><span data-stu-id="2c134-184">The query options give the client a lot of control over the query that is run on the server.</span></span> <span data-ttu-id="2c134-185">In alcuni casi, si potrebbe voler limitare le opzioni disponibili per motivi di sicurezza o di prestazioni.</span><span class="sxs-lookup"><span data-stu-id="2c134-185">In some cases, you might want to limit the available options for security or performance reasons.</span></span> <span data-ttu-id="2c134-186">Il **[Queryable]** attributo ha alcuni incorporati nelle proprietà per questo oggetto.</span><span class="sxs-lookup"><span data-stu-id="2c134-186">The **[Queryable]** attribute has some built in properties for this.</span></span> <span data-ttu-id="2c134-187">Ecco alcuni esempi.</span><span class="sxs-lookup"><span data-stu-id="2c134-187">Here are some examples.</span></span>

<span data-ttu-id="2c134-188">Consenti solo $skip e $top, per supportare il paging e nient'altro:</span><span class="sxs-lookup"><span data-stu-id="2c134-188">Allow only $skip and $top, to support paging and nothing else:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample8.cs)]

<span data-ttu-id="2c134-189">Consenti ordinamento solo per determinate proprietà evitare che l'ordinamento in base a proprietà che non vengono indicizzate nel database:</span><span class="sxs-lookup"><span data-stu-id="2c134-189">Allow ordering only by certain properties, to prevent sorting on properties that are not indexed in the database:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample9.cs)]

<span data-ttu-id="2c134-190">Consenti la funzione logica "eq" ma non altre funzioni logiche:</span><span class="sxs-lookup"><span data-stu-id="2c134-190">Allow the "eq" logical function but no other logical functions:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample10.cs)]

<span data-ttu-id="2c134-191">Non consentire tutti gli operatori aritmetici:</span><span class="sxs-lookup"><span data-stu-id="2c134-191">Do not allow any arithmetic operators:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample11.cs)]

<span data-ttu-id="2c134-192">È possibile limitare le opzioni a livello globale costruendo un **QueryableAttribute** istanza e passarlo al **EnableQuerySupport** funzione:</span><span class="sxs-lookup"><span data-stu-id="2c134-192">You can restrict options globally by constructing a **QueryableAttribute** instance and passing it to the **EnableQuerySupport** function:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample12.cs)]

<a id="ODataQueryOptions"></a>
## <a name="invoking-query-options-directly"></a><span data-ttu-id="2c134-193">Richiamare direttamente le opzioni di Query</span><span class="sxs-lookup"><span data-stu-id="2c134-193">Invoking Query Options Directly</span></span>

<span data-ttu-id="2c134-194">Invece di usare la **[Queryable]** attributo, è possibile richiamare le opzioni di query direttamente nel controller di.</span><span class="sxs-lookup"><span data-stu-id="2c134-194">Instead of using the **[Queryable]** attribute, you can invoke the query options directly in your controller.</span></span> <span data-ttu-id="2c134-195">A tale scopo, aggiungere un' **ODataQueryOptions** parametro al metodo del controller.</span><span class="sxs-lookup"><span data-stu-id="2c134-195">To do so, add an **ODataQueryOptions** parameter to the controller method.</span></span> <span data-ttu-id="2c134-196">In questo caso, non è necessario il **[Queryable]** attributo.</span><span class="sxs-lookup"><span data-stu-id="2c134-196">In this case, you don't need the **[Queryable]** attribute.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample13.cs)]

<span data-ttu-id="2c134-197">API Web popola la **ODataQueryOptions** dall'URI stringa di query.</span><span class="sxs-lookup"><span data-stu-id="2c134-197">Web API populates the **ODataQueryOptions** from the URI query string.</span></span> <span data-ttu-id="2c134-198">Per applicare la query, passare un **IQueryable** per il **ApplyTo** (metodo).</span><span class="sxs-lookup"><span data-stu-id="2c134-198">To apply the query, pass an **IQueryable** to the **ApplyTo** method.</span></span> <span data-ttu-id="2c134-199">Il metodo restituisce un'altra **IQueryable**.</span><span class="sxs-lookup"><span data-stu-id="2c134-199">The method returns another **IQueryable**.</span></span>

<span data-ttu-id="2c134-200">Per scenari avanzati, se non è un **IQueryable** provider di query, è possibile esaminare le **ODataQueryOptions** e convertire le opzioni di query in un altro formato.</span><span class="sxs-lookup"><span data-stu-id="2c134-200">For advanced scenarios, if you do not have an **IQueryable** query provider, you can examine the **ODataQueryOptions** and translate the query options into another form.</span></span> <span data-ttu-id="2c134-201">(Ad esempio, vedere di RaghuRam Nadiminti post di blog [le query OData traduzione HQL](https://blogs.msdn.com/b/webdev/archive/2013/02/25/translating-odata-queries-to-hql.aspx), che include anche una [esempio](http://aspnet.codeplex.com/SourceControl/changeset/view/75a56ec99968#Samples/WebApi/NHibernateQueryableSample/Readme.txt).)</span><span class="sxs-lookup"><span data-stu-id="2c134-201">(For example, see RaghuRam Nadiminti's blog post [Translating OData queries to HQL](https://blogs.msdn.com/b/webdev/archive/2013/02/25/translating-odata-queries-to-hql.aspx), which also includes a [sample](http://aspnet.codeplex.com/SourceControl/changeset/view/75a56ec99968#Samples/WebApi/NHibernateQueryableSample/Readme.txt).)</span></span>

<a id="query-validation"></a>
## <a name="query-validation"></a><span data-ttu-id="2c134-202">Convalida query</span><span class="sxs-lookup"><span data-stu-id="2c134-202">Query Validation</span></span>

<span data-ttu-id="2c134-203">Il **[Queryable]** attributo convalida la query prima dell'esecuzione.</span><span class="sxs-lookup"><span data-stu-id="2c134-203">The **[Queryable]** attribute validates the query before executing it.</span></span> <span data-ttu-id="2c134-204">Il passaggio di convalida viene eseguito nel **QueryableAttribute.ValidateQuery** (metodo).</span><span class="sxs-lookup"><span data-stu-id="2c134-204">The validation step is performed in the **QueryableAttribute.ValidateQuery** method.</span></span> <span data-ttu-id="2c134-205">È anche possibile personalizzare il processo di convalida.</span><span class="sxs-lookup"><span data-stu-id="2c134-205">You can also customize the validation process.</span></span>

<span data-ttu-id="2c134-206">Vedere anche [OData Security Guidance](odata-security-guidance.md).</span><span class="sxs-lookup"><span data-stu-id="2c134-206">Also see [OData Security Guidance](odata-security-guidance.md).</span></span>

<span data-ttu-id="2c134-207">In primo luogo, eseguire l'override di uno dei validator le classi che è definito nel **Web.Http.OData.Query.Validators** dello spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="2c134-207">First, override one of the validator classes that is defined in the **Web.Http.OData.Query.Validators** namespace.</span></span> <span data-ttu-id="2c134-208">Ad esempio, la classe di convalida seguente disabilita l'opzione 'desc' per l'opzione $orderby.</span><span class="sxs-lookup"><span data-stu-id="2c134-208">For example, the following validator class disables the 'desc' option for the $orderby option.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample14.cs)]

<span data-ttu-id="2c134-209">Sottoclasse di **[Queryable]** attributo per eseguire l'override di **ValidateQuery** (metodo).</span><span class="sxs-lookup"><span data-stu-id="2c134-209">Subclass the **[Queryable]** attribute to override the **ValidateQuery** method.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample15.cs)]

<span data-ttu-id="2c134-210">Quindi impostare l'attributo personalizzato sia a livello globale o per ogni controller:</span><span class="sxs-lookup"><span data-stu-id="2c134-210">Then set your custom attribute either globally or per-controller:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample16.cs)]

<span data-ttu-id="2c134-211">Se si usa **ODataQueryOptions** direttamente, impostare il validator per le opzioni:</span><span class="sxs-lookup"><span data-stu-id="2c134-211">If you are using **ODataQueryOptions** directly, set the validator on the options:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample17.cs)]
