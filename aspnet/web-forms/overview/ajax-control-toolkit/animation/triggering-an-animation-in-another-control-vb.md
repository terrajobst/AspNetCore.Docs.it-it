---
uid: web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-vb
title: Attivazione di un'animazione in un altro controllo (VB) | Microsoft Docs
author: wenz
description: Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo. In generale, l'avvio di un...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 25ebaf1f-5a9f-423d-98c7-1d694e93664f
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-vb
msc.type: authoredcontent
ms.openlocfilehash: b316b22bf355d7abb0909e43f0c2c38ea3e68f24
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37367810"
---
<a name="triggering-an-animation-in-another-control-vb"></a><span data-ttu-id="a3919-104">Attivazione di un'animazione in un altro controllo (VB)</span><span class="sxs-lookup"><span data-stu-id="a3919-104">Triggering an Animation in another Control (VB)</span></span>
====================
<span data-ttu-id="a3919-105">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="a3919-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="a3919-106">[Scaricare il codice](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="a3919-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8VB.pdf)</span></span>

> <span data-ttu-id="a3919-107">Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="a3919-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="a3919-108">In generale, l'avvio di un'animazione viene attivata dall'interazione dell'utente con lo stesso controllo.</span><span class="sxs-lookup"><span data-stu-id="a3919-108">Generally, launching an animation is triggered by user interaction with the same control.</span></span> <span data-ttu-id="a3919-109">È tuttavia anche possibile interagire con un controllo e quindi animazione a un altro controllo.</span><span class="sxs-lookup"><span data-stu-id="a3919-109">It is however also possible to interact with one control and then animation another control.</span></span>


## <a name="overview"></a><span data-ttu-id="a3919-110">Panoramica</span><span class="sxs-lookup"><span data-stu-id="a3919-110">Overview</span></span>

<span data-ttu-id="a3919-111">Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="a3919-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="a3919-112">In generale, l'avvio di un'animazione viene attivata dall'interazione dell'utente con lo stesso controllo.</span><span class="sxs-lookup"><span data-stu-id="a3919-112">Generally, launching an animation is triggered by user interaction with the same control.</span></span> <span data-ttu-id="a3919-113">È tuttavia anche possibile interagire con un controllo e quindi animazione a un altro controllo.</span><span class="sxs-lookup"><span data-stu-id="a3919-113">It is however also possible to interact with one control and then animation another control.</span></span>

## <a name="steps"></a><span data-ttu-id="a3919-114">Passaggi</span><span class="sxs-lookup"><span data-stu-id="a3919-114">Steps</span></span>

<span data-ttu-id="a3919-115">Prima di tutto, includere il `ScriptManager` nella pagina; quindi, la libreria ASP.NET AJAX viene caricata, rendendo possibile usare il Toolkit di controllo:</span><span class="sxs-lookup"><span data-stu-id="a3919-115">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample1.aspx)]

<span data-ttu-id="a3919-116">Verrà applicata l'animazione a un pannello del testo che presenta un aspetto simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="a3919-116">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample2.aspx)]

<span data-ttu-id="a3919-117">Nella classe CSS associata per il pannello, definire un colore di sfondo interessante e anche impostare una larghezza fissa per il pannello:</span><span class="sxs-lookup"><span data-stu-id="a3919-117">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](triggering-an-animation-in-another-control-vb/samples/sample3.css)]

<span data-ttu-id="a3919-118">Per avviare il pannello Animazione, viene usato un pulsante HTML.</span><span class="sxs-lookup"><span data-stu-id="a3919-118">In order to start animating the panel, an HTML button is used.</span></span> <span data-ttu-id="a3919-119">Si noti che `<input type="button" />` è favoriti rispetto `<asp:Button />` poiché non vogliamo un postback quando l'utente fa clic su tale pulsante.</span><span class="sxs-lookup"><span data-stu-id="a3919-119">Note that `<input type="button" />` is favoured over `<asp:Button />` since we do not want a postback when the user clicks on that button.</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample4.aspx)]

