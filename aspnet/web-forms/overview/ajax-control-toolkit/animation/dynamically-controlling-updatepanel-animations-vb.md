---
uid: web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-vb
title: Controllo dinamico delle animazioni UpdatePanel (VB) | Microsoft Docs
author: wenz
description: Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo. Per il contenuto di un...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: bea66072-59b6-42b4-98fa-211812f5925f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-vb
msc.type: authoredcontent
ms.openlocfilehash: b93ed1c7994c7561396298d876af213ae872f787
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37803655"
---
<a name="dynamically-controlling-updatepanel-animations-vb"></a>Controllo dinamico delle animazioni UpdatePanel (VB)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare il codice](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation2.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation2VB.pdf)

> Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo. Per il contenuto di un UpdatePanel, è presente una speciale estensione che si basa principalmente sul framework di animazione: UpdatePanelAnimation. Può inoltre funzionare insieme ai trigger UpdatePanel.


## <a name="overview"></a>Panoramica

Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo. Per il contenuto di un `UpdatePanel`, è presente una speciale estensione che si basa principalmente sul framework di animazione: `UpdatePanelAnimation`. Anche possibile operare insieme `UpdatePanel` trigger.

## <a name="steps"></a>Passaggi

Il primo passaggio consiste nel modo consueto per includere il `ScriptManager` nella pagina in modo che la libreria ASP.NET AJAX viene caricata e il Toolkit di controllo possono essere usato:


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample1.aspx)]

L'animazione in questo scenario verrà applicato a una visualizzazione dell'ora corrente. Queste informazioni possono essere scritti in un'etichetta usando il `Page_Load()` metodo, o (per motivi di semplicità) viene usato il codice inline seguente:


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample2.aspx)]

Viene creato anche un pulsante per attivare aggiornando l'ora:


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample3.aspx)]

Questo codice viene inserito quindi nel `<ContentTemplate>` sezione di un `UpdatePanel` elemento. Il pannello `UpdateMode` attributo deve essere impostato su `"Conditional"`, dal momento che solo i trigger possono aggiornare il contenuto del pannello. Nel `<Triggers>` sezione del `UpdatePanel`, viene creato e associato a un trigger di postback asincrono di `Click` evento del pulsante. In questo modo, se l'utente fa clic sul pulsante, il `UpdatePanel` viene aggiornato. Ecco il markup per il `UpdatePanel` controllo:


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample4.aspx)]

Infine, il `UpdatePanelAnimationExtender` deve essere configurato: impostare il `TargetControlID` attributo per l'ID del pannello e definire un'animazione entro il dispositivo extender. La dissolvenza in rende senso, che crea un risalto visivo interessante sull'ora dell'aggiornamento. Il markup di estensione potrebbero quindi simile al seguente:


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample5.aspx)]

Eseguire il file nel browser. Quando si fa clic sul pulsante, l'ora corrente viene visualizzata nel pannello dei sempre la dissolvenza in entrata per la durata di un secondo.


[![L'ora corrente è la dissolvenza in entrata](dynamically-controlling-updatepanel-animations-vb/_static/image2.png)](dynamically-controlling-updatepanel-animations-vb/_static/image1.png)

L'ora corrente è la dissolvenza in entrata ([fare clic per visualizzare l'immagine con dimensioni normali](dynamically-controlling-updatepanel-animations-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](animating-an-updatepanel-control-vb.md)
