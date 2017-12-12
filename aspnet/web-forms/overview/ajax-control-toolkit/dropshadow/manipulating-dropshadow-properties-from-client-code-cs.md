---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-cs
title: "La modifica delle proprietà di DropShadow dal codice Client (c#) | Documenti Microsoft"
author: wenz
description: Personalizzazione dell'interfaccia di modifica del controllo DataList
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: c83ca3e6-c0bf-4158-a166-40c1ab0f33da
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 59f7d4610ce610ef4357510f0e861f107278b5da
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="manipulating-dropshadow-properties-from-client-code-c"></a>La modifica delle proprietà di DropShadow dal codice Client (c#)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare codice](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2CS.pdf)

> Il controllo DropShadow AJAX Control Toolkit estende un pannello con un'ombreggiatura. Proprietà di questo tipo di estensione possono essere modificate anche utilizzando il codice client JavaScript.


## <a name="overview"></a>Panoramica

Il controllo DropShadow AJAX Control Toolkit estende un pannello con un'ombreggiatura. Proprietà di questo tipo di estensione possono essere modificate anche utilizzando il codice client JavaScript.

## <a name="steps"></a>Passaggi

Il codice inizia con un pannello contenente alcune righe di testo:

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample1.aspx)]

La classe CSS associata offre il pannello di un colore di sfondo nice:

[!code-css[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample2.css)]

Il `DropShadowExtender` viene aggiunto al riquadro con un effetto di ombreggiatura, impostare su 50% di opacità estendere:

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample3.aspx)]

Quindi, ASP.NET AJAX `ScriptManager` controllo Abilita il Toolkit di controllo utilizzare:

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample4.aspx)]

Un altro pannello contiene due collegamenti di JavaScript per l'impostazione dell'opacità dell'ombreggiatura: il collegamento meno ridurre l'opacità dell'ombreggiatura, il collegamento più aumenta.

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample5.aspx)]

La funzione JavaScript `changeOpacity()` quindi necessario trovare il `DropShadowExtender` controllo nella pagina. ASP.NET AJAX definisce il `$find()` metodo per esattamente tale attività. Quindi, `get_Opacity()` che consente di recuperare l'opacità corrente, il `set_Opacity()` viene impostato dal metodo. Il valore di opacità del codice JavaScript viene posizionato nel `<label>` elemento:

[!code-html[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample6.html)]


[![L'opacità viene modificato sul lato client](manipulating-dropshadow-properties-from-client-code-cs/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-cs/_static/image1.png)

L'opacità viene modificato sul lato client ([fare clic per visualizzare l'immagine ingrandita](manipulating-dropshadow-properties-from-client-code-cs/_static/image3.png))

>[!div class="step-by-step"]
[Precedente](adjusting-the-z-index-of-a-dropshadow-cs.md)
[Successivo](adjusting-the-z-index-of-a-dropshadow-vb.md)
