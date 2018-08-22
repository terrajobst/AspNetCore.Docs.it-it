---
uid: web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-cs
title: Attivazione di un'animazione in un altro controllo (c#) | Microsoft Docs
author: wenz
description: Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo. In generale, l'avvio di un...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: e5d99c2b-d8ee-413c-80d5-c120cffb0a4c
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-cs
msc.type: authoredcontent
ms.openlocfilehash: ad6dfecf71a7577215e43222a8788e5c48d0c4c2
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825918"
---
<a name="triggering-an-animation-in-another-control-c"></a>Attivazione di un'animazione in un altro controllo (c#)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare il codice](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8CS.pdf)

> Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo. In generale, l'avvio di un'animazione viene attivata dall'interazione dell'utente con lo stesso controllo. È tuttavia anche possibile interagire con un controllo e quindi animazione a un altro controllo.


## <a name="overview"></a>Panoramica

Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo. In generale, l'avvio di un'animazione viene attivata dall'interazione dell'utente con lo stesso controllo. È tuttavia anche possibile interagire con un controllo e quindi animazione a un altro controllo.

## <a name="steps"></a>Passaggi

Prima di tutto, includere il `ScriptManager` nella pagina; quindi, la libreria ASP.NET AJAX viene caricata, rendendo possibile usare il Toolkit di controllo:

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample1.aspx)]

Verrà applicata l'animazione a un pannello del testo che presenta un aspetto simile al seguente:

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample2.aspx)]

Nella classe CSS associata per il pannello, definire un colore di sfondo interessante e anche impostare una larghezza fissa per il pannello:

[!code-css[Main](triggering-an-animation-in-another-control-cs/samples/sample3.css)]

Per avviare il pannello Animazione, viene usato un pulsante HTML. Si noti che `<input type="button" />` è favoriti rispetto `<asp:Button />` poiché non vogliamo un postback quando l'utente fa clic su tale pulsante.

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample4.aspx)]

Quindi, aggiungere il `AnimationExtender` alla pagina, fornendo un' `ID`, il `TargetControlID` attributo e l'obbligatoria `runat="server"`. È importante impostare `TargetControlID` all'ID del pulsante (l'elemento attivare l'animazione), non per l'ID del pannello (l'elemento viene aggiunta un'animazione)

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample5.aspx)]

All'interno di `<Animations>` nodo, le animazioni sul posto come di consueto. Per fare in modo del Pannello di modifica, non il pulsante, impostare il `AnimationTarget` attributo per ogni elemento animazione `AnimationExtender`. Il valore per `AnimationTarget` è l'ID del pannello, ovviamente. In questo modo, le animazioni verificano con il pannello, non con il pulsante di attivazione del trigger. Di seguito è riportato il `AnimationExtender` markup per questo scenario:

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample6.aspx)]

Si noti l'ordine speciale in cui vengono visualizzate le singole animazioni. Prima di tutto, il pulsante disattivato una volta che viene eseguita l'animazione. Poiché è presente alcun `AnimationTarget` attributo la `<EnableAction>` elemento, questa animazione viene applicata al controllo origine: il pulsante. I passaggi successivi due animazione devono essere effettuati parallelly (`<Parallel>` elemento). Entrambi hanno loro `AnimationTarget` attributi impostati `"Panel1"`, l'animazione in questo modo il pannello, non il pulsante.


[![Un clic del mouse sul pulsante Avvia l'animazione di pannello](triggering-an-animation-in-another-control-cs/_static/image2.png)](triggering-an-animation-in-another-control-cs/_static/image1.png)

Un clic del mouse sul pulsante Avvia l'animazione di pannello ([fare clic per visualizzare l'immagine con dimensioni normali](triggering-an-animation-in-another-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](disabling-actions-during-animation-cs.md)
> [Successivo](modifying-animations-from-the-server-side-cs.md)
