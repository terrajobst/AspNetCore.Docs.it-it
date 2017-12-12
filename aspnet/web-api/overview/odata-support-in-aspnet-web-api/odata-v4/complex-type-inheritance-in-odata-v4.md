---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
title: "Ereditarietà del tipo complesso in OData v4 con ASP.NET Web API | Documenti Microsoft"
author: microsoft
description: "In base alla specifica OData v4, un tipo complesso può ereditare da un altro tipo complesso. (Un tipo complesso è un tipo strutturato senza una chiave). API Web..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/16/2014
ms.topic: article
ms.assetid: a00d3600-9c2a-41bc-9460-06cc527904e2
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: be2dbfa82b99b6c48928e4e767716852c14a463b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="complex-type-inheritance-in-odata-v4-with-aspnet-web-api"></a>Ereditarietà del tipo complesso in OData v4 con ASP.NET Web API
====================
da [Microsoft](https://github.com/microsoft)

> In base a OData v4 [specifica](http://www.odata.org/documentation/odata-version-4-0/), un tipo complesso può ereditare da un altro tipo complesso. (A *complesso* tipo è un tipo strutturato senza una chiave.) Web API OData 5.3 supporta l'ereditarietà di tipo complesso.
> 
> In questo argomento viene illustrato come compilare un entity data model (EDM) con tipi complessi di ereditarietà. Per il codice sorgente completo, vedere [esempio ereditarietà di tipo complesso OData](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - Web API OData 5.3
> - OData v4


## <a name="model-hierarchy"></a>Gerarchia del modello

Per illustrare l'ereditarietà del tipo complesso, verrà utilizzata la seguente gerarchia di classi.

![](complex-type-inheritance-in-odata-v4/_static/image1.png)

`Shape`è un tipo complesso astratto. `Rectangle`, `Triangle`, e `Circle` sono tipi complessi derivati da `Shape`, e `RoundRectangle` deriva da `Rectangle`. `Window`è un tipo di entità e contiene un `Shape` istanza.

Di seguito sono le classi CLR che definiscono questi tipi.

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample1.cs)]

## <a name="build-the-edm-model"></a>Compilare il modello EDM

Per creare il modello EDM, è possibile utilizzare **ODataConventionModelBuilder**, che deriva le relazioni di ereditarietà tra i tipi CLR.

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample2.cs)]

È anche possibile compilare il modello EDM a in modo esplicito, tramite **ODataModelBuilder**. Accetta più codice, ma garantisce un maggiore controllo su EDM.

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample3.cs)]

I due esempi seguenti creano lo stesso schema EDM.

## <a name="metadata-document"></a>Documento di metadati

Di seguito è riportato il documento di metadati OData, che mostra ereditarietà del tipo complesso.

[!code-xml[Main](complex-type-inheritance-in-odata-v4/samples/sample4.xml?highlight=13,17,25,30)]

Nel documento di metadati, è possibile vedere che:

- Il `Shape` tipo complesso è astratta.
- Il `Rectangle`, `Triangle`, e `Circle` tipo complesso avere il tipo di base `Shape`.
- Il `RoundRectangle` tipo è di tipo base `Rectangle`.

## <a name="casting-complex-types"></a>Cast dei tipi complessi

È ora supportata l'esecuzione del cast per i tipi complessi. Ad esempio, la query seguente cast di un `Shape` per un `Rectangle`.

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample5.cmd)]

Di seguito è riportato il payload di risposta:

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample6.cmd)]
