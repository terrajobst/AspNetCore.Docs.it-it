---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-cs
title: Esecuzione di diverse animazioni una dopo l'altra (c#) | Microsoft Docs
author: wenz
description: Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo. Consente di eseguire severa...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 7dc02b18-2b5d-4844-b7c5-cbd818477163
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-cs
msc.type: authoredcontent
ms.openlocfilehash: 60fb7dacfe59eaafef71fcc9cde61b7840624343
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835649"
---
<a name="executing-several-animations-after-each-other-c"></a><span data-ttu-id="5f4e5-104">Esecuzione di diverse animazioni una dopo l'altra (c#)</span><span class="sxs-lookup"><span data-stu-id="5f4e5-104">Executing Several Animations after Each Other (C#)</span></span>
====================
<span data-ttu-id="5f4e5-105">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="5f4e5-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="5f4e5-106">[Scaricare il codice](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="5f4e5-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3CS.pdf)</span></span>

> <span data-ttu-id="5f4e5-107">Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="5f4e5-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="5f4e5-108">Consente l'esecuzione di diverse animazioni una dopo l'altro.</span><span class="sxs-lookup"><span data-stu-id="5f4e5-108">It allows to run several animations one after the other.</span></span>


<span data-ttu-id="5f4e5-109">Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="5f4e5-109">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="5f4e5-110">Consente l'esecuzione di diverse animazioni una dopo l'altro.</span><span class="sxs-lookup"><span data-stu-id="5f4e5-110">It allows to run several animations one after the other.</span></span>

## <a name="steps"></a><span data-ttu-id="5f4e5-111">Passaggi</span><span class="sxs-lookup"><span data-stu-id="5f4e5-111">Steps</span></span>

<span data-ttu-id="5f4e5-112">Prima di tutto, includere il `ScriptManager` nella pagina; quindi, la libreria ASP.NET AJAX viene caricata, rendendo possibile usare il Toolkit di controllo:</span><span class="sxs-lookup"><span data-stu-id="5f4e5-112">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample1.aspx)]

<span data-ttu-id="5f4e5-113">Verrà applicata l'animazione a un pannello del testo che presenta un aspetto simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="5f4e5-113">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample2.aspx)]

<span data-ttu-id="5f4e5-114">Nella classe CSS associata per il pannello, definire un colore di sfondo interessante e anche impostare una larghezza fissa per il pannello:</span><span class="sxs-lookup"><span data-stu-id="5f4e5-114">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](executing-several-animations-after-each-other-cs/samples/sample3.css)]

<span data-ttu-id="5f4e5-115">Quindi, aggiungere il `AnimationExtender` alla pagina, fornendo un' `ID`, il `TargetControlID` attributo e l'obbligatoria `runat="server":`</span><span class="sxs-lookup"><span data-stu-id="5f4e5-115">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server":`</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample4.aspx)]

<span data-ttu-id="5f4e5-116">All'interno di `<Animations>` nodo, usare `<OnLoad>` per eseguire le animazioni dopo la pagina è stata caricata completamente.</span><span class="sxs-lookup"><span data-stu-id="5f4e5-116">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="5f4e5-117">In generale, `<OnLoad>` accetta solo un'animazione.</span><span class="sxs-lookup"><span data-stu-id="5f4e5-117">Generally, `<OnLoad>` only accepts one animation.</span></span> <span data-ttu-id="5f4e5-118">Il framework di animazione consente di eseguire il join di diverse animazioni in uno solo tramite il `<Sequence>` elemento.</span><span class="sxs-lookup"><span data-stu-id="5f4e5-118">The Animation framework allows you to join several animations into one using the `<Sequence>` element.</span></span> <span data-ttu-id="5f4e5-119">Tutte le animazioni in `<Sequence>` vengono eseguite una dopo l'altro.</span><span class="sxs-lookup"><span data-stu-id="5f4e5-119">All animations within `<Sequence>` are executed one after the other.</span></span> <span data-ttu-id="5f4e5-120">Di seguito è riportato l'un markup possibili per il `AnimationExtender` controllo, prima di tutto rendendo più ampio del pannello e quindi diminuire l'altezza:</span><span class="sxs-lookup"><span data-stu-id="5f4e5-120">Here is the a possible markup for the `AnimationExtender` control, first making the panel wider and then decreasing its height:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample5.aspx)]

<span data-ttu-id="5f4e5-121">Quando si esegue questo script, il pannello prima ottiene più larghi e quindi più piccoli.</span><span class="sxs-lookup"><span data-stu-id="5f4e5-121">When you run this script, the panel first gets wider and then smaller.</span></span>


<span data-ttu-id="5f4e5-122">[![Prima di tutto la larghezza viene aumentata](executing-several-animations-after-each-other-cs/_static/image2.png)](executing-several-animations-after-each-other-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="5f4e5-122">[![First the width is increased](executing-several-animations-after-each-other-cs/_static/image2.png)](executing-several-animations-after-each-other-cs/_static/image1.png)</span></span>

<span data-ttu-id="5f4e5-123">Prima di tutto la larghezza viene aumentata ([fare clic per visualizzare l'immagine con dimensioni normali](executing-several-animations-after-each-other-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="5f4e5-123">First the width is increased ([Click to view full-size image](executing-several-animations-after-each-other-cs/_static/image3.png))</span></span>


<span data-ttu-id="5f4e5-124">[![Quindi viene diminuito l'altezza](executing-several-animations-after-each-other-cs/_static/image5.png)](executing-several-animations-after-each-other-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="5f4e5-124">[![Then the height is decreased](executing-several-animations-after-each-other-cs/_static/image5.png)](executing-several-animations-after-each-other-cs/_static/image4.png)</span></span>

<span data-ttu-id="5f4e5-125">Quindi viene diminuito l'altezza ([fare clic per visualizzare l'immagine con dimensioni normali](executing-several-animations-after-each-other-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="5f4e5-125">Then the height is decreased ([Click to view full-size image](executing-several-animations-after-each-other-cs/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="5f4e5-126">[Precedente](executing-several-animations-at-the-same-time-cs.md)
> [Successivo](animation-depending-on-a-condition-cs.md)</span><span class="sxs-lookup"><span data-stu-id="5f4e5-126">[Previous](executing-several-animations-at-the-same-time-cs.md)
[Next](animation-depending-on-a-condition-cs.md)</span></span>
