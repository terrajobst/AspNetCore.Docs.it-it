---
uid: web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-cs
title: Generazione di un'animazione in un altro controllo (c#) | Documenti Microsoft
author: wenz
description: Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework per aggiungere le animazioni a un controllo. Avvio in genere, un...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e5d99c2b-d8ee-413c-80d5-c120cffb0a4c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-cs
msc.type: authoredcontent
ms.openlocfilehash: e94046ca70607e37c1b5ef57d5cedef67a236b94
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="triggering-an-animation-in-another-control-c"></a>Generazione di un'animazione in un altro controllo (c#)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare codice](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8CS.pdf)

> Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework per aggiungere le animazioni a un controllo. In genere, l'avvio di un'animazione viene attivata dall'interazione dell'utente con il controllo stesso. È tuttavia anche possibile interagire con un controllo e quindi animazione a un altro controllo.


## <a name="overview"></a>Panoramica

Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework per aggiungere le animazioni a un controllo. In genere, l'avvio di un'animazione viene attivata dall'interazione dell'utente con il controllo stesso. È tuttavia anche possibile interagire con un controllo e quindi animazione a un altro controllo.

## <a name="steps"></a>Passaggi

Prima di tutto, includere il `ScriptManager` nella pagina; quindi, la libreria AJAX di ASP.NET viene caricata, che consente di usare il Toolkit di controllo:

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample1.aspx)]

Verrà applicata l'animazione a un riquadro del testo che presenta un aspetto simile al seguente:

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample2.aspx)]

Nella classe CSS associata per il pannello, definire un colore di sfondo nice e inoltre impostare una larghezza fissa per il pannello:

[!code-css[Main](triggering-an-animation-in-another-control-cs/samples/sample3.css)]

Per avviare il pannello Animazione, viene utilizzato un pulsante HTML. Si noti che `<input type="button" />` è favoriti rispetto `<asp:Button />` poiché non è desiderabile che un postback quando l'utente fa clic sul pulsante.

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample4.aspx)]

Quindi, aggiungere il `AnimationExtender` alla pagina, fornendo un `ID`, `TargetControlID` attributo e l'obbligatoria `runat="server"`. È importante impostare `TargetControlID` per l'ID del pulsante (elemento di attivazione dell'animazione), non per l'ID del pannello (l'elemento che viene animata)

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample5.aspx)]

All'interno di `<Animations>` nodo, le animazioni sul posto come al solito. Per renderle il pannello di modifica, non sul pulsante, impostare il `AnimationTarget` attributo per ogni elemento animazione `AnimationExtender`. Il valore per `AnimationTarget` è l'ID del pannello, ovviamente. In questo modo, le animazioni verificarsi con il pannello, non con il pulsante di attivazione. Ecco il `AnimationExtender` markup per questo scenario:

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample6.aspx)]

Si noti l'ordine speciale in cui vengono visualizzate le animazioni singoli. Prima di tutto, il pulsante viene disattivato quando viene eseguito l'animazione. Poiché non esiste alcun `AnimationTarget` attributo il `<EnableAction>` elemento, questa animazione viene applicata al controllo origine: il pulsante. I passaggi successivi due animazione devono essere effettuati parallelly (`<Parallel>` elemento). Entrambi hanno loro `AnimationTarget` attributi impostati `"Panel1"`, l'animazione in questo modo il pannello, non il pulsante.


[![Un clic del mouse sul pulsante Avvia l'animazione pannello](triggering-an-animation-in-another-control-cs/_static/image2.png)](triggering-an-animation-in-another-control-cs/_static/image1.png)

Un clic del mouse sul pulsante Avvia l'animazione di pannello ([fare clic per visualizzare l'immagine ingrandita](triggering-an-animation-in-another-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](disabling-actions-during-animation-cs.md)
> [Successivo](modifying-animations-from-the-server-side-cs.md)
