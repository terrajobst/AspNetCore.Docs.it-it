---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-6
title: Creare il Client JavaScript | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 20360326-b123-4b1e-abae-1d350edf4ce4
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-6
msc.type: authoredcontent
ms.openlocfilehash: 4967e21190c34f698e9c28fd9b921f07bef2ffaf
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827332"
---
<a name="create-the-javascript-client"></a>Creare il Client JavaScript
====================
da [Mike Wasson](https://github.com/MikeWasson)

[Download progetto completato](https://github.com/MikeWasson/BookService)

In questa sezione si creerà il client per l'applicazione, usando HTML, JavaScript e il [Knockout. js](http://knockoutjs.com/) libreria. Verrà compilata l'app client in più fasi:

- Che mostra un elenco di libri.
- Visualizzazione dei dettagli un libro.
- Aggiunta di un nuovo libro.

La libreria Knockout Usa il modello Model-View-ViewModel (MVVM):

- Il **modello** è la rappresentazione lato server dei dati nel dominio dell'azienda (nel nostro caso, libri e autori).
- Il **vista** è il livello di presentazione (HTML).
- Il **modello di visualizzazione** è un oggetto JavaScript che contiene i modelli. Il modello di visualizzazione è un'astrazione di codice dell'interfaccia utente. Dispone di informazioni relative alla relativa rappresentazione in HTML. Rappresenta invece uno funzionalità astratta della visualizzazione, ad esempio &quot;un elenco di libri&quot;.

La vista è associato a dati al modello di visualizzazione. Gli aggiornamenti al modello di visualizzazione vengono automaticamente riflesse nella visualizzazione. Il modello di visualizzazione ottiene anche gli eventi dalla visualizzazione, ad esempio un clic sul pulsante.

![](part-6/_static/image1.png)

Questo approccio semplifica modificare il layout e interfaccia utente dell'app, poiché è possibile modificare le associazioni, senza riscrivere il codice. Ad esempio, si potrebbe visualizzare un elenco di elementi come un `<ul>`, quindi modificarlo in seguito a una tabella.

## <a name="add-the-knockout-library"></a>Aggiungere la libreria Knockout

In Visual Studio dal **degli strumenti** dal menu **Library Package Manager**. Quindi selezionare **Console di gestione pacchetti**. Nella finestra della Console di gestione pacchetti immettere il comando seguente:

[!code-console[Main](part-6/samples/sample1.cmd)]

Questo comando aggiunge i file Knockout nella cartella degli script.

## <a name="create-the-view-model"></a>Creare il modello di visualizzazione

Aggiungere un file JavaScript denominato app. js nella cartella degli script. (In Esplora soluzioni fare doppio clic su cartella Scripts, selezionare **Add**, quindi selezionare **JavaScript File**.) Incollare il codice seguente:

[!code-javascript[Main](part-6/samples/sample2.js)]

In Knockout, il `observable` classe consente l'associazione dati. Quando si modifica il contenuto di un oggetto osservabile, la sequenza observable notifica a tutti i controlli con associazione a dati, in modo che possano aggiornare autonomamente. (Il `observableArray` classe è la versione di matrice di *observable*.) Per iniziare, il nostro modello di visualizzazione dispone di due oggetti osservabili:

- `books` contiene l'elenco di libri.
- `error` contiene un messaggio di errore se ha esito negativo di una chiamata AJAX.

Il `getAllBooks` metodo effettua una chiamata AJAX per ottenere l'elenco di libri. Quindi inserisce il risultato nel `books` matrice.

Il `ko.applyBindings` metodo fa parte della libreria Knockout. Accetta il modello di visualizzazione come parametro e imposta il data binding.

## <a name="add-a-script-bundle"></a>Aggiungere un Bundle di Script

Creazione di bundle è una funzionalità in ASP.NET 4.5 che rende più semplice combinare o più file del bundle in un unico file. Creazione di bundle consente di ridurre il numero di richieste al server, che consente di migliorare il tempo di caricamento pagina.

Aprire il file App\_Start/BundleConfig.cs. Aggiungere il codice seguente al metodo RegisterBundles.

[!code-csharp[Main](part-6/samples/sample3.cs)]

> [!div class="step-by-step"]
> [Precedente](part-5.md)
> [Successivo](part-7.md)
