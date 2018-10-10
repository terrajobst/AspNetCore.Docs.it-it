---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
title: La chiamata a un servizio OData da un Client .NET (c#) | Microsoft Docs
author: MikeWasson
description: Questa esercitazione illustra come chiamare un servizio OData da un'applicazione client in c#. Versioni del software utilizzate nell'esercitazione Visual Studio 2013 (compatibile con Visual S...
ms.author: riande
ms.date: 02/26/2014
ms.assetid: 6f448917-ad23-4dcc-9789-897fad74051b
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: 75f8e3eab7bd5667bbdcccbb5ae8a8e5b1f5fdba
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912052"
---
<a name="calling-an-odata-service-from-a-net-client-c"></a>La chiamata a un servizio OData da un Client .NET (c#)
====================
da [Mike Wasson](https://github.com/MikeWasson)

[Download progetto completato](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> Questa esercitazione illustra come chiamare un servizio OData da un'applicazione client in c#.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software utilizzate nell'esercitazione
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013) (funziona con Visual Studio 2012)
> - [Libreria client WCF Data Services](https://msdn.microsoft.com/library/cc668772.aspx)
> - API Web 2. (Il servizio OData di esempio viene compilato usando l'API Web 2, ma l'applicazione client non dipende da API Web).


In questa esercitazione verrà illustrato come creare un'applicazione client che chiama un servizio OData. Il servizio OData espone le entità seguenti:

- `Product`
- `Supplier`
- `ProductRating`

![](calling-an-odata-service-from-a-net-client/_static/image1.png)

Gli articoli seguenti descrivono come implementare il servizio OData nell'API Web. (Non è necessario leggere in modo da tenere presente, tuttavia questa esercitazione.)

- [Creazione di un OData Endpoint nell'API Web 2](creating-an-odata-endpoint.md)
- [Relazioni di entità OData nell'API Web 2](working-with-entity-relations.md)
- [Azioni OData nell'API Web 2](odata-actions.md)

## <a name="generate-the-service-proxy"></a>Generare il Proxy del servizio

Il primo passaggio è generare un proxy del servizio. Il proxy del servizio è una classe .NET che definisce i metodi per l'accesso al servizio OData. Il proxy converte le chiamate di metodo nelle richieste HTTP.

![](calling-an-odata-service-from-a-net-client/_static/image2.png)

Iniziare aprendo il progetto di servizio OData in Visual Studio. Premere CTRL+F5 per eseguire il servizio localmente in IIS Express. Annotare l'indirizzo locale, incluso il numero di porta assegnato dal Visual Studio. È questo indirizzo sarà necessario quando si crea il proxy.

Successivamente, aprire un'altra istanza di Visual Studio e creare un progetto di applicazione console. L'applicazione console sarà l'applicazione client OData. (È possibile anche aggiungere il progetto per la stessa soluzione del servizio.)

> [!NOTE]
> I passaggi rimanenti, vedere il progetto console.


In Esplora soluzioni fare doppio clic su **riferimenti** e selezionare **Aggiungi riferimento al servizio**.

![](calling-an-odata-service-from-a-net-client/_static/image3.png)

Nel **Aggiungi riferimento al servizio** finestra di dialogo digitare l'indirizzo del servizio OData:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample1.cmd)]

in cui *porta* è il numero di porta.

[![](calling-an-odata-service-from-a-net-client/_static/image5.png)](calling-an-odata-service-from-a-net-client/_static/image4.png)

Per la **Namespace**, digitare "ProductService". Questa opzione consente di definire lo spazio dei nomi della classe proxy.

Fare clic su **Vai**. Visual Studio legge il documento di metadati OData per individuare le entità nel servizio.

[![](calling-an-odata-service-from-a-net-client/_static/image7.png)](calling-an-odata-service-from-a-net-client/_static/image6.png)

Fare clic su **OK** per aggiungere la classe proxy al progetto.

![](calling-an-odata-service-from-a-net-client/_static/image8.png)

## <a name="create-an-instance-of-the-service-proxy-class"></a>Creare un'istanza della classe Proxy del servizio

All'interno di `Main` metodo, creare una nuova istanza della classe proxy, come indicato di seguito:

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample2.cs)]

Anche in questo caso, usare il numero di porta effettivo in cui è in esecuzione il servizio. Quando si distribuisce il servizio, si userà l'URI del servizio in tempo reale. Non è necessario aggiornare il proxy.

Il codice seguente aggiunge un gestore eventi che consente di stampare gli URI di richiesta alla finestra della console. Questo passaggio non è obbligatorio, ma è interessante visualizzare gli URI per ogni query.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample3.cs)]

## <a name="query-the-service"></a>Eseguire query sul servizio

Il codice seguente ottiene l'elenco dei prodotti dal servizio OData.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample4.cs)]

Si noti che non devi scrivere codice per inviare la richiesta HTTP o analizzare la risposta. La classe proxy non comporta automaticamente quando si enumerano le `Container.Products` insieme nella **foreach** ciclo.

Quando si esegue l'applicazione, l'output dovrebbe essere simile al seguente:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample5.cmd)]

Per ottenere un'entità dall'ID, utilizzare un `where` clausola.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample6.cs)]

