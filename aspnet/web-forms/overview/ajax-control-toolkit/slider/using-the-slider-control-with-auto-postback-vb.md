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
<a name="using-the-slider-control-with-auto-postback-vb"></a>Utilizzo del controllo dispositivo di scorrimento con Postback automatico (VB)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare codice](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1VB.pdf)

> Il controllo dispositivo di scorrimento in AJAX Control Toolkit fornisce un dispositivo di scorrimento con interfaccia grafica che può essere controllata utilizzando il mouse. È possibile eseguire un postback automatico il dispositivo di scorrimento di una volta cambia il relativo valore.


## <a name="overview"></a>Panoramica

Il controllo dispositivo di scorrimento in AJAX Control Toolkit fornisce un dispositivo di scorrimento con interfaccia grafica che può essere controllata utilizzando il mouse. È possibile eseguire un postback automatico il dispositivo di scorrimento di una volta cambia il relativo valore.

## <a name="steps"></a>Passaggi

Per rendere il dispositivo di scorrimento postback automaticamente dopo una modifica, entrambe le caselle di testo è necessario specificare l'attributo `AutoPostBack="true"`: la casella di testo che diventerà il dispositivo di scorrimento e la casella di testo che contiene la posizione del dispositivo di scorrimento. Di seguito è riportato il codice necessario per che:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample1.aspx)]

Il `SliderExtender` controllo di ASP.NET AJAX Control Toolkit assegna la funzionalità di dispositivo di scorrimento a due caselle di testo:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample2.aspx)]

Un elemento aggiuntivo dell'etichetta verrà successivamente utilizzato per informare l'utente di un postback:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample3.aspx)]

Infine, il `ScriptManager` controllo di ASP.NET AJAX carica il codice JavaScript necessari per il Toolkit di controllo utilizzare:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample4.aspx)]

A questo punto il dispositivo di scorrimento è postback; sul lato server, questo evento può essere rilevato ed elaborato:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample5.aspx)]


[![Spostando il cursore genera un postback](using-the-slider-control-with-auto-postback-vb/_static/image2.png)](using-the-slider-control-with-auto-postback-vb/_static/image1.png)

Spostando il cursore genera un postback ([fare clic per visualizzare l'immagine ingrandita](using-the-slider-control-with-auto-postback-vb/_static/image3.png))


[![Successivamente, la data di questa modifica viene scritta nell'etichetta](using-the-slider-control-with-auto-postback-vb/_static/image5.png)](using-the-slider-control-with-auto-postback-vb/_static/image4.png)

Successivamente, la data di questa modifica viene scritta nell'etichetta ([fare clic per visualizzare l'immagine ingrandita](using-the-slider-control-with-auto-postback-vb/_static/image6.png))

> [!div class="step-by-step"]
> [Precedente](databinding-the-slider-control-cs.md)
> [Successivo](databinding-the-slider-control-vb.md)
