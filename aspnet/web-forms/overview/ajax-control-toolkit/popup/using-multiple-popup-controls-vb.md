---
uid: web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-vb
title: Utilizzo di più controlli Popup (VB) | Documenti Microsoft
author: wenz
description: L'estensione PopupControl AJAX Control Toolkit offre un modo semplice per attivare una finestra popup quando viene attivato un altro controllo. È inoltre possibile utilizzare m...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 4da43d77-f6c4-43a8-9124-f1e8e1c8f0a2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: 7c57aab3ecf2c02a8488b5ea4e3e0ed33ac5e7fe
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="using-multiple-popup-controls-vb"></a>Utilizzo di più controlli Popup (VB)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare codice](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1VB.pdf)

> L'estensione PopupControl AJAX Control Toolkit offre un modo semplice per attivare una finestra popup quando viene attivato un altro controllo. È inoltre possibile utilizzare più di un controllo popup in un'unica pagina.


## <a name="overview"></a>Panoramica

L'estensione PopupControl AJAX Control Toolkit offre un modo semplice per attivare una finestra popup quando viene attivato un altro controllo. È inoltre possibile utilizzare più di un controllo popup in un'unica pagina.

## <a name="steps"></a>Passaggi

Per attivare le funzionalità di ASP.NET AJAX e il Toolkit di controllo, il `ScriptManager` controllo deve essere inserito in un punto qualsiasi della pagina (ma entro il `<form>` elemento):

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample1.aspx)]

Successivamente, aggiungere un pannello che funge da finestra popup. In questo scenario, il pannello contiene un `Calendar` controllo. Per evitare la pagina viene aggiornata dovuta postback del calendario, il pannello viene inserito all'interno di un `UpdatePanel` controllo:

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample2.aspx)]

La pagina contiene inoltre due caselle di testo. Per ogni casella di testo, appare la finestra popup di calendario dopo aver attivata la casella di testo.

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample3.aspx)]

Ora estendere ciascuna delle due caselle di testo con un `PopupControlExtender`. Il `TargetControlID` attributo fornisce l'ID del controllo associato all'oggetto di estensione. Il `PopupControlID` attributo contiene l'ID del pannello del popup. In questo caso, entrambi Extender Mostra stesso riquadro, ma sono anche possibili, diversi pannelli.

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample4.aspx)]

Ora ogni volta che si fa clic all'interno di un campo di testo, viene visualizzato un calendario sotto il campo, che consente di selezionare una data. (Ottenere la data selezionata nelle caselle di testo verrà trattato in un'esercitazione diversa.)


[![Il calendario viene visualizzato quando l'utente fa clic nella casella di testo](using-multiple-popup-controls-vb/_static/image2.png)](using-multiple-popup-controls-vb/_static/image1.png)

Il calendario viene visualizzato quando l'utente fa clic nella casella di testo ([fare clic per visualizzare l'immagine ingrandita](using-multiple-popup-controls-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)
> [Successivo](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb.md)
