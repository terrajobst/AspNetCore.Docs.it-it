---
uid: web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-cs
title: Utilizzo del controllo dispositivo di scorrimento con Postback automatico (c#) | Documenti Microsoft
author: wenz
description: "Il controllo dispositivo di scorrimento in AJAX Control Toolkit fornisce un dispositivo di scorrimento con interfaccia grafica che può essere controllata utilizzando il mouse. È possibile rendere il dispositivo di scorrimento autopost..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 4d85e9fb-91e6-41f2-9c13-754549b19c27
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-cs
msc.type: authoredcontent
ms.openlocfilehash: f6291f4162b3069a6316a60b4b29f82f55121aac
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="using-the-slider-control-with-auto-postback-c"></a>Utilizzo del controllo dispositivo di scorrimento con Postback automatico (c#)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare codice](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1CS.pdf)

> Il controllo dispositivo di scorrimento in AJAX Control Toolkit fornisce un dispositivo di scorrimento con interfaccia grafica che può essere controllata utilizzando il mouse. È possibile eseguire un postback automatico il dispositivo di scorrimento di una volta cambia il relativo valore.


## <a name="overview"></a>Panoramica

Il controllo dispositivo di scorrimento in AJAX Control Toolkit fornisce un dispositivo di scorrimento con interfaccia grafica che può essere controllata utilizzando il mouse. È possibile eseguire un postback automatico il dispositivo di scorrimento di una volta cambia il relativo valore.

## <a name="steps"></a>Passaggi

Per rendere il dispositivo di scorrimento postback automaticamente dopo una modifica, entrambe le caselle di testo è necessario specificare l'attributo `AutoPostBack="true"`: la casella di testo che diventerà il dispositivo di scorrimento e la casella di testo che contiene la posizione del dispositivo di scorrimento. Di seguito è riportato il codice necessario per che:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample1.aspx)]

Il `SliderExtender` controllo di ASP.NET AJAX Control Toolkit assegna la funzionalità di dispositivo di scorrimento a due caselle di testo:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample2.aspx)]

Un elemento aggiuntivo dell'etichetta verrà successivamente utilizzato per informare l'utente di un postback:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample3.aspx)]

Infine, il `ScriptManager` controllo di ASP.NET AJAX carica il codice JavaScript necessari per il Toolkit di controllo utilizzare:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample4.aspx)]

A questo punto il dispositivo di scorrimento è postback; sul lato server, questo evento può essere rilevato ed elaborato:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample5.aspx)]


[![Spostando il cursore genera un postback](using-the-slider-control-with-auto-postback-cs/_static/image2.png)](using-the-slider-control-with-auto-postback-cs/_static/image1.png)

Spostando il cursore genera un postback ([fare clic per visualizzare l'immagine ingrandita](using-the-slider-control-with-auto-postback-cs/_static/image3.png))


[![Successivamente, la data di questa modifica viene scritta nell'etichetta](using-the-slider-control-with-auto-postback-cs/_static/image5.png)](using-the-slider-control-with-auto-postback-cs/_static/image4.png)

Successivamente, la data di questa modifica viene scritta nell'etichetta ([fare clic per visualizzare l'immagine ingrandita](using-the-slider-control-with-auto-postback-cs/_static/image6.png))

>[!div class="step-by-step"]
[Successivo](databinding-the-slider-control-cs.md)
