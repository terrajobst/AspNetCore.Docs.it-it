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
<a name="security-guidance-for-aspnet-web-api-2-odata"></a>Guida alla sicurezza per ASP.NET Web API 2 OData
====================
da [Mike Wasson](https://github.com/MikeWasson)

In questo argomento vengono descritti alcuni dei problemi di sicurezza che è opportuno considerare quando si espone un set di dati tramite OData.

## <a name="edm-security"></a>Sicurezza EDM

La semantica di query è basata su entity data model (EDM), non i tipi di modello sottostante. È possibile escludere una proprietà dal modello EDM e non sarà visibile alla query. Si supponga, ad esempio, che il modello include un tipo di dipendente con una proprietà di stipendio. Si potrebbe voler escludere questa proprietà dal modello EDM per nasconderlo dai client.

Esistono due modi per esclude una proprietà da EDM. È possibile impostare il **[IgnoreDataMember]** attributo sulla proprietà nella classe del modello:

[!code-csharp[Main](odata-security-guidance/samples/sample1.cs)]

È anche possibile rimuovere la proprietà da EDM a livello di codice:

[!code-csharp[Main](odata-security-guidance/samples/sample2.cs)]

## <a name="query-security"></a>Sicurezza query

Un client dannoso o Naive potrà essere in grado di costruire una query che accetta un tempo molto lungo per l'esecuzione. Nel peggiore dei casi ciò può interferire con accesso al servizio.

Il **[Queryable]** attributo è un filtro azione che consente di analizzare, convalida e applica la query. Il filtro converte le opzioni di query in un'espressione LINQ. Quando il controller OData restituisce un **IQueryable** tipo, il **IQueryable** provider LINQ converte l'espressione LINQ in una query. Di conseguenza, le prestazioni dipendono sul provider LINQ che viene usato e anche caratteristiche specifiche dello schema del set di dati o database.

Per altre informazioni sull'uso di opzioni di query OData nell'API Web ASP.NET, vedere [che supportano le opzioni di Query OData](supporting-odata-query-options.md).

Se si sa che tutti i client sono considerati attendibili (ad esempio, in un ambiente aziendale) oppure se il set di dati è di piccole dimensioni, le prestazioni delle query potrebbe non essere un problema. In caso contrario, è necessario considerare le indicazioni seguenti.

- Testare il servizio con diverse query e il database di profilo.
- Attivare il paging guidato da server evitare la restituzione di un set di dati di grandi dimensioni in un'unica query. Per altre informazioni, vedere [Server-Driven Paging](supporting-odata-query-options.md#server-paging). 

    [!code-csharp[Main](odata-security-guidance/samples/sample3.cs)]
- È necessario $filter e $orderby? Alcune applicazioni potrebbero consentire a client, suddivisione in pagine, usando $top e $skip, ma disabilitare altre opzioni di query. 

    [!code-csharp[Main](odata-security-guidance/samples/sample4.cs)]
- È consigliabile limitare $orderby alle proprietà in un indice cluster. L'ordinamento di dati di grandi dimensioni senza un indice cluster è lenta. 

    [!code-csharp[Main](odata-security-guidance/samples/sample5.cs)]
- Numero massimo di nodi: la **MaxNodeCount** proprietà sul **[Queryable]** Imposta numero massimo di nodi nell'albero della sintassi $filter è consentito. Il valore predefinito è 100, ma è possibile impostare un valore inferiore, poiché un numero elevato di nodi può essere lento da compilare. Questo vale in particolare se si usa LINQ to Objects (ad esempio, le query LINQ su una raccolta in memoria, senza l'uso di un provider LINQ intermedio). 

    [!code-csharp[Main](odata-security-guidance/samples/sample6.cs)]
- È consigliabile disabilitare le funzioni All () e Any (), come queste può essere lenta. 

    [!code-csharp[Main](odata-security-guidance/samples/sample7.cs)]
- Se qualsiasi proprietà di stringa contengono stringhe di grandi dimensioni&#8212;, ad esempio, una descrizione del prodotto o una voce di blog&#8212;è consigliabile disabilitare le funzioni di stringa. 

    [!code-csharp[Main](odata-security-guidance/samples/sample8.cs)]
- È consigliabile impedire l'applicazione di filtri alle proprietà di navigazione. Applicazione di filtri alle proprietà di navigazione può comportare un join, che potrebbe risultare lento, a seconda dello schema del database. Il codice seguente illustra un validator della query che impedisce l'applicazione di filtri alle proprietà di navigazione. Per altre informazioni sulle convalide di query, vedere [convalida Query](supporting-odata-query-options.md#query-validation). 

    [!code-csharp[Main](odata-security-guidance/samples/sample9.cs)]
- È consigliabile limitare le query $filter scrivendo un validator personalizzato per il database. Si consideri, ad esempio, le due query seguenti: 

  - Tutti i film con gli attori il cui cognome inizia con "A".
  - Tutti i film rilasciati nel 1994.

    A meno che non sono indicizzati film da attori, la prima query potrebbero richiedere il motore di database per analizzare l'intero elenco di film. Mentre la seconda query può essere accettabile, presumendo un film vengono indicizzati per anno di produzione.

    Il codice seguente illustra un validator che consente di filtrare le proprietà "ReleaseYear" e "Title", ma non altre proprietà.

    [!code-csharp[Main](odata-security-guidance/samples/sample10.cs)]
- In generale, è possibile usare le funzioni $filter è necessario. Se i client non sono necessario l'espressività di $filter completo, è possibile limitare alcune delle funzioni consentite.
