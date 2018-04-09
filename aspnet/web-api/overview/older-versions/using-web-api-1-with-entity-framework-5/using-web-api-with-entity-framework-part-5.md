---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
title: "Parte 5: Creazione di un'interfaccia utente dinamica con Knockout.js | Documenti Microsoft"
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 9d9cb3b0-f4a7-434e-a508-9fc0ad0eb813
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
msc.type: authoredcontent
ms.openlocfilehash: b63446d076fbb1143641dead788042967b996bf8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="part-5-creating-a-dynamic-ui-with-knockoutjs"></a>Parte 5: Creazione di un'interfaccia utente dinamica con Knockout.js
====================
da [Mike Wasson](https://github.com/MikeWasson)

[Scaricare il progetto completato](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-a-dynamic-ui-with-knockoutjs"></a>Creazione di un'interfaccia utente dinamica con Knockout.js

In questa sezione, useremo Knockout.js per aggiungere funzionalità per la visualizzazione amministrazione.

[Knockout.js](http://knockoutjs.com/) è una libreria Javascript che rende più semplice associare i controlli HTML a dati. Knockout.js utilizza il modello Model-View-ViewModel (MVVM).

- Il *modello* è la rappresentazione sul lato server dei dati del dominio di business (in questo caso, products e orders).
- Il *visualizzazione* è il livello di presentazione (HTML).
- Il *modello di visualizzazione* è un oggetto Javascript che contiene i dati del modello. Il modello di visualizzazione è un'astrazione del codice dell'interfaccia utente. Ha alcuna conoscenza della rappresentazione HTML. Rappresenta invece astratta funzionalità della visualizzazione, ad esempio "un elenco di elementi".

La vista con associazione dati per il modello di visualizzazione. Gli aggiornamenti al modello di visualizzazione vengono applicati automaticamente nella vista. Il modello di visualizzazione, inoltre, ottiene gli eventi dalla visualizzazione, ad esempio pulsanti ed esegue operazioni sul modello, ad esempio la creazione di un ordine.

![](using-web-api-with-entity-framework-part-5/_static/image1.png)

Prima di tutto definiamo il modello di visualizzazione. Successivamente, si verrà associato il markup HTML per il modello di visualizzazione.

Aggiungere la seguente sezione Razor Admin.cshtml:

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample1.cshtml)]

È possibile aggiungere un punto qualsiasi in questa sezione nel file. Quando il rendering, la sezione viene visualizzata nella parte inferiore della pagina HTML destro del mouse prima della chiusura &lt;/body&gt; tag.

Tutti gli script di per questa pagina verrà inviata all'interno del tag di script indicato dal commento:

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample2.html)]

Prima di definire una classe di modello di visualizzazione:

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample3.js)]

**ko.observableArray** è un tipo speciale di oggetto in Knockout, chiamato un' *observable*. Dal [Knockout.js documentazione](http://knockoutjs.com/documentation/observables.html): observable è un "oggetto JavaScript che può inviare una notifica di sottoscrittori sulle modifiche." Quando il contenuto di un oggetto osservabile cambia, la vista viene aggiornata automaticamente in modo che corrisponda.

Per popolare il `products` matrice, effettuare una richiesta AJAX all'API web. Tenere presente che è archiviato l'URI di base per l'API nel contenitore delle visualizzazioni (vedere [parte 4](using-web-api-with-entity-framework-part-4.md) dell'esercitazione).

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample4.js?highlight=5)]

Successivamente, aggiungere le funzioni per il modello di visualizzazione per creare, aggiornare ed eliminare i prodotti. Queste funzioni inviare chiamate AJAX all'API web e utilizzano i risultati per aggiornare il modello di visualizzazione.

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample5.js?highlight=7)]

Ora la parte più importante: quando DOM è fulled chiamata caricato, il **ko.applyBindings** funzione e passare una nuova istanza del `ProductsViewModel`:

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample6.js)]

Il **ko.applyBindings** metodo attiva Knockout e collega il modello di visualizzazione per la visualizzazione.

Ora che è disponibile un modello di visualizzazione, è possibile creare le associazioni. In Knockout.js, questo scopo, aggiungere `data-bind` attributi agli elementi HTML. Ad esempio, per associare un elenco HTML in una matrice, utilizzare il `foreach` associazione:

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample7.html?highlight=1)]

Il `foreach` associazione, scorre la matrice e crea figlio di elementi per ogni oggetto nella matrice. Le associazioni sugli elementi figlio possono fare riferimento alle proprietà di oggetti di matrice.

Aggiungere associazioni seguenti all'elenco "-i prodotti di aggiornamento":

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample8.html)]

Il `<li>` elemento presente nell'ambito del **foreach** associazione. Che verrà Knockout indica il rendering dell'elemento di una volta per ogni prodotto nel `products` matrice. Tutte le associazioni all'interno di `<li>` elemento fare riferimento a tale istanza di un prodotto. Ad esempio, `$data.Name` si intende la `Name` proprietà del prodotto.

Per impostare i valori di input di testo, utilizzare il `value` associazione. I pulsanti sono associati a funzioni-vista del modello utilizzando il `click` associazione. L'istanza di un prodotto viene passato come parametro per ogni funzione. Per ulteriori informazioni, il [Knockout.js documentazione](http://knockoutjs.com/documentation/observables.html) ha buona descrizioni delle associazioni diverse.

Successivamente, aggiungere un'associazione per il **inviare** eventi del modulo aggiungere prodotto:

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample9.html)]

Questa associazione chiama il `create` funzione nel modello di visualizzazione per creare un nuovo prodotto.

Ecco il codice completo per la visualizzazione di amministrazione:

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample10.cshtml)]

Eseguire l'applicazione, accedere con l'account amministratore e fare clic sul collegamento "Admin". Vedere l'elenco dei prodotti e in grado di creare, aggiornare o eliminare i prodotti.

> [!div class="step-by-step"]
> [Precedente](using-web-api-with-entity-framework-part-4.md)
> [Successivo](using-web-api-with-entity-framework-part-6.md)
