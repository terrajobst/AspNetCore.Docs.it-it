---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
title: 'Parte 2: Creazione dei modelli di dominio | Documenti Microsoft'
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/03/2012
ms.topic: article
ms.assetid: fe3ef85f-bdc6-4e10-9768-25aa565c01d0
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
msc.type: authoredcontent
ms.openlocfilehash: a573b47d27767dc78d557cd2b6c73714eb9e94f4
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="part-2-creating-the-domain-models"></a>Parte 2: Creazione dei modelli di dominio
====================
da [Mike Wasson](https://github.com/MikeWasson)

[Scaricare il progetto completato](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-models"></a>Aggiungere modelli

Esistono tre modi per l'approccio Entity Framework:

- Database-first: iniziare con un database e di Entity Framework genera il codice.
- Model-first: iniziare con un modello visivo ed Entity Framework genera il codice sia il database.
- Prima di codice: si inizia con il codice ed Entity Framework genera il database.

Viene usato l'approccio incentrato codice, in modo da iniziare definendo gli oggetti di dominio come POCOs (oggetti CLR normale precedente). Con l'approccio incentrato codice, gli oggetti di dominio non necessario codice aggiuntivo per supportare il livello di database, ad esempio le transazioni o di persistenza. (In particolare, non è necessario ereditare la [EntityObject](https://msdn.microsoft.com/library/system.data.objects.dataclasses.entityobject.aspx) classe.) È comunque possibile utilizzare le annotazioni dei dati per controllare come Entity Framework crea lo schema del database.

Poiché non sono dotati di qualsiasi proprietà aggiuntive che descrivono POCOs [stato del database](https://msdn.microsoft.com/library/system.data.entitystate.aspx), può facilmente essere serializzati in JSON o XML. Tuttavia, non implica che è sempre opportuno esporre i modelli di Entity Framework direttamente al client, come vedremo più avanti nell'esercitazione.

Verrà creata la POCOs seguenti:

- Prodotto
- Ordinamento
- OrderDetail

Per creare ogni classe, fare clic sulla cartella modelli in Esplora soluzioni. Dal menu di scelta rapida, selezionare **Aggiungi** e quindi selezionare **classe.**

![](using-web-api-with-entity-framework-part-2/_static/image1.png)

Aggiungere un `Product` classe con l'implementazione seguente:

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample1.cs)]

Per convenzione, Entity Framework Usa il `Id` proprietà come chiave primaria e ne esegue il mapping a una colonna identity nella tabella di database. Quando si crea un nuovo `Product` istanza, non impostare un valore per `Id`, perché il database genera il valore.

Il **ScaffoldColumn** attributo indica a ASP.NET MVC per ignorare la `Id` proprietà durante la generazione di un editor form. Il **necessari** attributo viene utilizzato per convalidare il modello. Specifica che il `Name` proprietà deve essere una stringa non vuota.

Aggiungere la `Order` classe:

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample2.cs)]

Aggiungere la `OrderDetail` classe:

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample3.cs)]

## <a name="foreign-key-relations"></a>Relazioni di chiave esterne

Un ordine contiene molti dettagli dell'ordine e i dettagli di ciascun ordine si riferisce a un singolo prodotto. Per rappresentare queste relazioni, la `OrderDetail` classe definisce le proprietà denominate `OrderId` e `ProductId`. Entity Framework dedurrà queste proprietà rappresentano le chiavi esterne che aggiungerà i vincoli di chiave esterna nel database.

![](using-web-api-with-entity-framework-part-2/_static/image2.png)

Il `Order` e `OrderDetail` classi includono inoltre la proprietà "spostamento", che contengono riferimenti agli oggetti correlati. Dato un ordine, è possibile passare ai prodotti nell'ordine seguendo le proprietà di navigazione.

Compilare il progetto ora. Entity Framework utilizza la reflection per individuare le proprietà dei modelli, pertanto richiede un assembly compilato creare lo schema del database.

## <a name="configure-the-media-type-formatters"></a>Configurare i formattatori di Media Type

Oggetto [formattatore di media type](../../formats-and-model-binding/media-formatters.md) è un oggetto che serializza i dati quando l'API Web scrive il corpo della risposta HTTP. I formattatori predefiniti supportano JSON e XML di output. Per impostazione predefinita, entrambi i formattatori serializzare tutti gli oggetti per valore.

Serializzazione da valore costituisce un problema se un oggetto grafico contiene riferimenti circolari. Che è esattamente il caso di `Order` e `OrderDetail` classi, perché ogni contiene un riferimento a altro. Il formattatore verrà seguire i riferimenti, la scrittura di ogni oggetto per valore e anche in cerchi. Pertanto, è necessario modificare il comportamento predefinito.

In Esplora soluzioni, espandere l'applicazione\_avviare cartella e aprire il file denominato WebApiConfig.cs. Aggiungere il codice seguente alla classe `WebApiConfig` :

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample4.cs?highlight=11)]

Questo codice imposta il formattatore JSON per mantenere i riferimenti all'oggetto e rimuove il formattatore XML dalla pipeline completamente. (È possibile configurare il formattatore XML per mantenere i riferimenti agli oggetti, ma è più lunga e dobbiamo solo JSON per questa applicazione. Per ulteriori informazioni, vedere [la gestione di riferimenti circolari oggetto](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).)

>[!div class="step-by-step"]
[Precedente](using-web-api-with-entity-framework-part-1.md)
[Successivo](using-web-api-with-entity-framework-part-3.md)
