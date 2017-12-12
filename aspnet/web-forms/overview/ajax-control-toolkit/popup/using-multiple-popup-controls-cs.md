---
uid: web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-cs
title: "Utilizzo di più controlli Popup (c#) | Documenti Microsoft"
author: wenz
description: "L'estensione PopupControl AJAX Control Toolkit offre un modo semplice per attivare una finestra popup quando viene attivato un altro controllo. È inoltre possibile utilizzare m..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 91511b0b-311d-481f-9e7c-73f07b813b79
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: d8c9742bb39b655115cb1862ff6e867989e63665
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="using-multiple-popup-controls-c"></a>Utilizzo di più controlli Popup (c#)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare codice](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1CS.pdf)

> L'estensione PopupControl AJAX Control Toolkit offre un modo semplice per attivare una finestra popup quando viene attivato un altro controllo. È inoltre possibile utilizzare più di un controllo popup in un'unica pagina.


## <a name="overview"></a>Panoramica

L'estensione PopupControl AJAX Control Toolkit offre un modo semplice per attivare una finestra popup quando viene attivato un altro controllo. È inoltre possibile utilizzare più di un controllo popup in un'unica pagina.

## <a name="steps"></a>Passaggi

Per attivare le funzionalità di ASP.NET AJAX e il Toolkit di controllo, il `ScriptManager` controllo deve essere inserito in un punto qualsiasi della pagina (ma entro il `<form>` elemento):

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample1.aspx)]

Successivamente, aggiungere un pannello che funge da finestra popup. In questo scenario, il pannello contiene un `Calendar` controllo. Per evitare la pagina viene aggiornata dovuta postback del calendario, il pannello viene inserito all'interno di un `UpdatePanel` controllo:

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample2.aspx)]

La pagina contiene inoltre due caselle di testo. Per ogni casella di testo, appare la finestra popup di calendario dopo aver attivata la casella di testo.

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample3.aspx)]

Ora estendere ciascuna delle due caselle di testo con un `PopupControlExtender`. Il `TargetControlID` attributo fornisce l'ID del controllo associato all'oggetto di estensione. Il `PopupControlID` attributo contiene l'ID del pannello del popup. In questo caso, entrambi Extender Mostra stesso riquadro, ma sono anche possibili, diversi pannelli.

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample4.aspx)]

Ora ogni volta che si fa clic all'interno di un campo di testo, viene visualizzato un calendario sotto il campo, che consente di selezionare una data. (Ottenere la data selezionata nelle caselle di testo verrà trattato in un'esercitazione diversa.)


[![Il calendario viene visualizzato quando l'utente fa clic nella casella di testo](using-multiple-popup-controls-cs/_static/image2.png)](using-multiple-popup-controls-cs/_static/image1.png)

Il calendario viene visualizzato quando l'utente fa clic nella casella di testo ([fare clic per visualizzare l'immagine ingrandita](using-multiple-popup-controls-cs/_static/image3.png))

>[!div class="step-by-step"]
[Successivo](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs.md)
