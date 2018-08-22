---
uid: web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-cs
title: Attivazione di un'animazione in un altro controllo (c#) | Microsoft Docs
author: wenz
description: Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo. In generale, l'avvio di un...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: e5d99c2b-d8ee-413c-80d5-c120cffb0a4c
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-cs
msc.type: authoredcontent
ms.openlocfilehash: ad6dfecf71a7577215e43222a8788e5c48d0c4c2
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825918"
---
<a name="triggering-an-animation-in-another-control-c"></a><span data-ttu-id="28298-104">Attivazione di un'animazione in un altro controllo (c#)</span><span class="sxs-lookup"><span data-stu-id="28298-104">Triggering an Animation in another Control (C#)</span></span>
====================
<span data-ttu-id="28298-105">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="28298-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="28298-106">[Scaricare il codice](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="28298-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8CS.pdf)</span></span>

> <span data-ttu-id="28298-107">Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="28298-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="28298-108">In generale, l'avvio di un'animazione viene attivata dall'interazione dell'utente con lo stesso controllo.</span><span class="sxs-lookup"><span data-stu-id="28298-108">Generally, launching an animation is triggered by user interaction with the same control.</span></span> <span data-ttu-id="28298-109">È tuttavia anche possibile interagire con un controllo e quindi animazione a un altro controllo.</span><span class="sxs-lookup"><span data-stu-id="28298-109">It is however also possible to interact with one control and then animation another control.</span></span>


## <a name="overview"></a><span data-ttu-id="28298-110">Panoramica</span><span class="sxs-lookup"><span data-stu-id="28298-110">Overview</span></span>

<span data-ttu-id="28298-111">Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="28298-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="28298-112">In generale, l'avvio di un'animazione viene attivata dall'interazione dell'utente con lo stesso controllo.</span><span class="sxs-lookup"><span data-stu-id="28298-112">Generally, launching an animation is triggered by user interaction with the same control.</span></span> <span data-ttu-id="28298-113">È tuttavia anche possibile interagire con un controllo e quindi animazione a un altro controllo.</span><span class="sxs-lookup"><span data-stu-id="28298-113">It is however also possible to interact with one control and then animation another control.</span></span>

## <a name="steps"></a><span data-ttu-id="28298-114">Passaggi</span><span class="sxs-lookup"><span data-stu-id="28298-114">Steps</span></span>

<span data-ttu-id="28298-115">Prima di tutto, includere il `ScriptManager` nella pagina; quindi, la libreria ASP.NET AJAX viene caricata, rendendo possibile usare il Toolkit di controllo:</span><span class="sxs-lookup"><span data-stu-id="28298-115">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample1.aspx)]

<span data-ttu-id="28298-116">Verrà applicata l'animazione a un pannello del testo che presenta un aspetto simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="28298-116">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample2.aspx)]

<span data-ttu-id="28298-117">Nella classe CSS associata per il pannello, definire un colore di sfondo interessante e anche impostare una larghezza fissa per il pannello:</span><span class="sxs-lookup"><span data-stu-id="28298-117">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](triggering-an-animation-in-another-control-cs/samples/sample3.css)]

<span data-ttu-id="28298-118">Per avviare il pannello Animazione, viene usato un pulsante HTML.</span><span class="sxs-lookup"><span data-stu-id="28298-118">In order to start animating the panel, an HTML button is used.</span></span> <span data-ttu-id="28298-119">Si noti che `<input type="button" />` è favoriti rispetto `<asp:Button />` poiché non vogliamo un postback quando l'utente fa clic su tale pulsante.</span><span class="sxs-lookup"><span data-stu-id="28298-119">Note that `<input type="button" />` is favoured over `<asp:Button />` since we do not want a postback when the user clicks on that button.</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample4.aspx)]

