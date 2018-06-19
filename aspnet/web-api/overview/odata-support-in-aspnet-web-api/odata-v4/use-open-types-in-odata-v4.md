---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
title: Aprire i tipi in OData v4 con ASP.NET Web API | Documenti Microsoft
author: microsoft
description: In OData v4, un tipo aperto è un tipo stuctured che contiene le proprietà dinamiche, oltre a tutte le proprietà che vengono dichiarate nella definizione del tipo. Apri...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/15/2014
ms.topic: article
ms.assetid: f25f5ac5-4800-4950-abe5-c97750a27fc6
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: fe67b9a11a82b55d5f3e0e5f1b0cee10a58833d2
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/12/2018
ms.locfileid: "29153272"
---
<a name="open-types-in-odata-v4-with-aspnet-web-api"></a>Aprire i tipi in OData v4 con ASP.NET Web API
====================
by [Microsoft](https://github.com/microsoft)

> In OData v4, un *aprire tipo* è un tipo stuctured che contiene le proprietà dinamiche, oltre a tutte le proprietà che vengono dichiarate nella definizione del tipo. I tipi aperti consentono di aggiungere la flessibilità per i modelli di data. In questa esercitazione viene illustrato come utilizzare i tipi aperti in ASP.NET Web API OData.
> 
> In questa esercitazione si presuppone che si conosce già la modalità creare un endpoint OData in ASP.NET Web API. In caso contrario, iniziare leggendo [creare un Endpoint di OData v4](create-an-odata-v4-endpoint.md) prima.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - Web API OData 5.3
> - OData v4


Innanzitutto, la terminologia OData:

- Tipo di entità: un tipo strutturato con una chiave.
- Tipo complesso: un tipo strutturato senza una chiave.
- Tipo Open: un tipo con le proprietà dinamiche. È possibile aprire i tipi di entità e complessi.

Il valore di una proprietà dinamica può essere un tipo primitivo, un tipo complesso o un tipo di enumerazione. o una raccolta di uno qualsiasi di tali tipi. Per ulteriori informazioni sui tipi aperti, vedere il [OData v4 specifica](http://www.odata.org/documentation/odata-version-4-0/).

## <a name="install-the-web-odata-libraries"></a>Installare le librerie di OData Web

Usare Gestione pacchetti NuGet per installare le librerie più recenti di Web API OData. Dalla finestra della Console di gestione pacchetti:

[!code-console[Main](use-open-types-in-odata-v4/samples/sample1.cmd)]

## <a name="define-the-clr-types"></a>Definire i tipi CLR

Per iniziare, definire i modelli EDM come tipi CLR.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample2.cs)]

Quando si crea l'Entity Data Model (EDM),

- `Category`è un tipo di enumerazione.
- `Address`è un tipo complesso. (Non ha una chiave, pertanto non è un tipo di entità.)
- `Customer`è un tipo di entità. (Con una chiave).
- `Press`è un tipo complesso aperto.
- `Book`è un tipo di entità aperto.

Per creare un tipo aperto, il tipo di Common Language Runtime deve avere una proprietà di tipo `IDictionary<string, object>`, che contiene le proprietà dinamiche.

## <a name="build-the-edm-model"></a>Compilare il modello EDM

Se si utilizza **ODataConventionModelBuilder** per creare il modello EDM, `Press` e `Book` vengono automaticamente aggiunti come tipi aperti, in base alla presenza di un `IDictionary<string, object>` proprietà.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample3.cs)]

È anche possibile compilare il modello EDM a in modo esplicito, tramite **ODataModelBuilder**.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample4.cs)]

## <a name="add-an-odata-controller"></a>Aggiungere un Controller OData

Successivamente, aggiungere un controller OData. Per questa esercitazione, si userà un controller semplificato che supporta solo GET e post-richieste e utilizza un elenco in memoria per archiviare entità.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample5.cs)]

Si noti che il primo `Book` istanza non dispone di alcuna proprietà dinamiche. Il secondo `Book` istanza ha le proprietà dinamiche seguenti:

- "Pubblicati": tipo primitivo
- "Gli autori": raccolta di tipi primitivi
- "OtherCategories": raccolta di tipi di enumerazione.

Inoltre, il `Press` proprietà di tale `Book` istanza ha le proprietà dinamiche seguenti:

- "Blog": tipo primitivo
- "Address": tipo complesso

## <a name="query-the-metadata"></a>Query sui metadati

Per ottenere il documento di metadati OData, inviare una richiesta GET al `~/$metadata`. Il corpo della risposta dovrebbe essere simile al seguente:

[!code-xml[Main](use-open-types-in-odata-v4/samples/sample6.xml?highlight=5,21)]

Nel documento di metadati, è possibile vedere che:

- Per il `Book` e `Press` tipi, il valore di `OpenType` attributo è true. Il `Customer` e `Address` tipi non dispongono di questo attributo.
- Il `Book` tipo di entità include tre proprietà dichiarate: ISBN, titolo e premere. I metadati di OData non include il `Book.Properties` proprietà dalla classe CLR.
- Analogamente, il `Press` tipo complesso ha solo due proprietà dichiarate: nome e la categoria. I metadati includono la `Press.DynamicProperties` proprietà dalla classe CLR.

## <a name="query-an-entity"></a>Query un'entità

Per ottenere il libro con codice ISBN uguale a "978-0-7356-7942-9", inviare una richiesta GET al `~/Books('978-0-7356-7942-9')`. Il corpo della risposta sarà simile al seguente. (Rientrati per rendere più leggibili).

[!code-console[Main](use-open-types-in-odata-v4/samples/sample7.cmd?highlight=8-13,15-23)]

Si noti che le proprietà dinamiche sono incluse inline con le proprietà dichiarate.

## <a name="post-an-entity"></a>REGISTRARE un'entità

Per aggiungere un'entità di libro, inviare una richiesta POST per `~/Books`. Il client può impostare le proprietà dinamiche nel payload della richiesta.

Di seguito è riportato un esempio di richiesta. Si noti la proprietà "Price" e "Pubblicato".

[!code-console[Main](use-open-types-in-odata-v4/samples/sample8.cmd?highlight=10)]

Se si imposta un punto di interruzione nel metodo del controller, è possibile vedere che l'API Web aggiunto queste proprietà per il `Properties` dizionario.

![](use-open-types-in-odata-v4/_static/image1.png)

## <a name="additional-resources"></a>Risorse aggiuntive

[Esempio di tipo Open OData](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataOpenTypeSample/ReadMe.txt)
