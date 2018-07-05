---
uid: web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-cs
title: Disabilitazione delle azioni durante l'animazione (c#) | Microsoft Docs
author: wenz
description: Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo. Supporta anche azioni...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 918026b4-2f63-421d-8546-df12856960a8
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-cs
msc.type: authoredcontent
ms.openlocfilehash: 396411332735503d1781e171bead8ab6aaed6efa
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37828195"
---
<a name="disabling-actions-during-animation-c"></a><span data-ttu-id="b68b9-104">Disabilitazione delle azioni durante l'animazione (c#)</span><span class="sxs-lookup"><span data-stu-id="b68b9-104">Disabling Actions during Animation (C#)</span></span>
====================
<span data-ttu-id="b68b9-105">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="b68b9-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="b68b9-106">[Scaricare il codice](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="b68b9-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7CS.pdf)</span></span>

> <span data-ttu-id="b68b9-107">Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="b68b9-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="b68b9-108">Supporta anche azioni, ad esempio un clic del mouse.</span><span class="sxs-lookup"><span data-stu-id="b68b9-108">It also supports actions, like mouse clicks.</span></span> <span data-ttu-id="b68b9-109">Tuttavia quando un clic del mouse viene avviata un'animazione, è consigliabile disabilitare i clic del mouse durante l'animazione.</span><span class="sxs-lookup"><span data-stu-id="b68b9-109">However when a mouse click starts an animation, it is desirable to disable mouse clicks during the animation.</span></span>


## <a name="overview"></a><span data-ttu-id="b68b9-110">Panoramica</span><span class="sxs-lookup"><span data-stu-id="b68b9-110">Overview</span></span>

<span data-ttu-id="b68b9-111">Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="b68b9-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="b68b9-112">Supporta anche azioni, ad esempio un clic del mouse.</span><span class="sxs-lookup"><span data-stu-id="b68b9-112">It also supports actions, like mouse clicks.</span></span> <span data-ttu-id="b68b9-113">Tuttavia quando un clic del mouse viene avviata un'animazione, è consigliabile disabilitare i clic del mouse durante l'animazione.</span><span class="sxs-lookup"><span data-stu-id="b68b9-113">However when a mouse click starts an animation, it is desirable to disable mouse clicks during the animation.</span></span>

## <a name="steps"></a><span data-ttu-id="b68b9-114">Passaggi</span><span class="sxs-lookup"><span data-stu-id="b68b9-114">Steps</span></span>

<span data-ttu-id="b68b9-115">Prima di tutto, includere il `ScriptManager` nella pagina; quindi, la libreria ASP.NET AJAX viene caricata, rendendo possibile usare il Toolkit di controllo:</span><span class="sxs-lookup"><span data-stu-id="b68b9-115">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample1.aspx)]

<span data-ttu-id="b68b9-116">Verrà applicata l'animazione a un pulsante HTML simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="b68b9-116">The animation will be applied to an HTML button like this:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample2.aspx)]

<span data-ttu-id="b68b9-117">Si noti che un controllo HTML viene utilizzato invece di un controllo Web poiché non vogliamo pulsante per creare un postback; deve avviare l'animazione lato client per noi.</span><span class="sxs-lookup"><span data-stu-id="b68b9-117">Note that an HTML Control is used instead of a Web Control since we do not want the button to create a postback; it shall just launch the client-side animation for us.</span></span>

<span data-ttu-id="b68b9-118">Quindi, aggiungere il `AnimationExtender` alla pagina, fornendo un' `ID`, il `TargetControlID` attributo e l'obbligatoria `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="b68b9-118">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample3.aspx)]

<span data-ttu-id="b68b9-119">All'interno di `<Animations>` nodo, `<OnClick>` è l'elemento a destra per gestire i clic del mouse.</span><span class="sxs-lookup"><span data-stu-id="b68b9-119">Within the `<Animations>` node, `<OnClick>` is the right element to handle the mouse click.</span></span> <span data-ttu-id="b68b9-120">Tuttavia, potrebbe essere fatto clic sul pulsante durante l'animazione, nonché.</span><span class="sxs-lookup"><span data-stu-id="b68b9-120">However, the button could be clicked during the animation, as well.</span></span> <span data-ttu-id="b68b9-121">Il `<EnableAction>` può gestire di tale elemento.</span><span class="sxs-lookup"><span data-stu-id="b68b9-121">The `<EnableAction>` element can take care of that.</span></span> <span data-ttu-id="b68b9-122">Impostazione `Enabled="false"` disabilita il pulsante come parte dell'animazione.</span><span class="sxs-lookup"><span data-stu-id="b68b9-122">Setting `Enabled="false"` disables the button as part of the animation.</span></span> <span data-ttu-id="b68b9-123">Poiché si sta usando diverse singole animazioni (disabilita il pulsante e le animazioni effettive), il `<Parallel>` elemento è obbligatorio per associare le animazioni insieme in una singole.</span><span class="sxs-lookup"><span data-stu-id="b68b9-123">Since we are using several individual animations (disabling the button and the actual animations), the `<Parallel>` element is required to glue the single animations together into one.</span></span> <span data-ttu-id="b68b9-124">Ecco il markup completo per `AnimationExtender`:</span><span class="sxs-lookup"><span data-stu-id="b68b9-124">Here is the complete markup for `AnimationExtender`:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample4.aspx)]

<span data-ttu-id="b68b9-125">È anche possibile abilitare nuovamente al pulsante dopo l'animazione, utilizzando l'elemento XML seguente alla fine dell'elenco:</span><span class="sxs-lookup"><span data-stu-id="b68b9-125">It would also be possible to re-enable to button after the animation, using the following XML element at the end of the list:</span></span>

[!code-xml[Main](disabling-actions-during-animation-cs/samples/sample5.xml)]

<span data-ttu-id="b68b9-126">Tuttavia in questo scenario specifico questo sarebbe inutile perché il pulsante dissolve e non è visibile alla fine dell'animazione.</span><span class="sxs-lookup"><span data-stu-id="b68b9-126">However in the given scenario this would be useless since the button fades out and is not visible at the end of the animation.</span></span>


<span data-ttu-id="b68b9-127">[![Il pulsante è disabilitato, non appena viene eseguita l'animazione](disabling-actions-during-animation-cs/_static/image2.png)](disabling-actions-during-animation-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="b68b9-127">[![The button is disabled as soon as the animation runs](disabling-actions-during-animation-cs/_static/image2.png)](disabling-actions-during-animation-cs/_static/image1.png)</span></span>

<span data-ttu-id="b68b9-128">Il pulsante è disabilitato, non appena viene eseguita l'animazione ([fare clic per visualizzare l'immagine con dimensioni normali](disabling-actions-during-animation-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="b68b9-128">The button is disabled as soon as the animation runs ([Click to view full-size image](disabling-actions-during-animation-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="b68b9-129">[Precedente](animating-in-response-to-user-interaction-cs.md)
> [Successivo](triggering-an-animation-in-another-control-cs.md)</span><span class="sxs-lookup"><span data-stu-id="b68b9-129">[Previous](animating-in-response-to-user-interaction-cs.md)
[Next](triggering-an-animation-in-another-control-cs.md)</span></span>