<span data-ttu-id="28298-120">Quindi, aggiungere il `AnimationExtender` alla pagina, fornendo un' `ID`, il `TargetControlID` attributo e l'obbligatoria `runat="server"`.</span><span class="sxs-lookup"><span data-stu-id="28298-120">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`.</span></span> <span data-ttu-id="28298-121">È importante impostare `TargetControlID` all'ID del pulsante (l'elemento attivare l'animazione), non per l'ID del pannello (l'elemento viene aggiunta un'animazione)</span><span class="sxs-lookup"><span data-stu-id="28298-121">It is important to set `TargetControlID` to the ID of the button (the element triggering the animation), not to the ID of the panel (the element being animated)</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample5.aspx)]

<span data-ttu-id="28298-122">All'interno di `<Animations>` nodo, le animazioni sul posto come di consueto.</span><span class="sxs-lookup"><span data-stu-id="28298-122">Within the `<Animations>` node, place animations as usual.</span></span> <span data-ttu-id="28298-123">Per fare in modo del Pannello di modifica, non il pulsante, impostare il `AnimationTarget` attributo per ogni elemento animazione `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="28298-123">In order to make them change the panel, not the button, set the `AnimationTarget` attribute for every animation element within `AnimationExtender`.</span></span> <span data-ttu-id="28298-124">Il valore per `AnimationTarget` è l'ID del pannello, ovviamente.</span><span class="sxs-lookup"><span data-stu-id="28298-124">The value for `AnimationTarget` is the ID of the panel, of course.</span></span> <span data-ttu-id="28298-125">In questo modo, le animazioni verificano con il pannello, non con il pulsante di attivazione del trigger.</span><span class="sxs-lookup"><span data-stu-id="28298-125">That way, the animations happen with the panel, not with the triggering button.</span></span> <span data-ttu-id="28298-126">Di seguito è riportato il `AnimationExtender` markup per questo scenario:</span><span class="sxs-lookup"><span data-stu-id="28298-126">Here is the `AnimationExtender` markup for this scenario:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample6.aspx)]

<span data-ttu-id="28298-127">Si noti l'ordine speciale in cui vengono visualizzate le singole animazioni.</span><span class="sxs-lookup"><span data-stu-id="28298-127">Note the special order in which the individual animations appear.</span></span> <span data-ttu-id="28298-128">Prima di tutto, il pulsante disattivato una volta che viene eseguita l'animazione.</span><span class="sxs-lookup"><span data-stu-id="28298-128">First of all, the button gets deactivated once the animation runs.</span></span> <span data-ttu-id="28298-129">Poiché è presente alcun `AnimationTarget` attributo la `<EnableAction>` elemento, questa animazione viene applicata al controllo origine: il pulsante.</span><span class="sxs-lookup"><span data-stu-id="28298-129">Since there is no `AnimationTarget` attribute in the `<EnableAction>` element, this animation is applied to the originating control: the button.</span></span> <span data-ttu-id="28298-130">I passaggi successivi due animazione devono essere effettuati parallelly (`<Parallel>` elemento).</span><span class="sxs-lookup"><span data-stu-id="28298-130">The next two animation steps shall be carried out parallelly (`<Parallel>` element).</span></span> <span data-ttu-id="28298-131">Entrambi hanno loro `AnimationTarget` attributi impostati `"Panel1"`, l'animazione in questo modo il pannello, non il pulsante.</span><span class="sxs-lookup"><span data-stu-id="28298-131">Both have their `AnimationTarget` attributes set to `"Panel1"`, thus animating the panel, not the button.</span></span>


<span data-ttu-id="28298-132">[![Un clic del mouse sul pulsante Avvia l'animazione di pannello](triggering-an-animation-in-another-control-cs/_static/image2.png)](triggering-an-animation-in-another-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="28298-132">[![A mouse click on the button starts the panel animation](triggering-an-animation-in-another-control-cs/_static/image2.png)](triggering-an-animation-in-another-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="28298-133">Un clic del mouse sul pulsante Avvia l'animazione di pannello ([fare clic per visualizzare l'immagine con dimensioni normali](triggering-an-animation-in-another-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="28298-133">A mouse click on the button starts the panel animation ([Click to view full-size image](triggering-an-animation-in-another-control-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="28298-134">[Precedente](disabling-actions-during-animation-cs.md)
> [Successivo](modifying-animations-from-the-server-side-cs.md)</span><span class="sxs-lookup"><span data-stu-id="28298-134">[Previous](disabling-actions-during-animation-cs.md)
[Next](modifying-animations-from-the-server-side-cs.md)</span></span>
