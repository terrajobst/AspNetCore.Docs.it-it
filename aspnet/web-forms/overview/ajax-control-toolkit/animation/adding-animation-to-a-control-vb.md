---
uid: web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-vb
title: Aggiunta di animazione a un controllo (VB) | Documenti Microsoft
author: wenz
description: Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework per aggiungere le animazioni a un controllo. Questa esercitazione viene illustrato come...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: c120187e-963e-4439-bb85-32771bc7f1f4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 3da98e478c45213875b3829e51351d03571a05b8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="adding-animation-to-a-control-vb"></a>Aggiunta di animazione a un controllo (VB)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare codice](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1VB.pdf)

> Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework per aggiungere le animazioni a un controllo. In questa esercitazione viene illustrato come impostare tali un'animazione.


## <a name="overview"></a>Panoramica

Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework per aggiungere le animazioni a un controllo. In questa esercitazione viene illustrato come impostare tali un'animazione.

## <a name="steps"></a>Passaggi

Il primo passaggio consiste nel modo consueto per includere il `ScriptManager` nella pagina in modo che la libreria ASP.NET AJAX è caricata e può essere utilizzato il Toolkit di controllo:

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample1.aspx)]

L'animazione in questo scenario verrà applicata a un riquadro del testo che presenta un aspetto simile al seguente:

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample2.aspx)]

La classe CSS associata per il pannello definisce un colore di sfondo e una larghezza:

[!code-css[Main](adding-animation-to-a-control-vb/samples/sample3.css)]

Successivamente, è necessario il `AnimationExtender`. Dopo aver fornito un `ID` e le consuete `runat="server"`, `TargetControlID` attributo deve essere impostato per il controllo per aggiungere un'animazione in questo caso, il pannello:

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample4.aspx)]

L'intera animazione viene applicata in modo dichiarativo, utilizzare la sintassi XML, purtroppo attualmente non è completamente supportata da IntelliSense di Visual Studio. Il nodo radice è `<Animations>;` all'interno di questo nodo sono consentiti diversi eventi che determinano quando animazione adottino sul posto:

- `OnClick` (clic del mouse)
- `OnHoverOut` (quando il puntatore del mouse esce da un controllo)
- `OnHoverOver` (quando il mouse viene spostato su un controllo, l'arresto di `OnHoverOut` animazione)
- `OnLoad` (quando il caricamento della pagina)
- `OnMouseOut` (quando il puntatore del mouse esce da un controllo)
- `OnMouseOver` (quando il mouse viene spostato su un controllo, non arrestare il `OnMouseOut` animazione)

Il framework viene fornito con un insieme di animazioni, ognuno rappresentato da un proprio elemento XML. Di seguito è riportata una selezione:

- `<Color>` (modifica di un colore)
- `<FadeIn>` (la dissolvenza in entrata)
- `<FadeOut>` (dissolvenza in uscita)
- `<Property>` (modifica delle proprietà di un controllo)
- `<Pulse>` (pulsating)
- `<Resize>` (modifica delle dimensioni)
- `<Scale>` (in modo proporzionale modifica delle dimensioni)

In questo esempio, il pannello è dissolvenza in uscita. L'animazione adottano 1,5 secondi (`Duration` attributi), la visualizzazione 24 fotogrammi (passaggi animazione) al secondo (`Fps` attributs). Ecco il codice completo per il `AnimationExtender` controllo:

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample5.aspx)]

Quando si esegue questo script, il pannello viene visualizzato e dissolvenza in 1,5 secondi.


[![Il pannello è dissolvenza in uscita](adding-animation-to-a-control-vb/_static/image2.png)](adding-animation-to-a-control-vb/_static/image1.png)

Il pannello è dissolvenza in uscita ([fare clic per visualizzare l'immagine ingrandita](adding-animation-to-a-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](dynamically-controlling-updatepanel-animations-cs.md)
> [Successivo](executing-several-animations-at-the-same-time-vb.md)
