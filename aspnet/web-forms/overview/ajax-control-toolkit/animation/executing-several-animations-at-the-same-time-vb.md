---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-at-the-same-time-vb
title: L'esecuzione di numerose animazioni contemporaneamente (VB) | Documenti Microsoft
author: wenz
description: Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework per aggiungere le animazioni a un controllo. Consente di eseguire severa...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 2469f7ea-1489-42fb-a8e1-414c90141ce9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-at-the-same-time-vb
msc.type: authoredcontent
ms.openlocfilehash: 823764abd4444b5cb8d9bc6e8e8ed34d6f670310
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879439"
---
<a name="executing-several-animations-at-the-same-time-vb"></a>L'esecuzione di numerose animazioni contemporaneamente (VB)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare codice](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation2.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation2VB.pdf)

> Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework per aggiungere le animazioni a un controllo. Consente di eseguire numerose animazioni in parallelo.


## <a name="overview"></a>Panoramica

Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework per aggiungere le animazioni a un controllo. Consente di eseguire numerose animazioni in parallelo.

## <a name="steps"></a>Passaggi

Prima di tutto, includere il `ScriptManager` nella pagina; quindi, la libreria AJAX di ASP.NET viene caricata, che consente di usare il Toolkit di controllo:

[!code-aspx[Main](executing-several-animations-at-the-same-time-vb/samples/sample1.aspx)]

Verrà applicata l'animazione a un riquadro del testo che presenta un aspetto simile al seguente:

[!code-aspx[Main](executing-several-animations-at-the-same-time-vb/samples/sample2.aspx)]

Nella classe CSS associata per il pannello, definire un colore di sfondo nice e inoltre impostare una larghezza fissa per il pannello:

[!code-css[Main](executing-several-animations-at-the-same-time-vb/samples/sample3.css)]

Quindi, aggiungere il `AnimationExtender` alla pagina, fornendo un `ID`, `TargetControlID` attributo e l'obbligatoria `runat="server"`:

[!code-aspx[Main](executing-several-animations-at-the-same-time-vb/samples/sample4.aspx)]

All'interno di `<Animations>` nodo, utilizzare `<OnLoad>` per eseguire le animazioni dopo la pagina è stata caricata completamente. In genere, `<OnLoad>` accetta solo un'animazione. Il framework di animazione consente di unire più animazioni in uno solo tramite il `<Parallel>` elemento. Tutte le animazioni all'interno di `<Parallel>` vengono eseguiti nello stesso momento.

Ecco l'un markup possibili per il `AnimationExtender` controllo dissolvenza in uscita e ridimensionare il pannello contemporaneamente:

[!code-aspx[Main](executing-several-animations-at-the-same-time-vb/samples/sample5.aspx)]

Infatti: quando si esegue questo script, il pannello viene visualizzato, quindi ridimensiona (più threefolding larghezza e halfing l'altezza) e in dissolvenza nello stesso momento.


[![Il pannello è dissolvenza in uscita e il ridimensionamento (incluso il relativo contenuto, grazie al motore di rendering del browser)](executing-several-animations-at-the-same-time-vb/_static/image2.png)](executing-several-animations-at-the-same-time-vb/_static/image1.png)

Il pannello è dissolvenza in uscita e il ridimensionamento (incluso il relativo contenuto, grazie al motore di rendering del browser) ([fare clic per visualizzare l'immagine ingrandita](executing-several-animations-at-the-same-time-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](adding-animation-to-a-control-vb.md)
> [Successivo](executing-several-animations-after-each-other-vb.md)
