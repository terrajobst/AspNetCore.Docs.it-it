---
uid: web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-vb
title: Generazione di un'animazione in un altro controllo (VB) | Documenti Microsoft
author: wenz
description: "Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework per aggiungere le animazioni a un controllo. Avvio in genere, un..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 25ebaf1f-5a9f-423d-98c7-1d694e93664f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-vb
msc.type: authoredcontent
ms.openlocfilehash: ce1d29cbd06ef8a470780ff4c7bda8039575d59f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="triggering-an-animation-in-another-control-vb"></a><span data-ttu-id="f9128-104">Generazione di un'animazione in un altro controllo (VB)</span><span class="sxs-lookup"><span data-stu-id="f9128-104">Triggering an Animation in another Control (VB)</span></span>
====================
<span data-ttu-id="f9128-105">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="f9128-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="f9128-106">[Scaricare codice](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="f9128-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8VB.pdf)</span></span>

> <span data-ttu-id="f9128-107">Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework per aggiungere le animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="f9128-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="f9128-108">In genere, l'avvio di un'animazione viene attivata dall'interazione dell'utente con il controllo stesso.</span><span class="sxs-lookup"><span data-stu-id="f9128-108">Generally, launching an animation is triggered by user interaction with the same control.</span></span> <span data-ttu-id="f9128-109">È tuttavia anche possibile interagire con un controllo e quindi animazione a un altro controllo.</span><span class="sxs-lookup"><span data-stu-id="f9128-109">It is however also possible to interact with one control and then animation another control.</span></span>


## <a name="overview"></a><span data-ttu-id="f9128-110">Panoramica</span><span class="sxs-lookup"><span data-stu-id="f9128-110">Overview</span></span>

<span data-ttu-id="f9128-111">Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework per aggiungere le animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="f9128-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="f9128-112">In genere, l'avvio di un'animazione viene attivata dall'interazione dell'utente con il controllo stesso.</span><span class="sxs-lookup"><span data-stu-id="f9128-112">Generally, launching an animation is triggered by user interaction with the same control.</span></span> <span data-ttu-id="f9128-113">È tuttavia anche possibile interagire con un controllo e quindi animazione a un altro controllo.</span><span class="sxs-lookup"><span data-stu-id="f9128-113">It is however also possible to interact with one control and then animation another control.</span></span>

## <a name="steps"></a><span data-ttu-id="f9128-114">Passaggi</span><span class="sxs-lookup"><span data-stu-id="f9128-114">Steps</span></span>

<span data-ttu-id="f9128-115">Prima di tutto, includere il `ScriptManager` nella pagina; quindi, la libreria AJAX di ASP.NET viene caricata, che consente di usare il Toolkit di controllo:</span><span class="sxs-lookup"><span data-stu-id="f9128-115">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample1.aspx)]

<span data-ttu-id="f9128-116">Verrà applicata l'animazione a un riquadro del testo che presenta un aspetto simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="f9128-116">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample2.aspx)]

<span data-ttu-id="f9128-117">Nella classe CSS associata per il pannello, definire un colore di sfondo nice e inoltre impostare una larghezza fissa per il pannello:</span><span class="sxs-lookup"><span data-stu-id="f9128-117">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](triggering-an-animation-in-another-control-vb/samples/sample3.css)]

<span data-ttu-id="f9128-118">Per avviare il pannello Animazione, viene utilizzato un pulsante HTML.</span><span class="sxs-lookup"><span data-stu-id="f9128-118">In order to start animating the panel, an HTML button is used.</span></span> <span data-ttu-id="f9128-119">Si noti che `<input type="button" />` è favoriti rispetto `<asp:Button />` poiché non è desiderabile che un postback quando l'utente fa clic sul pulsante.</span><span class="sxs-lookup"><span data-stu-id="f9128-119">Note that `<input type="button" />` is favoured over `<asp:Button />` since we do not want a postback when the user clicks on that button.</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample4.aspx)]

