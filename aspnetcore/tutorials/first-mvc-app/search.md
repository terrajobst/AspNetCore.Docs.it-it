---
title: "Aggiunta della funzionalità di ricerca"
author: rick-anderson
description: "Illustra come aggiungere la funzionalità di ricerca a una semplice app ASP.NET Core MVC"
manager: wpickett
ms.author: riande
ms.date: 03/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app/search
ms.openlocfilehash: 3ab9086275ec4c3651383c4c845e40db55f67f4c
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/30/2018
---
[!INCLUDE[adding-model](../../includes/mvc-intro/search1.md)]

È possibile rinominare rapidamente il parametro `searchString` `id` con il comando **Rinomina**. Fare clic con il pulsante destro del mouse su `searchString` **> Rinomina**.

![Menu di scelta rapida](search/_static/rename.png)

Vengono evidenziate le destinazioni da rinominare.

![Editor di codice che mostra la variabile evidenziata in tutto il metodo ActionResult Index](search/_static/rename2.png)

Modificare il parametro in `id` e tutte le occorrenze di `searchString` cambiano in `id`.

![Editor di codice che mostra che la variabile è stata modificata in id](search/_static/rename3.png)

[!INCLUDE[adding-model](../../includes/mvc-intro/search2.md)]

Si noti come l'aggiornamento del markup sia più semplice con intelliSense.

![Menu di scelta rapida IntelliSense con method selezionato nell'elenco di attributi per l'elemento modulo](search/_static/int_m.png)

![Menu di scelta rapida IntelliSense con get selezionato nell'elenco di valori dell'attributo method](search/_static/int_get.png)

Si noti il tipo di carattere distintivo nel tag `<form>`. Il tipo di carattere distintivo indica che il tag è supportato dagli [helper tag](../../mvc/views/tag-helpers/intro.md).

![tag form con il testo di colore viola](search/_static/th_font.png)

[!INCLUDE[adding-model](../../includes/mvc-intro/search3.md)]

>[!div class="step-by-step"]
[Precedente](controller-methods-views.md)
[Successivo](new-field.md)  
