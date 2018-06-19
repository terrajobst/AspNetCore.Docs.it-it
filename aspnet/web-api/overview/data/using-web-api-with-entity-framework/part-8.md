---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-8
title: Visualizzare i dettagli dell'elemento | Documenti Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 75ef94b1-bbec-4681-9210-452dba816144
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-8
msc.type: authoredcontent
ms.openlocfilehash: 94863e94f2a8b3f1ce8a8fb85d877bc0768f3d8a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868087"
---
<a name="display-item-details"></a>Visualizzare i dettagli degli elementi
====================
da [Mike Wasson](https://github.com/MikeWasson)

[Scaricare il progetto completato](https://github.com/MikeWasson/BookService)

In questa sezione si aggiungerà la possibilità di visualizzare i dettagli per ogni libro. In app.js, aggiungere il codice seguente per il modello di visualizzazione:

[!code-javascript[Main](part-8/samples/sample1.js)]

In Views/Home/Index.cshtml, aggiungere un elemento di associazione dati per il collegamento dettagli:

[!code-html[Main](part-8/samples/sample2.html?highlight=5)]

Ciò consente di associare il gestore click per il &lt;un&gt; elemento per il `getBookDetail` funzione nel modello di visualizzazione.

Nello stesso file, sostituire il contrassegno-up seguenti:

[!code-html[Main](part-8/samples/sample3.html)]

con il seguente:

[!code-html[Main](part-8/samples/sample4.html)]

Questo markup si crea una tabella in cui è associato alle proprietà del `detail` observable nel modello di visualizzazione.

Il "&lt;! - ko -&gt; &quot; sintassi consente di includere un'associazione Knockout all'esterno di un elemento DOM. In questo caso, il `if` associazione fa sì che in questa sezione di markup deve essere visualizzato solo quando `details` è diverso da null.

[!code-html[Main](part-8/samples/sample5.html)]

Se si esegue l'app e fare clic su uno del &quot;dettaglio&quot; collegamenti, l'app verranno visualizzati i dettagli di libro.

[![](part-8/_static/image2.png)](part-8/_static/image1.png)

> [!div class="step-by-step"]
> [Precedente](part-7.md)
> [Successivo](part-9.md)
