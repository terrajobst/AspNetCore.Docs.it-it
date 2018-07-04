---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-at-the-same-time-vb
title: Esecuzione di diverse animazioni contemporaneamente (VB) | Microsoft Docs
author: wenz
description: Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo. Consente di eseguire severa...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 2469f7ea-1489-42fb-a8e1-414c90141ce9
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-at-the-same-time-vb
msc.type: authoredcontent
ms.openlocfilehash: 28a6bcabf765c134255e4b45555226198c978fef
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37394489"
---
<a name="executing-several-animations-at-the-same-time-vb"></a><span data-ttu-id="14f74-104">Esecuzione di diverse animazioni contemporaneamente (VB)</span><span class="sxs-lookup"><span data-stu-id="14f74-104">Executing Several Animations at The Same Time (VB)</span></span>
====================
<span data-ttu-id="14f74-105">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="14f74-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="14f74-106">[Scaricare il codice](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation2.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="14f74-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation2.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation2VB.pdf)</span></span>

> <span data-ttu-id="14f74-107">Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="14f74-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="14f74-108">Consente l'esecuzione di diverse animazioni in parallelo.</span><span class="sxs-lookup"><span data-stu-id="14f74-108">It allows to run several animations in a parallel fashion.</span></span>


## <a name="overview"></a><span data-ttu-id="14f74-109">Panoramica</span><span class="sxs-lookup"><span data-stu-id="14f74-109">Overview</span></span>

<span data-ttu-id="14f74-110">Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="14f74-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="14f74-111">Consente l'esecuzione di diverse animazioni in parallelo.</span><span class="sxs-lookup"><span data-stu-id="14f74-111">It allows to run several animations in a parallel fashion.</span></span>

## <a name="steps"></a><span data-ttu-id="14f74-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="14f74-112">Steps</span></span>

<span data-ttu-id="14f74-113">Prima di tutto, includere il `ScriptManager` nella pagina; quindi, la libreria ASP.NET AJAX viene caricata, rendendo possibile usare il Toolkit di controllo:</span><span class="sxs-lookup"><span data-stu-id="14f74-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](executing-several-animations-at-the-same-time-vb/samples/sample1.aspx)]

<span data-ttu-id="14f74-114">Verrà applicata l'animazione a un pannello del testo che presenta un aspetto simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="14f74-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](executing-several-animations-at-the-same-time-vb/samples/sample2.aspx)]

<span data-ttu-id="14f74-115">Nella classe CSS associata per il pannello, definire un colore di sfondo interessante e anche impostare una larghezza fissa per il pannello:</span><span class="sxs-lookup"><span data-stu-id="14f74-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](executing-several-animations-at-the-same-time-vb/samples/sample3.css)]

<span data-ttu-id="14f74-116">Quindi, aggiungere il `AnimationExtender` alla pagina, fornendo un' `ID`, il `TargetControlID` attributo e l'obbligatoria `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="14f74-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](executing-several-animations-at-the-same-time-vb/samples/sample4.aspx)]

<span data-ttu-id="14f74-117">All'interno di `<Animations>` nodo, usare `<OnLoad>` per eseguire le animazioni dopo la pagina è stata caricata completamente.</span><span class="sxs-lookup"><span data-stu-id="14f74-117">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="14f74-118">In generale, `<OnLoad>` accetta solo un'animazione.</span><span class="sxs-lookup"><span data-stu-id="14f74-118">Generally, `<OnLoad>` only accepts one animation.</span></span> <span data-ttu-id="14f74-119">Il framework di animazione consente di eseguire il join di diverse animazioni in uno solo tramite il `<Parallel>` elemento.</span><span class="sxs-lookup"><span data-stu-id="14f74-119">The Animation framework allows you to join several animations into one using the `<Parallel>` element.</span></span> <span data-ttu-id="14f74-120">Tutte le animazioni in `<Parallel>` vengono eseguiti nello stesso momento.</span><span class="sxs-lookup"><span data-stu-id="14f74-120">All animations within `<Parallel>` are executed at the same time.</span></span>

<span data-ttu-id="14f74-121">Di seguito è riportato l'un markup possibili per il `AnimationExtender` controllo, dissolvenza in uscita e ridimensionare il pannello allo stesso tempo:</span><span class="sxs-lookup"><span data-stu-id="14f74-121">Here is the a possible markup for the `AnimationExtender` control, fading out and resizing the panel at the same time:</span></span>

[!code-aspx[Main](executing-several-animations-at-the-same-time-vb/samples/sample5.aspx)]

<span data-ttu-id="14f74-122">E in effetti: quando si esegue questo script, viene visualizzato il pannello e quindi ridimensiona (più di threefolding relativa larghezza e halfing altezza) e dissolve nello stesso momento.</span><span class="sxs-lookup"><span data-stu-id="14f74-122">And indeed: when you run this script, the panel is displayed, then resizes (more than threefolding its width and halfing its height) and fades out at the same time.</span></span>


<span data-ttu-id="14f74-123">[![Il pannello è dissolvenza in uscita e il ridimensionamento (incluso il relativo contenuto, grazie al motore di rendering del browser)](executing-several-animations-at-the-same-time-vb/_static/image2.png)](executing-several-animations-at-the-same-time-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="14f74-123">[![The panel is fading out and resizing (including its content, thanks to the browser's rendering engine)](executing-several-animations-at-the-same-time-vb/_static/image2.png)](executing-several-animations-at-the-same-time-vb/_static/image1.png)</span></span>

<span data-ttu-id="14f74-124">Il pannello è dissolvenza in uscita e il ridimensionamento (incluso il relativo contenuto, grazie al motore di rendering del browser) ([fare clic per visualizzare l'immagine con dimensioni normali](executing-several-animations-at-the-same-time-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="14f74-124">The panel is fading out and resizing (including its content, thanks to the browser's rendering engine) ([Click to view full-size image](executing-several-animations-at-the-same-time-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="14f74-125">[Precedente](adding-animation-to-a-control-vb.md)
> [Successivo](executing-several-animations-after-each-other-vb.md)</span><span class="sxs-lookup"><span data-stu-id="14f74-125">[Previous](adding-animation-to-a-control-vb.md)
[Next](executing-several-animations-after-each-other-vb.md)</span></span>
