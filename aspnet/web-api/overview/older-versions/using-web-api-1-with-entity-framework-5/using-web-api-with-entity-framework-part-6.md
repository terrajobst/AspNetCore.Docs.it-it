---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
title: 'Parte 6: Creazione di prodotto e ordine controller | Documenti Microsoft'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 91ee29ee-0689-40ee-914a-e7dd733b6622
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
msc.type: authoredcontent
ms.openlocfilehash: 6bd485d29821af12b9ebe31b2d04a2d9ab826731
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869387"
---
<a name="part-6-creating-product-and-order-controllers"></a>Parte 6: Creazione di prodotto e il controller di ordine
====================
da [Mike Wasson](https://github.com/MikeWasson)

[Scaricare il progetto completato](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-a-products-controller"></a>Aggiungere un Controller di prodotti

Il controller di amministratore sia per gli utenti che dispongono dei privilegi di amministratore. I clienti, d'altra parte, possono visualizzare i prodotti ma non è possibile creare, aggiornare o eliminarli.

È possibile limitare facilmente l'accesso ai metodi Post, Put e Delete, lasciando aperto i metodi Get. Tuttavia, esaminare i dati che viene restituiti per un prodotto:

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample1.json?highlight=1)]

Il `ActualCost` proprietà non deve essere visibile ai clienti. La soluzione consiste nel definire un *oggetto di trasferimento dati* (DTO) che include un subset di proprietà che devono essere visibili ai clienti. Si utilizzerà LINQ al progetto `Product` istanze per `ProductDTO` istanze.

Aggiungere una classe denominata `ProductDTO` nella cartella Models.

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample2.cs)]

Aggiungere il controller. In Esplora soluzioni, fare clic sulla cartella controller. Selezionare **Aggiungi**, quindi selezionare **Controller**. Nel **Aggiungi Controller** finestra di dialogo, nome del controller &quot;ProductsController&quot;. In **modello**selezionare **controller API vuoto**.

![](using-web-api-with-entity-framework-part-6/_static/image1.png)

Sostituire tutto il contenuto nel file di origine con il codice seguente:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample3.cs)]

Il controller utilizza ancora la `OrdersContext` per eseguire query sul database. Ma anziché restituire `Product` istanze direttamente, definiamo `MapProducts` al progetto in `ProductDTO` istanze:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample4.cs?highlight=1)]

Il `MapProducts` metodo restituisce un **IQueryable**, pertanto è possibile comporre il risultato con altri parametri di query. È possibile visualizzare nel `GetProduct` metodo che aggiunge un **dove** clausola alla query:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample5.cs?highlight=2)]

## <a name="add-an-orders-controller"></a>Aggiungere un Controller di ordini

Successivamente, aggiungere un controller che consente agli utenti di creare e visualizzare gli ordini.

Si inizierà con un altro DTO. In Esplora soluzioni, fare clic sulla cartella di modelli e aggiungere una classe denominata `OrderDTO` usare l'implementazione seguente:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample6.cs)]

Aggiungere il controller. In Esplora soluzioni, fare clic sulla cartella controller. Selezionare **Aggiungi**, quindi selezionare **Controller**. Nel **Aggiungi Controller** finestra di dialogo, impostare le opzioni seguenti:

- In **nome Controller**, immettere "OrdersController".
- In **modello**, selezionare "Controller API con azioni di lettura/scrittura, mediante Entity Framework".
- In **classe modello**selezionare &quot;ordine (ProductStore.Models)&quot;.
- In **classe contesto dati**selezionare &quot;OrdersContext (ProductStore.Models)&quot;.

![](using-web-api-with-entity-framework-part-6/_static/image2.png)

Fare clic su **Aggiungi**. Consente di aggiungere un file denominato OrdersController.cs. Successivamente, è necessario modificare l'implementazione predefinita del controller.

Eliminare prima il `PutOrder` e `DeleteOrder` metodi. Per questo esempio, i clienti non è possibile modificare o eliminare gli ordini esistenti. In un'applicazione reale, è necessario un numero elevato di logica di back-end per gestire questi casi. (Ad esempio, è stato eseguito l'ordine già?)

Modifica il `GetOrders` per restituire solo gli ordini che appartengono all'utente:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample7.cs)]

Modifica il `GetOrder` metodo come indicato di seguito:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample8.cs)]

Di seguito sono le modifiche apportate al metodo:

- Il valore restituito è un `OrderDTO` istanza, invece di un `Order`.
- Quando si esegue una query del database per l'ordine, utilizziamo la [DbQuery.Include](https://msdn.microsoft.com/library/gg696395) metodo per recuperare l'oggetto correlato `OrderDetail` e `Product` entità.
- Il risultato è bidimensionale utilizzando una proiezione.

La risposta HTTP conterrà una matrice di prodotti con quantità:

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample9.json)]

Questo formato è più semplice per i client di utilizzare il grafico di oggetto originale, che contiene le entità annidate (ordine, dettagli e prodotti).

L'ultimo metodo prenderla in considerazione `PostOrder`. Attualmente, questo metodo accetta un `Order` istanza. Ma si consideri cosa accade se un client invia un corpo della richiesta simile al seguente:

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample10.json)]

Si tratta di un ordine ben strutturato ed Entity Framework verrà tranquillamente inserirlo nel database. Ma contiene un'entità Product che non esisteva in precedenza. Il client appena creato un nuovo prodotto nel database. Questo valore sarà una novità per il reparto fullfilment ordine, quando viene visualizzato un ordine per koala orsi. È il morale, effettivamente attenzione i dati che si accetta una richiesta POST o PUT.

Per evitare questo problema, modificare il `PostOrder` metodo per richiedere un `OrderDTO` istanza. Utilizzare il `OrderDTO` per creare il `Order`.

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample11.cs)]

Si noti che viene usato il `ProductID` e `Quantity` proprietà e se ignorano i valori che il client ha inviato per il nome del prodotto o prezzo. Se l'ID prodotto non è valido, violeranno il vincolo di chiave esterno nel database e l'inserimento avrà esito negativo, come previsto.

Ecco l'intero `PostOrder` metodo:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample12.cs)]

Aggiungere infine il **Authorize** attributo al controller:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample13.cs)]

Solo gli utenti registrati possono ora creare o visualizzare gli ordini.

> [!div class="step-by-step"]
> [Precedente](using-web-api-with-entity-framework-part-5.md)
> [Successivo](using-web-api-with-entity-framework-part-7.md)
