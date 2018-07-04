---
uid: web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-cs
title: Selezione di un'animazione da un elenco (c#) | Microsoft Docs
author: wenz
description: Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo. Il framework conse inoltre...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 06a776fe-7c73-4ca7-8e02-5260a86edc03
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-cs
msc.type: authoredcontent
ms.openlocfilehash: 4b0193f47697e072605ed8c01a37fbfa128eb0e0
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37380746"
---
<a name="picking-one-animation-out-of-a-list-c"></a>Selezione di un'animazione da un elenco (c#)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare il codice](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation5.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation5CS.pdf)

> Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo. Il framework consente inoltre al programmatore di scegliere un'animazione da un elenco di animazioni, a seconda della valutazione del codice JavaScript.


## <a name="overview"></a>Panoramica

Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo. Il framework consente inoltre al programmatore di scegliere un'animazione da un elenco di animazioni, a seconda della valutazione del codice JavaScript.

## <a name="steps"></a>Passaggi

Prima di tutto, includere il `ScriptManager` nella pagina; quindi, la libreria ASP.NET AJAX viene caricata, rendendo possibile usare il Toolkit di controllo:

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample1.aspx)]

Verrà applicata l'animazione a un pannello del testo che presenta un aspetto simile al seguente:

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample2.aspx)]

Nella classe CSS associata per il pannello, definire un colore di sfondo interessante e anche impostare una larghezza fissa per il pannello:

[!code-css[Main](picking-one-animation-out-of-a-list-cs/samples/sample3.css)]

Quindi, aggiungere il `AnimationExtender` alla pagina, fornendo un' `ID`, il `TargetControlID` attributo e l'obbligatoria `runat="server":`

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample4.aspx)]

All'interno di `<Animations>` nodo, usare `<OnLoad>` per eseguire le animazioni dopo la pagina è stata caricata completamente. Invece di uno delle animazioni, regolare il `<Case>` entra in gioco l'elemento. Viene valutato il valore dell'attributo SelectScript; il valore restituito deve essere numerico. A seconda di questo numero, una dei dipendenti all'interno &lt;caso&gt; viene eseguita. Ad esempio, se SelectScript restituisce 2, il Toolkit di controllo viene eseguito l'animazione terzo entro &lt;caso&gt; (il conteggio inizia da 0).

Il markup seguente definisce tre dipendenti: la larghezza di ridimensionamento, il ridimensionamento l'altezza e dissolvenza in uscita. Il codice JavaScript (`Math.floor(3 * Math.random())`) quindi rileva un numero compreso tra 0 e 2, in modo che uno dei tre animazioni viene eseguito:

[!code-aspx[Main](picking-one-animation-out-of-a-list-cs/samples/sample5.aspx)]


[![Una delle animazioni di tre possibili: il pannello si allunga](picking-one-animation-out-of-a-list-cs/_static/image2.png)](picking-one-animation-out-of-a-list-cs/_static/image1.png)

Una delle animazioni di tre possibili: il pannello si allunga ([fare clic per visualizzare l'immagine con dimensioni normali](picking-one-animation-out-of-a-list-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](animation-depending-on-a-condition-cs.md)
> [Successivo](animating-in-response-to-user-interaction-cs.md)
