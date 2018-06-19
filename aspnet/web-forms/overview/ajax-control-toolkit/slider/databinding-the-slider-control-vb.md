---
uid: web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-vb
title: Associazione dati controllo dispositivo di scorrimento (VB) | Documenti Microsoft
author: wenz
description: Il controllo dispositivo di scorrimento in AJAX Control Toolkit fornisce un dispositivo di scorrimento con interfaccia grafica che può essere controllata utilizzando il mouse. È possibile associare la posizione corrente...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 4f3ba53f-d166-422d-b29c-403348057836
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 3ecd8598cd7fdcbbb4812e501bb30fa1f563a054
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879140"
---
<a name="databinding-the-slider-control-vb"></a>Associazione dati controllo dispositivo di scorrimento (VB)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare codice](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0VB.pdf)

> Il controllo dispositivo di scorrimento in AJAX Control Toolkit fornisce un dispositivo di scorrimento con interfaccia grafica che può essere controllata utilizzando il mouse. È possibile associare la posizione corrente del dispositivo di scorrimento a un altro controllo ASP.NET.


## <a name="overview"></a>Panoramica

Il controllo dispositivo di scorrimento in AJAX Control Toolkit fornisce un dispositivo di scorrimento con interfaccia grafica che può essere controllata utilizzando il mouse. È possibile associare la posizione corrente del dispositivo di scorrimento a un altro controllo ASP.NET.

## <a name="steps"></a>Passaggi

Per attivare le funzionalità di ASP.NET AJAX e il Toolkit di controllo, il `ScriptManager` controllo deve essere inserito in un punto qualsiasi della pagina (ma entro il `<form>` elemento):

[!code-aspx[Main](databinding-the-slider-control-vb/samples/sample1.aspx)]

Successivamente, aggiungere due `TextBox` controlli alla pagina. Uno verrà trasformato in un dispositivo di scorrimento con interfaccia grafica e l'altro conterrà la posizione del dispositivo di scorrimento.

[!code-aspx[Main](databinding-the-slider-control-vb/samples/sample2.aspx)]

Il passaggio successivo è già il passaggio finale. Il `SliderExtender` controllo di ASP.NET AJAX Control Toolkit rende un dispositivo di scorrimento dalla casella di testo prima e seconda casella di testo vengono aggiornati automaticamente quando il dispositivo di scorrimento posizionare le modifiche. In modo che funzioni correttamente, il `SliderExtender`del `TargetControlID` attributo deve essere impostato per l'ID della prima casella di testo; il `BoundControlID` attributo deve essere impostato per l'ID della seconda casella di testo.

[!code-aspx[Main](databinding-the-slider-control-vb/samples/sample3.aspx)]

Come è possibile visualizzare nel browser, l'associazione dati funziona in entrambe le direzioni: immettendo un nuovo valore nella casella di testo Aggiorna la posizione del dispositivo di scorrimento. Se si apportano seconda casella di testo di sola lettura, è possibile aggiungere un livello di protezione basso per il campo di testo in modo che sia più difficile per l'utente aggiornare manualmente il valore in questa posizione.


[![Dispositivo di scorrimento e casella di testo sono sincronizzati](databinding-the-slider-control-vb/_static/image2.png)](databinding-the-slider-control-vb/_static/image1.png)

Dispositivo di scorrimento e casella di testo sono sincronizzati ([fare clic per visualizzare l'immagine ingrandita](databinding-the-slider-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](using-the-slider-control-with-auto-postback-vb.md)
