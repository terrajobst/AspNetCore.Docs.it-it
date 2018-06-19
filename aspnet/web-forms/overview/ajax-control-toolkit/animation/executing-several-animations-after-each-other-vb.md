---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-vb
title: L'esecuzione di numerose animazioni dopo l'altro (VB) | Documenti Microsoft
author: wenz
description: Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework per aggiungere le animazioni a un controllo. Consente di eseguire severa...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 21ece509-79cc-4d9d-892d-7b6e9c4d3502
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-vb
msc.type: authoredcontent
ms.openlocfilehash: 700946b9f32c5ed2dcb8586e7c0e84d2238ff103
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873758"
---
<a name="executing-several-animations-after-each-other-vb"></a>L'esecuzione di numerose animazioni dopo l'altro (Visual Basic)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare codice](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3VB.pdf)

> Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework per aggiungere le animazioni a un controllo. Consente di eseguire numerose animazioni uno dopo l'altro.


## <a name="overview"></a>Panoramica

Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework per aggiungere le animazioni a un controllo. Consente di eseguire numerose animazioni uno dopo l'altro.

## <a name="steps"></a>Passaggi

Prima di tutto, includere il `ScriptManager` nella pagina; quindi, la libreria AJAX di ASP.NET viene caricata, che consente di usare il Toolkit di controllo:

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample1.aspx)]

Verrà applicata l'animazione a un riquadro del testo che presenta un aspetto simile al seguente:

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample2.aspx)]

Nella classe CSS associata per il pannello, definire un colore di sfondo nice e inoltre impostare una larghezza fissa per il pannello:

[!code-css[Main](executing-several-animations-after-each-other-vb/samples/sample3.css)]

Quindi, aggiungere il `AnimationExtender` alla pagina, fornendo un' `ID`, il `TargetControlID` attributo e l'obbligatoria `runat="server":`

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample4.aspx)]

All'interno di `<Animations>` nodo, utilizzare `<OnLoad>` per eseguire le animazioni dopo la pagina è stata caricata completamente. In genere, `<OnLoad>` accetta solo un'animazione. Il framework di animazione consente di unire più animazioni in uno solo tramite il `<Sequence>` elemento. Tutte le animazioni all'interno di `<Sequence>` vengono eseguite una dopo l'altro. Ecco l'un markup possibili per il `AnimationExtender` controllo, effettua prima il pannello più ampio e quindi diminuire l'altezza:

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample5.aspx)]

Quando si esegue questo script, il pannello riceve più larghi e quindi più piccoli.


[![Innanzitutto viene aumentata la larghezza](executing-several-animations-after-each-other-vb/_static/image2.png)](executing-several-animations-after-each-other-vb/_static/image1.png)

Innanzitutto viene aumentata la larghezza ([fare clic per visualizzare l'immagine ingrandita](executing-several-animations-after-each-other-vb/_static/image3.png))


[![Quindi viene diminuita l'altezza](executing-several-animations-after-each-other-vb/_static/image5.png)](executing-several-animations-after-each-other-vb/_static/image4.png)

Quindi viene ridotto l'altezza ([fare clic per visualizzare l'immagine ingrandita](executing-several-animations-after-each-other-vb/_static/image6.png))

> [!div class="step-by-step"]
> [Precedente](executing-several-animations-at-the-same-time-vb.md)
> [Successivo](animation-depending-on-a-condition-vb.md)
