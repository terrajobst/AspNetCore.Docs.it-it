---
title: Aggiunta della funzionalità di ricerca
author: rick-anderson
description: Illustra come aggiungere la funzionalità di ricerca a una semplice app ASP.NET Core MVC
manager: wpickett
ms.author: riande
ms.date: 03/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app/search
ms.openlocfilehash: 772409f11a43e1d130265d8bba3bad1da5a41b86
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
[!INCLUDE [adding-model](../../includes/mvc-intro/search1.md)]

<span data-ttu-id="95de1-103">È possibile rinominare rapidamente il parametro `searchString` `id` con il comando **Rinomina**.</span><span class="sxs-lookup"><span data-stu-id="95de1-103">You can quickly rename the `searchString` parameter to `id` with the **rename** command.</span></span> <span data-ttu-id="95de1-104">Fare clic con il pulsante destro del mouse su `searchString` **> Rinomina**.</span><span class="sxs-lookup"><span data-stu-id="95de1-104">Right click on `searchString` **> Rename**.</span></span>

![Menu di scelta rapida](search/_static/rename.png)

<span data-ttu-id="95de1-106">Vengono evidenziate le destinazioni da rinominare.</span><span class="sxs-lookup"><span data-stu-id="95de1-106">The rename targets are highlighted.</span></span>

![Editor di codice che mostra la variabile evidenziata in tutto il metodo ActionResult Index](search/_static/rename2.png)

<span data-ttu-id="95de1-108">Modificare il parametro in `id` e tutte le occorrenze di `searchString` cambiano in `id`.</span><span class="sxs-lookup"><span data-stu-id="95de1-108">Change the parameter to `id` and all occurrences of `searchString` change to `id`.</span></span>

![Editor di codice che mostra che la variabile è stata modificata in id](search/_static/rename3.png)

[!INCLUDE [adding-model](../../includes/mvc-intro/search2.md)]

<span data-ttu-id="95de1-110">Si noti come l'aggiornamento del markup sia più semplice con intelliSense.</span><span class="sxs-lookup"><span data-stu-id="95de1-110">Notice how intelliSense helps us update the markup.</span></span>

![Menu di scelta rapida IntelliSense con method selezionato nell'elenco di attributi per l'elemento modulo](search/_static/int_m.png)

![Menu di scelta rapida IntelliSense con get selezionato nell'elenco di valori dell'attributo method](search/_static/int_get.png)

<span data-ttu-id="95de1-113">Si noti il tipo di carattere distintivo nel tag `<form>`.</span><span class="sxs-lookup"><span data-stu-id="95de1-113">Notice the distinctive font in the `<form>` tag.</span></span> <span data-ttu-id="95de1-114">Il tipo di carattere distintivo indica che il tag è supportato dagli [helper tag](../../mvc/views/tag-helpers/intro.md).</span><span class="sxs-lookup"><span data-stu-id="95de1-114">That distinctive font indicates the tag is supported by [Tag Helpers](../../mvc/views/tag-helpers/intro.md).</span></span>

![tag form con il testo di colore viola](search/_static/th_font.png)

[!INCLUDE [adding-model](../../includes/mvc-intro/search3.md)]

> [!div class="step-by-step"]
> <span data-ttu-id="95de1-116">[Precedente](controller-methods-views.md)
> [Successivo](new-field.md)</span><span class="sxs-lookup"><span data-stu-id="95de1-116">[Previous](controller-methods-views.md)
[Next](new-field.md)</span></span>  
