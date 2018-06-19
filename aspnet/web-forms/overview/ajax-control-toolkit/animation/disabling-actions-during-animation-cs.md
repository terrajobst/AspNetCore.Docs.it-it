---
uid: web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-cs
title: La disabilitazione delle azioni durante l'animazione (c#) | Documenti Microsoft
author: wenz
description: Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework per aggiungere le animazioni a un controllo. Supporta inoltre l'azione...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 918026b4-2f63-421d-8546-df12856960a8
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-cs
msc.type: authoredcontent
ms.openlocfilehash: 7862c5026a48fbee6eb48beb411e5e1d60c8b406
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870817"
---
<a name="disabling-actions-during-animation-c"></a>La disabilitazione delle azioni durante l'animazione (c#)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare codice](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7CS.pdf)

> Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework per aggiungere le animazioni a un controllo. Supporta inoltre le azioni, ad esempio un clic del mouse. Tuttavia, quando un clic del mouse viene avviata un'animazione, è opportuno disabilitare clic del mouse durante l'animazione.


## <a name="overview"></a>Panoramica

Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework per aggiungere le animazioni a un controllo. Supporta inoltre le azioni, ad esempio un clic del mouse. Tuttavia, quando un clic del mouse viene avviata un'animazione, è opportuno disabilitare clic del mouse durante l'animazione.

## <a name="steps"></a>Passaggi

Prima di tutto, includere il `ScriptManager` nella pagina; quindi, la libreria AJAX di ASP.NET viene caricata, che consente di usare il Toolkit di controllo:

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample1.aspx)]

Verrà applicata l'animazione a un pulsante HTML simile al seguente:

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample2.aspx)]

Si noti che un controllo HTML viene utilizzato invece di un controllo Web poiché non si desidera che il pulsante per creare un postback; deve essere avviato solo l'animazione sul lato client per noi.

Quindi, aggiungere il `AnimationExtender` alla pagina, fornendo un `ID`, `TargetControlID` attributo e l'obbligatoria `runat="server"`:

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample3.aspx)]

All'interno di `<Animations>` nodo `<OnClick>` è l'elemento corretto per gestire i clic del mouse. Tuttavia, è stato scelto il pulsante durante l'animazione. Il `<EnableAction>` possibile definite in un elemento. Impostazione `Enabled="false"` disabilita il pulsante come parte dell'animazione. Poiché si sono utilizzando numerose animazioni singoli (disabilita il pulsante e le animazioni effettivi), il `<Parallel>` deve associare le animazioni insieme in un singole elemento. Ecco il codice completo per `AnimationExtender`:

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample4.aspx)]

Anche possibile abilitare nuovamente al pulsante dopo l'animazione, utilizzando l'elemento XML seguente alla fine dell'elenco:

[!code-xml[Main](disabling-actions-during-animation-cs/samples/sample5.xml)]

Tuttavia in questo scenario specifico questo sarebbe inutile poiché pulsante Dissolvenza in uscita e non è visibile alla fine dell'animazione.


[![Il pulsante è disabilitato, non appena viene eseguito l'animazione](disabling-actions-during-animation-cs/_static/image2.png)](disabling-actions-during-animation-cs/_static/image1.png)

Il pulsante è disabilitato, non appena viene eseguita l'animazione ([fare clic per visualizzare l'immagine ingrandita](disabling-actions-during-animation-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](animating-in-response-to-user-interaction-cs.md)
> [Successivo](triggering-an-animation-in-another-control-cs.md)
