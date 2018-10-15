---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-security-guidance
title: Guida alla sicurezza per ASP.NET Web API 2 OData | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 02/06/2013
ms.assetid: b91e6424-1544-4747-bd0b-d1f8418c9653
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-security-guidance
msc.type: authoredcontent
ms.openlocfilehash: 4ba53e15dab83368097a58ba4d0d2e46d113d1d2
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/15/2018
ms.locfileid: "49325718"
---
<a name="security-guidance-for-aspnet-web-api-2-odata"></a><span data-ttu-id="1bb42-102">Guida alla sicurezza per ASP.NET Web API 2 OData</span><span class="sxs-lookup"><span data-stu-id="1bb42-102">Security Guidance for ASP.NET Web API 2 OData</span></span>
====================
<span data-ttu-id="1bb42-103">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="1bb42-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="1bb42-104">In questo argomento vengono descritti alcuni dei problemi di sicurezza che è opportuno considerare quando si espone un set di dati tramite OData.</span><span class="sxs-lookup"><span data-stu-id="1bb42-104">This topic describes some of the security issues that you should consider when exposing a dataset through OData.</span></span>

## <a name="edm-security"></a><span data-ttu-id="1bb42-105">Sicurezza EDM</span><span class="sxs-lookup"><span data-stu-id="1bb42-105">EDM Security</span></span>

<span data-ttu-id="1bb42-106">La semantica di query è basata su entity data model (EDM), non i tipi di modello sottostante.</span><span class="sxs-lookup"><span data-stu-id="1bb42-106">The query semantics are based on the entity data model (EDM), not the underlying model types.</span></span> <span data-ttu-id="1bb42-107">È possibile escludere una proprietà dal modello EDM e non sarà visibile alla query.</span><span class="sxs-lookup"><span data-stu-id="1bb42-107">You can exclude a property from the EDM and it will not be visible to the query.</span></span> <span data-ttu-id="1bb42-108">Si supponga, ad esempio, che il modello include un tipo di dipendente con una proprietà di stipendio.</span><span class="sxs-lookup"><span data-stu-id="1bb42-108">For example, suppose your model includes an Employee type with a Salary property.</span></span> <span data-ttu-id="1bb42-109">Si potrebbe voler escludere questa proprietà dal modello EDM per nasconderlo dai client.</span><span class="sxs-lookup"><span data-stu-id="1bb42-109">You might want to exclude this property from the EDM to hide it from clients.</span></span>

<span data-ttu-id="1bb42-110">Esistono due modi per esclude una proprietà da EDM.</span><span class="sxs-lookup"><span data-stu-id="1bb42-110">There are two ways to exlude a property from the EDM.</span></span> <span data-ttu-id="1bb42-111">È possibile impostare il **[IgnoreDataMember]** attributo sulla proprietà nella classe del modello:</span><span class="sxs-lookup"><span data-stu-id="1bb42-111">You can set the **[IgnoreDataMember]** attribute on the property in the model class:</span></span>

[!code-csharp[Main](odata-security-guidance/samples/sample1.cs)]

<span data-ttu-id="1bb42-112">È anche possibile rimuovere la proprietà da EDM a livello di codice:</span><span class="sxs-lookup"><span data-stu-id="1bb42-112">You can also remove the property from the EDM programmatically:</span></span>

[!code-csharp[Main](odata-security-guidance/samples/sample2.cs)]

## <a name="query-security"></a><span data-ttu-id="1bb42-113">Sicurezza query</span><span class="sxs-lookup"><span data-stu-id="1bb42-113">Query Security</span></span>

<span data-ttu-id="1bb42-114">Un client dannoso o Naive potrà essere in grado di costruire una query che accetta un tempo molto lungo per l'esecuzione.</span><span class="sxs-lookup"><span data-stu-id="1bb42-114">A malicious or naive client may be able to construct a query that takes a very long time to execute.</span></span> <span data-ttu-id="1bb42-115">Nel peggiore dei casi ciò può interferire con accesso al servizio.</span><span class="sxs-lookup"><span data-stu-id="1bb42-115">In the worst case this can disrupt access to your service.</span></span>

