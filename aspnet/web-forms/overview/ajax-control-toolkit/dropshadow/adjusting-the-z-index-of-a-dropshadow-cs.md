---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-cs
title: Modifica dell'ordine z di un DropShadow (c#) | Documenti Microsoft
author: wenz
description: Il controllo DropShadow AJAX Control Toolkit estende un pannello con un'ombreggiatura. Tuttavia questo shadow talvolta è in conflitto con altri controlli, per insta...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 14133833-e518-4347-87b9-6b6f71f14a77
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-cs
msc.type: authoredcontent
ms.openlocfilehash: 82add8427c8e574b213b67315e69bb4c28846095
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="adjusting-the-z-index-of-a-dropshadow-c"></a><span data-ttu-id="4fa0a-104">Modifica dell'ordine z di un DropShadow (c#)</span><span class="sxs-lookup"><span data-stu-id="4fa0a-104">Adjusting the Z-Index of a DropShadow (C#)</span></span>
====================
<span data-ttu-id="4fa0a-105">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="4fa0a-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="4fa0a-106">[Scaricare codice](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="4fa0a-106">[Download Code](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1CS.pdf)</span></span>

> <span data-ttu-id="4fa0a-107">Il controllo DropShadow AJAX Control Toolkit estende un pannello con un'ombreggiatura.</span><span class="sxs-lookup"><span data-stu-id="4fa0a-107">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="4fa0a-108">Tuttavia questo shadow talvolta è in conflitto con altri controlli, ad esempio il controllo Menu ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="4fa0a-108">However this shadow sometimes conflicts with other controls, for instance the ASP.NET Menu control.</span></span> <span data-ttu-id="4fa0a-109">Quando una voce di menu viene visualizzata, viene visualizzato dietro l'ombreggiatura.</span><span class="sxs-lookup"><span data-stu-id="4fa0a-109">When a menu entry pops up, it appears behind the drop shadow.</span></span>


## <a name="overview"></a><span data-ttu-id="4fa0a-110">Panoramica</span><span class="sxs-lookup"><span data-stu-id="4fa0a-110">Overview</span></span>

<span data-ttu-id="4fa0a-111">Il controllo DropShadow AJAX Control Toolkit estende un pannello con un'ombreggiatura.</span><span class="sxs-lookup"><span data-stu-id="4fa0a-111">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="4fa0a-112">Tuttavia questo shadow talvolta è in conflitto con altri controlli, ad esempio il controllo Menu ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="4fa0a-112">However this shadow sometimes conflicts with other controls, for instance the ASP.NET Menu control.</span></span> <span data-ttu-id="4fa0a-113">Quando una voce di menu viene visualizzata, viene visualizzato dietro l'ombreggiatura.</span><span class="sxs-lookup"><span data-stu-id="4fa0a-113">When a menu entry pops up, it appears behind the drop shadow.</span></span>

## <a name="steps"></a><span data-ttu-id="4fa0a-114">Passaggi</span><span class="sxs-lookup"><span data-stu-id="4fa0a-114">Steps</span></span>

<span data-ttu-id="4fa0a-115">Il codice inizia con il pannello, contenente testo in modo che il pannello contiene testo sufficiente per l'effetto sia visibile:</span><span class="sxs-lookup"><span data-stu-id="4fa0a-115">The code commences with the Panel itself, containing enough text so that the panel contains enough text for the effect to be visible:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample1.aspx)]

<span data-ttu-id="4fa0a-116">Un altro pannello viene eseguito immediatamente prima di `panelShadow` pannello.</span><span class="sxs-lookup"><span data-stu-id="4fa0a-116">Another panel is placed directly before the `panelShadow` panel.</span></span> <span data-ttu-id="4fa0a-117">Contiene un menu con orientamento orizzontale in modo che le voci di menu visualizzate in (oppure piuttosto: sotto) il `dropShadow` pannello):</span><span class="sxs-lookup"><span data-stu-id="4fa0a-117">It contains a menu with horizontal orientation so that menu entries appear over (or rather: under) the `dropShadow` panel):</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample2.aspx)]

<span data-ttu-id="4fa0a-118">Quindi, `DropShadowExtender` viene aggiunto per estendere il `panelShadow` pannello con un effetto di ombreggiatura:</span><span class="sxs-lookup"><span data-stu-id="4fa0a-118">Then, the `DropShadowExtender` is added to extend the `panelShadow` panel with a drop shadow effect:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample3.aspx)]

<span data-ttu-id="4fa0a-119">Infine, ASP.NET AJAX `ScriptManager` controllo Abilita il Toolkit di controllo utilizzare:</span><span class="sxs-lookup"><span data-stu-id="4fa0a-119">Finally, the ASP.NET AJAX `ScriptManager` control enables the Control Toolkit to work:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample4.aspx)]

<span data-ttu-id="4fa0a-120">Quando si esegue questo script, le voci di menu vengono visualizzate sotto il pannello.</span><span class="sxs-lookup"><span data-stu-id="4fa0a-120">When you run this script, the menu entries appear underneath the panel.</span></span> <span data-ttu-id="4fa0a-121">Tuttavia il menu utilizza la classe CSS `panel` in cui è sufficiente definire due operazioni per visualizzare gli elementi davanti a un pannello di:</span><span class="sxs-lookup"><span data-stu-id="4fa0a-121">However the menu uses the CSS class `panel` where you just have to define two things to make elements appear in front of the other panel:</span></span>

- <span data-ttu-id="4fa0a-122">Posizionamento relativo</span><span class="sxs-lookup"><span data-stu-id="4fa0a-122">Relative positioning</span></span>
- <span data-ttu-id="4fa0a-123">Indice z positivo</span><span class="sxs-lookup"><span data-stu-id="4fa0a-123">A positive z-index</span></span>

[!code-css[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample5.css)]

<span data-ttu-id="4fa0a-124">Quindi, `DropShadowExtender` controllo non è in conflitto più con il controllo Menu.</span><span class="sxs-lookup"><span data-stu-id="4fa0a-124">Then, the `DropShadowExtender` control does not conflict any longer with the Menu control.</span></span>


<span data-ttu-id="4fa0a-125">[![Prima: La voce di menu non è visibile](adjusting-the-z-index-of-a-dropshadow-cs/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="4fa0a-125">[![Before: The menu entry is not visible](adjusting-the-z-index-of-a-dropshadow-cs/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image1.png)</span></span>

<span data-ttu-id="4fa0a-126">Prima: La voce di menu non è visibile ([fare clic per visualizzare l'immagine ingrandita](adjusting-the-z-index-of-a-dropshadow-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="4fa0a-126">Before: The menu entry is not visible ([Click to view full-size image](adjusting-the-z-index-of-a-dropshadow-cs/_static/image3.png))</span></span>


<span data-ttu-id="4fa0a-127">[![Dopo: Viene visualizzata la voce di menu](adjusting-the-z-index-of-a-dropshadow-cs/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="4fa0a-127">[![After: The menu entry appears](adjusting-the-z-index-of-a-dropshadow-cs/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image4.png)</span></span>

<span data-ttu-id="4fa0a-128">Dopo: Viene visualizzata la voce di menu ([fare clic per visualizzare l'immagine ingrandita](adjusting-the-z-index-of-a-dropshadow-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="4fa0a-128">After: The menu entry appears ([Click to view full-size image](adjusting-the-z-index-of-a-dropshadow-cs/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="4fa0a-129">avanti</span><span class="sxs-lookup"><span data-stu-id="4fa0a-129">Next</span></span>](manipulating-dropshadow-properties-from-client-code-cs.md)
