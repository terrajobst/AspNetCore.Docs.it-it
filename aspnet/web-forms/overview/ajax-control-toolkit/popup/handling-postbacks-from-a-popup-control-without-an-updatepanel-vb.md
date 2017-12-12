---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-vb
title: Gestione dei postback da un controllo Popup senza un UpdatePanel (VB) | Documenti Microsoft
author: wenz
description: L'estensione PopupControl AJAX Control Toolkit offre un modo semplice per attivare una finestra popup quando viene attivato un altro controllo. Quando si verifica un postback in su...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: a0b9186c-0912-4fff-916a-6d17e696a50b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-vb
msc.type: authoredcontent
ms.openlocfilehash: 7c4afee37eab33036e5e563e78f873275951700b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="handling-postbacks-from-a-popup-control-without-an-updatepanel-vb"></a>Gestione dei postback da un controllo Popup senza un UpdatePanel (VB)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare codice](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3VB.pdf)

> L'estensione PopupControl AJAX Control Toolkit offre un modo semplice per attivare una finestra popup quando viene attivato un altro controllo. Quando si verifica un postback in tali pannelli e sono presenti diversi riquadri della pagina è difficile determinare quale pannello è stato fatto clic.


## <a name="overview"></a>Panoramica

L'estensione PopupControl AJAX Control Toolkit offre un modo semplice per attivare una finestra popup quando viene attivato un altro controllo. Quando si verifica un postback in tali pannelli e sono presenti diversi riquadri della pagina è difficile determinare quale pannello è stato fatto clic.

## <a name="steps"></a>Passaggi

Quando si utilizza un `PopupControl` con un postback, ma senza un `UpdatePanel` nella pagina, il Toolkit di controllo non offre un modo per determinare quale elemento client è attivata la finestra popup che a sua volta ha provocato il postback. Un piccolo stratagemma tuttavia offre una soluzione alternativa per questo scenario.

Ecco prima di tutto, il programma di installazione di base: due caselle di testo che attivano la stessa finestra popup, un calendario. Due `PopupControlExtenders` riunire le caselle di testo e popup.

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample1.aspx)]

L'idea di base consiste nell'aggiungere un campo del form nascosto il &lt; `form` &gt; elemento che contiene la casella di testo quale avviata la finestra popup:

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample2.aspx)]

Quando viene caricata la pagina, il codice JavaScript aggiunge un gestore eventi per entrambe le caselle di testo: ogni volta che l'utente fa clic su una casella di testo, il relativo nome viene scritto nel campo del form nascosto:

[!code-html[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample3.html)]

Nel codice sul lato server, è necessario leggere il valore del campo nascosto. Poiché i campi del form nascosto sono semplici da gestire, è necessario un approccio di whitelist per convalidare il valore hidden. Dopo aver individuata la casella di testo corretto, la data dal calendario viene scritto.

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample4.aspx)]


[![Il calendario viene visualizzato quando l'utente fa clic nella casella di testo](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image1.png)

Il calendario viene visualizzato quando l'utente fa clic nella casella di testo ([fare clic per visualizzare l'immagine ingrandita](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image3.png))


[![Facendo clic su una data lo inserisce nella casella di testo](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image4.png)

Facendo clic su una data lo inserisce nella casella di testo ([fare clic per visualizzare l'immagine ingrandita](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image6.png))

>[!div class="step-by-step"]
[Precedente](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb.md)
