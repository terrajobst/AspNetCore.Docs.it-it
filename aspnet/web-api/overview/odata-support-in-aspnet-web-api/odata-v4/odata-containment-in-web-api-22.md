---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
title: Contenimento in OData v4 Using Web API 2.2 | Microsoft Docs
author: rick-anderson
description: In genere, un'entità è stato possibile accedere solo se si sono stato incapsulato all'interno di un set di entità. Ma OData v4 fornisce due opzioni aggiuntive, Singleton e Con...
ms.author: aspnetcontent
ms.date: 06/27/2014
ms.assetid: 5fbfefad-a17a-4c46-8646-f1ccd154cd56
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-containment-in-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 56e550b56e9ad237dbf4fab04f2bd545164ee90a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37824912"
---
<a name="containment-in-odata-v4-using-web-api-22"></a>Contenimento in OData v4 tramite l'API Web 2.2
====================
da Jinfu Tan

> In genere, un'entità è stato possibile accedere solo se si sono stato incapsulato all'interno di un set di entità. Ma OData v4 fornisce due opzioni aggiuntive, Singleton e contenimento, che supporta l'API Web 2.2.


In questo argomento viene illustrato come definire un contenimento in un endpoint OData nell'API Web 2.2. Per altre informazioni sul contenuto, vedere [contenimento proviene con OData v4](https://blogs.msdn.com/b/odatateam/archive/2014/03/13/containment-is-coming-with-odata-v4.aspx). Per creare un endpoint OData V4 nell'API Web, vedere [creare un OData v4 Endpoint Using ASP.NET Web API 2.2](create-an-odata-v4-endpoint.md).

In primo luogo, si creerà un modello di dominio di contenimento del servizio OData, utilizzando questo modello di dati:

![Modello di dati](odata-containment-in-web-api-22/_static/image1.png)

Un account contiene molti PaymentInstruments (PI), ma non definiamo una set di entità per un dispositivo PI. Al contrario, PI sono accessibile solo tramite un Account.

È possibile scaricare la soluzione usata in questo argomento dalla [CodePlex](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/).

## <a name="defining-the-data-model"></a>La definizione del modello di dati

1. Definire i tipi CLR.

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample1.cs)]

    Il `Contained` viene usato per le proprietà di navigazione di contenimento.
2. Generare il modello EDM in base ai tipi CLR.

    [!code-csharp[Main](odata-containment-in-web-api-22/samples/sample2.cs)]

    Il `ODataConventionModelBuilder` gestirà la compilazione del modello EDM, se il `Contained` attributo viene aggiunto alla proprietà di navigazione corrispondente. Se la proprietà è un tipo di raccolta una `GetCount(string NameContains)` funzione verrà inoltre creata.

    I metadati generati avrà un aspetto simile al seguente:

    [!code-xml[Main](odata-containment-in-web-api-22/samples/sample3.xml?highlight=10)]

    Il `ContainsTarget` attributo indica che la proprietà di navigazione rappresenta un contenimento.

## <a name="define-the-containing-entity-set-controller"></a>Definire il controller di set di entità che lo contiene

Entità indipendenti non abbiano i propri controller; l'azione è definita nel controller di set di entità che lo contiene. In questo esempio, è un AccountsController, ma nessun PaymentInstrumentsController.

[!code-csharp[Main](odata-containment-in-web-api-22/samples/sample4.cs)]

Se il percorso OData è 4 o più segmenti, solo attributo funzionamento del routing, ad esempio `[ODataRoute("Accounts({accountId})/PayinPIs({paymentInstrumentId})")]` nel controller precedente. In caso contrario, attributi e routing convenzionale funziona: ad esempio, `GetPayInPIs(int key)` corrisponde a `GET ~/Accounts(1)/PayinPIs`.

*Grazie a Leo Hu per il contenuto originale di questo articolo.*
