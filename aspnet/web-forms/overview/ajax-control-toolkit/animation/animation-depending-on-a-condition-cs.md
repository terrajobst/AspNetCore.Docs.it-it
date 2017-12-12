---
uid: web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-cs
title: Animazione in base a una condizione (c#) | Documenti Microsoft
author: wenz
description: "Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework per aggiungere le animazioni a un controllo. Se è un'animazione..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: b7a28c0d-efb9-443a-80a4-1a5ee54671cd
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animation-depending-on-a-condition-cs
msc.type: authoredcontent
ms.openlocfilehash: 13366a86be01f41e27db1869b93192520190387a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="animation-depending-on-a-condition-c"></a><span data-ttu-id="4ddcf-104">Animazione in base a una condizione (c#)</span><span class="sxs-lookup"><span data-stu-id="4ddcf-104">Animation Depending On a Condition (C#)</span></span>
====================
<span data-ttu-id="4ddcf-105">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="4ddcf-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="4ddcf-106">[Scaricare codice](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation4.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation4CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="4ddcf-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation4.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation4CS.pdf)</span></span>

> <span data-ttu-id="4ddcf-107">Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework per aggiungere le animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="4ddcf-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="4ddcf-108">Se un'animazione viene eseguita o non può dipendere anche una condizione in forma di codice JavaScript.</span><span class="sxs-lookup"><span data-stu-id="4ddcf-108">Whether an animation is run or not can also depend on a condition in form of some JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="4ddcf-109">Panoramica</span><span class="sxs-lookup"><span data-stu-id="4ddcf-109">Overview</span></span>

<span data-ttu-id="4ddcf-110">Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework per aggiungere le animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="4ddcf-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="4ddcf-111">Se un'animazione viene eseguita o non può dipendere anche una condizione in forma di codice JavaScript.</span><span class="sxs-lookup"><span data-stu-id="4ddcf-111">Whether an animation is run or not can also depend on a condition in form of some JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="4ddcf-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="4ddcf-112">Steps</span></span>

<span data-ttu-id="4ddcf-113">Prima di tutto, includere il `ScriptManager` nella pagina; quindi, la libreria AJAX di ASP.NET viene caricata, che consente di usare il Toolkit di controllo:</span><span class="sxs-lookup"><span data-stu-id="4ddcf-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample1.aspx)]

<span data-ttu-id="4ddcf-114">Verrà applicata l'animazione a un riquadro del testo che presenta un aspetto simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="4ddcf-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample2.aspx)]

<span data-ttu-id="4ddcf-115">Nella classe CSS associata per il pannello, definire un colore di sfondo nice e inoltre impostare una larghezza fissa per il pannello:</span><span class="sxs-lookup"><span data-stu-id="4ddcf-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](animation-depending-on-a-condition-cs/samples/sample3.css)]

<span data-ttu-id="4ddcf-116">Quindi, aggiungere il `AnimationExtender` alla pagina, fornendo un `ID`, `TargetControlID` attributo e l'obbligatoria`runat="server":`</span><span class="sxs-lookup"><span data-stu-id="4ddcf-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server":`</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample4.aspx)]

<span data-ttu-id="4ddcf-117">All'interno di `<Animations>` nodo, utilizzare `<OnLoad>` per eseguire le animazioni dopo la pagina è stata caricata completamente.</span><span class="sxs-lookup"><span data-stu-id="4ddcf-117">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="4ddcf-118">Invece di una delle animazioni, regolare il `<Condition>` elemento entra in gioco.</span><span class="sxs-lookup"><span data-stu-id="4ddcf-118">Instead of one of the regular animations, the `<Condition>` element comes into play.</span></span> <span data-ttu-id="4ddcf-119">Il codice JavaScript fornito come valore della `ConditionScript` attributo viene eseguito in fase di esecuzione.</span><span class="sxs-lookup"><span data-stu-id="4ddcf-119">The JavaScript code provided as the value of the `ConditionScript` attribute is executed at runtime.</span></span> <span data-ttu-id="4ddcf-120">Se restituisce true, l'animazione viene eseguita, in caso contrario non.</span><span class="sxs-lookup"><span data-stu-id="4ddcf-120">If it evaluates to true, the animation is executed, otherwise not.</span></span> <span data-ttu-id="4ddcf-121">Il markup seguente fornisce due animazioni, ognuno di essi in esecuzione al 50% dei case al momento casuale.</span><span class="sxs-lookup"><span data-stu-id="4ddcf-121">The following markup provides two animations, each of them being executed in 50% of cases upon random.</span></span> <span data-ttu-id="4ddcf-122">Poiché potrebbe essere presente solo un'animazione all'interno di `<OnLoad>`, i due `<Condition>` animazioni vengono unite tramite un insieme di `<Sequence>` elemento:</span><span class="sxs-lookup"><span data-stu-id="4ddcf-122">Since there may only be one animation within `<OnLoad>`, the two `<Condition>` animations are joined together using the `<Sequence>` element:</span></span>

[!code-aspx[Main](animation-depending-on-a-condition-cs/samples/sample5.aspx)]

<span data-ttu-id="4ddcf-123">Si noti che il segno di minore di (`<`) nei `ConditionScript` attributo deve essere sottoposto a escape ().</span><span class="sxs-lookup"><span data-stu-id="4ddcf-123">Note that the less than sign (`<`) in the `ConditionScript` attribute must be escaped ().</span></span> <span data-ttu-id="4ddcf-124">Quando si esegue questo script viene eseguito alcuna animazione, o uno dei due non oppure eseguire entrambe.</span><span class="sxs-lookup"><span data-stu-id="4ddcf-124">When you run this script, either no animation runs, or one of the two does, or both do.</span></span>


<span data-ttu-id="4ddcf-125">[![Il pannello è dissolvenza in uscita senza alcun ridimensionamento, pertanto l'esecuzione del secondo animazione, il primo non](animation-depending-on-a-condition-cs/_static/image2.png)](animation-depending-on-a-condition-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="4ddcf-125">[![The panel is fading out without resizing, so the second animation runs, the first one didn't](animation-depending-on-a-condition-cs/_static/image2.png)](animation-depending-on-a-condition-cs/_static/image1.png)</span></span>

<span data-ttu-id="4ddcf-126">Il pannello è dissolvenza in uscita senza alcun ridimensionamento, in modo non ha l'esecuzione del secondo animazione, il primo ([fare clic per visualizzare l'immagine ingrandita](animation-depending-on-a-condition-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="4ddcf-126">The panel is fading out without resizing, so the second animation runs, the first one didn't ([Click to view full-size image](animation-depending-on-a-condition-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="4ddcf-127">[Precedente](executing-several-animations-after-each-other-cs.md)
[Successivo](picking-one-animation-out-of-a-list-cs.md)</span><span class="sxs-lookup"><span data-stu-id="4ddcf-127">[Previous](executing-several-animations-after-each-other-cs.md)
[Next](picking-one-animation-out-of-a-list-cs.md)</span></span>
