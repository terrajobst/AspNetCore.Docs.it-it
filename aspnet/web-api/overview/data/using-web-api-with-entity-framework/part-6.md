---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-6
title: Creare il Client JavaScript | Documenti Microsoft
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 20360326-b123-4b1e-abae-1d350edf4ce4
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-6
msc.type: authoredcontent
ms.openlocfilehash: b397c5a413ae213c9b79da1c0e0626efe21c7e21
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="create-the-javascript-client"></a>Creare il Client JavaScript
====================
da [Mike Wasson](https://github.com/MikeWasson)

[Scaricare il progetto completato](https://github.com/MikeWasson/BookService)

In questa sezione si creerà il client per l'applicazione, in formato HTML, JavaScript e [Knockout.js](http://knockoutjs.com/) libreria. Verrà creata l'app client in varie fasi:

- Visualizzazione di un elenco di libri.
- Visualizzazione dei dettagli un libro.
- Aggiunta di un libro di nuovo.

La libreria Knockout utilizza il modello Model-View-ViewModel (MVVM):

- Il **modello** è la rappresentazione sul lato server dei dati del dominio di business (in questo caso documentazione e gli autori).
- Il **visualizzazione** è il livello di presentazione (HTML).
- Il **modello di visualizzazione** è un oggetto JavaScript che contiene i modelli. Il modello di visualizzazione è un'astrazione del codice dell'interfaccia utente. Ha alcuna conoscenza della rappresentazione HTML. Rappresenta invece astratta funzionalità della visualizzazione, ad esempio &quot;un elenco di libri&quot;.

La vista con associazione dati per il modello di visualizzazione. Gli aggiornamenti al modello di visualizzazione vengono applicati automaticamente nella vista. Il modello di visualizzazione ottiene anche gli eventi dalla visualizzazione, ad esempio, fa clic sul pulsante.

![](part-6/_static/image1.png)

Questo approccio rende facile modificare il layout e l'interfaccia utente dell'applicazione, poiché è possibile modificare le associazioni, senza riscrivere il codice. Ad esempio, si potrebbe visualizzare un elenco di elementi come un `<ul>`, quindi cambiarlo in seguito a una tabella.

## <a name="add-the-knockout-library"></a>Aggiungere la libreria di ritaglio

In Visual Studio, dal **strumenti** dal menu **Gestione pacchetti libreria**. Selezionare quindi **Package Manager Console**. Nella finestra della Console di gestione pacchetti, immettere il comando seguente:

[!code-console[Main](part-6/samples/sample1.cmd)]

Questo comando aggiunge i file Knockout alla cartella degli script.

## <a name="create-the-view-model"></a>Creare il modello di visualizzazione

Aggiungere un file JavaScript denominato app.js alla cartella degli script. (In Esplora soluzioni fare doppio clic sulla cartella script, selezionare **Aggiungi**, quindi selezionare **JavaScript File**.) Incollare il codice seguente:

[!code-javascript[Main](part-6/samples/sample2.js)]

In, Knockout la `observable` classe consente l'associazione dati. Quando cambia il contenuto di un oggetto observable, observable notifica a tutti i controlli con associazione a dati, in modo che possano aggiornare autonomamente. (Il `observableArray` classe è la versione della matrice di *observable*.) Per iniziare, il modello di visualizzazione dispone di due oggetti osservabili:

- `books`contiene l'elenco di libri.
- `error`contiene un messaggio di errore se ha esito negativo di una chiamata AJAX.

Il `getAllBooks` metodo effettua una chiamata AJAX per ottenere l'elenco di libri. Quindi inserisce il risultato nel `books` matrice.

Il `ko.applyBindings` metodo fa parte della libreria di ritaglio. Accetta come parametro, il modello di visualizzazione e impostare l'associazione dati.

## <a name="add-a-script-bundle"></a>Aggiungere un Bundle di Script

Creazione di bundle è una funzionalità in ASP.NET 4.5 che rende più semplice combinare o aggregare più file in un singolo file. Creazione di bundle riduce il numero di richieste al server, che consente di migliorare il tempo di caricamento pagina.

Aprire il file App\_Start/BundleConfig.cs. Aggiungere il codice seguente al metodo RegisterBundles.

[!code-csharp[Main](part-6/samples/sample3.cs)]

>[!div class="step-by-step"]
[Precedente](part-5.md)
[Successivo](part-7.md)
