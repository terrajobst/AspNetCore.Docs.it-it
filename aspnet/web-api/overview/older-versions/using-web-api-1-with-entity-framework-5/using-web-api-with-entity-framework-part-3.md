---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
title: 'Parte 3: Creazione di un Controller di amministrazione | Documenti Microsoft'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 6b9ae3c4-0274-4170-a1bb-9df9c546b2a9
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
msc.type: authoredcontent
ms.openlocfilehash: 588d9d1b5d27759692cd840faabf2c3549c309d6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870544"
---
<a name="part-3-creating-an-admin-controller"></a>Parte 3: Creazione di un Controller di amministrazione
====================
da [Mike Wasson](https://github.com/MikeWasson)

[Scaricare il progetto completato](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-controller"></a>Aggiungere un Controller di amministrazione

In questa sezione verrà aggiunto un controller API Web che supporta CRUD (creare, leggere, aggiornare ed eliminare) operazioni sui prodotti. Entity Framework verrà utilizzata dal controller per comunicare con il livello di database. Solo gli amministratori potranno usare questo controller. I clienti potranno accedere i prodotti tramite un altro controller.

In Esplora soluzioni, fare clic sulla cartella controller. Selezionare **aggiungere** e quindi **Controller**.

![](using-web-api-with-entity-framework-part-3/_static/image1.png)

Nel **Aggiungi Controller** finestra di dialogo, nome del controller `AdminController`. In **modello**selezionare &quot;controller API con azioni di lettura/scrittura, mediante Entity Framework&quot;. In **classe modello**, selezionare "Prodotto (ProductStore.Models)". In **contesto dati**, selezionare "&lt;nuovo contesto dati&gt;".

![](using-web-api-with-entity-framework-part-3/_static/image2.png)

> [!NOTE]
> Se il **classe modello** elenco a discesa non mostra tutte le classi modello, assicurarsi che è stato compilato il progetto. Entity Framework Usa la reflection, pertanto è necessario l'assembly compilato.


Selezione di "&lt;nuovo contesto dati&gt;" verrà aperta la **nuovo contesto dati** finestra di dialogo. Nome del contesto dati `ProductStore.Models.OrdersContext`.

![](using-web-api-with-entity-framework-part-3/_static/image3.png)

Fare clic su **OK** per chiudere la **nuovo contesto dati** finestra di dialogo. Nel **Aggiungi Controller** finestra di dialogo, fare clic su **Aggiungi**.

Ecco cosa è stata aggiunta al progetto:

- Una classe denominata `OrdersContext` che deriva da **DbContext**. Questa classe fornisce l'associazione tra i modelli POCO e il database.
- Controller API Web denominato `AdminController`. Il controller supporta operazioni CRUD su `Product` istanze. Usa il `OrdersContext` classe per comunicare con Entity Framework.
- Nuova stringa di connessione del database nel file Web. config.

![](using-web-api-with-entity-framework-part-3/_static/image4.png)

Aprire il file OrdersContext.cs. Si noti che il costruttore viene specificato il nome della stringa di connessione di database. Questo nome fa riferimento alla stringa di connessione che è stato aggiunto al file Web. config.

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample1.cs)]

Aggiungere le proprietà seguenti alla classe `OrdersContext`:

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample2.cs)]

Oggetto **DbSet** rappresenta un set di entità che è possibile eseguire query. Di seguito è riportato il listato completo per la `OrdersContext` classe:

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample3.cs)]

La `AdminController` classe definisce i cinque metodi che implementano funzionalità CRUD di base. Ogni metodo corrisponde a un URI che il client può richiamare:

| Metodo del controller | Descrizione | URI | Metodo HTTP |
| --- | --- | --- | --- |
| GetProducts | Ottiene tutti i prodotti. | API/prodotti | GET |
| GetProduct | Trova un prodotto in base all'ID. | api/products/*id* | GET |
| PutProduct | Aggiorna un prodotto. | api/products/*id* | PUT |
| PostProduct | Crea un nuovo prodotto. | API/prodotti | INSERISCI |
| DeleteProduct | Elimina un prodotto. | api/products/*id* | DELETE |

Ogni metodo chiama `OrdersContext` per eseguire query sul database. Chiamano i metodi che modificano la raccolta (PUT, POST e DELETE) `db.SaveChanges` per rendere permanenti le modifiche al database. I controller vengono creati per ogni richiesta HTTP e quindi eliminati, pertanto è necessario rendere permanenti le modifiche prima che venga restituito un metodo.

## <a name="add-a-database-initializer"></a>Aggiungere un inizializzatore del Database

Entity Framework è una funzionalità interessante che consente di popolare il database all'avvio e ricreare automaticamente il database ogni volta che i modelli di modifica. Questa funzionalità è utile durante lo sviluppo, poiché è sempre alcuni dati di test, anche se si modificano i modelli.

In Esplora soluzioni, fare clic sulla cartella di modelli e creare una nuova classe denominata `OrdersContextInitializer`. Incollare l'implementazione seguente:

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample4.cs)]

Tramite l'eredità dal **DropCreateDatabaseIfModelChanges** (classe), si definiranno Entity Framework per eliminare il database ogni volta che si modifica classi del modello. Quando Entity Framework consente di creare o ricrea il database, chiama il **valore di inizializzazione** metodo per popolare le tabelle. Utilizziamo la **valore di inizializzazione** metodo per aggiungere alcuni prodotti di esempio più di un ordine di esempio.

Questa funzionalità è molto utile per il test, ma non usano il **DropCreateDatabaseIfModelChanges** classe nell'ambiente di produzione, perché si potrebbe perdere i dati se vengono apportate modifiche a una classe di modello.

Successivamente, aprire Global. asax e aggiungere il codice seguente per il **applicazione\_avviare** metodo:

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample5.cs)]

## <a name="send-a-request-to-the-controller"></a>Inviare una richiesta per il Controller

A questo punto, è non scrivere alcun codice client, ma è possibile richiamare web API usando un web browser o per il debug di un HTTP strumento, ad esempio [Fiddler](http://www.fiddler2.com/fiddler2/). In Visual Studio, premere F5 per avviare il debug. Aprire il browser web per `http://localhost:*portnum*/`, dove *portnum* rappresenta un numero di porta.

Inviare una richiesta HTTP per "`http://localhost:*portnum*/api/admin`. La prima richiesta potrebbe essere lenta da completare, Entify Framework in quanto deve creare e inizializzare il database. La risposta è necessario un codice simile al seguente:

[!code-console[Main](using-web-api-with-entity-framework-part-3/samples/sample6.cmd)]

> [!div class="step-by-step"]
> [Precedente](using-web-api-with-entity-framework-part-2.md)
> [Successivo](using-web-api-with-entity-framework-part-4.md)
