---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
title: 'Parte 7: Creazione principale pagina | Documenti Microsoft'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: eb32a17b-626c-4373-9a7d-3387992f3c04
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
msc.type: authoredcontent
ms.openlocfilehash: 2c378e68a1e6600daf655c19afbfe355e89496d4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/10/2018
---
<a name="part-7-creating-the-main-page"></a>Parte 7: Creazione principale pagina
====================
da [Mike Wasson](https://github.com/MikeWasson)

[Scaricare il progetto completato](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-the-main-page"></a>Creazione di principale pagina

In questa sezione si creerà la pagina principale dell'applicazione. Questa pagina sarà più complessa rispetto alla pagina di amministrazione, in modo ci sarà approccio in diversi passaggi. Inoltre, si noterà alcune tecniche Knockout.js più avanzate. Di seguito è riportato il layout di base della pagina:

![](using-web-api-with-entity-framework-part-7/_static/image1.png)

- "Prodotti" contiene una matrice di prodotti.
- "Carrello" contiene una matrice di prodotti con quantità. Fare clic su "Aggiungi al carrello" Aggiorna il carrello.
- "Orders" contiene una matrice di ID degli ordini.
- "Dettagli" contiene un dettaglio di ordine, ovvero una matrice di elementi (prodotti con quantità)

Si inizierà definendo un layout di base in formato HTML, senza associazione a dati o script. Aprire il file Views/Home/Index.cshtml e sostituire tutto il contenuto con quanto segue:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample1.html)]

Successivamente, aggiungere una sezione di script e creare un modello di visualizzazione vuoto:

[!code-cshtml[Main](using-web-api-with-entity-framework-part-7/samples/sample2.cshtml)]

In base alla struttura sketch in precedenza, il modello di visualizzazione deve Observable per i prodotti, carrello, ordini e dettagli. Aggiungere le seguenti variabili per il `AppViewModel` oggetto:

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample3.js)]

Gli utenti possono aggiungere elementi nell'elenco di prodotti nel carrello e rimuovere elementi dal carrello. Per incapsulare queste funzioni, si creerà un'altra classe di modello di visualizzazione che rappresenta un prodotto. Aggiungi il seguente codice a `AppViewModel`.

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample4.js?highlight=4)]

Il `ProductViewModel` classe contiene due funzioni che consentono di spostare il prodotto da e verso il carrello: `addItemToCart` aggiunge un'unità del prodotto al carrello, e `removeAllFromCart` rimuove tutte le quantità del prodotto.

Gli utenti possono selezionare un ordine esistente e ottenere i dettagli dell'ordine. È possibile incapsulare questa funzionalità in un altro modello di visualizzazione:

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample5.js?highlight=4)]

Il `OrderDetailsViewModel` viene inizializzata con un ordine, e vengono recuperati i dettagli dell'ordine inviando una richiesta AJAX al server.

Si noti inoltre il `total` proprietà il `OrderDetailsViewModel`. Questa proprietà è un tipo speciale di observable chiamato un [calcolata observable](http://knockoutjs.com/documentation/computedObservables.html). Come suggerisce il nome, un observable calcolata consente di associare dati a un valore calcolato&#8212;in questo caso, il costo totale dell'ordine.

Successivamente, aggiungere queste funzioni `AppViewModel`:

- `resetCart` Rimuove tutti gli elementi dal carrello.
- `getDetails` Ottiene i dettagli per un ordine (da pGrazie a un nuovo `OrderDetailsViewModel` sul `details` elenco).
- `createOrder` Crea un nuovo ordine e svuota il carrello.


[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample6.js?highlight=4)]

Infine, inizializzare il modello di visualizzazione eseguendo le richieste AJAX per i prodotti e gli ordini:

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample7.js)]

OK, che è la quantità di codice, ma è stato creato il backup dettagliate, ci auguriamo che la progettazione è crittografata. È ora possibile aggiungere alcune associazioni Knockout.js al formato HTML.

**Prodotti**

Ecco i binding per l'elenco dei prodotti:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample8.html)]

Si scorre la matrice di prodotti e visualizza il nome e il prezzo. Il pulsante "Aggiungi e Order" è visibile solo quando l'utente è connesso.

Le chiamate del pulsante "Aggiungi a ordine" `addItemToCart` sul `ProductViewModel` istanza per il prodotto. Ciò dimostra una funzionalità interessante di Knockout.js: quando un modello di visualizzazione contiene altri modelli di visualizzazione, è possibile applicare le associazioni a modello interno. In questo esempio, le associazioni all'interno di `foreach` vengono applicati a ogni il `ProductViewModel` istanze. Questo approccio è molto più chiara di tutte le funzionalità di inserimento in un singolo modello di visualizzazione.

**Carrello**

Ecco le associazioni per il carrello:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample9.html)]

Questo si scorre la matrice del carrello e visualizza il nome, prezzo e quantità. Si noti che il collegamento "Rimuovi" e il pulsante "Creazione dell'ordine" sono associati a funzioni di modello di visualizzazione.

**Ordini**

Ecco i binding per l'elenco di ordini:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample10.html)]

Questo scorre gli ordini e Mostra l'ID ordine. L'evento fare clic sul collegamento è associato il `getDetails` (funzione).

**Dettagli dell'ordine**

Ecco le associazioni per i dettagli dell'ordine:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample11.html)]

Questo scorre gli elementi nell'ordine e consente di visualizzare il prodotto, prezzo e quantità. Tag div circostante è visibile solo se la matrice di dettagli contiene uno o più elementi.

## <a name="conclusion"></a>Conclusione

In questa esercitazione è creata un'applicazione che usa Entity Framework per comunicare con il database e l'API Web ASP.NET per fornire un'interfaccia pubblico nel primo livello di dati. Utilizziamo ASP.NET MVC 4 per il rendering delle pagine HTML e Knockout.js più jQuery per fornire interazioni dinamiche senza caricamento pagina.

Risorse aggiuntive:

- [Mappa del contenuto di accesso ai dati ASP.NET](https://msdn.microsoft.com/library/6759sth4.aspx)
- [Centro per sviluppatori di Entity Framework](https://msdn.microsoft.com/data/ef)

> [!div class="step-by-step"]
> [Precedente](using-web-api-with-entity-framework-part-6.md)
