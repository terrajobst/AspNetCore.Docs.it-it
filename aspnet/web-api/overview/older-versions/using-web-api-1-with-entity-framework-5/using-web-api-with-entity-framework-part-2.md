---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
title: 'Parte 2: Creazione di modelli di dominio | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/03/2012
ms.topic: article
ms.assetid: fe3ef85f-bdc6-4e10-9768-25aa565c01d0
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-2
msc.type: authoredcontent
ms.openlocfilehash: 9f379c92f7795c9fe10f7860b72188a8cfc1b6d2
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37379725"
---
<a name="part-2-creating-the-domain-models"></a>Parte 2: Creazione di modelli di dominio
====================
da [Mike Wasson](https://github.com/MikeWasson)

[Download progetto completato](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-models"></a>Aggiungere modelli

Esistono tre modi per approccio Entity Framework:

- Database-first: iniziare con un database ed Entity Framework genera il codice.
- Model-first: iniziare con un modello visivo ed Entity Framework genera il codice sia il database.
- Code first: iniziare con il codice ed Entity Framework genera il database.

Viene usato l'approccio code first, iniziamo definendo gli oggetti di dominio come oggetti poco (plain-old CLR Object). Con l'approccio code first, gli oggetti di dominio non necessario alcun codice aggiuntivo per supportare il livello di database, ad esempio le transazioni o di persistenza. (In particolare, non è necessario ereditare il [EntityObject](https://msdn.microsoft.com/library/system.data.objects.dataclasses.entityobject.aspx) classe.) È comunque possibile usare le annotazioni dei dati per controllare come Entity Framework crea lo schema del database.

Perché non sono dotati di qualsiasi proprietà aggiuntiva che descrivono oggetti poco [stato del database](https://msdn.microsoft.com/library/system.data.entitystate.aspx), sono facilmente può essere serializzati come JSON o XML. Tuttavia, ciò non significa che è sempre opportuno esporre direttamente i modelli Entity Framework nel client, come vedremo più avanti nell'esercitazione.

Si creerà gli oggetti poco seguenti:

- Prodotto
- Ordinamento
- OrderDetail

Per creare ogni classe, fare clic sulla cartella modelli in Esplora soluzioni. Dal menu di scelta rapida, selezionare **Add** e quindi selezionare **classe.**

![](using-web-api-with-entity-framework-part-2/_static/image1.png)

Aggiungere un `Product` classe con l'implementazione seguente:

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample1.cs)]

Per convenzione, Entity Framework Usa il `Id` proprietà come chiave primaria e ne esegue il mapping a una colonna identity nella tabella di database. Quando si crea una nuova `Product` istanza, si non imposta un valore per `Id`, perché il database genera il valore.

Il **ScaffoldColumn** attributo indica a MVC ASP.NET per ignorare il `Id` proprietà durante la generazione di un form dell'editor. Il **necessari** attributo viene usato per convalidare il modello. Specifica che il `Name` proprietà deve essere una stringa non vuota.

Aggiungere il `Order` classe:

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample2.cs)]

Aggiungere il `OrderDetail` classe:

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample3.cs)]

## <a name="foreign-key-relations"></a>Relazioni di chiave esterne

Un ordine contiene molti dettagli dell'ordine e i dettagli di ciascun ordine si riferisce a un singolo prodotto. Per rappresentare le relazioni e le `OrderDetail` classe definisce le proprietà denominate `OrderId` e `ProductId`. Entity Framework verrà dedotto che queste proprietà rappresentano chiavi esterne e aggiungerà i vincoli di chiave esterna nel database.

![](using-web-api-with-entity-framework-part-2/_static/image2.png)

Il `Order` e `OrderDetail` classi includono inoltre la proprietà "spostamento", che contenga riferimenti agli oggetti correlati. Dato un ordine, è possibile passare ai prodotti nell'ordine seguendo le proprietà di navigazione.

A questo punto, compilare il progetto. Entity Framework Usa la reflection per individuare le proprietà dei modelli, pertanto è necessario un assembly compilato creare lo schema del database.

## <a name="configure-the-media-type-formatters"></a>Configurare i formattatori di Media Type

Oggetto [formattatore di media type](../../formats-and-model-binding/media-formatters.md) è un oggetto che serializza i dati quando si crea il corpo della risposta HTTP API Web. I formattatori predefiniti supportano l'output JSON e XML. Per impostazione predefinita, entrambi i formattatori serializza tutti gli oggetti per valore.

Serializzazione dal valore crea un problema se un oggetto grafico contiene i riferimenti circolari. Che avviene esattamente con le `Order` e `OrderDetail` classi, perché ognuno contiene un riferimento a altro. Il formattatore verrà seguire i riferimenti, la scrittura di ogni oggetto per valore che nelle cerchi. Pertanto, è necessario modificare il comportamento predefinito.

In Esplora soluzioni espandere l'App\_avviare cartella e aprire il file WebApiConfig.cs. Aggiungere il codice seguente alla classe `WebApiConfig` :

[!code-csharp[Main](using-web-api-with-entity-framework-part-2/samples/sample4.cs?highlight=11)]

Questo codice imposta il formattatore JSON per conservare i riferimenti all'oggetto e rimuove completamente il formattatore XML dalla pipeline. (È possibile configurare il formattatore XML per mantenere riferimenti a oggetti, ma è un po' più, e occorre solo JSON per questa applicazione. Per altre informazioni, vedere [gestisce i riferimenti all'oggetto circolare](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).)

> [!div class="step-by-step"]
> [Precedente](using-web-api-with-entity-framework-part-1.md)
> [Successivo](using-web-api-with-entity-framework-part-3.md)
