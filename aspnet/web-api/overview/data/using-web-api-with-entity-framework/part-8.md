---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-8
title: Visualizzare i dettagli dell'elemento | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 06/16/2014
ms.assetid: 75ef94b1-bbec-4681-9210-452dba816144
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-8
msc.type: authoredcontent
ms.openlocfilehash: 29402e70e5fcaac04972788499695ddde4b96531
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37832146"
---
<a name="display-item-details"></a>Visualizzare i dettagli elemento
====================
da [Mike Wasson](https://github.com/MikeWasson)

[Download progetto completato](https://github.com/MikeWasson/BookService)

In questa sezione si aggiungerà la possibilità di visualizzare i dettagli per ogni libro. Nell'app. js, aggiungere il codice seguente al modello di visualizzazione:

[!code-javascript[Main](part-8/samples/sample1.js)]

In Views/Home/Index.cshtml, aggiungere un elemento di associazione di dati per il collegamento di dettagli:

[!code-html[Main](part-8/samples/sample2.html?highlight=5)]

Ciò consente di associare il gestore di clic per il &lt;una&gt; elemento per il `getBookDetail` funzione nel modello di visualizzazione.

Nello stesso file, sostituire il ricarico seguente:

[!code-html[Main](part-8/samples/sample3.html)]

con il seguente:

[!code-html[Main](part-8/samples/sample4.html)]

Questo codice crea una tabella in cui è associato a dati per le proprietà del `detail` osservabile in modello di visualizzazione.

Il "&lt;! - ko -&gt; &quot; sintassi consente di includere un'associazione Knockout di fuori di un elemento DOM. In questo caso, il `if` binding fa sì che in questa sezione di markup deve essere visualizzato solo quando `details` è diverso da null.

[!code-html[Main](part-8/samples/sample5.html)]

Ora, se si esegue l'app e fare clic su uno dei &quot;dettaglio&quot; collegamenti, l'app verranno visualizzati i dettagli della Rubrica.

[![](part-8/_static/image2.png)](part-8/_static/image1.png)

> [!div class="step-by-step"]
> [Precedente](part-7.md)
> [Successivo](part-9.md)
