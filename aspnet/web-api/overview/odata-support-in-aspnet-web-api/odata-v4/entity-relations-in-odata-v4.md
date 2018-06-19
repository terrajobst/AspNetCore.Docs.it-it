---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/entity-relations-in-odata-v4
title: Entity Relations in OData v4 con ASP.NET Web API 2.2 | Documenti Microsoft
author: MikeWasson
description: 'La maggior parte dei set di dati definiscono le relazioni tra entità: sono presenti ordini; un libro può avere autori; prodotti siano fornitori. Utilizzo di OData, i client è possono navigare nei...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2014
ms.topic: article
ms.assetid: 72657550-ec09-4779-9bfc-2fb15ecd51c7
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/entity-relations-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: 4127fb2fa83f513a4faeb222068ca99f234ece22
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
ms.locfileid: "26508100"
---
<a name="entity-relations-in-odata-v4-using-aspnet-web-api-22"></a>Entity Relations in OData v4 con ASP.NET Web API 2.2
====================
da [Mike Wasson](https://github.com/MikeWasson)

> La maggior parte dei set di dati definiscono le relazioni tra entità: sono presenti ordini; un libro può avere autori; prodotti siano fornitori. Utilizzando OData, i client possono passare tramite relazioni di entità. Dato un prodotto, è possibile trovare il fornitore. È anche possibile creare o rimuovere le relazioni. Ad esempio, è possibile impostare il fornitore per un prodotto.
> 
> Questa esercitazione viene illustrato come supportare queste operazioni in OData v4 mediante ASP.NET Web API. L'esercitazione si basa sull'esercitazione [creare un OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
> 
> 
> - 2.1 API Web
> - OData v4
> - [Visual Studio 2013 Update 2](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - Entity Framework 6
> - .NET 4.5
> 
> 
> ## <a name="tutorial-versions"></a>Versioni dell'esercitazione
> 
> Per OData versione 3, vedere [supporto Entity Relations in OData v3](https://asp.net/web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations).


## <a name="add-a-supplier-entity"></a>Aggiungere un'entità Supplier

> [!NOTE]
> L'esercitazione si basa sull'esercitazione [creare un OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md).


Innanzitutto, è necessaria un'entità correlata. Aggiungere una classe denominata `Supplier` nella cartella Models.

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample1.cs)]

Aggiungere una proprietà di navigazione per la `Product` classe:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample2.cs?highlight=13-15)]

Aggiungere un nuovo **DbSet** per la `ProductsContext` classe, in modo che Entity Framework includerà la tabella fornitore nel database.

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample3.cs?highlight=10)]

In WebApiConfig.cs, aggiungere un &quot;Suppliers&quot; del set di entità per entity data model:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample4.cs?highlight=6)]

## <a name="add-a-suppliers-controller"></a>Aggiungere un Controller di fornitori

Aggiungere un `SuppliersController` classe nella cartella controller.

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample5.cs)]

Non verrà illustrato come aggiungere le operazioni CRUD per questo controller. I passaggi sono gli stessi controller Products (vedere [creare un Endpoint di OData v4](create-an-odata-v4-endpoint.md)).

## <a name="getting-related-entities"></a>Recupero di entità correlate

Per ottenere il fornitore per un prodotto, il client invia una richiesta GET:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample6.cmd)]

Per supportare questa richiesta, aggiungere il metodo seguente alla `ProductsController` classe:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample7.cs)]

Questo metodo utilizza una convenzione di denominazione predefinito

- Nome del metodo: GetX, dove X è la proprietà di navigazione.
- Nome del parametro: *chiave*

Se si segue questa convenzione di denominazione, API Web esegue automaticamente il mapping della richiesta HTTP al metodo del controller.

Richiesta di esempio, HTTP:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample8.cmd)]

Risposta di esempio, HTTP:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample9.cmd)]

### <a name="getting-a-related-collection"></a>Recupero di una raccolta correlata

