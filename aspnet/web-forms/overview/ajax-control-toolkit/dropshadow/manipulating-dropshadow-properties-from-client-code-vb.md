---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-vb
title: La modifica delle proprietà di DropShadow dal codice Client (VB) | Documenti Microsoft
author: wenz
description: Il controllo DropShadow AJAX Control Toolkit estende un pannello con un'ombreggiatura. Proprietà di questa estensione può essere modificata utilizzando client JavaScrip...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 11be4211-2fb9-4e15-b6d4-2aa623d81f3e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-vb
msc.type: authoredcontent
ms.openlocfilehash: b5b024811ea511e67ce180169de9f0b7e3ef51d9
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="manipulating-dropshadow-properties-from-client-code-vb"></a>La modifica delle proprietà di DropShadow dal codice Client (VB)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare codice](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2VB.pdf)

> Il controllo DropShadow AJAX Control Toolkit estende un pannello con un'ombreggiatura. Proprietà di questo tipo di estensione possono essere modificate anche utilizzando il codice client JavaScript.


## <a name="overview"></a>Panoramica

Il controllo DropShadow AJAX Control Toolkit estende un pannello con un'ombreggiatura. Proprietà di questo tipo di estensione possono essere modificate anche utilizzando il codice client JavaScript.

## <a name="steps"></a>Passaggi

Il codice inizia con un pannello contenente alcune righe di testo:

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample1.aspx)]

La classe CSS associata offre il pannello di un colore di sfondo nice:

[!code-css[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample2.css)]

Il `DropShadowExtender` viene aggiunto al riquadro con un effetto di ombreggiatura, impostare su 50% di opacità estendere:

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample3.aspx)]

Quindi, ASP.NET AJAX `ScriptManager` controllo Abilita il Toolkit di controllo utilizzare:

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample4.aspx)]

Un altro pannello contiene due collegamenti di JavaScript per l'impostazione dell'opacità dell'ombreggiatura: il collegamento meno ridurre l'opacità dell'ombreggiatura, il collegamento più aumenta.

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample5.aspx)]

La funzione JavaScript `changeOpacity()` quindi necessario trovare il `DropShadowExtender` controllo nella pagina. ASP.NET AJAX definisce il `$find()` metodo per esattamente tale attività. Quindi, `get_Opacity()` che consente di recuperare l'opacità corrente, il `set_Opacity()` viene impostato dal metodo. Il valore di opacità del codice JavaScript viene posizionato nel `<label>` elemento:

[!code-html[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample6.html)]


[![L'opacità viene modificato sul lato client](manipulating-dropshadow-properties-from-client-code-vb/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-vb/_static/image1.png)

L'opacità viene modificato sul lato client ([fare clic per visualizzare l'immagine ingrandita](manipulating-dropshadow-properties-from-client-code-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](adjusting-the-z-index-of-a-dropshadow-vb.md)