Per il resto di questo argomento, non verrà visualizzata l'intera `Main` funzioni, solo il codice necessario per chiamare il servizio.

## <a name="apply-query-options"></a>Applicare le opzioni di Query

OData definisce [opzioni query](../supporting-odata-query-options.md) che può essere utilizzato da filtro, ordinamento, paging dei dati e così via. Nel proxy del servizio, è possibile applicare queste opzioni tramite diverse espressioni LINQ.

In questa sezione verrà illustrato negli esempi brevi. Per altre informazioni, vedere l'argomento [considerazioni su LINQ (WCF Data Services)](https://msdn.microsoft.com/library/ee622463.aspx) su MSDN.

### <a name="filtering-filter"></a>Filtro ($filter)

Per filtrare, usare un `where` clausola. L'esempio seguente Filtra per categoria di prodotto.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample7.cs)]

Questo codice corrisponde alla query OData seguenti.

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample8.cmd)]

Si noti che il proxy consente di convertire le `where` clausola OData `$filter` espressione.

### <a name="sorting-orderby"></a>L'ordinamento ($orderby)

Per ordinare, utilizzare un `orderby` clausola. Nell'esempio seguente vengono ordinati in base al prezzo, dal maggiore al minore.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample9.cs)]

Ecco la richiesta di OData corrispondente.

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample10.cmd)]

### <a name="client-side-paging-skip-and-top"></a>Paging sul lato client ($skip e $top)

Per i set di entità di grandi dimensioni, il client potrebbe voler limitare il numero di risultati. Ad esempio, un client potrebbe mostrare 10 voci alla volta. Questa operazione viene definita *paging sul lato client*. (È anche disponibile [paging lato server](../supporting-odata-query-options.md#server-paging), in cui il server limita il numero di risultati.) Per eseguire il paging del lato client, utilizzare LINQ **Skip** e **richiedere** metodi. Nell'esempio seguente ignora i primi 40 risultati e accetta i 10 successivi.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample11.cs)]

Ecco la richiesta di OData corrispondente:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample12.cmd)]

### <a name="select-select-and-expand-expand"></a>($Select) Select ed Expand ($expand)

Per includere le entità correlate, usare il `DataServiceQuery<t>.Expand` (metodo). Ad esempio, per includere il `Supplier` per ogni `Product`:

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample13.cs)]

Ecco la richiesta di OData corrispondente:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample14.cmd)]

Per modificare la forma della risposta, utilizzare LINQ **seleziona** clausola. Nell'esempio seguente ottiene solo il nome di ogni prodotto, senza altre proprietà.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample15.cs)]

Ecco la richiesta di OData corrispondente:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample16.cmd)]

Una clausola select può includere le entità correlate. In tal caso, non chiamare **Espandi**; il proxy include automaticamente l'espansione in questo caso. Nell'esempio seguente ottiene il nome e il fornitore di ogni prodotto.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample17.cs)]

