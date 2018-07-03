---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-8
title: Visualizzare i dettagli dell'elemento | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 75ef94b1-bbec-4681-9210-452dba816144
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-8
msc.type: authoredcontent
ms.openlocfilehash: 268c44f842cc2beb32a0a3e4c74b83b7ca9fd787
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37375186"
---
<a name="display-item-details"></a><span data-ttu-id="8f187-102">Visualizzare i dettagli elemento</span><span class="sxs-lookup"><span data-stu-id="8f187-102">Display Item Details</span></span>
====================
<span data-ttu-id="8f187-103">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="8f187-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="8f187-104">Download progetto completato</span><span class="sxs-lookup"><span data-stu-id="8f187-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="8f187-105">In questa sezione si aggiungerà la possibilità di visualizzare i dettagli per ogni libro.</span><span class="sxs-lookup"><span data-stu-id="8f187-105">In this section, you will add the ability to view details for each book.</span></span> <span data-ttu-id="8f187-106">Nell'app. js, aggiungere il codice seguente al modello di visualizzazione:</span><span class="sxs-lookup"><span data-stu-id="8f187-106">In app.js, add to the following code to the view model:</span></span>

[!code-javascript[Main](part-8/samples/sample1.js)]

<span data-ttu-id="8f187-107">In Views/Home/Index.cshtml, aggiungere un elemento di associazione di dati per il collegamento di dettagli:</span><span class="sxs-lookup"><span data-stu-id="8f187-107">In Views/Home/Index.cshtml, add a data-bind element to the Details link:</span></span>

[!code-html[Main](part-8/samples/sample2.html?highlight=5)]

<span data-ttu-id="8f187-108">Ciò consente di associare il gestore di clic per il &lt;una&gt; elemento per il `getBookDetail` funzione nel modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="8f187-108">This binds the click handler for the &lt;a&gt; element to the `getBookDetail` function on the view model.</span></span>

<span data-ttu-id="8f187-109">Nello stesso file, sostituire il ricarico seguente:</span><span class="sxs-lookup"><span data-stu-id="8f187-109">In the same file, replace the following mark-up:</span></span>

[!code-html[Main](part-8/samples/sample3.html)]

<span data-ttu-id="8f187-110">con il seguente:</span><span class="sxs-lookup"><span data-stu-id="8f187-110">with this:</span></span>

[!code-html[Main](part-8/samples/sample4.html)]

<span data-ttu-id="8f187-111">Questo codice crea una tabella in cui è associato a dati per le proprietà del `detail` osservabile in modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="8f187-111">This markup creates a table that is data-bound to the properties of the `detail` observable in the view model.</span></span>

<span data-ttu-id="8f187-112">Il "&lt;! - ko -&gt; &quot; sintassi consente di includere un'associazione Knockout di fuori di un elemento DOM.</span><span class="sxs-lookup"><span data-stu-id="8f187-112">The "&lt;!-- ko --&gt;&quot; syntax lets you include a Knockout binding outside of a DOM element.</span></span> <span data-ttu-id="8f187-113">In questo caso, il `if` binding fa sì che in questa sezione di markup deve essere visualizzato solo quando `details` è diverso da null.</span><span class="sxs-lookup"><span data-stu-id="8f187-113">In this case, the `if` binding causes this section of markup to be displayed only when `details` is non-null.</span></span>

[!code-html[Main](part-8/samples/sample5.html)]

<span data-ttu-id="8f187-114">Ora, se si esegue l'app e fare clic su uno dei &quot;dettaglio&quot; collegamenti, l'app verranno visualizzati i dettagli della Rubrica.</span><span class="sxs-lookup"><span data-stu-id="8f187-114">Now if you run the app and click one of the &quot;Detail&quot; links, the app will display the book details.</span></span>

[![](part-8/_static/image2.png)](part-8/_static/image1.png)

> [!div class="step-by-step"]
> <span data-ttu-id="8f187-115">[Precedente](part-7.md)
> [Successivo](part-9.md)</span><span class="sxs-lookup"><span data-stu-id="8f187-115">[Previous](part-7.md)
[Next](part-9.md)</span></span>