<span data-ttu-id="1bb42-116">Il **[Queryable]** attributo è un filtro azione che consente di analizzare, convalida e applica la query.</span><span class="sxs-lookup"><span data-stu-id="1bb42-116">The **[Queryable]** attribute is an action filter that parses, validates, and applies the query.</span></span> <span data-ttu-id="1bb42-117">Il filtro converte le opzioni di query in un'espressione LINQ.</span><span class="sxs-lookup"><span data-stu-id="1bb42-117">The filter converts the query options into a LINQ expression.</span></span> <span data-ttu-id="1bb42-118">Quando il controller OData restituisce un **IQueryable** tipo, il **IQueryable** provider LINQ converte l'espressione LINQ in una query.</span><span class="sxs-lookup"><span data-stu-id="1bb42-118">When the OData controller returns an **IQueryable** type, the **IQueryable** LINQ provider converts the LINQ expression into a query.</span></span> <span data-ttu-id="1bb42-119">Di conseguenza, le prestazioni dipendono sul provider LINQ che viene usato e anche caratteristiche specifiche dello schema del set di dati o database.</span><span class="sxs-lookup"><span data-stu-id="1bb42-119">Therefore, performance depends on the LINQ provider that is used, and also on the particular characteristics of your dataset or database schema.</span></span>

<span data-ttu-id="1bb42-120">Per altre informazioni sull'uso di opzioni di query OData nell'API Web ASP.NET, vedere [che supportano le opzioni di Query OData](supporting-odata-query-options.md).</span><span class="sxs-lookup"><span data-stu-id="1bb42-120">For more information about using OData query options in ASP.NET Web API, see [Supporting OData Query Options](supporting-odata-query-options.md).</span></span>

<span data-ttu-id="1bb42-121">Se si sa che tutti i client sono considerati attendibili (ad esempio, in un ambiente aziendale) oppure se il set di dati è di piccole dimensioni, le prestazioni delle query potrebbe non essere un problema.</span><span class="sxs-lookup"><span data-stu-id="1bb42-121">If you know that all clients are trusted (for example, in an enterprise environment), or if your dataset is small, query performance might not be an issue.</span></span> <span data-ttu-id="1bb42-122">In caso contrario, è necessario considerare le indicazioni seguenti.</span><span class="sxs-lookup"><span data-stu-id="1bb42-122">Otherwise, you should consider the following recommendations.</span></span>

