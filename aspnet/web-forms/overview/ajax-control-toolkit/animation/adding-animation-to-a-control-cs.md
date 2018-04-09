---
uid: web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-cs
title: Aggiunta di animazione a un controllo (c#) | Documenti Microsoft
author: wenz
description: Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework per aggiungere le animazioni a un controllo. Questa esercitazione viene illustrato come...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 0f1fc1f5-9dbd-44e7-931e-387d42f0342b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-cs
msc.type: authoredcontent
ms.openlocfilehash: ba122660045c3f5dd4b11f118df174a79de814a1
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="adding-animation-to-a-control-c"></a><span data-ttu-id="ce52a-104">Aggiunta di animazione a un controllo (c#)</span><span class="sxs-lookup"><span data-stu-id="ce52a-104">Adding Animation to a Control (C#)</span></span>
====================
<span data-ttu-id="ce52a-105">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="ce52a-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="ce52a-106">[Scaricare codice](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="ce52a-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1CS.pdf)</span></span>

> <span data-ttu-id="ce52a-107">Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework per aggiungere le animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="ce52a-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="ce52a-108">In questa esercitazione viene illustrato come impostare tali un'animazione.</span><span class="sxs-lookup"><span data-stu-id="ce52a-108">This tutorial shows how to set up such an animation.</span></span>


## <a name="overview"></a><span data-ttu-id="ce52a-109">Panoramica</span><span class="sxs-lookup"><span data-stu-id="ce52a-109">Overview</span></span>

<span data-ttu-id="ce52a-110">Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework per aggiungere le animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="ce52a-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="ce52a-111">In questa esercitazione viene illustrato come impostare tali un'animazione.</span><span class="sxs-lookup"><span data-stu-id="ce52a-111">This tutorial shows how to set up such an animation.</span></span>

## <a name="steps"></a><span data-ttu-id="ce52a-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="ce52a-112">Steps</span></span>

<span data-ttu-id="ce52a-113">Il primo passaggio consiste nel modo consueto per includere il `ScriptManager` nella pagina in modo che la libreria ASP.NET AJAX è caricata e può essere utilizzato il Toolkit di controllo:</span><span class="sxs-lookup"><span data-stu-id="ce52a-113">The first step is as usual to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample1.aspx)]

<span data-ttu-id="ce52a-114">L'animazione in questo scenario verrà applicata a un riquadro del testo che presenta un aspetto simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="ce52a-114">The animation in this scenario will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample2.aspx)]

<span data-ttu-id="ce52a-115">La classe CSS associata per il pannello definisce un colore di sfondo e una larghezza:</span><span class="sxs-lookup"><span data-stu-id="ce52a-115">The associated CSS class for the panel defines a background color and a width:</span></span>

[!code-css[Main](adding-animation-to-a-control-cs/samples/sample3.css)]

