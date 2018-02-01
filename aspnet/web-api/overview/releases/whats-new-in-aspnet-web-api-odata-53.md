---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-odata-53
title: "Novità di ASP.NET Web API OData 5.3 | Documenti Microsoft"
author: microsoft
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/16/2014
ms.topic: article
ms.assetid: e39eaa25-83ff-41dc-869d-3818d59a88ae
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-odata-53
msc.type: authoredcontent
ms.openlocfilehash: e918f86ebd813f31ad0c21566e617482fc7dd963
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="whats-new-in-aspnet-web-api-odata-53"></a>Novità di ASP.NET Web API OData 5.3
====================
da [Microsoft](https://github.com/microsoft)

In questo argomento vengono descritte le novità di ASP.NET Web API OData 5.3.

- [Download](#download)
- [Documentazione](#documentation)
- [Librerie di base di OData](#corelib)
- [Nuove funzionalità](#newf)
- [Problemi noti e modifiche di rilievo](#known-issues)
- [Correzioni di bug](#bug-fixes)
- [ASP.NET Web API OData 5.3.1](#OD)

<a id="download"></a>
## <a name="download"></a>Download

Le funzionalità di runtime vengono rilasciate come pacchetti NuGet in Raccolta NuGet. È possibile installare o aggiornare i pacchetti NuGet rilasciati tramite la Console di gestione pacchetti NuGet:

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a>Documentazione

È possibile trovare le esercitazioni e altra documentazione sulle ASP.NET Web API OData nel [sito web ASP.NET](../odata-support-in-aspnet-web-api/index.md).

<a id="corelib"></a>
## <a name="odata-core-libraries"></a>Librerie di base di OData

Per OData v4, API Web viene ora utilizzato ODataLib versione 6.5.0

<a id="newf"></a>
## <a name="new-features-in-aspnet-web-api-odata-53"></a>Nuove funzionalità di ASP.NET Web API OData 5.3

### <a name="support-for-levels-in-expand"></a>Espandere il supporto per $levels in $

È possibile utilizzare il $levels query opzione in $espandere le query. Ad esempio:

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample2.cmd)]

Questa query equivale a:

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample3.cmd)]

<a id="open-entity-types"></a>
### <a name="support-for-open-entity-types"></a>Supporto per tipi di entità aperto

Un *aprire tipo* è un tipo stuctured che contiene le proprietà dinamiche, oltre a tutte le proprietà che vengono dichiarate nella definizione del tipo. I tipi aperti consentono di aggiungere la flessibilità per i modelli di data. Per ulteriori informazioni, vedere xxxx.

### <a name="support-for-dynamic-collection-properties-in-open-types"></a>Supporto per le proprietà di raccolta dinamica in tipi aperti

Una proprietà dinamica in precedenza, era necessario un solo valore. Versione 5.3, le proprietà dinamiche possono avere valori della raccolta. Ad esempio, nel payload JSON seguente, la `Emails` proprietà è una proprietà dinamica ed è di raccolta di tipo stringa:

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample4.cmd)]

### <a name="support-for-inheritance-for-complex-types"></a>Supporto per l'ereditarietà di tipi complessi

Ora i tipi complessi possono ereditare da un tipo di base. Ad esempio, un servizio OData è possibile definire tipi complessi seguenti:

[!code-csharp[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample5.cs)]

Di seguito è riportato il modello EDM per questo esempio:

[!code-xml[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample6.xml?highlight=8,15)]

Per ulteriori informazioni, vedere [esempio ereditarietà di tipo complesso OData](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a>Problemi noti e modifiche di rilievo

In questa sezione vengono descritti problemi noti e modifiche di rilievo nel 5.3 di ASP.NET Web API OData.

### <a name="odata-v4"></a>OData v4

#### <a name="query-options"></a>Opzioni di query

Problema: Usando $ nidificata espandere con $levels = max risultati in una profondità di espansione non corretto.

Ad esempio, poiché la richiesta seguente:

[!code-console[Main](whats-new-in-aspnet-web-api-odata-53/samples/sample7.cmd)]

Se `MaxExpansionDepth` è 5, la query darà origine a una profondità di espansione di 6.

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a>Correzioni di bug e aggiornamenti delle funzionalità secondarie

Questa versione include inoltre diverse correzioni di bug e le funzionalità secondarie gli aggiornamenti. È possibile trovare l'elenco completo di seguito:

- [Correzioni di bug](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.3%20Beta&assignedTo=All&component=Web%20API|Web%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="OD"></a>
## <a name="aspnet-web-api-odata-531"></a>ASP.NET Web API OData 5.3.1

In questa versione sono stati apportati un [correzione di bug](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.3.1%20Beta&amp;assignedTo=All&amp;component=Web%20API%20OData&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=All) ad alcune delle enumerazioni AllowedFunctions. Questa versione non dispone di qualsiasi altra funzionalità nuove o correzioni di bug.
