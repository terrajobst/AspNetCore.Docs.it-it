---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-cs
title: Animazione in risposta all'interazione dell'utente (c#) | Documenti Microsoft
author: wenz
description: "Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework per aggiungere le animazioni a un controllo. Le animazioni possano stelle..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: ea26549d-fbbf-4973-a108-b14cd1d6de26
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-cs
msc.type: authoredcontent
ms.openlocfilehash: efb9c34c317ec56b43c498f40a857a9b47fa50b2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="animating-in-response-to-user-interaction-c"></a>Animazione in risposta all'interazione dell'utente (c#)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare codice](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6CS.pdf)

> Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework per aggiungere le animazioni a un controllo. Le animazioni può essere avviata automaticamente o possono essere attivate dall'interazione dell'utente, ad esempio, facendo clic con il mouse.


## <a name="overview"></a>Panoramica

Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework per aggiungere le animazioni a un controllo. Le animazioni può essere avviata automaticamente o possono essere attivate dall'interazione dell'utente, ad esempio, facendo clic con il mouse.

## <a name="steps"></a>Passaggi

Prima di tutto, includere il `ScriptManager` nella pagina; quindi, la libreria AJAX di ASP.NET viene caricata, che consente di usare il Toolkit di controllo:

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample1.aspx)]

Verrà applicata l'animazione a un riquadro del testo che presenta un aspetto simile al seguente:

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample2.aspx)]

Nella classe CSS associata per il pannello, definire un colore di sfondo nice e inoltre impostare una larghezza fissa per il pannello:

[!code-css[Main](animating-in-response-to-user-interaction-cs/samples/sample3.css)]

Quindi, aggiungere il `AnimationExtender` alla pagina, fornendo un `ID`, `TargetControlID` attributo e l'obbligatoria `runat="server"`:

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample4.aspx)]

All'interno di `<Animations>` nodo, sono disponibili cinque modi per iniziare l'animazione tramite l'interazione dell'utente (l'elemento mancano è `<OnLoad>` che viene eseguito una volta l'intera pagina è stata caricata completamente):

- `<OnClick>`(fare clic del mouse sul controllo)
- `<OnHoverOut>`(mouse abbandona il controllo)
- `<OnHoverOver>`(mouse viene spostato su un controllo, l'arresto di `<OnHoverOut>` animazione)
- `<OnMouseOut>`(mouse esce da un controllo)
- `<OnMouseOver>`(mouse viene spostato su un controllo, non arrestare il `<OnMouseOut>` animazione)

In questo scenario, `<OnClick>` viene utilizzato. Quando l'utente fa clic sul riquadro, viene ridimensionato e dissolvenza nello stesso momento.

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample5.aspx)]


[![Un clic del mouse viene avviata l'animazione](animating-in-response-to-user-interaction-cs/_static/image2.png)](animating-in-response-to-user-interaction-cs/_static/image1.png)

Un clic del mouse viene avviata l'animazione ([fare clic per visualizzare l'immagine ingrandita](animating-in-response-to-user-interaction-cs/_static/image3.png))

>[!div class="step-by-step"]
[Precedente](picking-one-animation-out-of-a-list-cs.md)
[Successivo](disabling-actions-during-animation-cs.md)
