---
uid: web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-vb
title: Uso di più controlli Popup (VB) | Microsoft Docs
author: wenz
description: Il dispositivo extender PopupControl in AJAX Control Toolkit offre un modo semplice per attivare una finestra popup quando viene attivato un qualsiasi altro controllo. È anche possibile usare m...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 4da43d77-f6c4-43a8-9124-f1e8e1c8f0a2
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: accb9134cc355e0c042114873973bcbfe9a4a0a4
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37384561"
---
<a name="using-multiple-popup-controls-vb"></a>Uso di più controlli Popup (VB)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare il codice](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1VB.pdf)

> Il dispositivo extender PopupControl in AJAX Control Toolkit offre un modo semplice per attivare una finestra popup quando viene attivato un qualsiasi altro controllo. È anche possibile usare più di un controllo popup in un'unica pagina.


## <a name="overview"></a>Panoramica

Il dispositivo extender PopupControl in AJAX Control Toolkit offre un modo semplice per attivare una finestra popup quando viene attivato un qualsiasi altro controllo. È anche possibile usare più di un controllo popup in un'unica pagina.

## <a name="steps"></a>Passaggi

Per attivare la funzionalità di ASP.NET AJAX e il Toolkit di controllo, il `ScriptManager` controllo deve essere inserito in un punto qualsiasi della pagina (ma entro la `<form>` elemento):

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample1.aspx)]

Successivamente, aggiungere un pannello che funge da finestra popup. Nello scenario corrente, il pannello contiene un `Calendar` controllo. Per evitare la pagina viene aggiornata causata dai postback del calendario, il pannello viene inserito all'interno di un `UpdatePanel` controllo:

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample2.aspx)]

La pagina contiene inoltre due caselle di testo. Per ogni casella di testo, il calendario popup verranno visualizzati dopo aver attivata la casella di testo.

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample3.aspx)]

A questo punto estendere ciascuna delle due caselle di testo con un `PopupControlExtender`. Il `TargetControlID` attributo fornisce l'ID del controllo associato all'oggetto extender. Il `PopupControlID` attributo contiene l'ID del pannello popup. In questo caso, entrambe le estensioni di visualizzare il pannello stesso, ma sono anche possibili, diversi pannelli.

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample4.aspx)]

Ora ogni volta che si fa clic all'interno di un campo di testo, viene visualizzato un calendario sotto il campo, è possibile selezionare una data. (Ricevendo la data selezionata nelle caselle di testo verrà trattato in un'esercitazione diversa.)


[![Quando l'utente fa clic nella casella di testo viene visualizzato il calendario](using-multiple-popup-controls-vb/_static/image2.png)](using-multiple-popup-controls-vb/_static/image1.png)

Il calendario viene visualizzato quando l'utente fa clic nella casella di testo ([fare clic per visualizzare l'immagine con dimensioni normali](using-multiple-popup-controls-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)
> [Successivo](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb.md)
