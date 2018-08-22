---
uid: web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-vb
title: Uso del controllo Slider con Postback automatico (VB) | Microsoft Docs
author: wenz
description: Il controllo dispositivo di scorrimento in AJAX Control Toolkit fornisce un dispositivo di scorrimento con interfaccia grafica che può essere controllata usando il mouse. È possibile rendere il dispositivo di scorrimento autopost...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 41d1abba-97a5-4a45-9b44-d05624c19777
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-vb
msc.type: authoredcontent
ms.openlocfilehash: 403e8fa6e54d84eb091769cbdb6ef1003ad4245c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826123"
---
<a name="using-the-slider-control-with-auto-postback-vb"></a>Uso del controllo Slider con Postback automatico (VB)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare il codice](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1VB.pdf)

> Il controllo dispositivo di scorrimento in AJAX Control Toolkit fornisce un dispositivo di scorrimento con interfaccia grafica che può essere controllata usando il mouse. È possibile eseguire autopostback il dispositivo di scorrimento di una volta cambia il relativo valore.


## <a name="overview"></a>Panoramica

Il controllo dispositivo di scorrimento in AJAX Control Toolkit fornisce un dispositivo di scorrimento con interfaccia grafica che può essere controllata usando il mouse. È possibile eseguire autopostback il dispositivo di scorrimento di una volta cambia il relativo valore.

## <a name="steps"></a>Passaggi

Per rendere il dispositivo di scorrimento postback automaticamente dopo una modifica, entrambe le caselle di testo è necessario specificare l'attributo `AutoPostBack="true"`: la casella di testo che diventerà il dispositivo di scorrimento e la casella di testo che contiene la posizione del dispositivo di scorrimento. Ecco il markup necessario per che:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample1.aspx)]

Il `SliderExtender` controllo da ASP.NET AJAX Control Toolkit assegna la funzionalità di dispositivo di scorrimento a due caselle di testo:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample2.aspx)]

Un elemento aggiuntivo dell'etichetta verrà successivamente utilizzato per informare l'utente di un postback:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample3.aspx)]

Infine, il `ScriptManager` viene caricato il codice JavaScript richiesto per il Toolkit di controllo utilizzare controllo di ASP.NET AJAX:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample4.aspx)]

A questo punto il dispositivo di scorrimento è postback al server; sul lato server, questo evento può essere intercettato e intervenire su:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-vb/samples/sample5.aspx)]


[![Spostare il dispositivo di scorrimento attiva un postback](using-the-slider-control-with-auto-postback-vb/_static/image2.png)](using-the-slider-control-with-auto-postback-vb/_static/image1.png)

Spostare il dispositivo di scorrimento di un postback attiva ([fare clic per visualizzare l'immagine con dimensioni normali](using-the-slider-control-with-auto-postback-vb/_static/image3.png))


[![In un secondo momento, la data di questa modifica viene scritta nell'etichetta](using-the-slider-control-with-auto-postback-vb/_static/image5.png)](using-the-slider-control-with-auto-postback-vb/_static/image4.png)

In un secondo momento, la data di questa modifica viene scritta nell'etichetta ([fare clic per visualizzare l'immagine con dimensioni normali](using-the-slider-control-with-auto-postback-vb/_static/image6.png))

> [!div class="step-by-step"]
> [Precedente](databinding-the-slider-control-cs.md)
> [Successivo](databinding-the-slider-control-vb.md)
