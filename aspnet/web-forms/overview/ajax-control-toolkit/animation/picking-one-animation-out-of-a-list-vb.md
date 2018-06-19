---
uid: web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-vb
title: Selezione di un'animazione da un elenco (VB) | Documenti Microsoft
author: wenz
description: Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework per aggiungere le animazioni a un controllo. Il framework anche autoriz...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 81ba9116-d485-40c0-8ff6-7e9ae23e0a0c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-vb
msc.type: authoredcontent
ms.openlocfilehash: f2bd1b3cc72595da7e8901786ea8415d7c1c524a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872065"
---
<a name="picking-one-animation-out-of-a-list-vb"></a>Selezione di un'animazione da un elenco (VB)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare codice](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation5.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation5VB.pdf)

> Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework per aggiungere le animazioni a un controllo. Inoltre, il framework consente ai programmatori di scegliere un'animazione da un elenco di animazioni, a seconda della valutazione del codice JavaScript.


## <a name="overview"></a>Panoramica

Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework per aggiungere le animazioni a un controllo. Inoltre, il framework consente ai programmatori di scegliere un'animazione da un elenco di animazioni, a seconda della valutazione del codice JavaScript.

## <a name="steps"></a>Passaggi

Prima di tutto, includere il `ScriptManager` nella pagina; quindi, la libreria AJAX di ASP.NET viene caricata, che consente di usare il Toolkit di controllo:

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample1.aspx)]

Verrà applicata l'animazione a un riquadro del testo che presenta un aspetto simile al seguente:

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample2.aspx)]

Nella classe CSS associata per il pannello, definire un colore di sfondo nice e inoltre impostare una larghezza fissa per il pannello:

[!code-css[Main](picking-one-animation-out-of-a-list-vb/samples/sample3.css)]

Quindi, aggiungere il `AnimationExtender` alla pagina, fornendo un' `ID`, il `TargetControlID` attributo e l'obbligatoria `runat="server":`

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample4.aspx)]

All'interno di `<Animations>` nodo, utilizzare `<OnLoad>` per eseguire le animazioni dopo la pagina è stata caricata completamente. Invece di una delle animazioni, regolare il `<Case>` elemento entra in gioco. Viene valutato il valore dell'attributo SelectScript; il valore restituito deve essere numerico. A seconda di questo numero, tra i dipendenti all'interno di &lt;Case&gt; viene eseguita. Ad esempio, se SelectScript restituisce 2, il Toolkit di controllo viene eseguita la terza animazione all'interno di &lt;Case&gt; (conteggio inizia da 0).

Il markup seguente definisce tre dipendenti: ridimensionare la larghezza e l'altezza di ridimensionamento dissolvenza in uscita. Il codice JavaScript (`Math.floor(3 * Math.random())`) quindi sceglie un numero compreso tra 0 e 2, in modo che uno dei tre animazioni viene eseguito:

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample5.aspx)]


[![Uno dei tre animazioni possibili: il pannello ottiene più ampio](picking-one-animation-out-of-a-list-vb/_static/image2.png)](picking-one-animation-out-of-a-list-vb/_static/image1.png)

Una delle tre animazioni possibili: il pannello si allunga ([fare clic per visualizzare l'immagine ingrandita](picking-one-animation-out-of-a-list-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](animation-depending-on-a-condition-vb.md)
> [Successivo](animating-in-response-to-user-interaction-vb.md)
