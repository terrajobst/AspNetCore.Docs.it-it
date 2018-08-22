---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
title: 'Parte 3: Creazione di un Controller di amministrazione | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: 6b9ae3c4-0274-4170-a1bb-9df9c546b2a9
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
msc.type: authoredcontent
ms.openlocfilehash: 7ad0ec27021514b447e569e479a9e9127e3f75fa
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826274"
---
<a name="part-3-creating-an-admin-controller"></a>Parte 3: Creazione di un Controller di amministrazione
====================
da [Mike Wasson](https://github.com/MikeWasson)

[Download progetto completato](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-controller"></a>Aggiungere un Controller di amministrazione

In questa sezione si aggiungerà un controller API Web che supporta CRUD (creare, leggere, aggiornare ed eliminare) le operazioni sui prodotti. Il controller userà Entity Framework per comunicare con il livello di database. Solo gli amministratori saranno in grado di utilizzare il controller. I clienti potranno accedere i prodotti tramite un altro controller.

In Esplora soluzioni fare clic sulla cartella Controllers. Selezionare **aggiungere** e quindi **Controller**.

![](using-web-api-with-entity-framework-part-3/_static/image1.png)

Nel **Aggiungi Controller** finestra di dialogo nome al controller `AdminController`. Sotto **modello**, selezionare &quot;controller API con azioni di lettura/scrittura, che usa Entity Framework&quot;. Sotto **classe modello**, selezionare "Prodotto (ProductStore.Models)". Sotto **DataContext**, selezionare "&lt;nuovo contesto dati&gt;".

![](using-web-api-with-entity-framework-part-3/_static/image2.png)

> [!NOTE]
> Se il **classe modello** elenco a discesa non include tutte le classi modello, assicurarsi che è stato compilato il progetto. Entity Framework Usa la reflection, pertanto è necessario l'assembly compilato.


Selezione di "&lt;nuovo contesto dati&gt;" verrà aperta la **nuovo contesto dati** finestra di dialogo. Nome del contesto dati `ProductStore.Models.OrdersContext`.

![](using-web-api-with-entity-framework-part-3/_static/image3.png)

Fare clic su **OK** per chiudere la **nuovo contesto dati** finestra di dialogo. Nel **Aggiungi Controller** finestra di dialogo, fare clic su **Add**.

Ecco cosa è stato aggiunto al progetto:

- Una classe denominata `OrdersContext` che deriva da **DbContext**. Questa classe fornisce l'associazione tra i modelli POCO e il database.
- Controller Web API denominato `AdminController`. Questo controller supporta operazioni CRUD su `Product` istanze. Usa il `OrdersContext` classi per comunicare con Entity Framework.
- Una nuova stringa di connessione del database nel file Web. config.

![](using-web-api-with-entity-framework-part-3/_static/image4.png)

Aprire il file OrdersContext.cs. Si noti che il costruttore viene specificato il nome della stringa di connessione al database. Questo nome fa riferimento alla stringa di connessione che è stato aggiunto al file Web. config.

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample1.cs)]

Aggiungere le proprietà seguenti alla classe `OrdersContext`:

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample2.cs)]

Oggetto **DbSet** rappresenta un set di entità che è possibile eseguire query. Di seguito è riportato il listato completo per il `OrdersContext` classe:

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample3.cs)]

Il `AdminController` classe definisce cinque metodi che implementano la funzionalità CRUD di base. Ogni metodo corrisponde a un URI che il client può richiamare:

| Metodo del controller | Descrizione | URI | Metodo HTTP |
| --- | --- | --- | --- |
| GetProducts | Ottiene tutti i prodotti. | API/prodotti | GET |
| GetProduct | Trova un prodotto base all'ID. | api/products/*id* | GET |
| PutProduct | Consente di aggiornare un prodotto. | api/products/*id* | PUT |
| PostProduct | Crea un nuovo prodotto. | API/prodotti | INSERISCI |
| DeleteProduct | Elimina un prodotto. | api/products/*id* | DELETE |

Chiama ogni metodo `OrdersContext` eseguire query sul database. Chiamano i metodi che modificano la raccolta (PUT, POST e DELETE) `db.SaveChanges` per rendere permanenti le modifiche al database. I controller sono creati per ogni richiesta HTTP e quindi eliminati, pertanto è necessario rendere persistenti le modifiche prima che restituisca un metodo.

## <a name="add-a-database-initializer"></a>Aggiungere un inizializzatore di Database

Entity Framework è una funzionalità interessante che consente di popolare il database all'avvio, quindi di nuovo automaticamente il database ogni volta che i modelli di modifica. Questa funzionalità è utile durante lo sviluppo, perché hai sempre alcuni dati di test, anche se si modificano i modelli.

In Esplora soluzioni fare doppio clic su cartella Models e creare una nuova classe denominata `OrdersContextInitializer`. Incollare l'implementazione seguente:

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample4.cs)]

Ereditando dal **DropCreateDatabaseIfModelChanges** (classe), si definiranno Entity Framework per eliminare il database ogni volta che si modifica classi del modello. Quando Entity Framework consente di creare o ricrea il database, chiama il **Seed** metodo per popolare le tabelle. Usiamo il **Seed** metodo per aggiungere alcuni prodotti di esempio oltre a un ordine di esempio.

Questa funzionalità è molto utile per i test, ma non usare la **DropCreateDatabaseIfModelChanges** classe nell'ambiente di produzione, perché si potrebbero perdere i dati se vengono apportate modifiche a una classe di modello.

Successivamente, aprire Global. asax e aggiungere il codice seguente per il **Application\_avviare** metodo:

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample5.cs)]

## <a name="send-a-request-to-the-controller"></a>Inviare una richiesta al Controller

A questo punto, è ancora stato scritto alcun codice client, ma è possibile richiamare API usando un web browser o un debug HTTP strumento, ad esempio web [Fiddler](http://www.fiddler2.com/fiddler2/). In Visual Studio, premere F5 per avviare il debug. Web browser aprirà `http://localhost:*portnum*/`, dove *portnum* è un numero di porta.

Inviare una richiesta HTTP per "`http://localhost:*portnum*/api/admin`. La prima richiesta potrebbe essere lenta per il completamento, poiché Entify Framework è necessaria creare ed effettuare il seeding del database. La risposta deve eseguire un'operazione simile al seguente:

[!code-console[Main](using-web-api-with-entity-framework-part-3/samples/sample6.cmd)]

> [!div class="step-by-step"]
> [Precedente](using-web-api-with-entity-framework-part-2.md)
> [Successivo](using-web-api-with-entity-framework-part-4.md)
