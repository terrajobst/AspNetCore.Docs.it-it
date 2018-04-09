---
uid: web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-cs
title: Selezione di un'animazione da un elenco (c#) | Documenti Microsoft
author: wenz
description: Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework per aggiungere le animazioni a un controllo. Il framework anche autoriz...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 06a776fe-7c73-4ca7-8e02-5260a86edc03
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-cs
msc.type: authoredcontent
ms.openlocfilehash: 2d4aac447fcdfbf296560091cfcdf5eb51997a7b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="picking-one-animation-out-of-a-list-c"></a><span data-ttu-id="d8bed-104">Selezione di un'animazione da un elenco (c#)</span><span class="sxs-lookup"><span data-stu-id="d8bed-104">Picking One Animation Out Of a List (C#)</span></span>
====================
<span data-ttu-id="d8bed-105">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="d8bed-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="d8bed-106">[Scaricare codice](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation5.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation5CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="d8bed-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation5.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation5CS.pdf)</span></span>

> <span data-ttu-id="d8bed-107">Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework per aggiungere le animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="d8bed-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="d8bed-108">Inoltre, il framework consente ai programmatori di scegliere un'animazione da un elenco di animazioni, a seconda della valutazione del codice JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d8bed-108">The framework also allows the programmer to pick one animation out of a list of animations, depending on the evaluation of some JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="d8bed-109">Panoramica</span><span class="sxs-lookup"><span data-stu-id="d8bed-109">Overview</span></span>

<span data-ttu-id="d8bed-110">Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework per aggiungere le animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="d8bed-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="d8bed-111">Inoltre, il framework consente ai programmatori di scegliere un'animazione da un elenco di animazioni, a seconda della valutazione del codice JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d8bed-111">The framework also allows the programmer to pick one animation out of a list of animations, depending on the evaluation of some JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="d8bed-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="d8bed-112">Steps</span></span>

<span data-ttu-id="d8bed-113">Prima di tutto, includere il `ScriptManager` nella pagina; quindi, la libreria AJAX di ASP.NET viene caricata, che consente di usare il Toolkit di controllo:</span><span class="sxs-lookup"><span data-stu-id="d8bed-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample1.aspx)]

<span data-ttu-id="d8bed-114">Verrà applicata l'animazione a un riquadro del testo che presenta un aspetto simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="d8bed-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample2.aspx)]

<span data-ttu-id="d8bed-115">Nella classe CSS associata per il pannello, definire un colore di sfondo nice e inoltre impostare una larghezza fissa per il pannello:</span><span class="sxs-lookup"><span data-stu-id="d8bed-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](picking-one-animation-out-of-a-list-cs/samples/sample3.css)]

<span data-ttu-id="d8bed-116">Quindi, aggiungere il `AnimationExtender` alla pagina, fornendo un' `ID`, il `TargetControlID` attributo e l'obbligatoria `runat="server":`</span><span class="sxs-lookup"><span data-stu-id="d8bed-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server":`</span></span>

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample4.aspx)]

<span data-ttu-id="d8bed-117">All'interno di `<Animations>` nodo, utilizzare `<OnLoad>` per eseguire le animazioni dopo la pagina è stata caricata completamente.</span><span class="sxs-lookup"><span data-stu-id="d8bed-117">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="d8bed-118">Invece di una delle animazioni, regolare il `<Case>` elemento entra in gioco.</span><span class="sxs-lookup"><span data-stu-id="d8bed-118">Instead of one of the regular animations, the `<Case>` element comes into play.</span></span> <span data-ttu-id="d8bed-119">Viene valutato il valore dell'attributo SelectScript; il valore restituito deve essere numerico.</span><span class="sxs-lookup"><span data-stu-id="d8bed-119">The value of its SelectScript attribute is evaluated; the return value must be numerical.</span></span> <span data-ttu-id="d8bed-120">A seconda di questo numero, tra i dipendenti all'interno di &lt;Case&gt; viene eseguita.</span><span class="sxs-lookup"><span data-stu-id="d8bed-120">Depending on this number, one of the subanimations within &lt;Case&gt; is executed.</span></span> <span data-ttu-id="d8bed-121">Ad esempio, se SelectScript restituisce 2, il Toolkit di controllo viene eseguita la terza animazione all'interno di &lt;Case&gt; (conteggio inizia da 0).</span><span class="sxs-lookup"><span data-stu-id="d8bed-121">For instance, if SelectScript evaluates to 2, the Control Toolkit runs the third animation within &lt;Case&gt; (counting starts at 0).</span></span>

<span data-ttu-id="d8bed-122">Il markup seguente definisce tre dipendenti: ridimensionare la larghezza e l'altezza di ridimensionamento dissolvenza in uscita. Il codice JavaScript (`Math.floor(3 * Math.random())`) quindi sceglie un numero compreso tra 0 e 2, in modo che uno dei tre animazioni viene eseguito:</span><span class="sxs-lookup"><span data-stu-id="d8bed-122">The following markup defines three subanimations: Resizing the width, resizing the height, and fading out. The JavaScript code (`Math.floor(3 * Math.random())`) then picks a number between 0 and 2, so that one of the three animations is run:</span></span>

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample5.aspx)]


<span data-ttu-id="d8bed-123">[![Uno dei tre animazioni possibili: il pannello ottiene più ampio](picking-one-animation-out-of-a-list-cs/_static/image2.png)](picking-one-animation-out-of-a-list-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="d8bed-123">[![One of the possible three animations: The panel gets wider](picking-one-animation-out-of-a-list-cs/_static/image2.png)](picking-one-animation-out-of-a-list-cs/_static/image1.png)</span></span>

<span data-ttu-id="d8bed-124">Una delle tre animazioni possibili: il pannello si allunga ([fare clic per visualizzare l'immagine ingrandita](picking-one-animation-out-of-a-list-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="d8bed-124">One of the possible three animations: The panel gets wider ([Click to view full-size image](picking-one-animation-out-of-a-list-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d8bed-125">[Precedente](animation-depending-on-a-condition-cs.md)
> [Successivo](animating-in-response-to-user-interaction-cs.md)</span><span class="sxs-lookup"><span data-stu-id="d8bed-125">[Previous](animation-depending-on-a-condition-cs.md)
[Next](animating-in-response-to-user-interaction-cs.md)</span></span>
