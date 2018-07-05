---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
title: Ereditarietà del tipo complesso in OData v4 con l'API Web ASP.NET | Microsoft Docs
author: microsoft
description: In base alla specifica OData v4, un tipo complesso può ereditare da un altro tipo complesso. (Un tipo complesso è un tipo strutturato senza una chiave). API Web...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/16/2014
ms.topic: article
ms.assetid: a00d3600-9c2a-41bc-9460-06cc527904e2
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/complex-type-inheritance-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: 84a887b445959c4aa6d1ee372f067f93cd725d77
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37372057"
---
<a name="complex-type-inheritance-in-odata-v4-with-aspnet-web-api"></a>Ereditarietà del tipo complesso in OData v4 con l'API Web ASP.NET
====================
da [Microsoft](https://github.com/microsoft)

> In base a OData v4 [specification](http://www.odata.org/documentation/odata-version-4-0/), un tipo complesso può ereditare da un altro tipo complesso. (Un *complessi* tipo è un tipo strutturato senza una chiave.) API Web OData 5.3 supporta l'ereditarietà del tipo complesso.
> 
> Questo argomento illustra come creare un entity data model (EDM) con i tipi di eredità complessa. Per il codice sorgente completo, vedere [esempio di ereditarietà di tipo complesso OData](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataComplexTypeInheritanceSample/ReadMe.txt).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - API Web OData 5.3
> - OData v4


## <a name="model-hierarchy"></a>Gerarchia del modello

Per illustrare l'ereditarietà del tipo complesso, si userà la seguente gerarchia di classi.

![](complex-type-inheritance-in-odata-v4/_static/image1.png)

`Shape` è un tipo complesso astratto. `Rectangle`, `Triangle`, e `Circle` sono tipi complessi derivati dal `Shape`, e `RoundRectangle` deriva da `Rectangle`. `Window` è un tipo di entità e contiene un `Shape` istanza.

Di seguito sono le classi CLR che definiscono questi tipi.

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample1.cs)]

## <a name="build-the-edm-model"></a>Compilare il modello EDM

Per creare il modello EDM, è possibile usare **ODataConventionModelBuilder**, che viene dedotto le relazioni di ereditarietà tra i tipi CLR.

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample2.cs)]

È anche possibile compilare il modello EDM a in modo esplicito, tramite **ODataModelBuilder**. Questo passa altro codice, ma ti offre maggiore controllo sul modello EDM.

[!code-csharp[Main](complex-type-inheritance-in-odata-v4/samples/sample3.cs)]

I due esempi seguenti creano lo stesso schema EDM.

## <a name="metadata-document"></a>Documento di metadati

Ecco il documento di metadati OData, che mostra l'ereditarietà di tipi complessi.

[!code-xml[Main](complex-type-inheritance-in-odata-v4/samples/sample4.xml?highlight=13,17,25,30)]

Il documento di metadati, è possibile osservare che:

- Il `Shape` tipo complesso è astratto.
- Il `Rectangle`, `Triangle`, e `Circle` tipo complesso hanno il tipo di base `Shape`.
- Il `RoundRectangle` tipo con il tipo di base `Rectangle`.

## <a name="casting-complex-types"></a>Il cast dei tipi complessi

È ora supportata l'esecuzione del cast per i tipi complessi. Ad esempio, la query seguente cast di un `Shape` a un `Rectangle`.

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample5.cmd)]

Ecco il payload di risposta:

[!code-console[Main](complex-type-inheritance-in-odata-v4/samples/sample6.cmd)]
