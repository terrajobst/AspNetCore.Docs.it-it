---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
title: Supporto Entity Relations in OData v3 con Web API 2 | Documenti Microsoft
author: MikeWasson
description: 'La maggior parte dei set di dati definiscono le relazioni tra entità: sono presenti ordini; un libro può avere autori; prodotti siano fornitori. Utilizzo di OData, i client è possono navigare nei...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/26/2014
ms.topic: article
ms.assetid: 1e4c2eb4-b6cf-42ff-8a65-4d71ddca0394
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
msc.type: authoredcontent
ms.openlocfilehash: dec7e10e59cc2441c967afe062df227b105106a1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
ms.locfileid: "26508410"
---
<a name="supporting-entity-relations-in-odata-v3-with-web-api-2"></a>Supporto Entity Relations in OData v3 con Web API 2
====================
da [Mike Wasson](https://github.com/MikeWasson)

[Scaricare il progetto completato](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> La maggior parte dei set di dati definiscono le relazioni tra entità: sono presenti ordini; un libro può avere autori; prodotti siano fornitori. Utilizzando OData, i client possono passare tramite relazioni di entità. Dato un prodotto, è possibile trovare il fornitore. È anche possibile creare o rimuovere le relazioni. Ad esempio, è possibile impostare il fornitore per un prodotto.
> 
> In questa esercitazione viene illustrato come supportare queste operazioni nell'API Web ASP.NET. L'esercitazione si basa sull'esercitazione [creazione di un Endpoint di OData v3 con Web API 2](creating-an-odata-endpoint.md).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - Web API 2
> - OData versione 3
> - Entity Framework 6


## <a name="add-a-supplier-entity"></a>Aggiungere un'entità Supplier

È necessario prima aggiungere un nuovo tipo di entità per il feed OData. Verrà aggiunto un `Supplier` classe.

[!code-csharp[Main](working-with-entity-relations/samples/sample1.cs)]

Questa classe viene utilizzata una stringa per la chiave di entità. In pratica, che potrebbe essere meno comune rispetto all'utilizzo di una chiave di tipo integer. Ma vale la pena vedere come OData gestisce altri tipi di chiave oltre ai numeri interi.

Successivamente, si creerà una relazione mediante l'aggiunta di un `Supplier` proprietà per il `Product` classe:

[!code-csharp[Main](working-with-entity-relations/samples/sample2.cs)]

Aggiungere un nuovo **DbSet** per il `ProductServiceContext` classe, in modo che Entity Framework includerà il `Supplier` tabella nel database.

[!code-csharp[Main](working-with-entity-relations/samples/sample3.cs?highlight=9)]

In WebApiConfig.cs, aggiungere un'entità "Suppliers" per il modello EDM:

[!code-csharp[Main](working-with-entity-relations/samples/sample4.cs?highlight=4)]

## <a name="navigation-properties"></a>Proprietà di navigazione

Per ottenere il fornitore per un prodotto, il client invia una richiesta GET:

[!code-console[Main](working-with-entity-relations/samples/sample5.cmd)]

Di seguito "Fornitore" è una proprietà di navigazione nel `Product` tipo. In questo caso, `Supplier` fa riferimento a un singolo elemento, ma una navigazione può inoltre restituire una raccolta (relazione uno-a-molti o molti-a-molti).

Per supportare questa richiesta, aggiungere il metodo seguente alla `ProductsController` classe:

[!code-csharp[Main](working-with-entity-relations/samples/sample6.cs)]

Il *chiave* parametro è la chiave del prodotto. Il metodo restituisce l'entità correlata & #8212 in questo caso, un `Supplier` istanza. Il nome del metodo e il nome di parametro sono importanti. In generale, se la proprietà di navigazione è denominata "X", è necessario aggiungere un metodo denominato "GetX". Il metodo deve accettare un parametro denominato "*chiave*" che corrisponde al tipo di dati della chiave dell'elemento padre.

È anche importante includere il **[FromOdataUri]** attributo la *chiave* parametro. Questo attributo indica l'API Web per utilizzare le regole di sintassi di OData quando analizza la chiave dall'URI della richiesta.

## <a name="creating-and-deleting-links"></a>Creazione ed eliminazione di collegamenti

OData supporta la creazione o la rimozione di relazioni tra due entità. Nella terminologia di OData, la relazione è un "collegamento". Ogni collegamento dispone di un URI con formato *entità*/$links /*entità*. Ad esempio, il collegamento dal prodotto fornitore simile al seguente:

[!code-console[Main](working-with-entity-relations/samples/sample7.cmd)]

Per creare un nuovo collegamento, il client invia una richiesta POST per l'URI del collegamento. Il corpo della richiesta è l'URI dell'entità di destinazione. Si supponga, ad esempio, che è un fornitore con la chiave "CTSO". Per creare un collegamento da "Product(1)" a "Supplier('CTSO')", il client invia una richiesta simile al seguente:

[!code-console[Main](working-with-entity-relations/samples/sample8.cmd)]

Per eliminare un collegamento, il client invia una richiesta di eliminazione per l'URI del collegamento.

**Creazione di collegamenti**

Per abilitare un client creare collegamenti fornitore del prodotto, aggiungere il codice seguente per la `ProductsController` classe:

[!code-csharp[Main](working-with-entity-relations/samples/sample9.cs)]

Questo metodo accetta tre parametri:

- *chiave*: la chiave per l'entità padre (prodotto)
- *navigationProperty*: il nome della proprietà di navigazione. In questo esempio, la proprietà di navigazione solo valido è "Fornitore".
- *collegamento*: URI OData dell'entità correlata. Questo valore viene recuperato dal corpo della richiesta. Ad esempio, il collegamento URI potrebbe essere "`http://localhost/odata/Suppliers('CTSO')`, vale a dire il fornitore con ID = 'CTSO'.

Il metodo utilizza il collegamento per cercare il fornitore. Se il fornitore corrispondente viene trovato, il metodo imposta la `Product.Supplier` proprietà e il risultato viene salvato nel database.

La parte più difficile viene analizzato l'URI del collegamento. In pratica, è necessario simulare il risultato dell'invio di una richiesta GET a quell'URI. Il seguente metodo helper viene illustrato come eseguire questa operazione. Il metodo richiama il processo di routing di API Web e consente di ottenere un **ODataPath** istanza che rappresenta il percorso OData analizzato. Per un URI del collegamento, uno dei segmenti deve essere la chiave di entità. (In caso contrario il client ha inviato un URI non valido.)

[!code-csharp[Main](working-with-entity-relations/samples/sample10.cs)]

**L'eliminazione di collegamenti**

Per eliminare un collegamento, aggiungere il codice seguente per la `ProductsController` classe:

[!code-csharp[Main](working-with-entity-relations/samples/sample11.cs)]

In questo esempio, la proprietà di navigazione è un singolo `Supplier` entità. Se la proprietà di navigazione è una raccolta, l'URI per eliminare un collegamento deve includere una chiave per l'entità correlata. Ad esempio:

[!code-console[Main](working-with-entity-relations/samples/sample12.cmd)]

Questa richiesta rimuove ordine 1 dal cliente 1. In questo caso, il metodo DeleteLink possono essere utilizzati avrà la firma seguente:

[!code-csharp[Main](working-with-entity-relations/samples/sample13.cs)]

Il *relatedKey* parametro fornisce la chiave per l'entità correlata. In tal caso il `DeleteLink` (metodo), l'entità primaria per la ricerca di *chiave* parametro, trovare l'entità correlata dal *relatedKey* parametro e quindi rimuovere l'associazione. A seconda del modello di dati, potrebbe essere necessario implementare entrambe le versioni di `DeleteLink`. API Web chiamerà la versione corretta in base all'URI della richiesta.
