---
uid: web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-vb
title: Utilizzo del controllo dispositivo di scorrimento con Postback automatico (VB) | Documenti Microsoft
author: wenz
description: Il controllo dispositivo di scorrimento in AJAX Control Toolkit fornisce un dispositivo di scorrimento con interfaccia grafica che può essere controllata utilizzando il mouse. È possibile rendere il dispositivo di scorrimento autopost...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 41d1abba-97a5-4a45-9b44-d05624c19777
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-vb
msc.type: authoredcontent
ms.openlocfilehash: edb8fa13716c3c0beb7cf86dd3843caaec939483
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="using-the-slider-control-with-auto-postback-vb"></a><span data-ttu-id="39a04-104">Utilizzo del controllo dispositivo di scorrimento con Postback automatico (VB)</span><span class="sxs-lookup"><span data-stu-id="39a04-104">Using the Slider Control With Auto-Postback (VB)</span></span>
====================
<span data-ttu-id="39a04-105">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="39a04-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="39a04-106">[Scaricare codice](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="39a04-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1VB.pdf)</span></span>

> <span data-ttu-id="39a04-107">Il controllo dispositivo di scorrimento in AJAX Control Toolkit fornisce un dispositivo di scorrimento con interfaccia grafica che può essere controllata utilizzando il mouse.</span><span class="sxs-lookup"><span data-stu-id="39a04-107">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="39a04-108">È possibile eseguire un postback automatico il dispositivo di scorrimento di una volta cambia il relativo valore.</span><span class="sxs-lookup"><span data-stu-id="39a04-108">It is possible to make the slider autopostback once its value changes.</span></span>


## <a name="overview"></a><span data-ttu-id="39a04-109">Panoramica</span><span class="sxs-lookup"><span data-stu-id="39a04-109">Overview</span></span>

<span data-ttu-id="39a04-110">Il controllo dispositivo di scorrimento in AJAX Control Toolkit fornisce un dispositivo di scorrimento con interfaccia grafica che può essere controllata utilizzando il mouse.</span><span class="sxs-lookup"><span data-stu-id="39a04-110">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="39a04-111">È possibile eseguire un postback automatico il dispositivo di scorrimento di una volta cambia il relativo valore.</span><span class="sxs-lookup"><span data-stu-id="39a04-111">It is possible to make the slider autopostback once its value changes.</span></span>

## <a name="steps"></a><span data-ttu-id="39a04-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="39a04-112">Steps</span></span>

<span data-ttu-id="39a04-113">Per rendere il dispositivo di scorrimento postback automaticamente dopo una modifica, entrambe le caselle di testo è necessario specificare l'attributo `AutoPostBack="true"`: la casella di testo che diventerà il dispositivo di scorrimento e la casella di testo che contiene la posizione del dispositivo di scorrimento.</span><span class="sxs-lookup"><span data-stu-id="39a04-113">In order to make the slider automatically postback upon a change, both text boxes need the attribute `AutoPostBack="true"`: The text box that will become the slider itself, and the text box that holds the slider's position.</span></span> <span data-ttu-id="39a04-114">Di seguito è riportato il codice necessario per che:</span><span class="sxs-lookup"><span data-stu-id="39a04-114">Here is the required markup for that:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample1.aspx)]

<span data-ttu-id="39a04-115">Il `SliderExtender` controllo di ASP.NET AJAX Control Toolkit assegna la funzionalità di dispositivo di scorrimento a due caselle di testo:</span><span class="sxs-lookup"><span data-stu-id="39a04-115">The `SliderExtender` control from the ASP.NET AJAX Control Toolkit assigns the slider functionality to the two text boxes:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample2.aspx)]

<span data-ttu-id="39a04-116">Un elemento aggiuntivo dell'etichetta verrà successivamente utilizzato per informare l'utente di un postback:</span><span class="sxs-lookup"><span data-stu-id="39a04-116">An additional label element will later be used to inform the user of a postback:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample3.aspx)]

<span data-ttu-id="39a04-117">Infine, il `ScriptManager` controllo di ASP.NET AJAX carica il codice JavaScript necessari per il Toolkit di controllo utilizzare:</span><span class="sxs-lookup"><span data-stu-id="39a04-117">Finally, the `ScriptManager` control of ASP.NET AJAX loads the required JavaScript for the Control Toolkit to work:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample4.aspx)]

<span data-ttu-id="39a04-118">A questo punto il dispositivo di scorrimento è postback; sul lato server, questo evento può essere rilevato ed elaborato:</span><span class="sxs-lookup"><span data-stu-id="39a04-118">Now the slider is posting back; on the server-side, this event may be caught and acted upon:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample5.aspx)]


<span data-ttu-id="39a04-119">[![Spostando il cursore genera un postback](using-the-slider-control-with-auto-postback-vb/_static/image2.png)](using-the-slider-control-with-auto-postback-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="39a04-119">[![Moving the slider triggers a postback](using-the-slider-control-with-auto-postback-vb/_static/image2.png)](using-the-slider-control-with-auto-postback-vb/_static/image1.png)</span></span>

<span data-ttu-id="39a04-120">Spostando il cursore genera un postback ([fare clic per visualizzare l'immagine ingrandita](using-the-slider-control-with-auto-postback-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="39a04-120">Moving the slider triggers a postback ([Click to view full-size image](using-the-slider-control-with-auto-postback-vb/_static/image3.png))</span></span>


<span data-ttu-id="39a04-121">[![Successivamente, la data di questa modifica viene scritta nell'etichetta](using-the-slider-control-with-auto-postback-vb/_static/image5.png)](using-the-slider-control-with-auto-postback-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="39a04-121">[![Afterwards, the date of this change is written in the label](using-the-slider-control-with-auto-postback-vb/_static/image5.png)](using-the-slider-control-with-auto-postback-vb/_static/image4.png)</span></span>

<span data-ttu-id="39a04-122">Successivamente, la data di questa modifica viene scritta nell'etichetta ([fare clic per visualizzare l'immagine ingrandita](using-the-slider-control-with-auto-postback-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="39a04-122">Afterwards, the date of this change is written in the label ([Click to view full-size image](using-the-slider-control-with-auto-postback-vb/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="39a04-123">[Precedente](databinding-the-slider-control-cs.md)
> [Successivo](databinding-the-slider-control-vb.md)</span><span class="sxs-lookup"><span data-stu-id="39a04-123">[Previous](databinding-the-slider-control-cs.md)
[Next](databinding-the-slider-control-vb.md)</span></span>
