---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-vb
title: Animazione in risposta all'interazione dell'utente (VB) | Documenti Microsoft
author: wenz
description: Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework per aggiungere le animazioni a un controllo. Le animazioni possano stelle...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: c8204c05-ec27-40fe-933d-88e4e727a482
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-vb
msc.type: authoredcontent
ms.openlocfilehash: e12467bfeb88c2ab9d1cfb866506e9e8e7f9ae25
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="animating-in-response-to-user-interaction-vb"></a><span data-ttu-id="12406-104">Animazione in risposta all'interazione dell'utente (VB)</span><span class="sxs-lookup"><span data-stu-id="12406-104">Animating in Response To User Interaction (VB)</span></span>
====================
<span data-ttu-id="12406-105">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="12406-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="12406-106">[Scaricare codice](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="12406-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6VB.pdf)</span></span>

> <span data-ttu-id="12406-107">Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework per aggiungere le animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="12406-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="12406-108">Le animazioni può essere avviata automaticamente o possono essere attivate dall'interazione dell'utente, ad esempio, facendo clic con il mouse.</span><span class="sxs-lookup"><span data-stu-id="12406-108">The animations can start automatically or may be triggered by user interaction, e.g. by clicking with the mouse.</span></span>


## <a name="overview"></a><span data-ttu-id="12406-109">Panoramica</span><span class="sxs-lookup"><span data-stu-id="12406-109">Overview</span></span>

<span data-ttu-id="12406-110">Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework per aggiungere le animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="12406-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="12406-111">Le animazioni può essere avviata automaticamente o possono essere attivate dall'interazione dell'utente, ad esempio, facendo clic con il mouse.</span><span class="sxs-lookup"><span data-stu-id="12406-111">The animations can start automatically or may be triggered by user interaction, e.g. by clicking with the mouse.</span></span>

## <a name="steps"></a><span data-ttu-id="12406-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="12406-112">Steps</span></span>

<span data-ttu-id="12406-113">Prima di tutto, includere il `ScriptManager` nella pagina; quindi, la libreria AJAX di ASP.NET viene caricata, che consente di usare il Toolkit di controllo:</span><span class="sxs-lookup"><span data-stu-id="12406-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample1.aspx)]

<span data-ttu-id="12406-114">Verrà applicata l'animazione a un riquadro del testo che presenta un aspetto simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="12406-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample2.aspx)]

<span data-ttu-id="12406-115">Nella classe CSS associata per il pannello, definire un colore di sfondo nice e inoltre impostare una larghezza fissa per il pannello:</span><span class="sxs-lookup"><span data-stu-id="12406-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](animating-in-response-to-user-interaction-vb/samples/sample3.css)]

<span data-ttu-id="12406-116">Quindi, aggiungere il `AnimationExtender` alla pagina, fornendo un `ID`, `TargetControlID` attributo e l'obbligatoria `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="12406-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample4.aspx)]

<span data-ttu-id="12406-117">All'interno di `<Animations>` nodo, sono disponibili cinque modi per iniziare l'animazione tramite l'interazione dell'utente (l'elemento mancano è `<OnLoad>` che viene eseguito una volta l'intera pagina è stata caricata completamente):</span><span class="sxs-lookup"><span data-stu-id="12406-117">Within the `<Animations>` node, there are five ways to start the animation via user interaction (the missing element is `<OnLoad>` which is executed once the whole page has been fully loaded):</span></span>

- <span data-ttu-id="12406-118">`<OnClick>` (fare clic del mouse sul controllo)</span><span class="sxs-lookup"><span data-stu-id="12406-118">`<OnClick>` (mouse click on the control)</span></span>
- <span data-ttu-id="12406-119">`<OnHoverOut>` (mouse esce dall'area di controllo)</span><span class="sxs-lookup"><span data-stu-id="12406-119">`<OnHoverOut>` (mouse leaves the control)</span></span>
- <span data-ttu-id="12406-120">`<OnHoverOver>` (mouse viene spostato su un controllo, l'arresto di `<OnHoverOut>` animazione)</span><span class="sxs-lookup"><span data-stu-id="12406-120">`<OnHoverOver>` (mouse hovers over a control, stopping the `<OnHoverOut>` animation)</span></span>
- <span data-ttu-id="12406-121">`<OnMouseOut>` (mouse esce dall'area di un controllo)</span><span class="sxs-lookup"><span data-stu-id="12406-121">`<OnMouseOut>` (mouse leaves a control)</span></span>
- <span data-ttu-id="12406-122">`<OnMouseOver>` (mouse viene spostato su un controllo, non arrestare il `<OnMouseOut>` animazione)</span><span class="sxs-lookup"><span data-stu-id="12406-122">`<OnMouseOver>` (mouse hovers over a control, not stopping the `<OnMouseOut>` animation)</span></span>

<span data-ttu-id="12406-123">In questo scenario, `<OnClick>` viene utilizzato.</span><span class="sxs-lookup"><span data-stu-id="12406-123">In this scenario, `<OnClick>` is used.</span></span> <span data-ttu-id="12406-124">Quando l'utente fa clic sul riquadro, viene ridimensionato e dissolvenza nello stesso momento.</span><span class="sxs-lookup"><span data-stu-id="12406-124">When the user clicks on the panel, it is resized and fades out at the same time.</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample5.aspx)]


<span data-ttu-id="12406-125">[![Un clic del mouse viene avviata l'animazione](animating-in-response-to-user-interaction-vb/_static/image2.png)](animating-in-response-to-user-interaction-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="12406-125">[![A mouse click starts the animation](animating-in-response-to-user-interaction-vb/_static/image2.png)](animating-in-response-to-user-interaction-vb/_static/image1.png)</span></span>

<span data-ttu-id="12406-126">Un clic del mouse viene avviata l'animazione ([fare clic per visualizzare l'immagine ingrandita](animating-in-response-to-user-interaction-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="12406-126">A mouse click starts the animation ([Click to view full-size image](animating-in-response-to-user-interaction-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="12406-127">[Precedente](picking-one-animation-out-of-a-list-vb.md)
> [Successivo](disabling-actions-during-animation-vb.md)</span><span class="sxs-lookup"><span data-stu-id="12406-127">[Previous](picking-one-animation-out-of-a-list-vb.md)
[Next](disabling-actions-during-animation-vb.md)</span></span>
