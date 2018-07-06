---
uid: web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-vb
title: Selezione di un'animazione da un elenco (VB) | Microsoft Docs
author: wenz
description: Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo. Il framework conse inoltre...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 81ba9116-d485-40c0-8ff6-7e9ae23e0a0c
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-vb
msc.type: authoredcontent
ms.openlocfilehash: 126f1b03897763f0619f893d23ab2e763206d08e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37809887"
---
<a name="picking-one-animation-out-of-a-list-vb"></a>Selezione di un'animazione da un elenco (VB)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare il codice](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation5.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation5VB.pdf)

> Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo. Il framework consente inoltre al programmatore di scegliere un'animazione da un elenco di animazioni, a seconda della valutazione del codice JavaScript.


## <a name="overview"></a>Panoramica

Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo. Il framework consente inoltre al programmatore di scegliere un'animazione da un elenco di animazioni, a seconda della valutazione del codice JavaScript.

## <a name="steps"></a>Passaggi

Prima di tutto, includere il `ScriptManager` nella pagina; quindi, la libreria ASP.NET AJAX viene caricata, rendendo possibile usare il Toolkit di controllo:

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample1.aspx)]

Verrà applicata l'animazione a un pannello del testo che presenta un aspetto simile al seguente:

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample2.aspx)]

Nella classe CSS associata per il pannello, definire un colore di sfondo interessante e anche impostare una larghezza fissa per il pannello:

[!code-css[Main](picking-one-animation-out-of-a-list-vb/samples/sample3.css)]

Quindi, aggiungere il `AnimationExtender` alla pagina, fornendo un' `ID`, il `TargetControlID` attributo e l'obbligatoria `runat="server":`

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample4.aspx)]

All'interno di `<Animations>` nodo, usare `<OnLoad>` per eseguire le animazioni dopo la pagina è stata caricata completamente. Invece di uno delle animazioni, regolare il `<Case>` entra in gioco l'elemento. Viene valutato il valore dell'attributo SelectScript; il valore restituito deve essere numerico. A seconda di questo numero, una dei dipendenti all'interno &lt;caso&gt; viene eseguita. Ad esempio, se SelectScript restituisce 2, il Toolkit di controllo viene eseguito l'animazione terzo entro &lt;caso&gt; (il conteggio inizia da 0).

Il markup seguente definisce tre dipendenti: la larghezza di ridimensionamento, il ridimensionamento l'altezza e dissolvenza in uscita. Il codice JavaScript (`Math.floor(3 * Math.random())`) quindi rileva un numero compreso tra 0 e 2, in modo che uno dei tre animazioni viene eseguito:

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample5.aspx)]


[![Una delle animazioni di tre possibili: il pannello si allunga](picking-one-animation-out-of-a-list-vb/_static/image2.png)](picking-one-animation-out-of-a-list-vb/_static/image1.png)

Una delle animazioni di tre possibili: il pannello si allunga ([fare clic per visualizzare l'immagine con dimensioni normali](picking-one-animation-out-of-a-list-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](animation-depending-on-a-condition-vb.md)
> [Successivo](animating-in-response-to-user-interaction-vb.md)