Nell'esempio precedente, un prodotto è un fornitore. Una proprietà di navigazione possa inoltre restituire una raccolta. Il codice seguente ottiene i prodotti per un fornitore:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample10.cs)]

In questo caso, il metodo restituisce un **IQueryable** anziché un **SingleResult&lt;T&gt;**

Richiesta di esempio, HTTP:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample11.cmd)]

Risposta di esempio, HTTP:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample12.cmd)]

## <a name="creating-a-relationship-between-entities"></a>Creazione di una relazione tra entità

OData supporta la creazione o la rimozione di relazioni tra due entità esistente. Nella terminologia di OData v4, la relazione è un &quot;riferimento&quot;. (OData v3, la relazione è stata chiamata una *collegamento*. Le differenze di protocollo non è rilevante per questa esercitazione.)

Un riferimento con il proprio URI, con il modulo `/Entity/NavigationProperty/$ref`. Di seguito è ad esempio, l'URI per risolvere il riferimento tra un prodotto e il fornitore:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample13.cmd)]

Per aggiungere una relazione, il client invia una richiesta POST o PUT a questo indirizzo.

- Se la proprietà di navigazione è una singola entità, ad esempio `Product.Supplier`.
- REGISTRA se la proprietà di navigazione è una raccolta, ad esempio `Supplier.Products`.

Il corpo della richiesta contiene l'URI di altra entità nella relazione. Di seguito è riportato un esempio di richiesta:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample14.cmd)]

In questo esempio, il client invia una richiesta PUT per `/Products(6)/Supplier/$ref`, che corrisponde all'URI $ref per il `Supplier` del prodotto con ID = 6. Se la richiesta ha esito positivo, il server invia una risposta 204 (nessun contenuto):

[!code-console[Main](entity-relations-in-odata-v4/samples/sample15.cmd)]

Ecco il metodo del controller per aggiungere una relazione a un `Product`:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample16.cs)]

Il *navigationProperty* parametro specifica la relazione da impostare. (Se è presente più di una proprietà di navigazione sull'entità, è possibile aggiungere più `case` istruzioni.)

Il *collegamento* parametro contiene l'URI del fornitore. API Web analizza automaticamente il corpo della richiesta per ottenere il valore per questo parametro.

Per cercare il fornitore, è necessario l'ID (o chiave), che fa parte di *collegamento* parametro. A tale scopo, utilizzare il metodo di supporto seguenti:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample17.cs)]

In pratica, questo metodo utilizza la libreria OData per suddividere il percorso dell'URI in segmenti, individuare il segmento che contiene la chiave e convertire la chiave nel tipo corretto.

## <a name="deleting-a-relationship-between-entities"></a>Eliminazione di una relazione tra entità

Per eliminare una relazione, il client invia una richiesta HTTP DELETE all'URI $ref:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample18.cmd)]

Ecco il metodo del controller per eliminare la relazione tra un prodotto e un fornitore:

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample19.cs)]

In questo caso, `Product.Supplier` è il &quot;1&quot; fine di una relazione 1-a-molti, pertanto è possibile eliminare la relazione solo impostando `Product.Supplier` a `null`.

Nel &quot;molti&quot; fine di una relazione, il client è necessario specificare quali correlati entità da rimuovere. A tale scopo, il client invia l'URI dell'entità correlata nella stringa di query della richiesta. Ad esempio, per rimuovere "Prodotto 1" da "1" fornitore:

[!code-console[Main](entity-relations-in-odata-v4/samples/sample20.cmd?highlight=1)]

A tale scopo nell'API Web, è necessario includere un parametro supplementare nel `DeleteRef` metodo. Ecco il metodo del controller per un prodotto da rimuovere il `Supplier.Products` relazione.

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample21.cs)]

Il *chiave* parametro è la chiave per il fornitore e il *relatedKey* parametro è la chiave per il prodotto rimuovere il `Products` relazione. Si noti che API Web ottiene automaticamente la chiave dalla stringa di query.
