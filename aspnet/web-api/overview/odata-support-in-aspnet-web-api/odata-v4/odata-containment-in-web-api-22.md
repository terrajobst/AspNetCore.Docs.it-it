---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
title: Il contenimento OData v4 Using Web API 2.2 | Documenti Microsoft
author: rick-anderson
description: "In genere, un'entità può accedere solo se è stato incapsulato all'interno di un set di entità. Ma OData v4 fornisce due opzioni aggiuntive, Singleton e Con..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/27/2014
ms.topic: article
ms.assetid: 5fbfefad-a17a-4c46-8646-f1ccd154cd56
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 7d3c81bf3d2a43faa3e71155637e031f81143782
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="containment-in-odata-v4-using-web-api-22"></a>Contenimento OData v4 Using Web API 2.2
====================
da Jinfu Tan

> In genere, un'entità può accedere solo se è stato incapsulato all'interno di un set di entità. Ma OData v4 fornisce due opzioni aggiuntive, Singleton e contenuto, che supporta WebAPI 2.2.


In questo argomento viene illustrato come definire un contenuto in un endpoint OData WebApi 2.2. Per ulteriori informazioni sul contenuto, vedere [contenimento proviene con OData v4](https://blogs.msdn.com/b/odatateam/archive/2014/03/13/containment-is-coming-with-odata-v4.aspx). Per creare un endpoint OData V4 in Web API, vedere [creare un OData v4 Endpoint Using ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md).

In primo luogo, si creerà un modello di dominio di contenimento del servizio OData, utilizzando questo modello di dati:

![Modello di dati](odata-containment-in-web-api-22/_static/image1.png)

Un account contiene molti PaymentInstruments (PI), ma non è possibile definire una set di entità per un PI. Al contrario, l'elaborazione solo accessibili tramite un Account.

È possibile scaricare la soluzione usata in questo argomento da [CodePlex](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/).

## <a name="defining-the-data-model"></a>La definizione del modello di dati

1. Definire i tipi CLR.

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample1.cs)]

    Il `Contained` attributo viene utilizzato per le proprietà di navigazione di contenimento.
2. Generare il modello EDM in base ai tipi CLR.

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample2.cs)]

    Il `ODataConventionModelBuilder` gestirà la compilazione del modello EDM se il `Contained` attributo viene aggiunto alla proprietà di navigazione corrispondente. Se la proprietà è un tipo di raccolta, un `GetCount(string NameContains)` funzione verrà inoltre creata.

    Metadati generati avrà un aspetto simile al seguente:

    [!code-xml[Main](odata-containment-in-web-api-22/samples/sample3.xml?highlight=10)]

    Il `ContainsTarget` attributo indica che la proprietà di navigazione è un contenimento.

## <a name="define-the-containing-entity-set-controller"></a>Definire il controller di set di entità che lo contiene

Entità indipendenti non dispone di propri controller. l'azione è definita nel controller di set di entità che lo contiene. In questo esempio, è un AccountsController, ma non PaymentInstrumentsController.

[!code-csharp[Main](odata-containment-in-web-api-22/samples/sample4.cs)]

Se il percorso OData è 4 o più segmenti, solo attributo funzionamento del routing, ad esempio `[ODataRoute("Accounts({accountId})/PayinPIs({paymentInstrumentId})")]` nel controller precedente. In caso contrario, attributo e il routing convenzionale funziona: ad esempio, `GetPayInPIs(int key)` corrisponde `GET ~/Accounts(1)/PayinPIs`.

*Grazie a Leo Hu per il contenuto originale di questo articolo.*
