---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-cs
title: Gestione dei postback da un controllo Popup con un UpdatePanel (c#) | Documenti Microsoft
author: wenz
description: L'estensione PopupControl AJAX Control Toolkit offre un modo semplice per attivare una finestra popup quando viene attivato un altro controllo. Particolare attenzione deve essere eseguita...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 1f68f59d-9c1e-4cf3-b304-c13ae6b7203e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-cs
msc.type: authoredcontent
ms.openlocfilehash: abedb5247f710b02752651a7bfb011ab63d32844
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879634"
---
<a name="handling-postbacks-from-a-popup-control-with-an-updatepanel-c"></a>Gestione dei postback da un controllo Popup con un UpdatePanel (c#)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare codice](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2CS.pdf)

> L'estensione PopupControl AJAX Control Toolkit offre un modo semplice per attivare una finestra popup quando viene attivato un altro controllo. Particolare attenzione è da eseguire quando si verifica un postback all'interno di questo tipo una finestra popup.


## <a name="overview"></a>Panoramica

L'estensione PopupControl AJAX Control Toolkit offre un modo semplice per attivare una finestra popup quando viene attivato un altro controllo. Particolare attenzione è da eseguire quando si verifica un postback all'interno di questo tipo una finestra popup.

## <a name="steps"></a>Passaggi

Quando si utilizza un `PopupControl` con un postback, un `UpdatePanel` può impedire l'aggiornamento della pagina causato dal postback. Il markup seguente definisce un paio di aspetti importanti:

- Oggetto `ScriptManager` controllare in modo che il funzionamento di ASP.NET AJAX Control Toolkit
- Due `TextBox` controlli che verranno attivata una finestra popup
- Oggetto `Panel` controllo che verrà utilizzato come finestra popup
- All'interno del pannello, un `Calendar` controllo incorporato all'interno di un `UpdatePanel` controllo
- Due `PopupControlExtender` i controlli che assegna il pannello alle caselle di testo

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/samples/sample1.aspx)]

Si noti che il `OnSelectionChanged` attributo del `Calendar` NFS è impostata. Pertanto quando l'utente seleziona una data nel calendario, si verifica un postback e il metodo lato server `c1_SelectionChanged()` viene eseguita. All'interno di tale metodo, è necessario recuperare la data corrente e writeback per la casella di testo.

La sintassi necessaria è la seguente: prima di tutto, un proxy dell'oggetto per il `PopupControlExtender` della pagina deve essere generato. ASP.NET AJAX Control Toolkit offre il `GetProxyForCurrentPopup()` metodo. Supporta l'oggetto di questo metodo restituisce il `Commit()` metodo che invia un valore al controllo che ha attivato il popup (non il controllo che ha attivato la chiamata al metodo!). Il codice seguente fornisce la data selezionata come argomento per il `Commit()` metodo causando il codice per scrivere la data selezionata la casella di testo:

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/samples/sample2.aspx)]

Ora ogni volta che si fa clic su una data di calendario, viene visualizzata la data selezionata nella casella di testo associato, la creazione di un controllo selezione data che attualmente sono disponibili in molti siti Web.


[![Il calendario viene visualizzato quando l'utente fa clic nella casella di testo](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image1.png)

Il calendario viene visualizzato quando l'utente fa clic nella casella di testo ([fare clic per visualizzare l'immagine ingrandita](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image3.png))


[![Facendo clic su una data lo inserisce nella casella di testo](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image4.png)

Facendo clic su una data lo inserisce nella casella di testo ([fare clic per visualizzare l'immagine ingrandita](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image6.png))

> [!div class="step-by-step"]
> [Precedente](using-multiple-popup-controls-cs.md)
> [Successivo](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)
