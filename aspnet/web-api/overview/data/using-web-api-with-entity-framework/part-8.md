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
---
<a name="display-item-details"></a><span data-ttu-id="c1a37-102">Visualizzare i dettagli degli elementi</span><span class="sxs-lookup"><span data-stu-id="c1a37-102">Display Item Details</span></span>
====================
<span data-ttu-id="c1a37-103">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="c1a37-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="c1a37-104">Scaricare il progetto completato</span><span class="sxs-lookup"><span data-stu-id="c1a37-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="c1a37-105">In questa sezione si aggiungerà la possibilità di visualizzare i dettagli per ogni libro.</span><span class="sxs-lookup"><span data-stu-id="c1a37-105">In this section, you will add the ability to view details for each book.</span></span> <span data-ttu-id="c1a37-106">In app.js, aggiungere il codice seguente per il modello di visualizzazione:</span><span class="sxs-lookup"><span data-stu-id="c1a37-106">In app.js, add to the following code to the view model:</span></span>

[!code-javascript[Main](part-8/samples/sample1.js)]

<span data-ttu-id="c1a37-107">In Views/Home/Index.cshtml, aggiungere un elemento di associazione dati per il collegamento dettagli:</span><span class="sxs-lookup"><span data-stu-id="c1a37-107">In Views/Home/Index.cshtml, add a data-bind element to the Details link:</span></span>

[!code-html[Main](part-8/samples/sample2.html?highlight=5)]

<span data-ttu-id="c1a37-108">Ciò consente di associare il gestore click per il &lt;un&gt; elemento per il `getBookDetail` funzione nel modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="c1a37-108">This binds the click handler for the &lt;a&gt; element to the `getBookDetail` function on the view model.</span></span>

<span data-ttu-id="c1a37-109">Nello stesso file, sostituire il contrassegno-up seguenti:</span><span class="sxs-lookup"><span data-stu-id="c1a37-109">In the same file, replace the following mark-up:</span></span>

[!code-html[Main](part-8/samples/sample3.html)]

<span data-ttu-id="c1a37-110">con il seguente:</span><span class="sxs-lookup"><span data-stu-id="c1a37-110">with this:</span></span>

[!code-html[Main](part-8/samples/sample4.html)]

<span data-ttu-id="c1a37-111">Questo markup si crea una tabella in cui è associato alle proprietà del `detail` observable nel modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="c1a37-111">This markup creates a table that is data-bound to the properties of the `detail` observable in the view model.</span></span>

<span data-ttu-id="c1a37-112">Il "&lt;! - ko -&gt; &quot; sintassi consente di includere un'associazione Knockout all'esterno di un elemento DOM.</span><span class="sxs-lookup"><span data-stu-id="c1a37-112">The "&lt;!-- ko --&gt;&quot; syntax lets you include a Knockout binding outside of a DOM element.</span></span> <span data-ttu-id="c1a37-113">In questo caso, il `if` associazione fa sì che in questa sezione di markup deve essere visualizzato solo quando `details` è diverso da null.</span><span class="sxs-lookup"><span data-stu-id="c1a37-113">In this case, the `if` binding causes this section of markup to be displayed only when `details` is non-null.</span></span>

[!code-html[Main](part-8/samples/sample5.html)]

<span data-ttu-id="c1a37-114">Se si esegue l'app e fare clic su uno del &quot;dettaglio&quot; collegamenti, l'app verranno visualizzati i dettagli di libro.</span><span class="sxs-lookup"><span data-stu-id="c1a37-114">Now if you run the app and click one of the &quot;Detail&quot; links, the app will display the book details.</span></span>

[![](part-8/_static/image2.png)](part-8/_static/image1.png)

> [!div class="step-by-step"]
> <span data-ttu-id="c1a37-115">[Precedente](part-7.md)
> [Successivo](part-9.md)</span><span class="sxs-lookup"><span data-stu-id="c1a37-115">[Previous](part-7.md)
[Next](part-9.md)</span></span>
