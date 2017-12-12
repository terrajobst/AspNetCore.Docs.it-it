---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-9
title: Aggiungere un nuovo elemento al Database | Documenti Microsoft
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 0967c29e-e124-4db0-a788-c45d0ff5aff2
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-9
msc.type: authoredcontent
ms.openlocfilehash: d33355b1bd286513958f71ce5521942a6cbb584f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="add-a-new-item-to-the-database"></a>Aggiungere un nuovo elemento al Database
====================
da [Mike Wasson](https://github.com/MikeWasson)

[Scaricare il progetto completato](https://github.com/MikeWasson/BookService)

In questa sezione si aggiungerà la possibilità agli utenti di creare una nuova rubrica. In app.js, aggiungere il codice seguente per il modello di visualizzazione:

[!code-javascript[Main](part-9/samples/sample1.js)]

In cshtml, sostituire il markup seguente:

[!code-html[Main](part-9/samples/sample2.html)]

con:

[!code-html[Main](part-9/samples/sample3.html)]

Questo codice crea un modulo per l'invio di un nuovo autore. I valori per l'elenco di riepilogo a discesa di autore vengono associati a dati per il `authors` observable nel modello di visualizzazione. Per gli altri form, i valori sono associati a dati per il `newBook` proprietà del modello di visualizzazione.

Il gestore di invio del form è associato il `addBook` funzione:

[!code-html[Main](part-9/samples/sample4.html)]

Il `addBook` funzione legge i valori correnti degli input form associato a dati per creare un oggetto JSON. Quindi invia l'oggetto JSON per `/api/books`.

>[!div class="step-by-step"]
[Precedente](part-8.md)
[Successivo](part-10.md)