<span data-ttu-id="a3919-120">Quindi, aggiungere il `AnimationExtender` alla pagina, fornendo un' `ID`, il `TargetControlID` attributo e l'obbligatoria `runat="server"`.</span><span class="sxs-lookup"><span data-stu-id="a3919-120">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`.</span></span> <span data-ttu-id="a3919-121">È importante impostare `TargetControlID` all'ID del pulsante (l'elemento attivare l'animazione), non per l'ID del pannello (l'elemento viene aggiunta un'animazione)</span><span class="sxs-lookup"><span data-stu-id="a3919-121">It is important to set `TargetControlID` to the ID of the button (the element triggering the animation), not to the ID of the panel (the element being animated)</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample5.aspx)]

<span data-ttu-id="a3919-122">All'interno di `<Animations>` nodo, le animazioni sul posto come di consueto.</span><span class="sxs-lookup"><span data-stu-id="a3919-122">Within the `<Animations>` node, place animations as usual.</span></span> <span data-ttu-id="a3919-123">Per fare in modo del Pannello di modifica, non il pulsante, impostare il `AnimationTarget` attributo per ogni elemento animazione `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="a3919-123">In order to make them change the panel, not the button, set the `AnimationTarget` attribute for every animation element within `AnimationExtender`.</span></span> <span data-ttu-id="a3919-124">Il valore per `AnimationTarget` è l'ID del pannello, ovviamente.</span><span class="sxs-lookup"><span data-stu-id="a3919-124">The value for `AnimationTarget` is the ID of the panel, of course.</span></span> <span data-ttu-id="a3919-125">In questo modo, le animazioni verificano con il pannello, non con il pulsante di attivazione del trigger.</span><span class="sxs-lookup"><span data-stu-id="a3919-125">That way, the animations happen with the panel, not with the triggering button.</span></span> <span data-ttu-id="a3919-126">Di seguito è riportato il `AnimationExtender` markup per questo scenario:</span><span class="sxs-lookup"><span data-stu-id="a3919-126">Here is the `AnimationExtender` markup for this scenario:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample6.aspx)]

<span data-ttu-id="a3919-127">Si noti l'ordine speciale in cui vengono visualizzate le singole animazioni.</span><span class="sxs-lookup"><span data-stu-id="a3919-127">Note the special order in which the individual animations appear.</span></span> <span data-ttu-id="a3919-128">Prima di tutto, il pulsante disattivato una volta che viene eseguita l'animazione.</span><span class="sxs-lookup"><span data-stu-id="a3919-128">First of all, the button gets deactivated once the animation runs.</span></span> <span data-ttu-id="a3919-129">Poiché è presente alcun `AnimationTarget` attributo la `<EnableAction>` elemento, questa animazione viene applicata al controllo origine: il pulsante.</span><span class="sxs-lookup"><span data-stu-id="a3919-129">Since there is no `AnimationTarget` attribute in the `<EnableAction>` element, this animation is applied to the originating control: the button.</span></span> <span data-ttu-id="a3919-130">I passaggi successivi due animazione devono essere effettuati parallelly (`<Parallel>` elemento).</span><span class="sxs-lookup"><span data-stu-id="a3919-130">The next two animation steps shall be carried out parallelly (`<Parallel>` element).</span></span> <span data-ttu-id="a3919-131">Entrambi hanno loro `AnimationTarget` attributi impostati `"Panel1"`, l'animazione in questo modo il pannello, non il pulsante.</span><span class="sxs-lookup"><span data-stu-id="a3919-131">Both have their `AnimationTarget` attributes set to `"Panel1"`, thus animating the panel, not the button.</span></span>


<span data-ttu-id="a3919-132">[![Un clic del mouse sul pulsante Avvia l'animazione di pannello](triggering-an-animation-in-another-control-vb/_static/image2.png)](triggering-an-animation-in-another-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="a3919-132">[![A mouse click on the button starts the panel animation](triggering-an-animation-in-another-control-vb/_static/image2.png)](triggering-an-animation-in-another-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="a3919-133">Un clic del mouse sul pulsante Avvia l'animazione di pannello ([fare clic per visualizzare l'immagine con dimensioni normali](triggering-an-animation-in-another-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="a3919-133">A mouse click on the button starts the panel animation ([Click to view full-size image](triggering-an-animation-in-another-control-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a3919-134">[Precedente](disabling-actions-during-animation-vb.md)
> [Successivo](modifying-animations-from-the-server-side-vb.md)</span><span class="sxs-lookup"><span data-stu-id="a3919-134">[Previous](disabling-actions-during-animation-vb.md)
[Next](modifying-animations-from-the-server-side-vb.md)</span></span>
