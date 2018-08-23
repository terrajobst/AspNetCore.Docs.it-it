---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-cs
title: Gestione dei postback da un controllo Popup con UpdatePanel (c#) | Microsoft Docs
author: wenz
description: Il dispositivo extender PopupControl in AJAX Control Toolkit offre un modo semplice per attivare una finestra popup quando viene attivato un qualsiasi altro controllo. Deve essere adottata particolare attenzione...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 1f68f59d-9c1e-4cf3-b304-c13ae6b7203e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-cs
msc.type: authoredcontent
ms.openlocfilehash: 0f886ba0f3e79bc6d5daf193eaedfd627bc9b937
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833987"
---
<a name="handling-postbacks-from-a-popup-control-with-an-updatepanel-c"></a>Gestione dei postback da un controllo Popup con UpdatePanel (c#)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare il codice](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2CS.pdf)

> Il dispositivo extender PopupControl in AJAX Control Toolkit offre un modo semplice per attivare una finestra popup quando viene attivato un qualsiasi altro controllo. Particolare attenzione è da eseguire quando si verifica un postback all'interno di questo tipo una finestra popup.


## <a name="overview"></a>Panoramica

Il dispositivo extender PopupControl in AJAX Control Toolkit offre un modo semplice per attivare una finestra popup quando viene attivato un qualsiasi altro controllo. Particolare attenzione è da eseguire quando si verifica un postback all'interno di questo tipo una finestra popup.

## <a name="steps"></a>Passaggi

Quando si usa un' `PopupControl` con un postback, un `UpdatePanel` può impedire l'aggiornamento di pagina causato dal postback. Il markup seguente definisce un paio di aspetti importanti:

- Oggetto `ScriptManager` controllare in modo che funzioni ASP.NET AJAX Control Toolkit
- Due `TextBox` controlli che verranno attivata una finestra popup
- Oggetto `Panel` controllo che servirà come il controllo popup
- All'interno del pannello, una `Calendar` controllo incorporato all'interno di un `UpdatePanel` controllo
- Due `PopupControlExtender` controlli che assegnano il pannello per le caselle di testo

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/samples/sample1.aspx)]

Si noti che il `OnSelectionChanged` attributo del `Calendar` NFS è impostata. In modo che quando l'utente seleziona una data all'interno del calendario, si verifica un postback e il metodo lato server `c1_SelectionChanged()` viene eseguita. All'interno di tale metodo, la data corrente deve essere recuperata e il writeback per la casella di testo.

La sintassi che è il seguente: prima di tutto, un proxy dell'oggetto per il `PopupControlExtender` nella pagina deve essere generato. ASP.NET AJAX Control Toolkit offre il `GetProxyForCurrentPopup()` (metodo). L'oggetto restituisce questo metodo supporta il `Commit()` metodo che restituisce un valore al controllo che ha attivato il controllo popup (non il controllo che ha attivato la chiamata al metodo!). Il codice seguente fornisce la data selezionata come argomento per il `Commit()` metodo causando il codice da scrivere nuovamente la data selezionata nella casella di testo:

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/samples/sample2.aspx)]

Ora ogni volta che si fa clic su una data del calendario, la data selezionata viene visualizzato nella casella di testo associato, la creazione di un controllo selezione data che può attualmente disponibili in molti siti Web.


[![Quando l'utente fa clic nella casella di testo viene visualizzato il calendario](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image1.png)

Il calendario viene visualizzato quando l'utente fa clic nella casella di testo ([fare clic per visualizzare l'immagine con dimensioni normali](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image3.png))


[![Facendo clic su una data lo inserisce nella casella di testo](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image4.png)

Facendo clic su una data lo inserisce nella casella di testo ([fare clic per visualizzare l'immagine con dimensioni normali](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image6.png))

> [!div class="step-by-step"]
> [Precedente](using-multiple-popup-controls-cs.md)
> [Successivo](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)