<span data-ttu-id="f9128-120">Quindi, aggiungere il `AnimationExtender` alla pagina, fornendo un `ID`, `TargetControlID` attributo e l'obbligatoria `runat="server"`.</span><span class="sxs-lookup"><span data-stu-id="f9128-120">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`.</span></span> <span data-ttu-id="f9128-121">È importante impostare `TargetControlID` per l'ID del pulsante (elemento di attivazione dell'animazione), non per l'ID del pannello (l'elemento che viene animata)</span><span class="sxs-lookup"><span data-stu-id="f9128-121">It is important to set `TargetControlID` to the ID of the button (the element triggering the animation), not to the ID of the panel (the element being animated)</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample5.aspx)]

<span data-ttu-id="f9128-122">All'interno di `<Animations>` nodo, le animazioni sul posto come al solito.</span><span class="sxs-lookup"><span data-stu-id="f9128-122">Within the `<Animations>` node, place animations as usual.</span></span> <span data-ttu-id="f9128-123">Per renderle il pannello di modifica, non sul pulsante, impostare il `AnimationTarget` attributo per ogni elemento animazione `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="f9128-123">In order to make them change the panel, not the button, set the `AnimationTarget` attribute for every animation element within `AnimationExtender`.</span></span> <span data-ttu-id="f9128-124">Il valore per `AnimationTarget` è l'ID del pannello, ovviamente.</span><span class="sxs-lookup"><span data-stu-id="f9128-124">The value for `AnimationTarget` is the ID of the panel, of course.</span></span> <span data-ttu-id="f9128-125">In questo modo, le animazioni verificarsi con il pannello, non con il pulsante di attivazione.</span><span class="sxs-lookup"><span data-stu-id="f9128-125">That way, the animations happen with the panel, not with the triggering button.</span></span> <span data-ttu-id="f9128-126">Ecco il `AnimationExtender` markup per questo scenario:</span><span class="sxs-lookup"><span data-stu-id="f9128-126">Here is the `AnimationExtender` markup for this scenario:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample6.aspx)]

<span data-ttu-id="f9128-127">Si noti l'ordine speciale in cui vengono visualizzate le animazioni singoli.</span><span class="sxs-lookup"><span data-stu-id="f9128-127">Note the special order in which the individual animations appear.</span></span> <span data-ttu-id="f9128-128">Prima di tutto, il pulsante viene disattivato quando viene eseguito l'animazione.</span><span class="sxs-lookup"><span data-stu-id="f9128-128">First of all, the button gets deactivated once the animation runs.</span></span> <span data-ttu-id="f9128-129">Poiché non esiste alcun `AnimationTarget` attributo il `<EnableAction>` elemento, questa animazione viene applicata al controllo origine: il pulsante.</span><span class="sxs-lookup"><span data-stu-id="f9128-129">Since there is no `AnimationTarget` attribute in the `<EnableAction>` element, this animation is applied to the originating control: the button.</span></span> <span data-ttu-id="f9128-130">I passaggi successivi due animazione devono essere effettuati parallelly (`<Parallel>` elemento).</span><span class="sxs-lookup"><span data-stu-id="f9128-130">The next two animation steps shall be carried out parallelly (`<Parallel>` element).</span></span> <span data-ttu-id="f9128-131">Entrambi hanno loro `AnimationTarget` attributi impostati `"Panel1"`, l'animazione in questo modo il pannello, non il pulsante.</span><span class="sxs-lookup"><span data-stu-id="f9128-131">Both have their `AnimationTarget` attributes set to `"Panel1"`, thus animating the panel, not the button.</span></span>


<span data-ttu-id="f9128-132">[![Un clic del mouse sul pulsante Avvia l'animazione di pannello](triggering-an-animation-in-another-control-vb/_static/image2.png)](triggering-an-animation-in-another-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="f9128-132">[![A mouse click on the button starts the panel animation](triggering-an-animation-in-another-control-vb/_static/image2.png)](triggering-an-animation-in-another-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="f9128-133">Un clic del mouse sul pulsante Avvia l'animazione di pannello ([fare clic per visualizzare l'immagine ingrandita](triggering-an-animation-in-another-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="f9128-133">A mouse click on the button starts the panel animation ([Click to view full-size image](triggering-an-animation-in-another-control-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="f9128-134">[Precedente](disabling-actions-during-animation-vb.md)
[Successivo](modifying-animations-from-the-server-side-vb.md)</span><span class="sxs-lookup"><span data-stu-id="f9128-134">[Previous](disabling-actions-during-animation-vb.md)
[Next](modifying-animations-from-the-server-side-vb.md)</span></span>