<span data-ttu-id="ce52a-116">Successivamente, è necessario il `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="ce52a-116">Next up, we need the `AnimationExtender`.</span></span> <span data-ttu-id="ce52a-117">Dopo aver fornito un `ID` e le consuete `runat="server"`, `TargetControlID` attributo deve essere impostato per il controllo per aggiungere un'animazione in questo caso, il pannello:</span><span class="sxs-lookup"><span data-stu-id="ce52a-117">After providing an `ID` and the usual `runat="server"`, the `TargetControlID` attribute must be set to the control to animate in our case, the panel:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample4.aspx)]

<span data-ttu-id="ce52a-118">L'intera animazione viene applicata in modo dichiarativo, utilizzare la sintassi XML, purtroppo attualmente non è completamente supportata da IntelliSense di Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ce52a-118">The whole animation is applied declaratively, using an XML syntax, unfortunately currently not fully supported by Visual Studio's IntelliSense.</span></span> <span data-ttu-id="ce52a-119">Il nodo radice è `<Animations>;` all'interno di questo nodo sono consentiti diversi eventi che determinano quando animazione adottino sul posto:</span><span class="sxs-lookup"><span data-stu-id="ce52a-119">The root node is `<Animations>;` within this node, several events are allowed which determine when the animation(s) take(s) place:</span></span>

- <span data-ttu-id="ce52a-120">`OnClick` (clic del mouse)</span><span class="sxs-lookup"><span data-stu-id="ce52a-120">`OnClick` (mouse click)</span></span>
- <span data-ttu-id="ce52a-121">`OnHoverOut` (quando il puntatore del mouse esce da un controllo)</span><span class="sxs-lookup"><span data-stu-id="ce52a-121">`OnHoverOut` (when the mouse leaves a control)</span></span>
- <span data-ttu-id="ce52a-122">`OnHoverOver` (quando il mouse viene spostato su un controllo, l'arresto di `OnHoverOut` animazione)</span><span class="sxs-lookup"><span data-stu-id="ce52a-122">`OnHoverOver` (when the mouse hovers over a control, stopping the `OnHoverOut` animation)</span></span>
- <span data-ttu-id="ce52a-123">`OnLoad` (quando il caricamento della pagina)</span><span class="sxs-lookup"><span data-stu-id="ce52a-123">`OnLoad` (when the page has been loaded)</span></span>
- <span data-ttu-id="ce52a-124">`OnMouseOut` (quando il puntatore del mouse esce da un controllo)</span><span class="sxs-lookup"><span data-stu-id="ce52a-124">`OnMouseOut` (when the mouse leaves a control)</span></span>
- <span data-ttu-id="ce52a-125">`OnMouseOver` (quando il mouse viene spostato su un controllo, non arrestare il `OnMouseOut` animazione)</span><span class="sxs-lookup"><span data-stu-id="ce52a-125">`OnMouseOver` (when the mouse hovers over a control, not stopping the `OnMouseOut` animation)</span></span>

<span data-ttu-id="ce52a-126">Il framework viene fornito con un insieme di animazioni, ognuno rappresentato da un proprio elemento XML.</span><span class="sxs-lookup"><span data-stu-id="ce52a-126">The framework comes with a set of animations, each one represented by its own XML element.</span></span> <span data-ttu-id="ce52a-127">Di seguito è riportata una selezione:</span><span class="sxs-lookup"><span data-stu-id="ce52a-127">Here is a selection:</span></span>

- <span data-ttu-id="ce52a-128">`<Color>` (modifica di un colore)</span><span class="sxs-lookup"><span data-stu-id="ce52a-128">`<Color>` (changing a color)</span></span>
- <span data-ttu-id="ce52a-129">`<FadeIn>` (la dissolvenza in entrata)</span><span class="sxs-lookup"><span data-stu-id="ce52a-129">`<FadeIn>` (fading in)</span></span>
- <span data-ttu-id="ce52a-130">`<FadeOut>` (dissolvenza in uscita)</span><span class="sxs-lookup"><span data-stu-id="ce52a-130">`<FadeOut>` (fading out)</span></span>
- <span data-ttu-id="ce52a-131">`<Property>` (modifica delle proprietà di un controllo)</span><span class="sxs-lookup"><span data-stu-id="ce52a-131">`<Property>` (changing a control's property)</span></span>
- <span data-ttu-id="ce52a-132">`<Pulse>` (pulsating)</span><span class="sxs-lookup"><span data-stu-id="ce52a-132">`<Pulse>` (pulsating)</span></span>
- <span data-ttu-id="ce52a-133">`<Resize>` (modifica delle dimensioni)</span><span class="sxs-lookup"><span data-stu-id="ce52a-133">`<Resize>` (changing the size)</span></span>
- <span data-ttu-id="ce52a-134">`<Scale>` (in modo proporzionale modifica delle dimensioni)</span><span class="sxs-lookup"><span data-stu-id="ce52a-134">`<Scale>` (proportionally changing the size)</span></span>

<span data-ttu-id="ce52a-135">In questo esempio, il pannello è dissolvenza in uscita. L'animazione adottano 1,5 secondi (`Duration` attributi), la visualizzazione 24 fotogrammi (passaggi animazione) al secondo (`Fps` attributs).</span><span class="sxs-lookup"><span data-stu-id="ce52a-135">In this example, the panel shall fade out. The animation shall take 1.5 seconds (`Duration` attribute), displaying 24 frames (animation steps) per second (`Fps` attributs).</span></span> <span data-ttu-id="ce52a-136">Ecco il codice completo per il `AnimationExtender` controllo:</span><span class="sxs-lookup"><span data-stu-id="ce52a-136">Here is the complete markup for the `AnimationExtender` control:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample5.aspx)]

<span data-ttu-id="ce52a-137">Quando si esegue questo script, il pannello viene visualizzato e dissolvenza in 1,5 secondi.</span><span class="sxs-lookup"><span data-stu-id="ce52a-137">When you run this script, the panel is displayed and fades out in one and a half seconds.</span></span>


<span data-ttu-id="ce52a-138">[![Il pannello è dissolvenza in uscita](adding-animation-to-a-control-cs/_static/image2.png)](adding-animation-to-a-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="ce52a-138">[![The panel is fading out](adding-animation-to-a-control-cs/_static/image2.png)](adding-animation-to-a-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="ce52a-139">Il pannello è dissolvenza in uscita ([fare clic per visualizzare l'immagine ingrandita](adding-animation-to-a-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="ce52a-139">The panel is fading out ([Click to view full-size image](adding-animation-to-a-control-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="ce52a-140">avanti</span><span class="sxs-lookup"><span data-stu-id="ce52a-140">Next</span></span>](executing-several-animations-at-the-same-time-cs.md)
