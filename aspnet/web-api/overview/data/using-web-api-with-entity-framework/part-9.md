---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-9
title: Aggiungere un nuovo elemento al Database | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 06/16/2014
ms.assetid: 0967c29e-e124-4db0-a788-c45d0ff5aff2
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-9
msc.type: authoredcontent
ms.openlocfilehash: 36251ba907a6f580b63f0fded0591c26b6ff879e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37818508"
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
