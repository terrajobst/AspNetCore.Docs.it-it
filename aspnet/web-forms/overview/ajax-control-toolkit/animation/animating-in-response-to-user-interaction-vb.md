---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-vb
title: Animazione in risposta all'interazione dell'utente (VB) | Microsoft Docs
author: wenz
description: Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo. Le animazioni possono stelle...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: c8204c05-ec27-40fe-933d-88e4e727a482
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-vb
msc.type: authoredcontent
ms.openlocfilehash: d3a118d3dc44dd74780fafd00d139160f8fc3bc5
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37366877"
---
<a name="animating-in-response-to-user-interaction-vb"></a>Animazione in risposta all'interazione dell'utente (VB)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare il codice](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6VB.pdf)

> Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo. Le animazioni può essere avviata automaticamente o possono essere attivate dall'interazione dell'utente, ad esempio, facendo clic con il mouse.


## <a name="overview"></a>Panoramica

Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo. Le animazioni può essere avviata automaticamente o possono essere attivate dall'interazione dell'utente, ad esempio, facendo clic con il mouse.

## <a name="steps"></a>Passaggi

Prima di tutto, includere il `ScriptManager` nella pagina; quindi, la libreria ASP.NET AJAX viene caricata, rendendo possibile usare il Toolkit di controllo:

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample1.aspx)]

Verrà applicata l'animazione a un pannello del testo che presenta un aspetto simile al seguente:

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample2.aspx)]

Nella classe CSS associata per il pannello, definire un colore di sfondo interessante e anche impostare una larghezza fissa per il pannello:

[!code-css[Main](animating-in-response-to-user-interaction-vb/samples/sample3.css)]

Quindi, aggiungere il `AnimationExtender` alla pagina, fornendo un' `ID`, il `TargetControlID` attributo e l'obbligatoria `runat="server"`:

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample4.aspx)]

All'interno di `<Animations>` nodo, sono disponibili cinque modi per avviare l'animazione tramite l'interazione dell'utente (l'elemento mancano è `<OnLoad>` che viene eseguita una volta l'intera pagina è stata caricata completamente):

- `<OnClick>` (fare clic del mouse sul controllo)
- `<OnHoverOut>` (mouse lascia il controllo)
- `<OnHoverOver>` (passaggio del mouse su un controllo, l'arresto di `<OnHoverOut>` animazione)
- `<OnMouseOut>` (mouse esce da un controllo)
- `<OnMouseOver>` (passaggio del mouse su un controllo, non interrompe la `<OnMouseOut>` animazione)

In questo scenario, `<OnClick>` viene usato. Quando l'utente fa clic sul pannello, ridimensionamento e dissolve nello stesso momento.

[!code-aspx[Main](animating-in-response-to-user-interaction-vb/samples/sample5.aspx)]


[![Un clic del mouse viene avviata l'animazione](animating-in-response-to-user-interaction-vb/_static/image2.png)](animating-in-response-to-user-interaction-vb/_static/image1.png)

Un clic del mouse viene avviata l'animazione ([fare clic per visualizzare l'immagine con dimensioni normali](animating-in-response-to-user-interaction-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](picking-one-animation-out-of-a-list-vb.md)
> [Successivo](disabling-actions-during-animation-vb.md)
