---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-7
title: Creare la vista (UI) | Documenti Microsoft
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: b2445062-a1fe-4133-8994-f510280f6d9a
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-7
msc.type: authoredcontent
ms.openlocfilehash: 8c5cc662e2e3c9cb07ca9e30ff57eb084d58e1bb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="create-the-view-ui"></a>Creare la vista (UI)
====================
da [Mike Wasson](https://github.com/MikeWasson)

[Scaricare il progetto completato](https://github.com/MikeWasson/BookService)

In questa sezione, si inizierà a definire il codice HTML per l'app e aggiungere un'associazione dati tra il codice HTML e il modello di visualizzazione.

Aprire il file Views/Home/Index.cshtml. Sostituire tutto il contenuto del file con il codice seguente.

[!code-cshtml[Main](part-7/samples/sample1.cshtml)]

La maggior parte del `div` elementi esistono per [Bootstrap](http://getbootstrap.com/) stile. Gli elementi importanti sono quelli con `data-bind` attributi. Questo attributo collega il codice HTML per il modello di visualizzazione.

Ad esempio:

[!code-html[Main](part-7/samples/sample2.html)]

In questo esempio, il &quot; `text` &quot; associazione cause il `<p>` elemento per visualizzare il valore di `error` proprietà dal modello di visualizzazione. Tenere presente che `error` è stato dichiarato come un `ko.observable`:

[!code-javascript[Main](part-7/samples/sample3.js)]

Ogni volta che viene assegnato un nuovo valore per `error`, Knockout aggiorna il testo di `<p>` elemento.

Il `foreach` associazione indica Knockout per scorrere il contenuto del `books` matrice. Per ogni elemento nella matrice, crea un nuovo Knockout &lt;li&gt; elemento. Le associazioni all'interno del contesto del `foreach` fare riferimento alle proprietà dell'elemento di matrice. Ad esempio:

[!code-html[Main](part-7/samples/sample4.html)]

In questo caso il `text` associazione legge la proprietà autore di ogni libro.

Se si esegue ora l'applicazione, dovrebbe essere simile al seguente:

![](part-7/_static/image1.png)

L'elenco di libri carica in modo asincrono, dopo il caricamento della pagina. Ora, il &quot;dettagli&quot; collegamenti non sono funzionali. Questa funzionalità verrà aggiunto nella sezione successiva.

>[!div class="step-by-step"]
[Precedente](part-6.md)
[Successivo](part-8.md)
