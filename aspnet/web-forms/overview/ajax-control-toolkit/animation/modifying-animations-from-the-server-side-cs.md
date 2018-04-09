---
uid: web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-cs
title: Modifica le animazioni dal lato Server (c#) | Documenti Microsoft
author: wenz
description: Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework per aggiungere le animazioni a un controllo. Le animazioni potrebbero inoltre...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: b0abec39-a1c9-422d-ba9a-ef16f6185af8
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-cs
msc.type: authoredcontent
ms.openlocfilehash: 6946875552c885ffb1f2a2eb7e728b85d7dd3973
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="modifying-animations-from-the-server-side-c"></a><span data-ttu-id="c0a5f-104">Modifica le animazioni dal lato Server (c#)</span><span class="sxs-lookup"><span data-stu-id="c0a5f-104">Modifying Animations From The Server Side (C#)</span></span>
====================
<span data-ttu-id="c0a5f-105">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="c0a5f-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="c0a5f-106">[Scaricare codice](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="c0a5f-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9CS.pdf)</span></span>

> <span data-ttu-id="c0a5f-107">Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework per aggiungere le animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="c0a5f-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="c0a5f-108">Le animazioni possono anche essere modificate sul lato server</span><span class="sxs-lookup"><span data-stu-id="c0a5f-108">The animations may also be changed on the server-side</span></span>


## <a name="overview"></a><span data-ttu-id="c0a5f-109">Panoramica</span><span class="sxs-lookup"><span data-stu-id="c0a5f-109">Overview</span></span>

<span data-ttu-id="c0a5f-110">Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework per aggiungere le animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="c0a5f-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="c0a5f-111">Le animazioni possono anche essere modificate sul lato server</span><span class="sxs-lookup"><span data-stu-id="c0a5f-111">The animations may also be changed on the server-side</span></span>

## <a name="steps"></a><span data-ttu-id="c0a5f-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="c0a5f-112">Steps</span></span>

<span data-ttu-id="c0a5f-113">Prima di tutto, includere il `ScriptManager` nella pagina; quindi, la libreria AJAX di ASP.NET viene caricata, che consente di usare il Toolkit di controllo:</span><span class="sxs-lookup"><span data-stu-id="c0a5f-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample1.aspx)]

<span data-ttu-id="c0a5f-114">Verrà applicata l'animazione a un riquadro del testo che presenta un aspetto simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="c0a5f-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample2.aspx)]

<span data-ttu-id="c0a5f-115">Nella classe CSS associata per il pannello, definire un colore di sfondo nice e inoltre impostare una larghezza fissa per il pannello:</span><span class="sxs-lookup"><span data-stu-id="c0a5f-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](modifying-animations-from-the-server-side-cs/samples/sample3.css)]

<span data-ttu-id="c0a5f-116">Il resto del codice viene eseguito sul lato server e non viene utilizzato markup. al contrario, viene utilizzato codice per creare il `AnimationExtender` controllo:</span><span class="sxs-lookup"><span data-stu-id="c0a5f-116">The rest of the code runs on the server-side and does not use markup; instead, it uses code to create the `AnimationExtender` control:</span></span>

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample4.aspx)]

<span data-ttu-id="c0a5f-117">Tuttavia, il Toolkit di controllo attualmente non fornisce un accesso all'API per creare le animazioni singoli.</span><span class="sxs-lookup"><span data-stu-id="c0a5f-117">However, the Control Toolkit currently does not provide an API access to create the individual animations.</span></span> <span data-ttu-id="c0a5f-118">È tuttavia possibile impostare il `AnimationExtender`proprietà animazioni in una stringa contenente il markup XML utilizzato quando si assegnano le animazioni in modo dichiarativo.</span><span class="sxs-lookup"><span data-stu-id="c0a5f-118">It is however possible to set the `AnimationExtender`'s Animations property to a string containing the XML markup used when assigning the animations declaratively.</span></span> <span data-ttu-id="c0a5f-119">Per creare il codice XML che non deve contenere il `<Animations>` elemento è possibile usare il XML di .NET Framework supporta o, come illustrato nel codice seguente, è sufficiente fornire la stringa:</span><span class="sxs-lookup"><span data-stu-id="c0a5f-119">In order to create the XML which must not contain the `<Animations>` element you could use the .NET Framework's XML support or, as in the following code, just provide the string:</span></span>

[!code-css[Main](modifying-animations-from-the-server-side-cs/samples/sample5.css)]

<span data-ttu-id="c0a5f-120">Aggiungere infine il `AnimationExtender` controllo alla pagina corrente, all'interno di `<form runat="server">` elemento, assicurandosi che l'animazione è incluso e viene eseguito:</span><span class="sxs-lookup"><span data-stu-id="c0a5f-120">Finally, add the `AnimationExtender` control to the current page, within the `<form runat="server">` element, making sure that the animation is included and runs:</span></span>

[!code-html[Main](modifying-animations-from-the-server-side-cs/samples/sample6.html)]


<span data-ttu-id="c0a5f-121">[![L'animazione viene creata usando codice c# /VB C lato server](modifying-animations-from-the-server-side-cs/_static/image2.png)](modifying-animations-from-the-server-side-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="c0a5f-121">[![The animation is created using server-side C#/VB code](modifying-animations-from-the-server-side-cs/_static/image2.png)](modifying-animations-from-the-server-side-cs/_static/image1.png)</span></span>

<span data-ttu-id="c0a5f-122">L'animazione viene creato utilizzando il codice c# /VB sul lato server C ([fare clic per visualizzare l'immagine ingrandita](modifying-animations-from-the-server-side-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="c0a5f-122">The animation is created using server-side C#/VB code ([Click to view full-size image](modifying-animations-from-the-server-side-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="c0a5f-123">[Precedente](triggering-an-animation-in-another-control-cs.md)
> [Successivo](executing-animations-using-client-side-code-cs.md)</span><span class="sxs-lookup"><span data-stu-id="c0a5f-123">[Previous](triggering-an-animation-in-another-control-cs.md)
[Next](executing-animations-using-client-side-code-cs.md)</span></span>
