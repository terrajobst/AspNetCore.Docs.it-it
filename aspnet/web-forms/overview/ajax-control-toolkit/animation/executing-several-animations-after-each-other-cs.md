---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-cs
title: L'esecuzione di numerose animazioni dopo l'altro (c#) | Documenti Microsoft
author: wenz
description: Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework per aggiungere le animazioni a un controllo. Consente di eseguire severa...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 7dc02b18-2b5d-4844-b7c5-cbd818477163
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-cs
msc.type: authoredcontent
ms.openlocfilehash: 836f0bba890a03e74ae62c2df029b7525b34275c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="executing-several-animations-after-each-other-c"></a>L'esecuzione di numerose animazioni dopo l'altro (c#)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare codice](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3CS.pdf)

> Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework per aggiungere le animazioni a un controllo. Consente di eseguire numerose animazioni uno dopo l'altro.


Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework per aggiungere le animazioni a un controllo. Consente di eseguire numerose animazioni uno dopo l'altro.

## <a name="steps"></a>Passaggi

Prima di tutto, includere il `ScriptManager` nella pagina; quindi, la libreria AJAX di ASP.NET viene caricata, che consente di usare il Toolkit di controllo:

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample1.aspx)]

Verrà applicata l'animazione a un riquadro del testo che presenta un aspetto simile al seguente:

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample2.aspx)]

Nella classe CSS associata per il pannello, definire un colore di sfondo nice e inoltre impostare una larghezza fissa per il pannello:

[!code-css[Main](executing-several-animations-after-each-other-cs/samples/sample3.css)]

Quindi, aggiungere il `AnimationExtender` alla pagina, fornendo un' `ID`, il `TargetControlID` attributo e l'obbligatoria `runat="server":`

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample4.aspx)]

All'interno di `<Animations>` nodo, utilizzare `<OnLoad>` per eseguire le animazioni dopo la pagina è stata caricata completamente. In genere, `<OnLoad>` accetta solo un'animazione. Il framework di animazione consente di unire più animazioni in uno solo tramite il `<Sequence>` elemento. Tutte le animazioni all'interno di `<Sequence>` vengono eseguite una dopo l'altro. Ecco l'un markup possibili per il `AnimationExtender` controllo, effettua prima il pannello più ampio e quindi diminuire l'altezza:

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample5.aspx)]

Quando si esegue questo script, il pannello riceve più larghi e quindi più piccoli.


[![Innanzitutto viene aumentata la larghezza](executing-several-animations-after-each-other-cs/_static/image2.png)](executing-several-animations-after-each-other-cs/_static/image1.png)

Innanzitutto viene aumentata la larghezza ([fare clic per visualizzare l'immagine ingrandita](executing-several-animations-after-each-other-cs/_static/image3.png))


[![Quindi viene diminuita l'altezza](executing-several-animations-after-each-other-cs/_static/image5.png)](executing-several-animations-after-each-other-cs/_static/image4.png)

Quindi viene ridotto l'altezza ([fare clic per visualizzare l'immagine ingrandita](executing-several-animations-after-each-other-cs/_static/image6.png))

> [!div class="step-by-step"]
> [Precedente](executing-several-animations-at-the-same-time-cs.md)
> [Successivo](animation-depending-on-a-condition-cs.md)
