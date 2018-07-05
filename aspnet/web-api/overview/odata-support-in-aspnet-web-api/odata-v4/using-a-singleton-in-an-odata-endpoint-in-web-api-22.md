---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
title: Creare un Singleton in OData v4 Using Web API 2.2 | Microsoft Docs
author: rick-anderson
description: In questo argomento viene illustrato come definire un singleton in un endpoint OData nell'API Web 2.2.
ms.author: aspnetcontent
ms.date: 06/27/2014
ms.assetid: 4064ab14-26ee-4d5c-ae58-1bdda525ad06
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: baccfed79949ae4efd45395bed3ad0058549cb4c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37830516"
---
<a name="create-a-singleton-in-odata-v4-using-web-api-22"></a>Creare un Singleton in OData v4 tramite l'API Web 2.2
====================
da Zoe Luo

> In genere, un'entità è stato possibile accedere solo se si sono stato incapsulato all'interno di un set di entità. Ma OData v4 fornisce due opzioni aggiuntive, Singleton e contenimento, che supporta l'API Web 2.2.


Questo articolo illustra come definire un singleton in un endpoint OData nell'API Web 2.2. Per informazioni su quali un singleton e come è possibile trarre vantaggio dal loro utilizzo, vedere [uso di un singleton per definire l'entità speciale](https://blogs.msdn.com/b/odatateam/archive/2014/03/05/use-singleton-to-define-your-special-entity.aspx). Per creare un endpoint OData V4 nell'API Web, vedere [creare un OData v4 Endpoint Using ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md). 

Si creerà un singleton nel progetto API Web usando il modello di dati seguenti:

![Modello dati](using-a-singleton-in-an-odata-endpoint-in-web-api-22/_static/image1.png)

Un singleton denominato `Umbrella` verranno definiti in base al tipo `Company`e un'entità, set denominato `Employees` verranno definiti in base al tipo `Employee`.

La soluzione usata in questa esercitazione può essere scaricata dal [CodePlex](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/).

## <a name="define-the-data-model"></a>Definizione del modello di dati

1. Definire i tipi CLR.

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample1.cs)]
2. Generare il modello EDM in base ai tipi CLR.

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample2.cs)]

    In questo caso, `builder.Singleton<Company>("Umbrella")` indica il generatore di modelli per creare un singleton denominato `Umbrella` nel modello EDM.

    I metadati generati avrà un aspetto simile al seguente:

    [!code-xml[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample3.xml)]

    Dai metadati è possibile osservare che la proprietà di navigazione `Company` nella `Employees` set di entità è associato a singleton `Umbrella`. L'associazione viene eseguito automaticamente dal `ODataConventionModelBuilder`, poiché solo `Umbrella` ha il `Company` tipo. Se è presente un'ambiguità nel modello, è possibile usare `HasSingletonBinding` associare in modo esplicito una proprietà di navigazione in un singleton; `HasSingletonBinding` ha lo stesso effetto dell'utilizzo di `Singleton` attributo nella definizione del tipo CLR:

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample4.cs)]

## <a name="define-the-singleton-controller"></a>Definire il controller singleton

Ad esempio il controller di EntitySet, il controller singleton eredita da `ODataController`, e deve essere il nome del controller singleton `[singletonName]Controller`.

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample5.cs)]

Per gestire diversi tipi di richieste, sono necessarie azioni per essere predefinite nel controller. **Routing con attributi** è abilitato per impostazione predefinita nell'API Web 2.2. Ad esempio, per definire un'azione per gestire l'esecuzione di query `Revenue` da `Company` usando il routing con attributi, usare il comando seguente:

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample6.cs)]

Se non si è disposti definire gli attributi per ogni azione, definire solo le azioni seguenti [convenzioni di Routing OData](../odata-routing-conventions.md). Poiché una chiave non è necessaria per l'esecuzione di query singleton, le azioni definite nel controller singleton sono leggermente diverse dalle azioni definite nel controller entityset.

Per riferimento, di seguito sono elencate le firme di metodo per ogni definizione di azione nel controller singleton.

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample7.cs)]

Si tratta in sostanza, è sufficiente per eseguire operazioni sul lato del servizio. Il [progetto di esempio](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/) contiene tutto il codice per la soluzione e il client OData che illustra come usare il singleton. Il client viene compilato, seguire i passaggi descritti in [creare un'App Client OData v4](create-an-odata-v4-client-app.md).

. 

*Grazie a Leo Hu per il contenuto originale di questo articolo.*
