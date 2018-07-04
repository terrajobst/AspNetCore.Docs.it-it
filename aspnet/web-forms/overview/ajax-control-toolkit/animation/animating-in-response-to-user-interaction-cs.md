---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-cs
title: Animazione in risposta all'interazione dell'utente (c#) | Microsoft Docs
author: wenz
description: Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo. Le animazioni possono stelle...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: ea26549d-fbbf-4973-a108-b14cd1d6de26
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-cs
msc.type: authoredcontent
ms.openlocfilehash: 9dea9daf3df76558eb19a524475cedd8e2085297
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37379819"
---
<a name="animating-in-response-to-user-interaction-c"></a><span data-ttu-id="83e7b-104">Animazione in risposta all'interazione dell'utente (c#)</span><span class="sxs-lookup"><span data-stu-id="83e7b-104">Animating in Response To User Interaction (C#)</span></span>
====================
<span data-ttu-id="83e7b-105">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="83e7b-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="83e7b-106">[Scaricare il codice](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="83e7b-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6CS.pdf)</span></span>

> <span data-ttu-id="83e7b-107">Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="83e7b-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="83e7b-108">Le animazioni può essere avviata automaticamente o possono essere attivate dall'interazione dell'utente, ad esempio, facendo clic con il mouse.</span><span class="sxs-lookup"><span data-stu-id="83e7b-108">The animations can start automatically or may be triggered by user interaction, e.g. by clicking with the mouse.</span></span>


## <a name="overview"></a><span data-ttu-id="83e7b-109">Panoramica</span><span class="sxs-lookup"><span data-stu-id="83e7b-109">Overview</span></span>

<span data-ttu-id="83e7b-110">Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="83e7b-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="83e7b-111">Le animazioni può essere avviata automaticamente o possono essere attivate dall'interazione dell'utente, ad esempio, facendo clic con il mouse.</span><span class="sxs-lookup"><span data-stu-id="83e7b-111">The animations can start automatically or may be triggered by user interaction, e.g. by clicking with the mouse.</span></span>

## <a name="steps"></a><span data-ttu-id="83e7b-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="83e7b-112">Steps</span></span>

<span data-ttu-id="83e7b-113">Prima di tutto, includere il `ScriptManager` nella pagina; quindi, la libreria ASP.NET AJAX viene caricata, rendendo possibile usare il Toolkit di controllo:</span><span class="sxs-lookup"><span data-stu-id="83e7b-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample1.aspx)]

<span data-ttu-id="83e7b-114">Verrà applicata l'animazione a un pannello del testo che presenta un aspetto simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="83e7b-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample2.aspx)]

<span data-ttu-id="83e7b-115">Nella classe CSS associata per il pannello, definire un colore di sfondo interessante e anche impostare una larghezza fissa per il pannello:</span><span class="sxs-lookup"><span data-stu-id="83e7b-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](animating-in-response-to-user-interaction-cs/samples/sample3.css)]

<span data-ttu-id="83e7b-116">Quindi, aggiungere il `AnimationExtender` alla pagina, fornendo un' `ID`, il `TargetControlID` attributo e l'obbligatoria `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="83e7b-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample4.aspx)]

<span data-ttu-id="83e7b-117">All'interno di `<Animations>` nodo, sono disponibili cinque modi per avviare l'animazione tramite l'interazione dell'utente (l'elemento mancano è `<OnLoad>` che viene eseguita una volta l'intera pagina è stata caricata completamente):</span><span class="sxs-lookup"><span data-stu-id="83e7b-117">Within the `<Animations>` node, there are five ways to start the animation via user interaction (the missing element is `<OnLoad>` which is executed once the whole page has been fully loaded):</span></span>

- <span data-ttu-id="83e7b-118">`<OnClick>` (fare clic del mouse sul controllo)</span><span class="sxs-lookup"><span data-stu-id="83e7b-118">`<OnClick>` (mouse click on the control)</span></span>
- <span data-ttu-id="83e7b-119">`<OnHoverOut>` (mouse lascia il controllo)</span><span class="sxs-lookup"><span data-stu-id="83e7b-119">`<OnHoverOut>` (mouse leaves the control)</span></span>
- <span data-ttu-id="83e7b-120">`<OnHoverOver>` (passaggio del mouse su un controllo, l'arresto di `<OnHoverOut>` animazione)</span><span class="sxs-lookup"><span data-stu-id="83e7b-120">`<OnHoverOver>` (mouse hovers over a control, stopping the `<OnHoverOut>` animation)</span></span>
- <span data-ttu-id="83e7b-121">`<OnMouseOut>` (mouse esce da un controllo)</span><span class="sxs-lookup"><span data-stu-id="83e7b-121">`<OnMouseOut>` (mouse leaves a control)</span></span>
- <span data-ttu-id="83e7b-122">`<OnMouseOver>` (passaggio del mouse su un controllo, non interrompe la `<OnMouseOut>` animazione)</span><span class="sxs-lookup"><span data-stu-id="83e7b-122">`<OnMouseOver>` (mouse hovers over a control, not stopping the `<OnMouseOut>` animation)</span></span>

<span data-ttu-id="83e7b-123">In questo scenario, `<OnClick>` viene usato.</span><span class="sxs-lookup"><span data-stu-id="83e7b-123">In this scenario, `<OnClick>` is used.</span></span> <span data-ttu-id="83e7b-124">Quando l'utente fa clic sul pannello, ridimensionamento e dissolve nello stesso momento.</span><span class="sxs-lookup"><span data-stu-id="83e7b-124">When the user clicks on the panel, it is resized and fades out at the same time.</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample5.aspx)]


<span data-ttu-id="83e7b-125">[![Un clic del mouse viene avviata l'animazione](animating-in-response-to-user-interaction-cs/_static/image2.png)](animating-in-response-to-user-interaction-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="83e7b-125">[![A mouse click starts the animation](animating-in-response-to-user-interaction-cs/_static/image2.png)](animating-in-response-to-user-interaction-cs/_static/image1.png)</span></span>

<span data-ttu-id="83e7b-126">Un clic del mouse viene avviata l'animazione ([fare clic per visualizzare l'immagine con dimensioni normali](animating-in-response-to-user-interaction-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="83e7b-126">A mouse click starts the animation ([Click to view full-size image](animating-in-response-to-user-interaction-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="83e7b-127">[Precedente](picking-one-animation-out-of-a-list-cs.md)
> [Successivo](disabling-actions-during-animation-cs.md)</span><span class="sxs-lookup"><span data-stu-id="83e7b-127">[Previous](picking-one-animation-out-of-a-list-cs.md)
[Next](disabling-actions-during-animation-cs.md)</span></span>
