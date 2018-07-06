---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-vb
title: Esecuzione di diverse animazioni una dopo l'altra (VB) | Microsoft Docs
author: wenz
description: Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo. Consente di eseguire severa...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 21ece509-79cc-4d9d-892d-7b6e9c4d3502
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-vb
msc.type: authoredcontent
ms.openlocfilehash: 3d94619ca40d288f8a5a391ef02103902bd2550c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37835955"
---
<a name="executing-several-animations-after-each-other-vb"></a>Esecuzione di diverse animazioni una dopo l'altra (VB)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare il codice](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3VB.pdf)

> Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo. Consente l'esecuzione di diverse animazioni una dopo l'altro.


## <a name="overview"></a>Panoramica

Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo. Consente l'esecuzione di diverse animazioni una dopo l'altro.

## <a name="steps"></a>Passaggi

Prima di tutto, includere il `ScriptManager` nella pagina; quindi, la libreria ASP.NET AJAX viene caricata, rendendo possibile usare il Toolkit di controllo:

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample1.aspx)]

Verrà applicata l'animazione a un pannello del testo che presenta un aspetto simile al seguente:

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample2.aspx)]

Nella classe CSS associata per il pannello, definire un colore di sfondo interessante e anche impostare una larghezza fissa per il pannello:

[!code-css[Main](executing-several-animations-after-each-other-vb/samples/sample3.css)]

Quindi, aggiungere il `AnimationExtender` alla pagina, fornendo un' `ID`, il `TargetControlID` attributo e l'obbligatoria `runat="server":`

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample4.aspx)]

All'interno di `<Animations>` nodo, usare `<OnLoad>` per eseguire le animazioni dopo la pagina è stata caricata completamente. In generale, `<OnLoad>` accetta solo un'animazione. Il framework di animazione consente di eseguire il join di diverse animazioni in uno solo tramite il `<Sequence>` elemento. Tutte le animazioni in `<Sequence>` vengono eseguite una dopo l'altro. Di seguito è riportato l'un markup possibili per il `AnimationExtender` controllo, prima di tutto rendendo più ampio del pannello e quindi diminuire l'altezza:

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample5.aspx)]

Quando si esegue questo script, il pannello prima ottiene più larghi e quindi più piccoli.


[![Prima di tutto la larghezza viene aumentata](executing-several-animations-after-each-other-vb/_static/image2.png)](executing-several-animations-after-each-other-vb/_static/image1.png)

Prima di tutto la larghezza viene aumentata ([fare clic per visualizzare l'immagine con dimensioni normali](executing-several-animations-after-each-other-vb/_static/image3.png))


[![Quindi viene diminuito l'altezza](executing-several-animations-after-each-other-vb/_static/image5.png)](executing-several-animations-after-each-other-vb/_static/image4.png)

Quindi viene diminuito l'altezza ([fare clic per visualizzare l'immagine con dimensioni normali](executing-several-animations-after-each-other-vb/_static/image6.png))

> [!div class="step-by-step"]
> [Precedente](executing-several-animations-at-the-same-time-vb.md)
> [Successivo](animation-depending-on-a-condition-vb.md)
