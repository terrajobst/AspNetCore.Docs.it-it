---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
title: Tipi aperti in OData v4 con l'API Web ASP.NET | Microsoft Docs
author: microsoft
description: In OData v4, un tipo aperto è un tipo stuctured che contiene le proprietà dinamiche, oltre a tutte le proprietà che vengono dichiarate nella definizione del tipo. Apri...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/15/2014
ms.topic: article
ms.assetid: f25f5ac5-4800-4950-abe5-c97750a27fc6
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: 0eae376830e70199a9692df5a154168646f99716
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37375525"
---
<a name="open-types-in-odata-v4-with-aspnet-web-api"></a>Tipi aperti in OData v4 con l'API Web ASP.NET
====================
da [Microsoft](https://github.com/microsoft)

> In OData v4, un' *tipo open* è un tipo stuctured che contiene le proprietà dinamiche, oltre a tutte le proprietà che vengono dichiarate nella definizione del tipo. Tipi aperti consentono di aggiungere una flessibilità ai modelli di dati. Questa esercitazione illustra come usare tipi aperti in ASP.NET Web API OData.
> 
> Questa esercitazione si presuppone che conosci già come creare un endpoint OData nell'API Web ASP.NET. In caso contrario, iniziare leggendo [creare un Endpoint OData v4](create-an-odata-v4-endpoint.md) prima.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - API Web OData 5.3
> - OData v4


Innanzitutto, alcuni termini di OData:

- Tipo di entità: un tipo strutturato con una chiave.
- Tipo complesso: un tipo strutturato senza una chiave.
- Tipo Open: un tipo con le proprietà dinamiche. È possibile Apri entrambi i tipi di entità e tipi complessi.

Il valore di una proprietà dinamica può essere un tipo primitivo, un tipo complesso o un tipo di enumerazione; oppure una raccolta di uno qualsiasi di tali tipi. Per altre informazioni sui tipi aperti, vedere la [specifica OData v4](http://www.odata.org/documentation/odata-version-4-0/).

## <a name="install-the-web-odata-libraries"></a>Installare le librerie OData Web

Usare Gestione pacchetti NuGet per installare le librerie di API Web OData più recenti. Dalla finestra della Console di gestione pacchetti:

[!code-console[Main](use-open-types-in-odata-v4/samples/sample1.cmd)]

## <a name="define-the-clr-types"></a>Definire i tipi CLR

Iniziare definendo i modelli EDM come tipi CLR.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample2.cs)]

Quando viene creato l'Entity Data Model (EDM),

- `Category` è un tipo di enumerazione.
- `Address` è un tipo complesso. (Non presenta una chiave, pertanto non è un tipo di entità.)
- `Customer` è un tipo di entità. (Con una chiave).
- `Press` è un tipo complesso aperto.
- `Book` è un tipo di entità aperto.

Per creare un tipo open, il tipo CLR deve avere una proprietà di tipo `IDictionary<string, object>`, che contiene le proprietà dinamiche.

## <a name="build-the-edm-model"></a>Compilare il modello EDM

Se si usa **ODataConventionModelBuilder** per creare il modello EDM, `Press` e `Book` risultano automaticamente aggiunti come tipi aperti, in base alla presenza di un `IDictionary<string, object>` proprietà.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample3.cs)]

È anche possibile compilare il modello EDM a in modo esplicito, tramite **ODataModelBuilder**.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample4.cs)]

## <a name="add-an-odata-controller"></a>Aggiungere un Controller OData

Successivamente, aggiungere un controller OData. Per questa esercitazione si userà un controller semplificato che supporta solo GET e post-richieste e Usa un elenco in memoria per archiviare le entità.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample5.cs)]

Si noti che il primo `Book` istanza non dispone di alcuna proprietà dinamiche. Il secondo `Book` istanza presenta le proprietà dinamiche seguenti:

- "Pubblicato": tipo primitivo
- "Gli autori": raccolta di tipi primitivi
- "OtherCategories": raccolta di tipi di enumerazione.

Inoltre, il `Press` proprietà di tale `Book` istanza presenta le proprietà dinamiche seguenti:

- "Blog": tipo primitivo
- "Address": tipo complesso

## <a name="query-the-metadata"></a>Eseguire query sui metadati

Per ottenere il documento di metadati OData, inviare una richiesta GET a `~/$metadata`. Il corpo della risposta dovrebbe essere simile al seguente:

[!code-xml[Main](use-open-types-in-odata-v4/samples/sample6.xml?highlight=5,21)]

Il documento di metadati, è possibile osservare che:

- Per il `Book` e `Press` tipi, il valore del `OpenType` attributo è true. Il `Customer` e `Address` tipi non dispongono di questo attributo.
- Il `Book` tipo di entità ha tre proprietà dichiarate: ISBN, titolo e premere. I metadati OData non includono il `Book.Properties` proprietà dalla classe CLR.
- Analogamente, il `Press` tipo complesso contiene solo due proprietà dichiarate: nome e una categoria. I metadati non includono il `Press.DynamicProperties` proprietà dalla classe CLR.

## <a name="query-an-entity"></a>Query un'entità

Per ottenere il libro con codice ISBN uguale a "978-7356-7942-0-9", inviare una richiesta GET a `~/Books('978-0-7356-7942-9')`. Il corpo della risposta dovrebbe essere simile al seguente. (Rientrati per renderlo più leggibile).

[!code-console[Main](use-open-types-in-odata-v4/samples/sample7.cmd?highlight=8-13,15-23)]

Si noti che le proprietà dinamiche sono incluse inline con le proprietà dichiarate.

## <a name="post-an-entity"></a>REGISTRARE un'entità

Per aggiungere un'entità libro, inviare una richiesta POST a `~/Books`. Il client può impostare le proprietà dinamiche nel payload della richiesta.

Ecco un esempio di richiesta. Prendere nota delle proprietà "Price" e "Pubblicato".

[!code-console[Main](use-open-types-in-odata-v4/samples/sample8.cmd?highlight=10)]

Se si imposta un punto di interruzione nel metodo del controller, è possibile vedere che l'API Web aggiunto queste proprietà per il `Properties` dizionario.

![](use-open-types-in-odata-v4/_static/image1.png)

## <a name="additional-resources"></a>Risorse aggiuntive

[Esempio di tipo Open OData](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataOpenTypeSample/ReadMe.txt)