Ecco la richiesta di OData corrispondente. Si noti che include il **$expand** opzione.

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample18.cmd)]

Per altre informazioni su $select e $espandere, vedere [Usa $select, $expand, $value nell'API Web 2 e](../using-select-expand-and-value.md).

## <a name="add-a-new-entity"></a>Aggiungere una nuova entità

Per aggiungere una nuova entità in un set di entità, chiamare `AddToEntitySet`, dove *EntitySet* è il nome del set di entità. Ad esempio, `AddToProducts` aggiunge un nuovo `Product` per il `Products` set di entità. Quando si genera il proxy, WCF Data Services crea automaticamente questi fortemente tipizzate **AddTo** metodi.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample19.cs)]

Per aggiungere un collegamento tra due entità, usare il **AddLink** e **SetLink può essere utilizzato** metodi. Il codice seguente aggiunge un nuovo fornitore e un nuovo prodotto e quindi crea i collegamenti tra di essi.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample20.cs)]

Uso **AddLink** quando la proprietà di navigazione è una raccolta. In questo esempio, sarà presto disponibile un prodotto per il `Products` insieme al fornitore.

Uso **SetLink può essere utilizzato** quando la proprietà di navigazione è una singola entità. In questo esempio viene impostata la `Supplier` proprietà su un prodotto.

## <a name="update--patch"></a>Aggiornamento / Patch

Per aggiornare un'entità, chiamare il **UpdateObject** (metodo).

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample21.cs)]

L'aggiornamento viene eseguito quando si chiama **SaveChanges**. Per impostazione predefinita, WCF invia una richiesta HTTP MERGE. Il **PatchOnUpdate** opzione indica a WCF per inviare invece un HTTP PATCH.

> [!NOTE]
> Il motivo per cui applicare la PATCH e MERGE? La specifica HTTP 1.1 originale ([2616 RCF](http://tools.ietf.org/html/rfc2616)) non è stato definito alcun metodo HTTP con la semantica di "aggiornamento parziale". Per supportare gli aggiornamenti parziali, la specifica OData definito il metodo MERGE. Nel 2010 [RFC 5789](http://tools.ietf.org/html/rfc5789) definito il metodo PATCH per gli aggiornamenti parziali. È possibile leggere alcuni della cronologia in questo [post di blog](https://blogs.msdn.com/b/astoriateam/archive/2008/05/20/merge-vs-replace-semantics-for-update-operations.aspx) sul Blog di WCF Data Services. Oggi, PATCH è preferibile rispetto a MERGE. Il controller OData creato tramite lo scaffolding API Web supporta entrambi i metodi.


Se si desidera sostituire l'intera entità (la semantica di PUT), specificare il **ReplaceOnUpdate** opzione. Ciò fa sì che WCF inviare una richiesta HTTP PUT.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample22.cs)]

## <a name="delete-an-entity"></a>Eliminare un'entità

Per eliminare un'entità, chiamare **DeleteObject**.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample23.cs)]

## <a name="invoke-an-odata-action"></a>Richiamare un'azione OData

In OData [azioni](odata-actions.md) rappresentano un modo per aggiungere comportamenti lato server che non sono definiti facilmente come operazioni CRUD su entità.

Anche se il documento di metadati OData descrive le azioni, la classe proxy non crea alcun metodo fortemente tipizzata per loro. È comunque possibile richiamare un'azione OData utilizzando il tipo generico **Execute** (metodo). Tuttavia, è necessario conoscere i tipi di dati dei parametri e il valore restituito.

Ad esempio, il `RateProduct` azione accetta parametro denominato "Rating" di tipo `Int32` e restituisce un `double`. Il codice seguente viene illustrato come richiamare l'azione.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample24.cs)]

Per altre informazioni, vedere[chiamare operazioni del servizio e le azioni](https://msdn.microsoft.com/library/hh230677.aspx).

È possibile estendere il **contenitore** classe per fornire un metodo fortemente tipizzato che richiama l'azione:

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample25.cs)]
