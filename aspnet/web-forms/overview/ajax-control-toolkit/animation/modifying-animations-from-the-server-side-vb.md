---
uid: web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-vb
title: Modifica le animazioni dal lato Server (VB) | Documenti Microsoft
author: wenz
description: "Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework per aggiungere le animazioni a un controllo. Le animazioni potrebbero inoltre..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: addcf4aa-340a-460b-9c64-506424a1f725
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-vb
msc.type: authoredcontent
ms.openlocfilehash: c5b23cce529be24157a8a3f9136de7ad7bafc1ea
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="modifying-animations-from-the-server-side-vb"></a><span data-ttu-id="86e9e-104">Modifica le animazioni dal lato Server (VB)</span><span class="sxs-lookup"><span data-stu-id="86e9e-104">Modifying Animations From The Server Side (VB)</span></span>
====================
<span data-ttu-id="86e9e-105">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="86e9e-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="86e9e-106">[Scaricare codice](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="86e9e-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9VB.pdf)</span></span>

> <span data-ttu-id="86e9e-107">Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework per aggiungere le animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="86e9e-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="86e9e-108">Le animazioni possono anche essere modificate sul lato server</span><span class="sxs-lookup"><span data-stu-id="86e9e-108">The animations may also be changed on the server-side</span></span>


## <a name="overview"></a><span data-ttu-id="86e9e-109">Panoramica</span><span class="sxs-lookup"><span data-stu-id="86e9e-109">Overview</span></span>

<span data-ttu-id="86e9e-110">Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework per aggiungere le animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="86e9e-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="86e9e-111">Le animazioni possono anche essere modificate sul lato server</span><span class="sxs-lookup"><span data-stu-id="86e9e-111">The animations may also be changed on the server-side</span></span>

## <a name="steps"></a><span data-ttu-id="86e9e-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="86e9e-112">Steps</span></span>

<span data-ttu-id="86e9e-113">Prima di tutto, includere il `ScriptManager` nella pagina; quindi, la libreria AJAX di ASP.NET viene caricata, che consente di usare il Toolkit di controllo:</span><span class="sxs-lookup"><span data-stu-id="86e9e-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample1.aspx)]

<span data-ttu-id="86e9e-114">Verrà applicata l'animazione a un riquadro del testo che presenta un aspetto simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="86e9e-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample2.aspx)]

<span data-ttu-id="86e9e-115">Nella classe CSS associata per il pannello, definire un colore di sfondo nice e inoltre impostare una larghezza fissa per il pannello:</span><span class="sxs-lookup"><span data-stu-id="86e9e-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](modifying-animations-from-the-server-side-vb/samples/sample3.css)]

<span data-ttu-id="86e9e-116">Il resto del codice viene eseguito sul lato server e non viene utilizzato markup. al contrario, viene utilizzato codice per creare il `AnimationExtender` controllo:</span><span class="sxs-lookup"><span data-stu-id="86e9e-116">The rest of the code runs on the server-side and does not use markup; instead, it uses code to create the `AnimationExtender` control:</span></span>

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample4.aspx)]

<span data-ttu-id="86e9e-117">Tuttavia, il Toolkit di controllo attualmente non fornisce un accesso all'API per creare le animazioni singoli.</span><span class="sxs-lookup"><span data-stu-id="86e9e-117">However, the Control Toolkit currently does not provide an API access to create the individual animations.</span></span> <span data-ttu-id="86e9e-118">È tuttavia possibile impostare il `AnimationExtender`proprietà animazioni in una stringa contenente il markup XML utilizzato quando si assegnano le animazioni in modo dichiarativo.</span><span class="sxs-lookup"><span data-stu-id="86e9e-118">It is however possible to set the `AnimationExtender`'s Animations property to a string containing the XML markup used when assigning the animations declaratively.</span></span> <span data-ttu-id="86e9e-119">Per creare il codice XML che non deve contenere il `<Animations>` elemento è possibile usare il XML di .NET Framework supporta o, come illustrato nel codice seguente, è sufficiente fornire la stringa:</span><span class="sxs-lookup"><span data-stu-id="86e9e-119">In order to create the XML which must not contain the `<Animations>` element you could use the .NET Framework's XML support or, as in the following code, just provide the string:</span></span>

[!code-vb[Main](modifying-animations-from-the-server-side-vb/samples/sample5.vb)]

<span data-ttu-id="86e9e-120">Aggiungere infine il `AnimationExtender` controllo alla pagina corrente, all'interno di `<form runat="server">` elemento, assicurandosi che l'animazione è incluso e viene eseguito:</span><span class="sxs-lookup"><span data-stu-id="86e9e-120">Finally, add the `AnimationExtender` control to the current page, within the `<form runat="server">` element, making sure that the animation is included and runs:</span></span>

[!code-vb[Main](modifying-animations-from-the-server-side-vb/samples/sample6.vb)]


<span data-ttu-id="86e9e-121">[![L'animazione viene creato utilizzando il codice lato server C c# /VB](modifying-animations-from-the-server-side-vb/_static/image2.png)](modifying-animations-from-the-server-side-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="86e9e-121">[![The animation is created using server-side C#/VB code](modifying-animations-from-the-server-side-vb/_static/image2.png)](modifying-animations-from-the-server-side-vb/_static/image1.png)</span></span>

<span data-ttu-id="86e9e-122">L'animazione viene creato utilizzando il codice c# /VB sul lato server C ([fare clic per visualizzare l'immagine ingrandita](modifying-animations-from-the-server-side-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="86e9e-122">The animation is created using server-side C#/VB code ([Click to view full-size image](modifying-animations-from-the-server-side-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="86e9e-123">[Precedente](triggering-an-animation-in-another-control-vb.md)
[Successivo](executing-animations-using-client-side-code-vb.md)</span><span class="sxs-lookup"><span data-stu-id="86e9e-123">[Previous](triggering-an-animation-in-another-control-vb.md)
[Next](executing-animations-using-client-side-code-vb.md)</span></span>
