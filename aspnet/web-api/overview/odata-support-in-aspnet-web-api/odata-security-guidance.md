---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-security-guidance
title: Indicazioni sulla sicurezza per ASP.NET Web API 2 OData | Documenti Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/06/2013
ms.topic: article
ms.assetid: b91e6424-1544-4747-bd0b-d1f8418c9653
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-security-guidance
msc.type: authoredcontent
ms.openlocfilehash: 41b05f2a2f8247853d8358e6cc1246c8b438a6db
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868708"
---
<a name="security-guidance-for-aspnet-web-api-2-odata"></a>Indicazioni sulla sicurezza per ASP.NET Web API 2 OData
====================
da [Mike Wasson](https://github.com/MikeWasson)

In questo argomento vengono descritti alcuni dei problemi di protezione da prendere in considerazione quando si espone un set di dati tramite OData.

## <a name="edm-security"></a>Sicurezza EDM

La semantica di query è basata su entity data model (EDM), non i tipi di modello sottostante. È possibile escludere una proprietà da EDM e non sarà visibile alla query. Si supponga, ad esempio, che il modello include un tipo dipendente con una proprietà di stipendio. Si potrebbe voler escludere tale proprietà dal modello EDM per nasconderlo dal client.

Esistono due modi per esclude una proprietà da EDM. È possibile impostare il **[IgnoreDataMember]** attributo alla proprietà nella classe di modello:

[!code-csharp[Main](odata-security-guidance/samples/sample1.cs)]

È inoltre possibile rimuovere la proprietà da EDM a livello di codice:

[!code-csharp[Main](odata-security-guidance/samples/sample2.cs)]

## <a name="query-security"></a>Sicurezza delle query

Un client dannoso o naïve potrebbe essere in grado di creare una query per un periodo di tempo per l'esecuzione. Nel peggiore dei casi ciò può interferire con l'accesso al servizio.

Il **[Queryable]** attributo è un filtro azione che analizza, convalida e applica la query. Il filtro converte le opzioni di query in un'espressione LINQ. Quando il controller OData restituisce un **IQueryable** tipo, il **IQueryable** provider LINQ converte l'espressione LINQ in una query. Pertanto, le prestazioni dipendono sul provider LINQ che viene utilizzato, nonché alle caratteristiche dello schema del set di dati o un database specifiche.

Per ulteriori informazioni sull'utilizzo delle opzioni di query OData nell'API Web ASP.NET, vedere [che supporta le opzioni di Query OData](supporting-odata-query-options.md).

Se si è certi che tutti i client sono considerati attendibili (ad esempio, in un ambiente aziendale) o se il set di dati è piccolo, le prestazioni delle query potrebbero non essere un problema. In caso contrario, è necessario considerare quanto segue.

- Testare il servizio con varie query e il database di profilo.
- Abilita il paging basato su server evitare la restituzione di un set di dati di grandi dimensioni in un'unica query. Per ulteriori informazioni, vedere [Server-Driven Paging](supporting-odata-query-options.md#server-paging). 

    [!code-csharp[Main](odata-security-guidance/samples/sample3.cs)]
- È necessario $filter e $orderby? Alcune applicazioni potrebbero consentire il paging, l'utilizzo di $top e $skip di client, ma disabilitare altre opzioni di query. 

    [!code-csharp[Main](odata-security-guidance/samples/sample4.cs)]
- È consigliabile limitare $orderby alle proprietà in un indice cluster. Ordinamento dei dati di grandi dimensioni senza un indice cluster è lenta. 

    [!code-csharp[Main](odata-security-guidance/samples/sample5.cs)]
- Numero massimo di nodo: il **MaxNodeCount** proprietà **[Queryable]** imposta i nodi del numero massimi consentiti nell'albero della sintassi $filter. Il valore predefinito è 100, ma è possibile impostare un valore inferiore, perché un numero elevato di nodi può essere lento per la compilazione. Questo vale in particolare se si usa LINQ to Objects (ad esempio, le query LINQ su una raccolta in memoria, senza l'utilizzo di un provider LINQ intermedio). 

    [!code-csharp[Main](odata-security-guidance/samples/sample6.cs)]
- Provare a disabilitare le funzioni Any () e All (), come possono essere lenti. 

    [!code-csharp[Main](odata-security-guidance/samples/sample7.cs)]
- Se le proprietà di stringa contengono stringhe di grandi dimensioni & #8212for esempio, una descrizione del prodotto o di post di blog & #8212consider disabilitare le funzioni di stringa. 

    [!code-csharp[Main](odata-security-guidance/samples/sample8.cs)]
- Prendere in considerazione impedisce l'applicazione di filtri sulle proprietà di navigazione. Applicazione di filtri sulle proprietà di navigazione può comportare un join, che potrebbe essere lento, a seconda dello schema del database. Il codice seguente viene illustrato un validator della query che impedisce di applicare filtri sulle proprietà di navigazione. Per ulteriori informazioni sulle convalide di query, vedere [la convalida delle Query](supporting-odata-query-options.md#query-validation). 

    [!code-csharp[Main](odata-security-guidance/samples/sample9.cs)]
- È consigliabile limitare le query $filter scrivendo un validator personalizzato per il database. Si consideri, ad esempio, le due query seguenti: 

  - Tutti i filmati con attori il cui cognome inizia con 'A'.
  - Tutti i filmati rilasciati nel 1994.

    A meno che non sono indicizzati a filmati da attori, la prima query potrebbe richiedere il motore di database per analizzare l'intero elenco di film. Mentre la seconda query può essere accettabile, filmati presupponendo che vengono indicizzate per anno di produzione.

    Il codice seguente viene illustrato un validator che consente di filtrare le proprietà "ReleaseYear" e "Title" ma non altre proprietà.

    [!code-csharp[Main](odata-security-guidance/samples/sample10.cs)]
- In generale, le funzioni di $filter è necessario prendere in considerazione. Se i client non richiedono l'espressività completo di $filter, è possibile limitare le funzioni consentite.
