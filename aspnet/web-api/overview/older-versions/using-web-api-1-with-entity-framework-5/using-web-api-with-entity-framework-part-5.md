---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
title: "Parte 5: Creazione di un'interfaccia utente dinamica con Knockout. js | Microsoft Docs"
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 07/04/2012
ms.assetid: 9d9cb3b0-f4a7-434e-a508-9fc0ad0eb813
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
msc.type: authoredcontent
ms.openlocfilehash: 7d8bd4732ecf73c14251ceebfd667310f6e197b4
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37801693"
---
<a name="part-5-creating-a-dynamic-ui-with-knockoutjs"></a>Parte 5: Creazione di un'interfaccia utente dinamica con Knockout. js
====================
da [Mike Wasson](https://github.com/MikeWasson)

[Download progetto completato](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-a-dynamic-ui-with-knockoutjs"></a>Creazione di un'interfaccia utente dinamica con Knockout. js

In questa sezione si userà Knockout. js per aggiungere funzionalità alla visualizzazione amministrazione.

[Knockout. js](http://knockoutjs.com/) è una libreria Javascript che rende più semplice associare i controlli HTML ai dati. Knockout. js Usa il modello Model-View-ViewModel (MVVM).

- Il *modello* è la rappresentazione lato server dei dati nel dominio dell'azienda (nel nostro caso, i prodotti e ordini).
- Il *vista* è il livello di presentazione (HTML).
- Il *modello di visualizzazione* è un oggetto Javascript che contiene i dati del modello. Il modello di visualizzazione è un'astrazione di codice dell'interfaccia utente. Dispone di informazioni relative alla relativa rappresentazione in HTML. Rappresenta invece uno funzionalità astratta della visualizzazione, ad esempio "un elenco di elementi".

La vista è associato a dati al modello di visualizzazione. Gli aggiornamenti al modello di visualizzazione vengono automaticamente riflesse nella visualizzazione. Il modello di visualizzazione anche Ottiene gli eventi dalla visualizzazione, ad esempio pulsanti ed esegue operazioni sul modello, ad esempio la creazione di un ordine.

![](using-web-api-with-entity-framework-part-5/_static/image1.png)

Prima di tutto si definirà il modello di visualizzazione. Successivamente, si assocerà il markup HTML per il modello di visualizzazione.

Aggiungere la sezione seguente di Razor per Admin.cshtml:

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample1.cshtml)]

In questa sezione è possibile aggiungere un punto qualsiasi nel file. Durante il rendering, viene visualizzata la sezione nella parte inferiore della pagina HTML, pulsante destro del mouse prima del tag &lt;/body&gt; tag.

Tutti dello script per questa pagina passerà all'interno del tag di script indicato dal commento:

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample2.html)]

In primo luogo, definire una classe di modello di visualizzazione:

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample3.js)]

**ko.observableArray** è un tipo speciale di oggetto in Knockout, chiamato un' *observable*. Dal [Knockout. js documentazione](http://knockoutjs.com/documentation/observables.html): un oggetto osservabile è un "oggetto JavaScript che può inviare una notifica sulle modifiche apportate gli abbonati." Quando si modifica il contenuto di un oggetto osservabile, la visualizzazione viene aggiornata automaticamente in modo che corrisponda.

Per popolare il `products` matrice, effettuare una richiesta AJAX all'API web. È importante ricordare che è stato archiviato l'URI di base per le API nel contenitore di visualizzazione (vedere [parte 4](using-web-api-with-entity-framework-part-4.md) dell'esercitazione).

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample4.js?highlight=5)]

Successivamente, aggiungere le funzioni per il modello di visualizzazione da creare, aggiornare ed eliminare i prodotti. Queste funzioni inviare chiamate AJAX all'API web e utilizzano i risultati per aggiornare il modello di visualizzazione.

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample5.js?highlight=7)]

Ora la parte più importante: quando DOM è fulled chiamata caricato, il **ko.applyBindings** funzionare e si passa una nuova istanza del `ProductsViewModel`:

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample6.js)]

Il **ko.applyBindings** metodo attiva Knockout e collega il modello di visualizzazione per la visualizzazione.

Ora che abbiamo un modello di visualizzazione, è possibile creare le associazioni. In Knockout. js, eseguire questa operazione aggiungendo `data-bind` attributi agli elementi HTML. Ad esempio, per associare un elenco di HTML in una matrice, usare il `foreach` associazione:

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample7.html?highlight=1)]

Il `foreach` associazione scorre la matrice e crea figlio di elementi per ogni oggetto nella matrice. Le associazioni sugli elementi figlio possono fare riferimento alle proprietà di oggetti di matrice.

Aggiungere le associazioni seguenti all'elenco "prodotti di aggiornamento":

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample8.html)]

Il `<li>` elemento presente nell'ambito del **foreach** associazione. Che significa che verrà Knockout il rendering dell'elemento di una volta per ogni prodotto nel `products` matrice. Tutte le associazioni all'interno di `<li>` elemento fare riferimento a tale istanza del prodotto. Ad esempio, `$data.Name` si intende il `Name` proprietà su un prodotto.

Per impostare i valori di input di testo, usare il `value` associazione. I pulsanti sono associati a funzioni nella vista del modello, utilizzando il `click` associazione. L'istanza di un prodotto viene passato come parametro per ogni funzione. Per altre informazioni, il [Knockout. js documentazione](http://knockoutjs.com/documentation/observables.html) ha buon descrizioni delle associazioni diverse.

Successivamente, aggiungere un'associazione per il **inviare** evento nel form Aggiungi prodotto:

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample9.html)]

Questo binding chiama il `create` funzione nel modello di visualizzazione per creare un nuovo prodotto.

Ecco il codice completo per la visualizzazione di amministrazione:

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample10.cshtml)]

Eseguire l'applicazione, accedere con l'account amministratore e fare clic sul collegamento "Admin". È consigliabile vedere l'elenco dei prodotti ed essere in grado di creare, aggiornare o eliminare i prodotti.

> [!div class="step-by-step"]
> [Precedente](using-web-api-with-entity-framework-part-4.md)
> [Successivo](using-web-api-with-entity-framework-part-6.md)
