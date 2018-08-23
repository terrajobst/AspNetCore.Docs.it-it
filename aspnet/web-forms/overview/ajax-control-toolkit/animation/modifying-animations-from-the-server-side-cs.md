---
uid: web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-cs
title: Modifica delle animazioni dal lato Server (c#) | Microsoft Docs
author: wenz
description: Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo. Le animazioni possono inoltre...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: b0abec39-a1c9-422d-ba9a-ef16f6185af8
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-cs
msc.type: authoredcontent
ms.openlocfilehash: ac2c2b1e9cfceba7f818f3f2dcbd719e94bea83e
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837158"
---
<a name="modifying-animations-from-the-server-side-c"></a><span data-ttu-id="6579c-104">Modifica delle animazioni dal lato Server (c#)</span><span class="sxs-lookup"><span data-stu-id="6579c-104">Modifying Animations From The Server Side (C#)</span></span>
====================
<span data-ttu-id="6579c-105">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="6579c-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="6579c-106">[Scaricare il codice](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="6579c-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9CS.pdf)</span></span>

> <span data-ttu-id="6579c-107">Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="6579c-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="6579c-108">Le animazioni possono anche essere modificate sul lato server</span><span class="sxs-lookup"><span data-stu-id="6579c-108">The animations may also be changed on the server-side</span></span>


## <a name="overview"></a><span data-ttu-id="6579c-109">Panoramica</span><span class="sxs-lookup"><span data-stu-id="6579c-109">Overview</span></span>

<span data-ttu-id="6579c-110">Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="6579c-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="6579c-111">Le animazioni possono anche essere modificate sul lato server</span><span class="sxs-lookup"><span data-stu-id="6579c-111">The animations may also be changed on the server-side</span></span>

## <a name="steps"></a><span data-ttu-id="6579c-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="6579c-112">Steps</span></span>

<span data-ttu-id="6579c-113">Prima di tutto, includere il `ScriptManager` nella pagina; quindi, la libreria ASP.NET AJAX viene caricata, rendendo possibile usare il Toolkit di controllo:</span><span class="sxs-lookup"><span data-stu-id="6579c-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample1.aspx)]

<span data-ttu-id="6579c-114">Verrà applicata l'animazione a un pannello del testo che presenta un aspetto simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="6579c-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample2.aspx)]

<span data-ttu-id="6579c-115">Nella classe CSS associata per il pannello, definire un colore di sfondo interessante e anche impostare una larghezza fissa per il pannello:</span><span class="sxs-lookup"><span data-stu-id="6579c-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](modifying-animations-from-the-server-side-cs/samples/sample3.css)]

<span data-ttu-id="6579c-116">Il resto del codice viene eseguito sul lato server e non viene utilizzato markup. Usa invece codice per creare il `AnimationExtender` controllo:</span><span class="sxs-lookup"><span data-stu-id="6579c-116">The rest of the code runs on the server-side and does not use markup; instead, it uses code to create the `AnimationExtender` control:</span></span>

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample4.aspx)]

<span data-ttu-id="6579c-117">Tuttavia, il Toolkit di controllo attualmente non fornisce un accesso all'API per creare le animazioni singoli.</span><span class="sxs-lookup"><span data-stu-id="6579c-117">However, the Control Toolkit currently does not provide an API access to create the individual animations.</span></span> <span data-ttu-id="6579c-118">È tuttavia possibile impostare il `AnimationExtender`proprietà Animations in una stringa che contiene il tag XML utilizzato quando si assegnano le animazioni in modo dichiarativo.</span><span class="sxs-lookup"><span data-stu-id="6579c-118">It is however possible to set the `AnimationExtender`'s Animations property to a string containing the XML markup used when assigning the animations declaratively.</span></span> <span data-ttu-id="6579c-119">Per creare il codice XML che non deve contenere il `<Animations>` elemento è possibile usare il XML di .NET Framework supportano o, come nel codice seguente, è sufficiente fornire la stringa:</span><span class="sxs-lookup"><span data-stu-id="6579c-119">In order to create the XML which must not contain the `<Animations>` element you could use the .NET Framework's XML support or, as in the following code, just provide the string:</span></span>

[!code-css[Main](modifying-animations-from-the-server-side-cs/samples/sample5.css)]

<span data-ttu-id="6579c-120">Infine, aggiungere il `AnimationExtender` controllo alla pagina corrente, all'interno di `<form runat="server">` elemento, assicurandosi che l'animazione è inclusa e viene eseguito:</span><span class="sxs-lookup"><span data-stu-id="6579c-120">Finally, add the `AnimationExtender` control to the current page, within the `<form runat="server">` element, making sure that the animation is included and runs:</span></span>

[!code-html[Main](modifying-animations-from-the-server-side-cs/samples/sample6.html)]


<span data-ttu-id="6579c-121">[![L'animazione viene creata usando codice c# /VB C sul lato server](modifying-animations-from-the-server-side-cs/_static/image2.png)](modifying-animations-from-the-server-side-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="6579c-121">[![The animation is created using server-side C#/VB code](modifying-animations-from-the-server-side-cs/_static/image2.png)](modifying-animations-from-the-server-side-cs/_static/image1.png)</span></span>

<span data-ttu-id="6579c-122">L'animazione viene creata usando codice c# /VB C sul lato server ([fare clic per visualizzare l'immagine con dimensioni normali](modifying-animations-from-the-server-side-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="6579c-122">The animation is created using server-side C#/VB code ([Click to view full-size image](modifying-animations-from-the-server-side-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="6579c-123">[Precedente](triggering-an-animation-in-another-control-cs.md)
> [Successivo](executing-animations-using-client-side-code-cs.md)</span><span class="sxs-lookup"><span data-stu-id="6579c-123">[Previous](triggering-an-animation-in-another-control-cs.md)
[Next](executing-animations-using-client-side-code-cs.md)</span></span>
