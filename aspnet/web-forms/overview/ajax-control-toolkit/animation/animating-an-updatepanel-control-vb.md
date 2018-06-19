---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-vb
title: Animazione di un controllo UpdatePanel (VB) | Documenti Microsoft
author: wenz
description: Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework per aggiungere le animazioni a un controllo. Per il contenuto di un...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 4c306a2c-92b6-4904-b70b-365b847334fe
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 2c1114b74fd152a4ea85aa10850860f75573adee
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873157"
---
<a name="animating-an-updatepanel-control-vb"></a>Animazione di un controllo UpdatePanel (VB)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare codice](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation1.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation1VB.pdf)

> Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework per aggiungere le animazioni a un controllo. Per il contenuto dell'UpdatePanel, è presente una speciale estensione che si basa su framework animazione: UpdatePanelAnimation. In questa esercitazione viene illustrato come impostare tali un'animazione per un UpdatePanel.


## <a name="overview"></a>Panoramica

Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework per aggiungere le animazioni a un controllo. Per il contenuto di un `UpdatePanel`, una speciale estensione esistente che si basa su framework animazione: `UpdatePanelAnimation`. Questa esercitazione viene illustrato come configurare tali un'animazione per un `UpdatePanel`.

## <a name="steps"></a>Passaggi

Il primo passaggio consiste nel modo consueto per includere il `ScriptManager` nella pagina in modo che la libreria ASP.NET AJAX è caricata e può essere utilizzato il Toolkit di controllo:

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample1.aspx)]

In questo scenario l'animazione verrà applicato a un ASP.NET `Wizard` controllo web che si trova in un `UpdatePanel`. Tre passaggi (arbitrari) forniscono sufficiente opzioni per attivare i postback:

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample2.aspx)]

Il codice necessario per il `UpdatePanelAnimationExtender` controllo è simile al markup usato per il `AnimationExtender`. Nel `TargetControlID` attributo forniamo il `ID` del `UpdatePanel` animare; all'interno di `UpdatePanelAnimationExtender` (controllo), il `<Animations>` elemento contiene il markup XML dell'animazione. È tuttavia una differenza: la quantità di eventi e gestori eventi è limitata alla `AnimationExtender`. Per `UpdatePanels`, solo due di essi esiste:

- `<OnUpdated>` Quando l'UpdatePanel è stato aggiornato
- `<OnUpdating>` Quando l'UpdatePanel avvia l'aggiornamento

In questo scenario, il contenuto di nuovo il `UpdatePanel` (dopo il postback) sono dissolvenza in entrata. Questo è il codice necessario per che:

[!code-aspx[Main](animating-an-updatepanel-control-vb/samples/sample3.aspx)]

Ora ogni volta che si verifica un postback all'interno di UpdatePanel, il contenuto del Pannello di dissolvenza in entrata in modo uniforme.


[![Il passaggio successivo della procedura guidata è la dissolvenza in entrata](animating-an-updatepanel-control-vb/_static/image2.png)](animating-an-updatepanel-control-vb/_static/image1.png)

Il passaggio successivo della procedura guidata è la dissolvenza in entrata ([fare clic per visualizzare l'immagine ingrandita](animating-an-updatepanel-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](changing-an-animation-using-client-side-code-vb.md)
> [Successivo](dynamically-controlling-updatepanel-animations-vb.md)
