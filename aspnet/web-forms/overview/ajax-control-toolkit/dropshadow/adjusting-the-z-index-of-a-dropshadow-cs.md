---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-cs
title: Modifica dello Z Index di un controllo DropShadow (c#) | Microsoft Docs
author: wenz
description: Il controllo DropShadow in AJAX Control Toolkit estende un pannello con un'ombreggiatura. Tuttavia questo shadow in alcuni casi è in conflitto con altri controlli, di programma...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 14133833-e518-4347-87b9-6b6f71f14a77
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-cs
msc.type: authoredcontent
ms.openlocfilehash: 22732bc44a56819a2ec73346dc32659d027eaee0
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37369570"
---
<a name="adjusting-the-z-index-of-a-dropshadow-c"></a>Modifica dello Z Index di un controllo DropShadow (c#)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare il codice](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1CS.pdf)

> Il controllo DropShadow in AJAX Control Toolkit estende un pannello con un'ombreggiatura. Tuttavia questo shadow in alcuni casi è in conflitto con altri controlli, ad esempio il controllo di ASP.NET Menu. Quando una voce di menu viene visualizzata, viene visualizzato dietro l'ombreggiatura.


## <a name="overview"></a>Panoramica

Il controllo DropShadow in AJAX Control Toolkit estende un pannello con un'ombreggiatura. Tuttavia questo shadow in alcuni casi è in conflitto con altri controlli, ad esempio il controllo di ASP.NET Menu. Quando una voce di menu viene visualizzata, viene visualizzato dietro l'ombreggiatura.

## <a name="steps"></a>Passaggi

Il codice inizia con il pannello stesso, che contiene il testo sufficiente in modo che il pannello contiene testo sufficiente per l'effetto sia visibile:

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample1.aspx)]

Un altro pannello è posizionato direttamente prima il `panelShadow` pannello. Contiene un menu con orientamento orizzontale, in modo che le voci di menu visualizzate in (o piuttosto: sotto) il `dropShadow` pannello):

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample2.aspx)]

Successivamente, il `DropShadowExtender` viene aggiunto per estendere il `panelShadow` pannello con un effetto ombreggiatura:

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample3.aspx)]

Infine, ASP.NET AJAX `ScriptManager` controllo Abilita il Toolkit di controllo lavorare:

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample4.aspx)]

Quando si esegue questo script, le voci di menu vengono visualizzate sotto il pannello. Tuttavia il menu di scelta Usa la classe CSS `panel` in cui è sufficiente definire due cose come è possibile visualizzare gli elementi davanti a altro pannello:

- Posizionamento relativo
- Un indice z positivo

[!code-css[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample5.css)]

Quindi, `DropShadowExtender` controllo non è in conflitto più con il controllo Menu.


[![Prima: La voce di menu non è visibile](adjusting-the-z-index-of-a-dropshadow-cs/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image1.png)

Prima: La voce di menu non è visibile ([fare clic per visualizzare l'immagine con dimensioni normali](adjusting-the-z-index-of-a-dropshadow-cs/_static/image3.png))


[![Dopo: Viene visualizzata la voce di menu](adjusting-the-z-index-of-a-dropshadow-cs/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image4.png)

Dopo: La voce di menu viene visualizzata ([fare clic per visualizzare l'immagine con dimensioni normali](adjusting-the-z-index-of-a-dropshadow-cs/_static/image6.png))

> [!div class="step-by-step"]
> [avanti](manipulating-dropshadow-properties-from-client-code-cs.md)
