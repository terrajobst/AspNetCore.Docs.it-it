---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-odata-53
title: What ' s New in API Web ASP.NET OData 5.3 | Microsoft Docs
author: microsoft
description: ''
ms.author: riande
ms.date: 09/16/2014
ms.assetid: e39eaa25-83ff-41dc-869d-3818d59a88ae
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-odata-53
msc.type: authoredcontent
ms.openlocfilehash: 20ef263ce7cbaef40c16937eaa91d34239fc2ace
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829288"
---
<a name="whats-new-in-aspnet-web-api-odata-53"></a>What ' s New in API Web ASP.NET OData 5.3
====================
da [Microsoft](https://github.com/microsoft)

Questo argomento descrive cosa sono le novità di ASP.NET Web API OData 5.3.

- [Download](#download)
- [Documentazione](#documentation)
- [Librerie di base di OData](#corelib)
- [Nuove funzionalità](#newf)
- [Problemi noti e modifiche di rilievo](#known-issues)
- [Correzioni di bug](#bug-fixes)
- [API Web ASP.NET OData 5.3.1](#OD)

<a id="download"></a>
## <a name="download"></a>Download

Le funzionalità di runtime vengono rilasciate come pacchetti NuGet in NuGet gallery. È possibile installare o aggiornare ai pacchetti NuGet rilasciati tramite la Console di gestione pacchetti NuGet:

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a>Documentazione

È possibile trovare le esercitazioni e altri documenti su ASP.NET Web API OData, visitare il [sito web ASP.NET](../odata-support-in-aspnet-web-api/index.md).

<a id="corelib"></a>
## <a name="odata-core-libraries"></a>Librerie di base di OData

Per OData v4, API Web viene ora utilizzato ODataLib versione 6.5.0

<a id="newf"></a>
## <a name="new-features-in-aspnet-web-api-odata-53"></a>Nuove funzionalità in ASP.NET Web API OData 5.3

### <a name="support-for-levels-in-expand"></a>Supporto per $levels in $expand

È possibile usare il $levels query opzione in $espandere le query. Ad esempio:

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample2.cmd)]

Questa query è equivalente a:

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample3.cmd)]

<a id="open-entity-types"></a>
### <a name="support-for-open-entity-types"></a>Supporto per i tipi di entità aperto

Un' *tipo open* è un tipo stuctured che contiene le proprietà dinamiche, oltre a tutte le proprietà che vengono dichiarate nella definizione del tipo. Tipi aperti consentono di aggiungere una flessibilità ai modelli di dati. Per altre informazioni, vedere xxxx.

### <a name="support-for-dynamic-collection-properties-in-open-types"></a>Supporto per le proprietà di raccolta dinamica di tipi aperti

Una proprietà dinamica in precedenza, era necessario essere un singolo valore. Versione 5.3, le proprietà dinamiche possono avere valori della raccolta. Ad esempio, nel payload JSON seguente, il `Emails` proprietà è una proprietà dinamica ed è della raccolta di tipo stringa:

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample4.cmd)]

### <a name="support-for-inheritance-for-complex-types"></a>Supporto per l'ereditarietà per i tipi complessi

Ora i tipi complessi possono ereditare da un tipo di base. Ad esempio, un servizio OData è possibile definire i tipi complessi seguenti:

[!code-csharp[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample5.cs)]

Ecco il modello EDM per questo esempio:

[!code-xml[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample6.xml?highlight=8,15)]

Per altre informazioni, vedere [esempio di ereditarietà di tipo complesso OData](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a>Problemi noti e modifiche di rilievo

In questa sezione vengono descritti problemi noti e modifiche di rilievo in ASP.NET Web API OData 5.3.

### <a name="odata-v4"></a>OData v4

#### <a name="query-options"></a>Opzioni di query

Problema: Usando $ nidificata espandere con $levels = numero massimo di risultati in una profondità di espansione non corretto.

Ad esempio, data la richiesta seguente:

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample7.cmd)]

Se `MaxExpansionDepth` è 5, questa query comporterà una profondità di espansione di 6.

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a>Correzioni di bug e gli aggiornamenti delle funzionalità secondarie

Questa versione include anche diverse correzioni di bug e la funzionalità secondaria degli aggiornamenti. È possibile trovare l'elenco completo di seguito:

- [Correzioni di bug](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.3%20Beta&assignedTo=All&component=Web%20API|Web%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="OD"></a>
## <a name="aspnet-web-api-odata-531"></a>API Web ASP.NET OData 5.3.1

In questa versione è stata apportata una [correzione di bug](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.3.1%20Beta&amp;assignedTo=All&amp;component=Web%20API%20OData&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=All) ad alcune delle enumerazioni AllowedFunctions. Questa versione non include altre nuove funzionalità o correzioni di bug.
