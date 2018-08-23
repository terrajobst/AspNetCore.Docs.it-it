---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-cs
title: Animazione in risposta all'interazione dell'utente (c#) | Microsoft Docs
author: wenz
description: Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo. Le animazioni possono stelle...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: ea26549d-fbbf-4973-a108-b14cd1d6de26
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-cs
msc.type: authoredcontent
ms.openlocfilehash: 9de99b3194a5517047e40922d89c28e341ba8e20
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838024"
---
<a name="animating-in-response-to-user-interaction-c"></a><span data-ttu-id="9ecf6-104">Animazione in risposta all'interazione dell'utente (c#)</span><span class="sxs-lookup"><span data-stu-id="9ecf6-104">Animating in Response To User Interaction (C#)</span></span>
====================
<span data-ttu-id="9ecf6-105">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="9ecf6-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="9ecf6-106">[Scaricare il codice](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="9ecf6-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6CS.pdf)</span></span>

> <span data-ttu-id="9ecf6-107">Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="9ecf6-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="9ecf6-108">Le animazioni può essere avviata automaticamente o possono essere attivate dall'interazione dell'utente, ad esempio, facendo clic con il mouse.</span><span class="sxs-lookup"><span data-stu-id="9ecf6-108">The animations can start automatically or may be triggered by user interaction, e.g. by clicking with the mouse.</span></span>


## <a name="overview"></a><span data-ttu-id="9ecf6-109">Panoramica</span><span class="sxs-lookup"><span data-stu-id="9ecf6-109">Overview</span></span>

<span data-ttu-id="9ecf6-110">Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="9ecf6-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="9ecf6-111">Le animazioni può essere avviata automaticamente o possono essere attivate dall'interazione dell'utente, ad esempio, facendo clic con il mouse.</span><span class="sxs-lookup"><span data-stu-id="9ecf6-111">The animations can start automatically or may be triggered by user interaction, e.g. by clicking with the mouse.</span></span>

## <a name="steps"></a><span data-ttu-id="9ecf6-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="9ecf6-112">Steps</span></span>

<span data-ttu-id="9ecf6-113">Prima di tutto, includere il `ScriptManager` nella pagina; quindi, la libreria ASP.NET AJAX viene caricata, rendendo possibile usare il Toolkit di controllo:</span><span class="sxs-lookup"><span data-stu-id="9ecf6-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample1.aspx)]

<span data-ttu-id="9ecf6-114">Verrà applicata l'animazione a un pannello del testo che presenta un aspetto simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="9ecf6-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample2.aspx)]

<span data-ttu-id="9ecf6-115">Nella classe CSS associata per il pannello, definire un colore di sfondo interessante e anche impostare una larghezza fissa per il pannello:</span><span class="sxs-lookup"><span data-stu-id="9ecf6-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](animating-in-response-to-user-interaction-cs/samples/sample3.css)]

<span data-ttu-id="9ecf6-116">Quindi, aggiungere il `AnimationExtender` alla pagina, fornendo un' `ID`, il `TargetControlID` attributo e l'obbligatoria `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="9ecf6-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample4.aspx)]

<span data-ttu-id="9ecf6-117">All'interno di `<Animations>` nodo, sono disponibili cinque modi per avviare l'animazione tramite l'interazione dell'utente (l'elemento mancano è `<OnLoad>` che viene eseguita una volta l'intera pagina è stata caricata completamente):</span><span class="sxs-lookup"><span data-stu-id="9ecf6-117">Within the `<Animations>` node, there are five ways to start the animation via user interaction (the missing element is `<OnLoad>` which is executed once the whole page has been fully loaded):</span></span>

- <span data-ttu-id="9ecf6-118">`<OnClick>` (fare clic del mouse sul controllo)</span><span class="sxs-lookup"><span data-stu-id="9ecf6-118">`<OnClick>` (mouse click on the control)</span></span>
- <span data-ttu-id="9ecf6-119">`<OnHoverOut>` (mouse lascia il controllo)</span><span class="sxs-lookup"><span data-stu-id="9ecf6-119">`<OnHoverOut>` (mouse leaves the control)</span></span>
- <span data-ttu-id="9ecf6-120">`<OnHoverOver>` (passaggio del mouse su un controllo, l'arresto di `<OnHoverOut>` animazione)</span><span class="sxs-lookup"><span data-stu-id="9ecf6-120">`<OnHoverOver>` (mouse hovers over a control, stopping the `<OnHoverOut>` animation)</span></span>
- <span data-ttu-id="9ecf6-121">`<OnMouseOut>` (mouse esce da un controllo)</span><span class="sxs-lookup"><span data-stu-id="9ecf6-121">`<OnMouseOut>` (mouse leaves a control)</span></span>
- <span data-ttu-id="9ecf6-122">`<OnMouseOver>` (passaggio del mouse su un controllo, non interrompe la `<OnMouseOut>` animazione)</span><span class="sxs-lookup"><span data-stu-id="9ecf6-122">`<OnMouseOver>` (mouse hovers over a control, not stopping the `<OnMouseOut>` animation)</span></span>

<span data-ttu-id="9ecf6-123">In questo scenario, `<OnClick>` viene usato.</span><span class="sxs-lookup"><span data-stu-id="9ecf6-123">In this scenario, `<OnClick>` is used.</span></span> <span data-ttu-id="9ecf6-124">Quando l'utente fa clic sul pannello, ridimensionamento e dissolve nello stesso momento.</span><span class="sxs-lookup"><span data-stu-id="9ecf6-124">When the user clicks on the panel, it is resized and fades out at the same time.</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample5.aspx)]


<span data-ttu-id="9ecf6-125">[![Un clic del mouse viene avviata l'animazione](animating-in-response-to-user-interaction-cs/_static/image2.png)](animating-in-response-to-user-interaction-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="9ecf6-125">[![A mouse click starts the animation](animating-in-response-to-user-interaction-cs/_static/image2.png)](animating-in-response-to-user-interaction-cs/_static/image1.png)</span></span>

<span data-ttu-id="9ecf6-126">Un clic del mouse viene avviata l'animazione ([fare clic per visualizzare l'immagine con dimensioni normali](animating-in-response-to-user-interaction-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="9ecf6-126">A mouse click starts the animation ([Click to view full-size image](animating-in-response-to-user-interaction-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="9ecf6-127">[Precedente](picking-one-animation-out-of-a-list-cs.md)
> [Successivo](disabling-actions-during-animation-cs.md)</span><span class="sxs-lookup"><span data-stu-id="9ecf6-127">[Previous](picking-one-animation-out-of-a-list-cs.md)
[Next](disabling-actions-during-animation-cs.md)</span></span>
