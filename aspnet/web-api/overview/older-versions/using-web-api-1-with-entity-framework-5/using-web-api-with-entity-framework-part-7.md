---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
title: 'Parte 7: Creazione principale pagina | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: eb32a17b-626c-4373-9a7d-3387992f3c04
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
msc.type: authoredcontent
ms.openlocfilehash: bb4704e7f4f13fab04acdbdd642174884517e18a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827180"
---
<a name="part-7-creating-the-main-page"></a>Parte 7: Creazione principale pagina
====================
da [Mike Wasson](https://github.com/MikeWasson)

[Download progetto completato](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-the-main-page"></a>Creazione principale pagina

In questa sezione si creerà la pagina principale dell'applicazione. Questa pagina sarà più complessa rispetto alla pagina di amministrazione, in modo che abbiamo si avvicinerà in diversi passaggi. Lungo il percorso, si noterà alcune tecniche più avanzate Knockout. js. Ecco il layout di base della pagina:

![](using-web-api-with-entity-framework-part-7/_static/image1.png)

- "Prodotti" contiene una matrice di prodotti.
- "Carrello" contiene una matrice di prodotti con quantità. Facendo clic su "Aggiungi al carrello" Aggiorna il carrello della spesa.
- "Orders" contiene una matrice di ID degli ordini.
- "Dettagli" contiene un dettaglio ordine, ovvero una matrice di elementi (i prodotti con quantità)

Si inizierà con la definizione di alcuni layout di base in formato HTML, senza l'associazione dati o dello script. Aprire il file Views/Home/Index.cshtml e sostituire tutto il contenuto con quanto segue:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample1.html)]

Successivamente, aggiungere una sezione di script e creare un modello di visualizzazione vuoto:

[!code-cshtml[Main](using-web-api-with-entity-framework-part-7/samples/sample2.cshtml)]

In base alla struttura descritto in precedenza, il nostro modello di visualizzazione deve oggetti osservabili per i prodotti, carrello, ordini e dettagli. Aggiungere le variabili seguenti per il `AppViewModel` oggetto:

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample3.js)]

Gli utenti possono aggiungere elementi dall'elenco dei prodotti nel carrello e rimuovere elementi del carrello. Per incapsulare queste funzioni, si creerà un'altra classe di modello di visualizzazione che rappresenta un prodotto. Aggiungi il seguente codice a `AppViewModel`.

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample4.js?highlight=4)]

Il `ProductViewModel` classe contiene due funzioni che consentono di spostare il prodotto da e verso il carrello: `addItemToCart` aggiunge un'unità del prodotto al carrello e `removeAllFromCart` rimuove tutte le quantità del prodotto.

Gli utenti possono selezionare un ordine esistente e ottenere i dettagli dell'ordine. Si sarà incapsulare questa funzionalità in un altro modello di visualizzazione:

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample5.js?highlight=4)]

Il `OrderDetailsViewModel` viene inizializzata con un ordine, e recupera i dettagli dell'ordine inviando una richiesta AJAX al server.

Si noti anche il `total` proprietà di `OrderDetailsViewModel`. Questa proprietà è un tipo speciale di oggetto osservabile chiamato un' [calcolato observable](http://knockoutjs.com/documentation/computedObservables.html). Come suggerisce il nome, un oggetto osservabile calcolata consente di associare i dati a un valore calcolato&#8212;in questo caso, il costo totale dell'ordine.

Successivamente, aggiungere queste funzioni per `AppViewModel`:

- `resetCart` Rimuove tutti gli elementi del carrello.
- `getDetails` Ottiene i dettagli per un ordine (da pGrazie a un nuovo `OrderDetailsViewModel` nella `details` elenco).
- `createOrder` Crea un nuovo ordine e lo svuota il carrello della spesa.


[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample6.js?highlight=4)]

Infine, inizializzare il modello di visualizzazione da creare richieste AJAX per i prodotti e ordini:

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample7.js)]

OK, che è una grande quantità di codice, ma è progettata in backup dettagliata, ci auguriamo che la progettazione è chiara. A questo punto possiamo aggiungere alcune associazioni Knockout. js al codice HTML.

**Prodotti**

Ecco i binding per l'elenco dei prodotti:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample8.html)]

Si scorre la matrice di prodotti e visualizza il nome e il prezzo. Il pulsante "Aggiungi a Order" è visibile solo quando l'utente è connesso.

Pulsante "Aggiungi a Order" chiama `addItemToCart` nella `ProductViewModel` istanza per il prodotto. Questo esempio dimostra una funzionalità interessante di Knockout. js: quando un modello di visualizzazione contiene altri modelli di visualizzazione, è possibile applicare i binding per il modello interno. In questo esempio, le associazioni all'interno di `foreach` vengono applicati a ogni il `ProductViewModel` istanze. Questo approccio è molto più chiaro rispetto a tutte le funzionalità di inserimento in un singolo modello di visualizzazione.

**Carrello della spesa**

Di seguito sono le associazioni per il carrello della spesa:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample9.html)]

Si scorre la matrice carrello della spesa e visualizza il nome, prezzo e quantità. Si noti che il collegamento "Remove" e il pulsante di "Creazione dell'ordine" sono associati alle funzioni di modello di visualizzazione.

**Ordini**

Ecco i binding per l'elenco di ordini:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample10.html)]

Si scorre gli ordini e l'ID dell'ordine. L'evento clic sul collegamento associato ai `getDetails` (funzione).

**Dettagli dell'ordine**

Di seguito sono le associazioni per i dettagli dell'ordine:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample11.html)]

Questo scorre gli elementi nell'ordine e consente di visualizzare il prodotto, prezzo e Quantity. Il tag div circostante è visibile solo se la matrice di dettagli contiene uno o più elementi.

## <a name="conclusion"></a>Conclusione

In questa esercitazione è creata un'applicazione che usa Entity Framework per comunicare con il database e API Web ASP.NET per fornire un'interfaccia rivolte al pubblico nella parte superiore del livello dati. Si usa ASP.NET MVC 4 per il rendering delle pagine HTML e Knockout. js più jQuery per offrire le interazioni dinamiche senza Ricarica pagina.

Risorse aggiuntive:

- [Mappa del contenuto per l'accesso dei dati di ASP.NET](https://msdn.microsoft.com/library/6759sth4.aspx)
- [Centro per sviluppatori di Entity Framework](https://msdn.microsoft.com/data/ef)

> [!div class="step-by-step"]
> [Precedente](using-web-api-with-entity-framework-part-6.md)
