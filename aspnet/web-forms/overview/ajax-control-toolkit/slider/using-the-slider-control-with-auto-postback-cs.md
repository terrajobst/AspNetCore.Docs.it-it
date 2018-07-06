---
uid: web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-cs
title: Uso del controllo Slider con Postback automatico (c#) | Microsoft Docs
author: wenz
description: Il controllo dispositivo di scorrimento in AJAX Control Toolkit fornisce un dispositivo di scorrimento con interfaccia grafica che può essere controllata usando il mouse. È possibile rendere il dispositivo di scorrimento autopost...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 4d85e9fb-91e6-41f2-9c13-754549b19c27
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-cs
msc.type: authoredcontent
ms.openlocfilehash: b5cb3c041f2a8a499d27cbcbc2f8975eedcac12e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37834151"
---
<a name="using-the-slider-control-with-auto-postback-c"></a>Uso del controllo Slider con Postback automatico (c#)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare il codice](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1CS.pdf)

> Il controllo dispositivo di scorrimento in AJAX Control Toolkit fornisce un dispositivo di scorrimento con interfaccia grafica che può essere controllata usando il mouse. È possibile eseguire autopostback il dispositivo di scorrimento di una volta cambia il relativo valore.


## <a name="overview"></a>Panoramica

Il controllo dispositivo di scorrimento in AJAX Control Toolkit fornisce un dispositivo di scorrimento con interfaccia grafica che può essere controllata usando il mouse. È possibile eseguire autopostback il dispositivo di scorrimento di una volta cambia il relativo valore.

## <a name="steps"></a>Passaggi

Per rendere il dispositivo di scorrimento postback automaticamente dopo una modifica, entrambe le caselle di testo è necessario specificare l'attributo `AutoPostBack="true"`: la casella di testo che diventerà il dispositivo di scorrimento e la casella di testo che contiene la posizione del dispositivo di scorrimento. Ecco il markup necessario per che:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample1.aspx)]

Il `SliderExtender` controllo da ASP.NET AJAX Control Toolkit assegna la funzionalità di dispositivo di scorrimento a due caselle di testo:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample2.aspx)]

Un elemento aggiuntivo dell'etichetta verrà successivamente utilizzato per informare l'utente di un postback:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample3.aspx)]

Infine, il `ScriptManager` viene caricato il codice JavaScript richiesto per il Toolkit di controllo utilizzare controllo di ASP.NET AJAX:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample4.aspx)]

A questo punto il dispositivo di scorrimento è postback al server; sul lato server, questo evento può essere intercettato e intervenire su:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample5.aspx)]


[![Spostare il dispositivo di scorrimento attiva un postback](using-the-slider-control-with-auto-postback-cs/_static/image2.png)](using-the-slider-control-with-auto-postback-cs/_static/image1.png)

Spostare il dispositivo di scorrimento di un postback attiva ([fare clic per visualizzare l'immagine con dimensioni normali](using-the-slider-control-with-auto-postback-cs/_static/image3.png))


[![In un secondo momento, la data di questa modifica viene scritta nell'etichetta](using-the-slider-control-with-auto-postback-cs/_static/image5.png)](using-the-slider-control-with-auto-postback-cs/_static/image4.png)

In un secondo momento, la data di questa modifica viene scritta nell'etichetta ([fare clic per visualizzare l'immagine con dimensioni normali](using-the-slider-control-with-auto-postback-cs/_static/image6.png))

> [!div class="step-by-step"]
> [avanti](databinding-the-slider-control-cs.md)
