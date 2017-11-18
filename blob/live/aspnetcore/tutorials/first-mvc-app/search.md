---
title: "Aggiunta della funzionalità di ricerca"
author: rick-anderson
description: "Illustra come aggiungere la funzionalità di ricerca a una semplice app ASP.NET Core MVC"
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 03/07/2017
ms.topic: get-started-article
ms.assetid: d69e5529-8ef6-4628-855d-200206d962b9
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/search
ms.openlocfilehash: f811cd0063404872b0e993a99e8a92cc354064ce
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
[!INCLUDE[adding-model](../../includes/mvc-intro/search1.md)]

<span data-ttu-id="3e91a-104">È possibile rinominare rapidamente il parametro `searchString` `id` con il comando **Rinomina**.</span><span class="sxs-lookup"><span data-stu-id="3e91a-104">You can quickly rename the `searchString` parameter to `id` with the **rename** command.</span></span> <span data-ttu-id="3e91a-105">Fare clic con il pulsante destro del mouse su `searchString` **> Rinomina**.</span><span class="sxs-lookup"><span data-stu-id="3e91a-105">Right click on `searchString` **> Rename**.</span></span>

![Menu di scelta rapida](search/_static/rename.png)

<span data-ttu-id="3e91a-107">Vengono evidenziate le destinazioni da rinominare.</span><span class="sxs-lookup"><span data-stu-id="3e91a-107">The rename targets are highlighted.</span></span>

![Editor di codice che mostra la variabile evidenziata in tutto il metodo ActionResult Index](search/_static/rename2.png)

<span data-ttu-id="3e91a-109">Modificare il parametro in `id` e tutte le occorrenze di `searchString` cambiano in `id`.</span><span class="sxs-lookup"><span data-stu-id="3e91a-109">Change the parameter to `id` and all occurrences of `searchString` change to `id`.</span></span>

![Editor di codice che mostra che la variabile è stata modificata in id](search/_static/rename3.png)

[!INCLUDE[adding-model](../../includes/mvc-intro/search2.md)]

<span data-ttu-id="3e91a-111">Si noti come l'aggiornamento del markup sia più semplice con intelliSense.</span><span class="sxs-lookup"><span data-stu-id="3e91a-111">Notice how intelliSense helps us update the markup.</span></span>

![Menu di scelta rapida IntelliSense con method selezionato nell'elenco di attributi per l'elemento modulo](search/_static/int_m.png)

![Menu di scelta rapida IntelliSense con get selezionato nell'elenco di valori dell'attributo method](search/_static/int_get.png)

<span data-ttu-id="3e91a-114">Si noti il tipo di carattere distintivo nel tag `<form>`.</span><span class="sxs-lookup"><span data-stu-id="3e91a-114">Notice the distinctive font in the `<form>` tag.</span></span> <span data-ttu-id="3e91a-115">Il tipo di carattere distintivo indica che il tag è supportato dagli [helper tag](../../mvc/views/tag-helpers/intro.md).</span><span class="sxs-lookup"><span data-stu-id="3e91a-115">That distinctive font indicates the tag is supported by [Tag Helpers](../../mvc/views/tag-helpers/intro.md).</span></span>

![tag form con il testo di colore viola](search/_static/th_font.png)

[!INCLUDE[adding-model](../../includes/mvc-intro/search3.md)]

>[!div class="step-by-step"]
<span data-ttu-id="3e91a-117">[Precedente](controller-methods-views.md)
[Successivo](new-field.md)</span><span class="sxs-lookup"><span data-stu-id="3e91a-117">[Previous](controller-methods-views.md)
[Next](new-field.md)</span></span>  
