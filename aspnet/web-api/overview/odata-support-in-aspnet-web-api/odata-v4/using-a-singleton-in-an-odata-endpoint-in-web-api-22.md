---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
title: Creare un Singleton in OData v4 Using Web API 2.2 | Documenti Microsoft
author: rick-anderson
description: In questo argomento viene illustrato come definire un singleton in un endpoint OData in Web API 2.2.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/27/2014
ms.topic: article
ms.assetid: 4064ab14-26ee-4d5c-ae58-1bdda525ad06
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/using-a-singleton-in-an-odata-endpoint-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 92c5056548b1e39defb28ac36f83b001dd108f5f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="create-a-singleton-in-odata-v4-using-web-api-22"></a>Creare un Singleton in OData v4 Using Web API 2.2
====================
da Zoe Luo

> In genere, un'entità può accedere solo se è stato incapsulato all'interno di un set di entità. Ma OData v4 fornisce due opzioni aggiuntive, Singleton e contenuto, che supporta WebAPI 2.2.


In questo articolo viene illustrato come definire un singleton in un endpoint OData in Web API 2.2. Per informazioni su quali un singleton e come è possibile trarre vantaggio dall'utilizzo, vedere [utilizzando un singleton per definire l'entità speciale](https://blogs.msdn.com/b/odatateam/archive/2014/03/05/use-singleton-to-define-your-special-entity.aspx). Per creare un endpoint OData V4 in Web API, vedere [creare un OData v4 Endpoint Using ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md). 

Si creerà un singleton nel progetto API Web con il modello di dati seguenti:

![Modello dati](using-a-singleton-in-an-odata-endpoint-in-web-api-22/_static/image1.png)

Un singleton denominato `Umbrella` verranno definiti in base al tipo `Company`e un set denominato di entità `Employees` verranno definiti in base al tipo `Employee`.

La soluzione usata in questa esercitazione può essere scaricata da [CodePlex](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/).

## <a name="define-the-data-model"></a>Definizione del modello di dati

1. Definire i tipi CLR.

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample1.cs)]
2. Generare il modello EDM in base ai tipi CLR.

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample2.cs)]

    In questo caso, `builder.Singleton<Company>("Umbrella")` indica il generatore di modelli per creare un singleton denominato `Umbrella` nel modello EDM.

    Metadati generati avrà un aspetto simile al seguente:

    [!code-xml[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample3.xml)]

    Dai metadati possiamo vedere che la proprietà di navigazione `Company` nel `Employees` set di entità è associata a singleton `Umbrella`. L'associazione viene eseguita automaticamente da `ODataConventionModelBuilder`, poiché solo `Umbrella` ha il `Company` tipo. Se è presente qualsiasi ambiguità nel modello, è possibile utilizzare `HasSingletonBinding` associare in modo esplicito una proprietà di navigazione in un singleton; `HasSingletonBinding` ha lo stesso effetto dell'utilizzo di `Singleton` attributo nella definizione del tipo CLR:

    [!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample4.cs)]

## <a name="define-the-singleton-controller"></a>Definire il controller singleton

Ad esempio il controller di EntitySet, il controller singleton eredita da `ODataController`, e deve essere il nome del controller singleton `[singletonName]Controller`.

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample5.cs)]

Per gestire diversi tipi di richieste, sono necessarie azioni per essere predefinite nel controller. **Attributo routing** è abilitata per impostazione predefinita in WebApi 2.2. Ad esempio, per definire un'azione per gestire l'esecuzione di query `Revenue` da `Company` routing degli attributi, utilizzare le operazioni seguenti:

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample6.cs)]

Se non si è disposti definire gli attributi per ogni azione, solo per definire le azioni seguenti [convenzioni di Routing OData](../odata-routing-conventions.md). Poiché una chiave non è necessaria per l'esecuzione di query singleton, le azioni definite nel controller singleton sono leggermente diverse dalle azioni definite nel controller di entityset.

Per riferimento, di seguito sono elencate le firme del metodo per ogni definizione di azione nel controller singleton.

[!code-csharp[Main](using-a-singleton-in-an-odata-endpoint-in-web-api-22/samples/sample7.cs)]

In pratica, è sufficiente eseguire sul lato del servizio. Il [progetto di esempio](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataSingletonSample/) contiene tutto il codice per la soluzione e il client OData in cui viene illustrato come utilizzare il singleton. La compilazione del client seguendo i passaggi descritti in [creare un'App Client di OData v4](create-an-odata-v4-client-app.md).

. 

*Grazie a Leo Hu per il contenuto originale di questo articolo.*
