---
uid: web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-vb
title: Selezione di un'animazione da un elenco (VB) | Microsoft Docs
author: wenz
description: Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo. Il framework conse inoltre...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 81ba9116-d485-40c0-8ff6-7e9ae23e0a0c
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-vb
msc.type: authoredcontent
ms.openlocfilehash: 561f05e96888962cfe576963ce3905b171203939
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37387741"
---
<a name="picking-one-animation-out-of-a-list-vb"></a><span data-ttu-id="56f88-104">Selezione di un'animazione da un elenco (VB)</span><span class="sxs-lookup"><span data-stu-id="56f88-104">Picking One Animation Out Of a List (VB)</span></span>
====================
<span data-ttu-id="56f88-105">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="56f88-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="56f88-106">[Scaricare il codice](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation5.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation5VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="56f88-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation5.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation5VB.pdf)</span></span>

> <span data-ttu-id="56f88-107">Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="56f88-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="56f88-108">Il framework consente inoltre al programmatore di scegliere un'animazione da un elenco di animazioni, a seconda della valutazione del codice JavaScript.</span><span class="sxs-lookup"><span data-stu-id="56f88-108">The framework also allows the programmer to pick one animation out of a list of animations, depending on the evaluation of some JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="56f88-109">Panoramica</span><span class="sxs-lookup"><span data-stu-id="56f88-109">Overview</span></span>

<span data-ttu-id="56f88-110">Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="56f88-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="56f88-111">Il framework consente inoltre al programmatore di scegliere un'animazione da un elenco di animazioni, a seconda della valutazione del codice JavaScript.</span><span class="sxs-lookup"><span data-stu-id="56f88-111">The framework also allows the programmer to pick one animation out of a list of animations, depending on the evaluation of some JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="56f88-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="56f88-112">Steps</span></span>

<span data-ttu-id="56f88-113">Prima di tutto, includere il `ScriptManager` nella pagina; quindi, la libreria ASP.NET AJAX viene caricata, rendendo possibile usare il Toolkit di controllo:</span><span class="sxs-lookup"><span data-stu-id="56f88-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample1.aspx)]

<span data-ttu-id="56f88-114">Verrà applicata l'animazione a un pannello del testo che presenta un aspetto simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="56f88-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample2.aspx)]

<span data-ttu-id="56f88-115">Nella classe CSS associata per il pannello, definire un colore di sfondo interessante e anche impostare una larghezza fissa per il pannello:</span><span class="sxs-lookup"><span data-stu-id="56f88-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](picking-one-animation-out-of-a-list-vb/samples/sample3.css)]

<span data-ttu-id="56f88-116">Quindi, aggiungere il `AnimationExtender` alla pagina, fornendo un' `ID`, il `TargetControlID` attributo e l'obbligatoria `runat="server":`</span><span class="sxs-lookup"><span data-stu-id="56f88-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server":`</span></span>

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample4.aspx)]

<span data-ttu-id="56f88-117">All'interno di `<Animations>` nodo, usare `<OnLoad>` per eseguire le animazioni dopo la pagina è stata caricata completamente.</span><span class="sxs-lookup"><span data-stu-id="56f88-117">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="56f88-118">Invece di uno delle animazioni, regolare il `<Case>` entra in gioco l'elemento.</span><span class="sxs-lookup"><span data-stu-id="56f88-118">Instead of one of the regular animations, the `<Case>` element comes into play.</span></span> <span data-ttu-id="56f88-119">Viene valutato il valore dell'attributo SelectScript; il valore restituito deve essere numerico.</span><span class="sxs-lookup"><span data-stu-id="56f88-119">The value of its SelectScript attribute is evaluated; the return value must be numerical.</span></span> <span data-ttu-id="56f88-120">A seconda di questo numero, una dei dipendenti all'interno &lt;caso&gt; viene eseguita.</span><span class="sxs-lookup"><span data-stu-id="56f88-120">Depending on this number, one of the subanimations within &lt;Case&gt; is executed.</span></span> <span data-ttu-id="56f88-121">Ad esempio, se SelectScript restituisce 2, il Toolkit di controllo viene eseguito l'animazione terzo entro &lt;caso&gt; (il conteggio inizia da 0).</span><span class="sxs-lookup"><span data-stu-id="56f88-121">For instance, if SelectScript evaluates to 2, the Control Toolkit runs the third animation within &lt;Case&gt; (counting starts at 0).</span></span>

<span data-ttu-id="56f88-122">Il markup seguente definisce tre dipendenti: la larghezza di ridimensionamento, il ridimensionamento l'altezza e dissolvenza in uscita. Il codice JavaScript (`Math.floor(3 * Math.random())`) quindi rileva un numero compreso tra 0 e 2, in modo che uno dei tre animazioni viene eseguito:</span><span class="sxs-lookup"><span data-stu-id="56f88-122">The following markup defines three subanimations: Resizing the width, resizing the height, and fading out. The JavaScript code (`Math.floor(3 * Math.random())`) then picks a number between 0 and 2, so that one of the three animations is run:</span></span>

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample5.aspx)]


<span data-ttu-id="56f88-123">[![Una delle animazioni di tre possibili: il pannello si allunga](picking-one-animation-out-of-a-list-vb/_static/image2.png)](picking-one-animation-out-of-a-list-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="56f88-123">[![One of the possible three animations: The panel gets wider](picking-one-animation-out-of-a-list-vb/_static/image2.png)](picking-one-animation-out-of-a-list-vb/_static/image1.png)</span></span>

<span data-ttu-id="56f88-124">Una delle animazioni di tre possibili: il pannello si allunga ([fare clic per visualizzare l'immagine con dimensioni normali](picking-one-animation-out-of-a-list-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="56f88-124">One of the possible three animations: The panel gets wider ([Click to view full-size image](picking-one-animation-out-of-a-list-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="56f88-125">[Precedente](animation-depending-on-a-condition-vb.md)
> [Successivo](animating-in-response-to-user-interaction-vb.md)</span><span class="sxs-lookup"><span data-stu-id="56f88-125">[Previous](animation-depending-on-a-condition-vb.md)
[Next](animating-in-response-to-user-interaction-vb.md)</span></span>
