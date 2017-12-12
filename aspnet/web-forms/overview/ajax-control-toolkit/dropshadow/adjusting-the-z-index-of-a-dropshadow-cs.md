---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-cs
title: Modifica dell'ordine z di un DropShadow (c#) | Documenti Microsoft
author: wenz
description: "Il controllo DropShadow AJAX Control Toolkit estende un pannello con un'ombreggiatura. Tuttavia questo shadow talvolta è in conflitto con altri controlli, per insta..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 14133833-e518-4347-87b9-6b6f71f14a77
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-cs
msc.type: authoredcontent
ms.openlocfilehash: 161f02daa5dd1f0e21853c1b7c1a65c1a9aa5d03
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="adjusting-the-z-index-of-a-dropshadow-c"></a>Modifica dell'ordine z di un DropShadow (c#)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare codice](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1CS.pdf)

> Il controllo DropShadow AJAX Control Toolkit estende un pannello con un'ombreggiatura. Tuttavia questo shadow talvolta è in conflitto con altri controlli, ad esempio il controllo Menu ASP.NET. Quando una voce di menu viene visualizzata, viene visualizzato dietro l'ombreggiatura.


## <a name="overview"></a>Panoramica

Il controllo DropShadow AJAX Control Toolkit estende un pannello con un'ombreggiatura. Tuttavia questo shadow talvolta è in conflitto con altri controlli, ad esempio il controllo Menu ASP.NET. Quando una voce di menu viene visualizzata, viene visualizzato dietro l'ombreggiatura.

## <a name="steps"></a>Passaggi

Il codice inizia con il pannello, contenente testo in modo che il pannello contiene testo sufficiente per l'effetto sia visibile:

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample1.aspx)]

Un altro pannello viene eseguito immediatamente prima di `panelShadow` pannello. Contiene un menu con orientamento orizzontale in modo che le voci di menu visualizzate in (oppure piuttosto: sotto) il `dropShadow` pannello):

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample2.aspx)]

Quindi, `DropShadowExtender` viene aggiunto per estendere il `panelShadow` pannello con un effetto di ombreggiatura:

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample3.aspx)]

Infine, ASP.NET AJAX `ScriptManager` controllo Abilita il Toolkit di controllo utilizzare:

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample4.aspx)]

Quando si esegue questo script, le voci di menu vengono visualizzate sotto il pannello. Tuttavia il menu utilizza la classe CSS `panel` in cui è sufficiente definire due operazioni per visualizzare gli elementi davanti a un pannello di:

- Posizionamento relativo
- Indice z positivo

[!code-css[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample5.css)]

Quindi, `DropShadowExtender` controllo non è in conflitto più con il controllo Menu.


[![Prima: La voce di menu non è visibile](adjusting-the-z-index-of-a-dropshadow-cs/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image1.png)

Prima: La voce di menu non è visibile ([fare clic per visualizzare l'immagine ingrandita](adjusting-the-z-index-of-a-dropshadow-cs/_static/image3.png))


[![Dopo: Viene visualizzata la voce di menu](adjusting-the-z-index-of-a-dropshadow-cs/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image4.png)

Dopo: Viene visualizzata la voce di menu ([fare clic per visualizzare l'immagine ingrandita](adjusting-the-z-index-of-a-dropshadow-cs/_static/image6.png))

>[!div class="step-by-step"]
[Successivo](manipulating-dropshadow-properties-from-client-code-cs.md)
