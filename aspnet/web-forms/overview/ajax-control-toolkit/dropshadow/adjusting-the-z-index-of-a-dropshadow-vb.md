---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-vb
title: Modifica dell'ordine z di un DropShadow (VB) | Documenti Microsoft
author: wenz
description: Il controllo DropShadow AJAX Control Toolkit estende un pannello con un'ombreggiatura. Tuttavia questo shadow talvolta è in conflitto con altri controlli, per insta...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: ecb004b5-82c0-44fb-bcaf-233fffac6195
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-vb
msc.type: authoredcontent
ms.openlocfilehash: b484dc6bfa6f67bd6b70f7c36c2eb2ec7143edaf
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30871298"
---
<a name="adjusting-the-z-index-of-a-dropshadow-vb"></a>Modifica dell'ordine z di un DropShadow (VB)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare codice](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1VB.pdf)

> Il controllo DropShadow AJAX Control Toolkit estende un pannello con un'ombreggiatura. Tuttavia questo shadow talvolta è in conflitto con altri controlli, ad esempio il controllo Menu ASP.NET. Quando una voce di menu viene visualizzata, viene visualizzato dietro l'ombreggiatura.


## <a name="overview"></a>Panoramica

Il controllo DropShadow AJAX Control Toolkit estende un pannello con un'ombreggiatura. Tuttavia questo shadow talvolta è in conflitto con altri controlli, ad esempio il controllo Menu ASP.NET. Quando una voce di menu viene visualizzata, viene visualizzato dietro l'ombreggiatura.

## <a name="steps"></a>Passaggi

Il codice inizia con il pannello, contenente testo in modo che il pannello contiene testo sufficiente per l'effetto sia visibile:

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample1.aspx)]

Un altro pannello viene eseguito immediatamente prima di `panelShadow` pannello. Contiene un menu con orientamento orizzontale in modo che le voci di menu visualizzate in (oppure piuttosto: sotto) il `dropShadow` pannello):

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample2.aspx)]

Quindi, `DropShadowExtender` viene aggiunto per estendere il `panelShadow` pannello con un effetto di ombreggiatura:

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample3.aspx)]

Infine, ASP.NET AJAX `ScriptManager` controllo Abilita il Toolkit di controllo utilizzare:

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample4.aspx)]

Quando si esegue questo script, le voci di menu vengono visualizzate sotto il pannello. Tuttavia il menu utilizza la classe CSS `panel` in cui è sufficiente definire due operazioni per visualizzare gli elementi davanti a un pannello di:

- Posizionamento relativo
- Indice z positivo

[!code-css[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample5.css)]

Quindi, `DropShadowExtender` controllo non è in conflitto più con il controllo Menu.


[![Prima: La voce di menu non è visibile](adjusting-the-z-index-of-a-dropshadow-vb/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image1.png)

Prima: La voce di menu non è visibile ([fare clic per visualizzare l'immagine ingrandita](adjusting-the-z-index-of-a-dropshadow-vb/_static/image3.png))


[![Dopo: Viene visualizzata la voce di menu](adjusting-the-z-index-of-a-dropshadow-vb/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image4.png)

Dopo: Viene visualizzata la voce di menu ([fare clic per visualizzare l'immagine ingrandita](adjusting-the-z-index-of-a-dropshadow-vb/_static/image6.png))

> [!div class="step-by-step"]
> [Precedente](manipulating-dropshadow-properties-from-client-code-cs.md)
> [Successivo](manipulating-dropshadow-properties-from-client-code-vb.md)
