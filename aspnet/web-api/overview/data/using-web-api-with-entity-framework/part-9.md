---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-9
title: Aggiungere un nuovo elemento al Database | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 0967c29e-e124-4db0-a788-c45d0ff5aff2
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-9
msc.type: authoredcontent
ms.openlocfilehash: 01586ef6eecdbe1acfadfcfcc0c9ca8b442de2d3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838040"
---
<a name="add-a-new-item-to-the-database"></a>Aggiungere un nuovo elemento al Database
====================
da [Mike Wasson](https://github.com/MikeWasson)

[Download progetto completato](https://github.com/MikeWasson/BookService)

In questa sezione si aggiungerà la possibilità per gli utenti creare un nuovo libro. Nell'app. js, aggiungere il codice seguente al modello di visualizzazione:

[!code-javascript[Main](part-9/samples/sample1.js)]

In Index. cshtml, sostituire il markup seguente:

[!code-html[Main](part-9/samples/sample2.html)]

con:

[!code-html[Main](part-9/samples/sample3.html)]

Questo codice crea un form per l'invio di un autore di nuovo. I valori per l'elenco di riepilogo a discesa di autore sono associati a dati per il `authors` osservabile in modello di visualizzazione. Per gli altri input di modulo, i valori vengono associati per il `newBook` proprietà del modello di visualizzazione.

Il gestore di invio del form è associato ai `addBook` funzione:

[!code-html[Main](part-9/samples/sample4.html)]

Il `addBook` funzione legge i valori correnti degli input form associato a dati per creare un oggetto JSON. Quindi esegue il postback l'oggetto JSON da `/api/books`.

> [!div class="step-by-step"]
> [Precedente](part-8.md)
> [Successivo](part-10.md)
