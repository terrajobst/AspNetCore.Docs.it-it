---
uid: web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-vb
title: Disabilitazione delle azioni durante l'animazione (VB) | Microsoft Docs
author: wenz
description: Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo. Supporta anche azioni...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: a86c0276-6481-46ee-8b4f-8c2009399ee9
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-vb
msc.type: authoredcontent
ms.openlocfilehash: 811e1d75f79885f3f4c561d9211fec625fcf1807
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836468"
---
<a name="disabling-actions-during-animation-vb"></a><span data-ttu-id="6bc89-104">Disabilitazione delle azioni durante l'animazione (VB)</span><span class="sxs-lookup"><span data-stu-id="6bc89-104">Disabling Actions during Animation (VB)</span></span>
====================
<span data-ttu-id="6bc89-105">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="6bc89-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="6bc89-106">[Scaricare il codice](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="6bc89-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7VB.pdf)</span></span>

> <span data-ttu-id="6bc89-107">Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="6bc89-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="6bc89-108">Supporta anche azioni, ad esempio un clic del mouse.</span><span class="sxs-lookup"><span data-stu-id="6bc89-108">It also supports actions, like mouse clicks.</span></span> <span data-ttu-id="6bc89-109">Tuttavia quando un clic del mouse viene avviata un'animazione, è consigliabile disabilitare i clic del mouse durante l'animazione.</span><span class="sxs-lookup"><span data-stu-id="6bc89-109">However when a mouse click starts an animation, it is desirable to disable mouse clicks during the animation.</span></span>


## <a name="overview"></a><span data-ttu-id="6bc89-110">Panoramica</span><span class="sxs-lookup"><span data-stu-id="6bc89-110">Overview</span></span>

<span data-ttu-id="6bc89-111">Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="6bc89-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="6bc89-112">Supporta anche azioni, ad esempio un clic del mouse.</span><span class="sxs-lookup"><span data-stu-id="6bc89-112">It also supports actions, like mouse clicks.</span></span> <span data-ttu-id="6bc89-113">Tuttavia quando un clic del mouse viene avviata un'animazione, è consigliabile disabilitare i clic del mouse durante l'animazione.</span><span class="sxs-lookup"><span data-stu-id="6bc89-113">However when a mouse click starts an animation, it is desirable to disable mouse clicks during the animation.</span></span>

## <a name="steps"></a><span data-ttu-id="6bc89-114">Passaggi</span><span class="sxs-lookup"><span data-stu-id="6bc89-114">Steps</span></span>

<span data-ttu-id="6bc89-115">Prima di tutto, includere il `ScriptManager` nella pagina; quindi, la libreria ASP.NET AJAX viene caricata, rendendo possibile usare il Toolkit di controllo:</span><span class="sxs-lookup"><span data-stu-id="6bc89-115">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample1.aspx)]

<span data-ttu-id="6bc89-116">Verrà applicata l'animazione a un pulsante HTML simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="6bc89-116">The animation will be applied to an HTML button like this:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample2.aspx)]

<span data-ttu-id="6bc89-117">Si noti che un controllo HTML viene utilizzato invece di un controllo Web poiché non vogliamo pulsante per creare un postback; deve avviare l'animazione lato client per noi.</span><span class="sxs-lookup"><span data-stu-id="6bc89-117">Note that an HTML Control is used instead of a Web Control since we do not want the button to create a postback; it shall just launch the client-side animation for us.</span></span>

<span data-ttu-id="6bc89-118">Quindi, aggiungere il `AnimationExtender` alla pagina, fornendo un' `ID`, il `TargetControlID` attributo e l'obbligatoria `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="6bc89-118">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample3.aspx)]

<span data-ttu-id="6bc89-119">All'interno di `<Animations>` nodo, `<OnClick>` è l'elemento a destra per gestire i clic del mouse.</span><span class="sxs-lookup"><span data-stu-id="6bc89-119">Within the `<Animations>` node, `<OnClick>` is the right element to handle the mouse click.</span></span> <span data-ttu-id="6bc89-120">Tuttavia, potrebbe essere fatto clic sul pulsante durante l'animazione, nonché.</span><span class="sxs-lookup"><span data-stu-id="6bc89-120">However, the button could be clicked during the animation, as well.</span></span> <span data-ttu-id="6bc89-121">Il `<EnableAction>` può gestire di tale elemento.</span><span class="sxs-lookup"><span data-stu-id="6bc89-121">The `<EnableAction>` element can take care of that.</span></span> <span data-ttu-id="6bc89-122">Impostazione `Enabled="false"` disabilita il pulsante come parte dell'animazione.</span><span class="sxs-lookup"><span data-stu-id="6bc89-122">Setting `Enabled="false"` disables the button as part of the animation.</span></span> <span data-ttu-id="6bc89-123">Poiché si sta usando diverse singole animazioni (disabilita il pulsante e le animazioni effettive), il `<Parallel>` elemento è obbligatorio per associare le animazioni insieme in una singole.</span><span class="sxs-lookup"><span data-stu-id="6bc89-123">Since we are using several individual animations (disabling the button and the actual animations), the `<Parallel>` element is required to glue the single animations together into one.</span></span> <span data-ttu-id="6bc89-124">Ecco il markup completo per `AnimationExtender`:</span><span class="sxs-lookup"><span data-stu-id="6bc89-124">Here is the complete markup for `AnimationExtender`:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample4.aspx)]

<span data-ttu-id="6bc89-125">È anche possibile abilitare nuovamente al pulsante dopo l'animazione, utilizzando l'elemento XML seguente alla fine dell'elenco:</span><span class="sxs-lookup"><span data-stu-id="6bc89-125">It would also be possible to re-enable to button after the animation, using the following XML element at the end of the list:</span></span>

[!code-xml[Main](disabling-actions-during-animation-vb/samples/sample5.xml)]

<span data-ttu-id="6bc89-126">Tuttavia in questo scenario specifico questo sarebbe inutile perché il pulsante dissolve e non è visibile alla fine dell'animazione.</span><span class="sxs-lookup"><span data-stu-id="6bc89-126">However in the given scenario this would be useless since the button fades out and is not visible at the end of the animation.</span></span>


<span data-ttu-id="6bc89-127">[![Il pulsante è disabilitato, non appena viene eseguita l'animazione](disabling-actions-during-animation-vb/_static/image2.png)](disabling-actions-during-animation-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="6bc89-127">[![The button is disabled as soon as the animation runs](disabling-actions-during-animation-vb/_static/image2.png)](disabling-actions-during-animation-vb/_static/image1.png)</span></span>

<span data-ttu-id="6bc89-128">Il pulsante è disabilitato, non appena viene eseguita l'animazione ([fare clic per visualizzare l'immagine con dimensioni normali](disabling-actions-during-animation-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="6bc89-128">The button is disabled as soon as the animation runs ([Click to view full-size image](disabling-actions-during-animation-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="6bc89-129">[Precedente](animating-in-response-to-user-interaction-vb.md)
> [Successivo](triggering-an-animation-in-another-control-vb.md)</span><span class="sxs-lookup"><span data-stu-id="6bc89-129">[Previous](animating-in-response-to-user-interaction-vb.md)
[Next](triggering-an-animation-in-another-control-vb.md)</span></span>
