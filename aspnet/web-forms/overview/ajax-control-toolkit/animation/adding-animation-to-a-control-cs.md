---
uid: web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-cs
title: Aggiunta di animazione a un controllo (c#) | Microsoft Docs
author: wenz
description: Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo. Questa esercitazione viene illustrato come...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 0f1fc1f5-9dbd-44e7-931e-387d42f0342b
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 80c982e041af2d0b9ee789665613ced0311dfcc9
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37396572"
---
<a name="adding-animation-to-a-control-c"></a>Aggiunta di animazione a un controllo (c#)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare il codice](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1CS.pdf)

> Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo. Questa esercitazione illustra come configurare un'animazione di questo tipo.


## <a name="overview"></a>Panoramica

Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo. Questa esercitazione illustra come configurare un'animazione di questo tipo.

## <a name="steps"></a>Passaggi

Il primo passaggio consiste nel modo consueto per includere il `ScriptManager` nella pagina in modo che la libreria ASP.NET AJAX viene caricata e il Toolkit di controllo possono essere usato:

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample1.aspx)]

L'animazione in questo scenario verrà applicata a un pannello del testo che presenta un aspetto simile al seguente:

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample2.aspx)]

La classe CSS associata per il pannello definisce un colore di sfondo e una larghezza:

[!code-css[Main](adding-animation-to-a-control-cs/samples/sample3.css)]

Successivamente, è necessario il `AnimationExtender`. Dopo aver specificato un' `ID` e il consueto `runat="server"`, il `TargetControlID` attributo deve essere impostato per il controllo per aggiungere un'animazione in questo caso, il pannello:

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample4.aspx)]

L'intera animazione viene applicata in modo dichiarativo, tramite una sintassi XML, sfortunatamente non è attualmente completamente supportata da IntelliSense di Visual Studio. Il nodo radice è `<Animations>;` all'interno di questo nodo, sono consentiti diversi eventi che determinano quando le animazioni adottino sul posto:

- `OnClick` (fare clic del mouse)
- `OnHoverOut` (quando il mouse esce da un controllo)
- `OnHoverOver` (quando il puntatore del mouse viene posizionato su un controllo, l'arresto di `OnHoverOut` animazione)
- `OnLoad` (quando il caricamento della pagina)
- `OnMouseOut` (quando il mouse esce da un controllo)
- `OnMouseOver` (quando il puntatore del mouse viene posizionato su un controllo, non l'arresto di `OnMouseOut` animazione)

Il framework include un set di animazioni, ognuno rappresentato da un proprio elemento XML. Ecco una selezione:

- `<Color>` (modifica un colore)
- `<FadeIn>` (la dissolvenza in entrata)
- `<FadeOut>` (dissolvenza)
- `<Property>` (modifica una proprietà del controllo)
- `<Pulse>` (pulsating)
- `<Resize>` (la dimensione a modifica)
- `<Scale>` (la dimensione a modifica in modo proporzionale)

In questo esempio, il pannello verrà dissolvenza. L'animazione adottano 1,5 secondi (`Duration` attributo), la visualizzazione (passaggi animazione) a 24 fotogrammi al secondo (`Fps` attributs). Ecco il markup completo per il `AnimationExtender` controllo:

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample5.aspx)]

Quando si esegue questo script, il pannello viene visualizzato e dissolve in 1,5 secondi.


[![Il pannello è dissolvenza in uscita](adding-animation-to-a-control-cs/_static/image2.png)](adding-animation-to-a-control-cs/_static/image1.png)

Il pannello è dissolvenza in uscita ([fare clic per visualizzare l'immagine con dimensioni normali](adding-animation-to-a-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [avanti](executing-several-animations-at-the-same-time-cs.md)
