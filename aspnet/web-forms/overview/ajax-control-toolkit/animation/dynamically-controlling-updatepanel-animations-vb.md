---
uid: web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-vb
title: Controllare in modo dinamico le animazioni UpdatePanel (VB) | Documenti Microsoft
author: wenz
description: Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework per aggiungere le animazioni a un controllo. Per il contenuto di un...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: bea66072-59b6-42b4-98fa-211812f5925f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-vb
msc.type: authoredcontent
ms.openlocfilehash: ff2853b4457a83a7367b4d1072d21929c40a3ed2
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30871545"
---
<a name="dynamically-controlling-updatepanel-animations-vb"></a>Controllare in modo dinamico le animazioni UpdatePanel (VB)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare codice](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation2.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation2VB.pdf)

> Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework per aggiungere le animazioni a un controllo. Per il contenuto dell'UpdatePanel, è presente una speciale estensione che si basa su framework animazione: UpdatePanelAnimation. Può inoltre interagire con i trigger di UpdatePanel.


## <a name="overview"></a>Panoramica

Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework per aggiungere le animazioni a un controllo. Per il contenuto di un `UpdatePanel`, una speciale estensione esistente che si basa su framework animazione: `UpdatePanelAnimation`. Anche possibile operare insieme `UpdatePanel` trigger.

## <a name="steps"></a>Passaggi

Il primo passaggio consiste nel modo consueto per includere il `ScriptManager` nella pagina in modo che la libreria ASP.NET AJAX è caricata e può essere utilizzato il Toolkit di controllo:


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample1.aspx)]

In questo scenario l'animazione verrà applicato a una visualizzazione dell'ora corrente. Queste informazioni possono essere scritti in un'etichetta usando il `Page_Load()` metodo, o (per ragioni di semplicità) viene utilizzato il seguente codice inline:


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample2.aspx)]

Inoltre, viene creato un pulsante per attivare il tempo di aggiornamento:


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample3.aspx)]

Questo codice viene quindi inserito il `<ContentTemplate>` sezione di un `UpdatePanel` elemento. Il pannello `UpdateMode` attributo deve essere impostato su `"Conditional"`, poiché solo i trigger possono aggiornare il contenuto del pannello. Nel `<Triggers>` sezione del `UpdatePanel`, un trigger di postback asincrono viene creato e associato al `Click` evento del pulsante. Pertanto, se l'utente fa clic sul pulsante, il `UpdatePanel` viene aggiornato. Ecco il markup per il `UpdatePanel` controllo:


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample4.aspx)]

Infine, il `UpdatePanelAnimationExtender` deve essere configurato: impostare il `TargetControlID` attributo per l'ID del pannello e definire un'animazione all'interno del programma di estensione. Dissolvenza in permette senso, che consente di creare un particolare visual nice l'ora dell'aggiornamento. Il markup di estensione può quindi simile al seguente:


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample5.aspx)]

Eseguire il file nel browser. Quando si fa clic sul pulsante, l'ora corrente viene visualizzata nel pannello, sempre la dissolvenza in entrata per la durata di un secondo.


[![L'ora corrente è la dissolvenza in entrata](dynamically-controlling-updatepanel-animations-vb/_static/image2.png)](dynamically-controlling-updatepanel-animations-vb/_static/image1.png)

L'ora corrente è la dissolvenza in entrata ([fare clic per visualizzare l'immagine ingrandita](dynamically-controlling-updatepanel-animations-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](animating-an-updatepanel-control-vb.md)
