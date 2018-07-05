---
uid: web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-vb
title: Disabilitazione delle azioni durante l'animazione (VB) | Microsoft Docs
author: wenz
description: Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo. Supporta anche azioni...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: a86c0276-6481-46ee-8b4f-8c2009399ee9
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-vb
msc.type: authoredcontent
ms.openlocfilehash: 18c1eb74c73876b3fc6f1e37f69c31642806d043
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37830879"
---
<a name="disabling-actions-during-animation-vb"></a>Disabilitazione delle azioni durante l'animazione (VB)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare il codice](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7VB.pdf)

> Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo. Supporta anche azioni, ad esempio un clic del mouse. Tuttavia quando un clic del mouse viene avviata un'animazione, è consigliabile disabilitare i clic del mouse durante l'animazione.


## <a name="overview"></a>Panoramica

Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo. Supporta anche azioni, ad esempio un clic del mouse. Tuttavia quando un clic del mouse viene avviata un'animazione, è consigliabile disabilitare i clic del mouse durante l'animazione.

## <a name="steps"></a>Passaggi

Prima di tutto, includere il `ScriptManager` nella pagina; quindi, la libreria ASP.NET AJAX viene caricata, rendendo possibile usare il Toolkit di controllo:

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample1.aspx)]

Verrà applicata l'animazione a un pulsante HTML simile al seguente:

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample2.aspx)]

Si noti che un controllo HTML viene utilizzato invece di un controllo Web poiché non vogliamo pulsante per creare un postback; deve avviare l'animazione lato client per noi.

Quindi, aggiungere il `AnimationExtender` alla pagina, fornendo un' `ID`, il `TargetControlID` attributo e l'obbligatoria `runat="server"`:

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample3.aspx)]

All'interno di `<Animations>` nodo, `<OnClick>` è l'elemento a destra per gestire i clic del mouse. Tuttavia, potrebbe essere fatto clic sul pulsante durante l'animazione, nonché. Il `<EnableAction>` può gestire di tale elemento. Impostazione `Enabled="false"` disabilita il pulsante come parte dell'animazione. Poiché si sta usando diverse singole animazioni (disabilita il pulsante e le animazioni effettive), il `<Parallel>` elemento è obbligatorio per associare le animazioni insieme in una singole. Ecco il markup completo per `AnimationExtender`:

[!code-aspx[Main](disabling-actions-during-animation-vb/samples/sample4.aspx)]

È anche possibile abilitare nuovamente al pulsante dopo l'animazione, utilizzando l'elemento XML seguente alla fine dell'elenco:

[!code-xml[Main](disabling-actions-during-animation-vb/samples/sample5.xml)]

Tuttavia in questo scenario specifico questo sarebbe inutile perché il pulsante dissolve e non è visibile alla fine dell'animazione.


[![Il pulsante è disabilitato, non appena viene eseguita l'animazione](disabling-actions-during-animation-vb/_static/image2.png)](disabling-actions-during-animation-vb/_static/image1.png)

Il pulsante è disabilitato, non appena viene eseguita l'animazione ([fare clic per visualizzare l'immagine con dimensioni normali](disabling-actions-during-animation-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](animating-in-response-to-user-interaction-vb.md)
> [Successivo](triggering-an-animation-in-another-control-vb.md)
