---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-vb
title: Animazione di un controllo UpdatePanel (VB) | Microsoft Docs
author: wenz
description: Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo. Per il contenuto di un...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 4c306a2c-92b6-4904-b70b-365b847334fe
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-vb
msc.type: authoredcontent
ms.openlocfilehash: c508d549c7740079b02da323c655c9f789cfd3b9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37805301"
---
<a name="animating-an-updatepanel-control-vb"></a>Animazione di un controllo UpdatePanel (VB)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare il codice](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation1.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation1VB.pdf)

> Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo. Per il contenuto di un UpdatePanel, è presente una speciale estensione che si basa principalmente sul framework di animazione: UpdatePanelAnimation. Questa esercitazione illustra come configurare un'animazione di questo tipo per un controllo UpdatePanel.


## <a name="overview"></a>Panoramica

Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo. Per il contenuto di un `UpdatePanel`, è presente una speciale estensione che si basa principalmente sul framework di animazione: `UpdatePanelAnimation`. Questa esercitazione illustra come configurare un'animazione di questo tipo per un `UpdatePanel`.

## <a name="steps"></a>Passaggi

Il primo passaggio consiste nel modo consueto per includere il `ScriptManager` nella pagina in modo che la libreria ASP.NET AJAX viene caricata e il Toolkit di controllo possono essere usato:

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample1.aspx)]

L'animazione in questo scenario verrà applicata a un ASP.NET `Wizard` controllo web che si trova in un `UpdatePanel`. Tre passaggi (arbitrari) offrono sufficienti opzioni per attivare i postback:

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample2.aspx)]

Il markup necessario per il `UpdatePanelAnimationExtender` controllo è abbastanza simile al markup utilizzato per il `AnimationExtender`. Nel `TargetControlID` attributo forniamo il `ID` del `UpdatePanel` per aggiungere un'animazione; all'interno del `UpdatePanelAnimationExtender` controllo, il `<Animations>` elemento contiene markup XML per le animazioni. Tuttavia c'è una differenza: la quantità di eventi e gestori eventi è limitata alla `AnimationExtender`. Per `UpdatePanels`, solo due di esse esiste:

- `<OnUpdated>` Quando è stato aggiornato UpdatePanel
- `<OnUpdating>` Avvio aggiornamento di UpdatePanel

In questo scenario, il contenuto di nuovo il `UpdatePanel` (dopo il postback) devono dissolvenza in entrata. Questo è il markup necessario per che:

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample3.aspx)]

Ora ogni volta che si verifica un postback all'interno di UpdatePanel, il nuovo contenuto del Pannello di dissolvenza in entrata senza problemi.


[![Il passaggio successivo della procedura guidata è la dissolvenza in entrata](animating-an-updatepanel-control-vb/_static/image2.png)](animating-an-updatepanel-control-vb/_static/image1.png)

Il passaggio successivo della procedura guidata è la dissolvenza in entrata ([fare clic per visualizzare l'immagine con dimensioni normali](animating-an-updatepanel-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](changing-an-animation-using-client-side-code-vb.md)
> [Successivo](dynamically-controlling-updatepanel-animations-vb.md)
