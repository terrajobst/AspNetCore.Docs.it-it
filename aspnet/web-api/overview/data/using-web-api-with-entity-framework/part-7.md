---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-7
title: Creare la vista (UI) | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: b2445062-a1fe-4133-8994-f510280f6d9a
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-7
msc.type: authoredcontent
ms.openlocfilehash: e9ebe60f88ecbf65a6f8d04de9a23d72a72fda83
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37364981"
---
<a name="create-the-view-ui"></a>Creare la vista (UI)
====================
da [Mike Wasson](https://github.com/MikeWasson)

[Download progetto completato](https://github.com/MikeWasson/BookService)

In questa sezione, si inizieranno a definire il codice HTML per l'app e aggiungere l'associazione dati tra il codice HTML e il modello di visualizzazione.

Aprire il file Views/Home/Index.cshtml. Sostituire l'intero contenuto del file con il codice seguente.

[!code-cshtml[Main](part-7/samples/sample1.cshtml)]

La maggior parte delle `div` sono per gli elementi [Bootstrap](http://getbootstrap.com/) lo stile. Gli elementi importanti sono quelli con `data-bind` attributi. Questo attributo collega il codice HTML per il modello di visualizzazione.

Ad esempio:

[!code-html[Main](part-7/samples/sample2.html)]

In questo esempio, il &quot; `text` &quot; associazione cause il `<p>` elemento per visualizzare il valore della `error` proprietà dal modello di visualizzazione. È importante ricordare che `error` è stato dichiarato come un `ko.observable`:

[!code-javascript[Main](part-7/samples/sample3.js)]

Ogni volta che un nuovo valore viene assegnato a `error`, Knockout aggiorna il testo di `<p>` elemento.

Il `foreach` associazione indica Knockout per eseguire il contenuto del ciclo di `books` matrice. Per ogni elemento nella matrice, crea un nuovo Knockout &lt;li&gt; elemento. Le associazioni all'interno del contesto del `foreach` fare riferimento alle proprietà dell'elemento di matrice. Ad esempio:

[!code-html[Main](part-7/samples/sample4.html)]

In questo caso il `text` associazione legge la proprietà dell'autore di ogni libro.

Se si esegue l'applicazione a questo punto, dovrebbe essere simile al seguente:

![](part-7/_static/image1.png)

L'elenco di libri carica in modo asincrono, dopo il caricamento della pagina. Ora, il &quot;dettagli&quot; collegamenti non sono attivi. Aggiungiamo questa funzionalità nella sezione successiva.

> [!div class="step-by-step"]
> [Precedente](part-6.md)
> [Successivo](part-8.md)
