---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-vb
title: Modifica dell'ordine z di un DropShadow (VB) | Documenti Microsoft
author: wenz
description: "Il controllo DropShadow AJAX Control Toolkit estende un pannello con un'ombreggiatura. Tuttavia questo shadow talvolta è in conflitto con altri controlli, per insta..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: ecb004b5-82c0-44fb-bcaf-233fffac6195
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-vb
msc.type: authoredcontent
ms.openlocfilehash: 844ea00c2ef1c974aa72c7dd627819b0429d612e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="adjusting-the-z-index-of-a-dropshadow-vb"></a><span data-ttu-id="8187e-104">Modifica dell'ordine z di un DropShadow (VB)</span><span class="sxs-lookup"><span data-stu-id="8187e-104">Adjusting the Z-Index of a DropShadow (VB)</span></span>
====================
<span data-ttu-id="8187e-105">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="8187e-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="8187e-106">[Scaricare codice](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="8187e-106">[Download Code](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1VB.pdf)</span></span>

> <span data-ttu-id="8187e-107">Il controllo DropShadow AJAX Control Toolkit estende un pannello con un'ombreggiatura.</span><span class="sxs-lookup"><span data-stu-id="8187e-107">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="8187e-108">Tuttavia questo shadow talvolta è in conflitto con altri controlli, ad esempio il controllo Menu ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="8187e-108">However this shadow sometimes conflicts with other controls, for instance the ASP.NET Menu control.</span></span> <span data-ttu-id="8187e-109">Quando una voce di menu viene visualizzata, viene visualizzato dietro l'ombreggiatura.</span><span class="sxs-lookup"><span data-stu-id="8187e-109">When a menu entry pops up, it appears behind the drop shadow.</span></span>


## <a name="overview"></a><span data-ttu-id="8187e-110">Panoramica</span><span class="sxs-lookup"><span data-stu-id="8187e-110">Overview</span></span>

<span data-ttu-id="8187e-111">Il controllo DropShadow AJAX Control Toolkit estende un pannello con un'ombreggiatura.</span><span class="sxs-lookup"><span data-stu-id="8187e-111">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="8187e-112">Tuttavia questo shadow talvolta è in conflitto con altri controlli, ad esempio il controllo Menu ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="8187e-112">However this shadow sometimes conflicts with other controls, for instance the ASP.NET Menu control.</span></span> <span data-ttu-id="8187e-113">Quando una voce di menu viene visualizzata, viene visualizzato dietro l'ombreggiatura.</span><span class="sxs-lookup"><span data-stu-id="8187e-113">When a menu entry pops up, it appears behind the drop shadow.</span></span>

## <a name="steps"></a><span data-ttu-id="8187e-114">Passaggi</span><span class="sxs-lookup"><span data-stu-id="8187e-114">Steps</span></span>

<span data-ttu-id="8187e-115">Il codice inizia con il pannello, contenente testo in modo che il pannello contiene testo sufficiente per l'effetto sia visibile:</span><span class="sxs-lookup"><span data-stu-id="8187e-115">The code commences with the Panel itself, containing enough text so that the panel contains enough text for the effect to be visible:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample1.aspx)]

<span data-ttu-id="8187e-116">Un altro pannello viene eseguito immediatamente prima di `panelShadow` pannello.</span><span class="sxs-lookup"><span data-stu-id="8187e-116">Another panel is placed directly before the `panelShadow` panel.</span></span> <span data-ttu-id="8187e-117">Contiene un menu con orientamento orizzontale in modo che le voci di menu visualizzate in (oppure piuttosto: sotto) il `dropShadow` pannello):</span><span class="sxs-lookup"><span data-stu-id="8187e-117">It contains a menu with horizontal orientation so that menu entries appear over (or rather: under) the `dropShadow` panel):</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample2.aspx)]

<span data-ttu-id="8187e-118">Quindi, `DropShadowExtender` viene aggiunto per estendere il `panelShadow` pannello con un effetto di ombreggiatura:</span><span class="sxs-lookup"><span data-stu-id="8187e-118">Then, the `DropShadowExtender` is added to extend the `panelShadow` panel with a drop shadow effect:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample3.aspx)]

<span data-ttu-id="8187e-119">Infine, ASP.NET AJAX `ScriptManager` controllo Abilita il Toolkit di controllo utilizzare:</span><span class="sxs-lookup"><span data-stu-id="8187e-119">Finally, the ASP.NET AJAX `ScriptManager` control enables the Control Toolkit to work:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample4.aspx)]

<span data-ttu-id="8187e-120">Quando si esegue questo script, le voci di menu vengono visualizzate sotto il pannello.</span><span class="sxs-lookup"><span data-stu-id="8187e-120">When you run this script, the menu entries appear underneath the panel.</span></span> <span data-ttu-id="8187e-121">Tuttavia il menu utilizza la classe CSS `panel` in cui è sufficiente definire due operazioni per visualizzare gli elementi davanti a un pannello di:</span><span class="sxs-lookup"><span data-stu-id="8187e-121">However the menu uses the CSS class `panel` where you just have to define two things to make elements appear in front of the other panel:</span></span>

- <span data-ttu-id="8187e-122">Posizionamento relativo</span><span class="sxs-lookup"><span data-stu-id="8187e-122">Relative positioning</span></span>
- <span data-ttu-id="8187e-123">Indice z positivo</span><span class="sxs-lookup"><span data-stu-id="8187e-123">A positive z-index</span></span>

[!code-css[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample5.css)]

<span data-ttu-id="8187e-124">Quindi, `DropShadowExtender` controllo non è in conflitto più con il controllo Menu.</span><span class="sxs-lookup"><span data-stu-id="8187e-124">Then, the `DropShadowExtender` control does not conflict any longer with the Menu control.</span></span>


<span data-ttu-id="8187e-125">[![Prima: La voce di menu non è visibile](adjusting-the-z-index-of-a-dropshadow-vb/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="8187e-125">[![Before: The menu entry is not visible](adjusting-the-z-index-of-a-dropshadow-vb/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image1.png)</span></span>

<span data-ttu-id="8187e-126">Prima: La voce di menu non è visibile ([fare clic per visualizzare l'immagine ingrandita](adjusting-the-z-index-of-a-dropshadow-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="8187e-126">Before: The menu entry is not visible ([Click to view full-size image](adjusting-the-z-index-of-a-dropshadow-vb/_static/image3.png))</span></span>


<span data-ttu-id="8187e-127">[![Dopo: Viene visualizzata la voce di menu](adjusting-the-z-index-of-a-dropshadow-vb/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="8187e-127">[![After: The menu entry appears](adjusting-the-z-index-of-a-dropshadow-vb/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image4.png)</span></span>

<span data-ttu-id="8187e-128">Dopo: Viene visualizzata la voce di menu ([fare clic per visualizzare l'immagine ingrandita](adjusting-the-z-index-of-a-dropshadow-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="8187e-128">After: The menu entry appears ([Click to view full-size image](adjusting-the-z-index-of-a-dropshadow-vb/_static/image6.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="8187e-129">[Precedente](manipulating-dropshadow-properties-from-client-code-cs.md)
[Successivo](manipulating-dropshadow-properties-from-client-code-vb.md)</span><span class="sxs-lookup"><span data-stu-id="8187e-129">[Previous](manipulating-dropshadow-properties-from-client-code-cs.md)
[Next](manipulating-dropshadow-properties-from-client-code-vb.md)</span></span>
