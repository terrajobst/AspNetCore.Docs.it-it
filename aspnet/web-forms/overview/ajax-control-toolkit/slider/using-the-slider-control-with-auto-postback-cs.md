---
uid: web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-cs
title: Utilizzo del controllo dispositivo di scorrimento con Postback automatico (c#) | Documenti Microsoft
author: wenz
description: Il controllo dispositivo di scorrimento in AJAX Control Toolkit fornisce un dispositivo di scorrimento con interfaccia grafica che può essere controllata utilizzando il mouse. È possibile rendere il dispositivo di scorrimento autopost...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 4d85e9fb-91e6-41f2-9c13-754549b19c27
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-cs
msc.type: authoredcontent
ms.openlocfilehash: e347d20c5c2ee48e6ed801e95459af6f0bcd2667
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="using-the-slider-control-with-auto-postback-c"></a><span data-ttu-id="78671-104">Utilizzo del controllo dispositivo di scorrimento con Postback automatico (c#)</span><span class="sxs-lookup"><span data-stu-id="78671-104">Using the Slider Control With Auto-Postback (C#)</span></span>
====================
<span data-ttu-id="78671-105">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="78671-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="78671-106">[Scaricare codice](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="78671-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1CS.pdf)</span></span>

> <span data-ttu-id="78671-107">Il controllo dispositivo di scorrimento in AJAX Control Toolkit fornisce un dispositivo di scorrimento con interfaccia grafica che può essere controllata utilizzando il mouse.</span><span class="sxs-lookup"><span data-stu-id="78671-107">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="78671-108">È possibile eseguire un postback automatico il dispositivo di scorrimento di una volta cambia il relativo valore.</span><span class="sxs-lookup"><span data-stu-id="78671-108">It is possible to make the slider autopostback once its value changes.</span></span>


## <a name="overview"></a><span data-ttu-id="78671-109">Panoramica</span><span class="sxs-lookup"><span data-stu-id="78671-109">Overview</span></span>

<span data-ttu-id="78671-110">Il controllo dispositivo di scorrimento in AJAX Control Toolkit fornisce un dispositivo di scorrimento con interfaccia grafica che può essere controllata utilizzando il mouse.</span><span class="sxs-lookup"><span data-stu-id="78671-110">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="78671-111">È possibile eseguire un postback automatico il dispositivo di scorrimento di una volta cambia il relativo valore.</span><span class="sxs-lookup"><span data-stu-id="78671-111">It is possible to make the slider autopostback once its value changes.</span></span>

## <a name="steps"></a><span data-ttu-id="78671-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="78671-112">Steps</span></span>

<span data-ttu-id="78671-113">Per rendere il dispositivo di scorrimento postback automaticamente dopo una modifica, entrambe le caselle di testo è necessario specificare l'attributo `AutoPostBack="true"`: la casella di testo che diventerà il dispositivo di scorrimento e la casella di testo che contiene la posizione del dispositivo di scorrimento.</span><span class="sxs-lookup"><span data-stu-id="78671-113">In order to make the slider automatically postback upon a change, both text boxes need the attribute `AutoPostBack="true"`: The text box that will become the slider itself, and the text box that holds the slider's position.</span></span> <span data-ttu-id="78671-114">Di seguito è riportato il codice necessario per che:</span><span class="sxs-lookup"><span data-stu-id="78671-114">Here is the required markup for that:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample1.aspx)]

<span data-ttu-id="78671-115">Il `SliderExtender` controllo di ASP.NET AJAX Control Toolkit assegna la funzionalità di dispositivo di scorrimento a due caselle di testo:</span><span class="sxs-lookup"><span data-stu-id="78671-115">The `SliderExtender` control from the ASP.NET AJAX Control Toolkit assigns the slider functionality to the two text boxes:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample2.aspx)]

<span data-ttu-id="78671-116">Un elemento aggiuntivo dell'etichetta verrà successivamente utilizzato per informare l'utente di un postback:</span><span class="sxs-lookup"><span data-stu-id="78671-116">An additional label element will later be used to inform the user of a postback:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample3.aspx)]

<span data-ttu-id="78671-117">Infine, il `ScriptManager` controllo di ASP.NET AJAX carica il codice JavaScript necessari per il Toolkit di controllo utilizzare:</span><span class="sxs-lookup"><span data-stu-id="78671-117">Finally, the `ScriptManager` control of ASP.NET AJAX loads the required JavaScript for the Control Toolkit to work:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample4.aspx)]

<span data-ttu-id="78671-118">A questo punto il dispositivo di scorrimento è postback; sul lato server, questo evento può essere rilevato ed elaborato:</span><span class="sxs-lookup"><span data-stu-id="78671-118">Now the slider is posting back; on the server-side, this event may be caught and acted upon:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample5.aspx)]


<span data-ttu-id="78671-119">[![Spostando il cursore genera un postback](using-the-slider-control-with-auto-postback-cs/_static/image2.png)](using-the-slider-control-with-auto-postback-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="78671-119">[![Moving the slider triggers a postback](using-the-slider-control-with-auto-postback-cs/_static/image2.png)](using-the-slider-control-with-auto-postback-cs/_static/image1.png)</span></span>

<span data-ttu-id="78671-120">Spostando il cursore genera un postback ([fare clic per visualizzare l'immagine ingrandita](using-the-slider-control-with-auto-postback-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="78671-120">Moving the slider triggers a postback ([Click to view full-size image](using-the-slider-control-with-auto-postback-cs/_static/image3.png))</span></span>


<span data-ttu-id="78671-121">[![Successivamente, la data di questa modifica viene scritta nell'etichetta](using-the-slider-control-with-auto-postback-cs/_static/image5.png)](using-the-slider-control-with-auto-postback-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="78671-121">[![Afterwards, the date of this change is written in the label](using-the-slider-control-with-auto-postback-cs/_static/image5.png)](using-the-slider-control-with-auto-postback-cs/_static/image4.png)</span></span>

<span data-ttu-id="78671-122">Successivamente, la data di questa modifica viene scritta nell'etichetta ([fare clic per visualizzare l'immagine ingrandita](using-the-slider-control-with-auto-postback-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="78671-122">Afterwards, the date of this change is written in the label ([Click to view full-size image](using-the-slider-control-with-auto-postback-cs/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="78671-123">avanti</span><span class="sxs-lookup"><span data-stu-id="78671-123">Next</span></span>](databinding-the-slider-control-cs.md)
