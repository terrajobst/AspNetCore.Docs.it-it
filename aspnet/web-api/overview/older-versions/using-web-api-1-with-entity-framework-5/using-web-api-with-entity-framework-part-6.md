---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
title: 'Parte 6: Creazione di prodotti e i controller di ordine | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: 91ee29ee-0689-40ee-914a-e7dd733b6622
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
msc.type: authoredcontent
ms.openlocfilehash: 642ff4554ed3664af0b5cc8e49d6b236c568131b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826312"
---
<a name="part-6-creating-product-and-order-controllers"></a>Parte 6: Creazione di prodotto e i controller di ordine
====================
da [Mike Wasson](https://github.com/MikeWasson)

[Download progetto completato](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-a-products-controller"></a>Aggiungere un Controller di prodotti

Il controller di amministrazione è per gli utenti che dispongono dei privilegi di amministratore. I clienti, d'altra parte, possono visualizzare i prodotti ma non è possibile creare, aggiornare o eliminarli.

È possibile limitare facilmente l'accesso ai metodi Post, Put e Delete, lasciando i metodi Get aperto. Tuttavia, esaminare i dati che viene restituiti per un prodotto:

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample1.json?highlight=1)]

Il `ActualCost` proprietà non deve essere visibile ai clienti. La soluzione consiste nel definire un *oggetto di trasferimento dati* (DTO) che include un subset di proprietà che devono essere visibili ai clienti. Si userà LINQ al progetto `Product` alle istanze `ProductDTO` istanze.

Aggiungere una classe denominata `ProductDTO` nella cartella Models.

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample2.cs)]

A questo punto aggiungere il controller. In Esplora soluzioni fare clic sulla cartella Controllers. Selezionare **Add**, quindi selezionare **Controller**. Nel **Aggiungi Controller** finestra di dialogo nome al controller &quot;ProductsController&quot;. Sotto **modello**, selezionare **controller API vuoto**.

![](using-web-api-with-entity-framework-part-6/_static/image1.png)

Sostituire tutto il contenuto nel file di origine con il codice seguente:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample3.cs)]

Il controller Usa ancora il `OrdersContext` eseguire query sul database. Ma anziché restituire `Product` istanze direttamente, chiamiamo `MapProducts` proiettare in `ProductDTO` istanze:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample4.cs?highlight=1)]

Il `MapProducts` metodo restituisce un **IQueryable**, pertanto è possibile comporre i risultati con altri parametri di query. È possibile verificarlo nel `GetProduct` metodo, che aggiunge una **dove** clausola per la query:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample5.cs?highlight=2)]

## <a name="add-an-orders-controller"></a>Aggiungere un Controller degli ordini

Successivamente, aggiungere un controller che consente agli utenti di creare e visualizzare gli ordini.

Si inizierà con un altro oggetto DTO. In Esplora soluzioni fare doppio clic su cartella Models e aggiungere una classe denominata `OrderDTO` usare l'implementazione seguente:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample6.cs)]

A questo punto aggiungere il controller. In Esplora soluzioni fare clic sulla cartella Controllers. Selezionare **Add**, quindi selezionare **Controller**. Nel **Aggiungi Controller** finestra di dialogo impostare le opzioni seguenti:

- Sotto **nome del Controller**, immettere "OrdersController".
- Sotto **modello**, selezionare "Controller API con azioni di lettura/scrittura, che usa Entity Framework".
- Sotto **classe modello**, selezionare &quot;ordine (ProductStore.Models)&quot;.
- Sotto **alla classe contesto dati**, selezionare &quot;OrdersContext (ProductStore.Models)&quot;.

![](using-web-api-with-entity-framework-part-6/_static/image2.png)

Fare clic su **Aggiungi**. Verrà aggiunto un file denominato OrdersController.cs. Successivamente, è necessario modificare l'implementazione predefinita del controller.

Eliminare innanzitutto le `PutOrder` e `DeleteOrder` metodi. Per questo esempio, i clienti non possono modificare o eliminare gli ordini esistenti. In un'applicazione reale, è necessario un numero elevato di logica di back-end per gestire questi casi. (Ad esempio, è stato eseguito l'ordine già?)

Modifica il `GetOrders` per restituire solo gli ordini che appartengono all'utente:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample7.cs)]

Modifica il `GetOrder` metodo come indicato di seguito:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample8.cs)]

Ecco le modifiche apportate al metodo:

- Il valore restituito è un `OrderDTO` istanza, invece di un `Order`.
- Quando si esegue una query del database per l'ordine, usiamo il [DbQuery.Include](https://msdn.microsoft.com/library/gg696395) metodo per recuperare i relativi `OrderDetail` e `Product` entità.
- È rendere flat il risultato usando una proiezione.

La risposta HTTP contiene una matrice di prodotti con quantità:

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample9.json)]

Questo formato è più semplice per i client da usare rispetto l'originale oggetto grafico, che contiene entità annidate (ordine, dettagli e i prodotti).

Quest'ultimo metodo prenderla in considerazione `PostOrder`. Al momento, questo metodo accetta un `Order` istanza. Ma si consideri che cosa accade se un client invia un corpo della richiesta simile alla seguente:

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample10.json)]

Si tratta di un ordine ben strutturato ed Entity Framework verrà Fortunatamente inserirlo nel database. Ma contiene un'entità Product che non esisteva in precedenza. Il client appena creato un nuovo prodotto nel nostro database! Questo sarà una grande sorpresa al reparto di produzione di ordine, quando viene visualizzato un ordine per orsi koala. È la morale, essere realmente necessario prestare particolare attenzione i dati che si accetta una richiesta POST o PUT.

Per evitare questo problema, modificare il `PostOrder` metodo per richiedere un `OrderDTO` istanza. Usare la `OrderDTO` per creare il `Order`.

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample11.cs)]

Si noti che viene usato il `ProductID` e `Quantity` proprietà e ignorare tutti i valori che il client ha inviato per il nome del prodotto o prezzo. Se l'ID prodotto non è valido, violeranno il vincolo di chiave esterna nel database e l'inserimento avrà esito negativo, come previsto.

Di seguito è riportato l'intero `PostOrder` metodo:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample12.cs)]

Infine, aggiungere il **Authorize** attributo al controller:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample13.cs)]

Solo gli utenti registrati a questo punto possono creare o visualizzare gli ordini.

> [!div class="step-by-step"]
> [Precedente](using-web-api-with-entity-framework-part-5.md)
> [Successivo](using-web-api-with-entity-framework-part-7.md)