- <span data-ttu-id="1bb42-123">Testare il servizio con diverse query e il database di profilo.</span><span class="sxs-lookup"><span data-stu-id="1bb42-123">Test your service with various queries and profile the DB.</span></span>
- <span data-ttu-id="1bb42-124">Attivare il paging guidato da server evitare la restituzione di un set di dati di grandi dimensioni in un'unica query.</span><span class="sxs-lookup"><span data-stu-id="1bb42-124">Enable server-driven paging, to avoid returning a large data set in one query.</span></span> <span data-ttu-id="1bb42-125">Per altre informazioni, vedere [Server-Driven Paging](supporting-odata-query-options.md#server-paging).</span><span class="sxs-lookup"><span data-stu-id="1bb42-125">For more information, see [Server-Driven Paging](supporting-odata-query-options.md#server-paging).</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample3.cs)]
- <span data-ttu-id="1bb42-126">È necessario $filter e $orderby?</span><span class="sxs-lookup"><span data-stu-id="1bb42-126">Do you need $filter and $orderby?</span></span> <span data-ttu-id="1bb42-127">Alcune applicazioni potrebbero consentire a client, suddivisione in pagine, usando $top e $skip, ma disabilitare altre opzioni di query.</span><span class="sxs-lookup"><span data-stu-id="1bb42-127">Some applications might allow client paging, using $top and $skip, but disable the other query options.</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample4.cs)]
- <span data-ttu-id="1bb42-128">È consigliabile limitare $orderby alle proprietà in un indice cluster.</span><span class="sxs-lookup"><span data-stu-id="1bb42-128">Consider restricting $orderby to properties in a clustered index.</span></span> <span data-ttu-id="1bb42-129">L'ordinamento di dati di grandi dimensioni senza un indice cluster è lenta.</span><span class="sxs-lookup"><span data-stu-id="1bb42-129">Sorting large data without a clustered index is slow.</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample5.cs)]
- <span data-ttu-id="1bb42-130">Numero massimo di nodi: la **MaxNodeCount** proprietà sul **[Queryable]** Imposta numero massimo di nodi nell'albero della sintassi $filter è consentito.</span><span class="sxs-lookup"><span data-stu-id="1bb42-130">Maximum node count: The **MaxNodeCount** property on **[Queryable]** sets the maximum number nodes allowed in the $filter syntax tree.</span></span> <span data-ttu-id="1bb42-131">Il valore predefinito è 100, ma è possibile impostare un valore inferiore, poiché un numero elevato di nodi può essere lento da compilare.</span><span class="sxs-lookup"><span data-stu-id="1bb42-131">The default value is 100, but you may want to set a lower value, because a large number of nodes can be slow to compile.</span></span> <span data-ttu-id="1bb42-132">Questo vale in particolare se si usa LINQ to Objects (ad esempio, le query LINQ su una raccolta in memoria, senza l'uso di un provider LINQ intermedio).</span><span class="sxs-lookup"><span data-stu-id="1bb42-132">This is particularly true if you are using LINQ to Objects (i.e., LINQ queries on a collection in memory, without the use of an intermediate LINQ provider).</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample6.cs)]
- <span data-ttu-id="1bb42-133">È consigliabile disabilitare le funzioni All () e Any (), come queste può essere lenta.</span><span class="sxs-lookup"><span data-stu-id="1bb42-133">Consider disabling the any() and all() functions, as these can be slow.</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample7.cs)]
- <span data-ttu-id="1bb42-134">Se qualsiasi proprietà di stringa contengono stringhe di grandi dimensioni&#8212;, ad esempio, una descrizione del prodotto o una voce di blog&#8212;è consigliabile disabilitare le funzioni di stringa.</span><span class="sxs-lookup"><span data-stu-id="1bb42-134">If any string properties contain large strings&#8212;for example, a product description or a blog entry&#8212;consider disabling the string functions.</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample8.cs)]
- <span data-ttu-id="1bb42-135">È consigliabile impedire l'applicazione di filtri alle proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="1bb42-135">Consider disallowing filtering on navigation properties.</span></span> <span data-ttu-id="1bb42-136">Applicazione di filtri alle proprietà di navigazione può comportare un join, che potrebbe risultare lento, a seconda dello schema del database.</span><span class="sxs-lookup"><span data-stu-id="1bb42-136">Filtering on navigation properties can result in a join, which might be slow, depending on your database schema.</span></span> <span data-ttu-id="1bb42-137">Il codice seguente illustra un validator della query che impedisce l'applicazione di filtri alle proprietà di navigazione.</span><span class="sxs-lookup"><span data-stu-id="1bb42-137">The following code shows a query validator that prevents filtering on navigation properties.</span></span> <span data-ttu-id="1bb42-138">Per altre informazioni sulle convalide di query, vedere [convalida Query](supporting-odata-query-options.md#query-validation).</span><span class="sxs-lookup"><span data-stu-id="1bb42-138">For more information about query validators, see [Query Validation](supporting-odata-query-options.md#query-validation).</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample9.cs)]
- <span data-ttu-id="1bb42-139">È consigliabile limitare le query $filter scrivendo un validator personalizzato per il database.</span><span class="sxs-lookup"><span data-stu-id="1bb42-139">Consider restricting $filter queries by writing a validator that is customized for your database.</span></span> <span data-ttu-id="1bb42-140">Si consideri, ad esempio, le due query seguenti:</span><span class="sxs-lookup"><span data-stu-id="1bb42-140">For example, consider these two queries:</span></span> 

  - <span data-ttu-id="1bb42-141">Tutti i film con gli attori il cui cognome inizia con "A".</span><span class="sxs-lookup"><span data-stu-id="1bb42-141">All movies with actors whose last name starts with ‘A'.</span></span>
  - <span data-ttu-id="1bb42-142">Tutti i film rilasciati nel 1994.</span><span class="sxs-lookup"><span data-stu-id="1bb42-142">All movies released in 1994.</span></span>

    <span data-ttu-id="1bb42-143">A meno che non sono indicizzati film da attori, la prima query potrebbero richiedere il motore di database per analizzare l'intero elenco di film.</span><span class="sxs-lookup"><span data-stu-id="1bb42-143">Unless movies are indexed by actors, the first query might require the DB engine to scan the entire list of movies.</span></span> <span data-ttu-id="1bb42-144">Mentre la seconda query può essere accettabile, presumendo un film vengono indicizzati per anno di produzione.</span><span class="sxs-lookup"><span data-stu-id="1bb42-144">Whereas the second query might be acceptable, assuming movies are indexed by release year.</span></span>

    <span data-ttu-id="1bb42-145">Il codice seguente illustra un validator che consente di filtrare le proprietà "ReleaseYear" e "Title", ma non altre proprietà.</span><span class="sxs-lookup"><span data-stu-id="1bb42-145">The following code shows a validator that allows filtering on the "ReleaseYear" and "Title" properties but no other properties.</span></span>

    [!code-csharp[Main](odata-security-guidance/samples/sample10.cs)]
- <span data-ttu-id="1bb42-146">In generale, è possibile usare le funzioni $filter è necessario.</span><span class="sxs-lookup"><span data-stu-id="1bb42-146">In general, consider which $filter functions you need.</span></span> <span data-ttu-id="1bb42-147">Se i client non sono necessario l'espressività di $filter completo, è possibile limitare alcune delle funzioni consentite.</span><span class="sxs-lookup"><span data-stu-id="1bb42-147">If your clients do not need the full expressiveness of $filter, you can limit the allowed functions.</span></span>
