---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-vb
title: Gestione dei postback da un controllo Popup senza UpdatePanel (VB) | Microsoft Docs
author: wenz
description: Il dispositivo extender PopupControl in AJAX Control Toolkit offre un modo semplice per attivare una finestra popup quando viene attivato un qualsiasi altro controllo. Quando si verifica un postback in unità di streaming...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: a0b9186c-0912-4fff-916a-6d17e696a50b
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-vb
msc.type: authoredcontent
ms.openlocfilehash: 8530d62b931782af662cd6445db78d666b32e329
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37808194"
---
<a name="handling-postbacks-from-a-popup-control-without-an-updatepanel-vb"></a>Gestione dei postback da un controllo Popup senza UpdatePanel (VB)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare il codice](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3VB.pdf)

> Il dispositivo extender PopupControl in AJAX Control Toolkit offre un modo semplice per attivare una finestra popup quando viene attivato un qualsiasi altro controllo. Quando si verifica un postback in un pannello di questo tipo e sono presenti diversi pannelli nella pagina è difficile individuare il pannello è stato selezionato.


## <a name="overview"></a>Panoramica

Il dispositivo extender PopupControl in AJAX Control Toolkit offre un modo semplice per attivare una finestra popup quando viene attivato un qualsiasi altro controllo. Quando si verifica un postback in un pannello di questo tipo e sono presenti diversi pannelli nella pagina è difficile individuare il pannello è stato selezionato.

## <a name="steps"></a>Passaggi

Quando si usa un' `PopupControl` con un postback, ma senza che sia un `UpdatePanel` nella pagina, il Toolkit di controllo non offre un modo per determinare quale elemento client ha avviato la finestra popup che a sua volta ha causato il postback. Tuttavia un piccolo trucco fornisce una soluzione alternativa per questo scenario.

Prima di tutto, ecco la configurazione di base: due caselle di testo che attivano il popup stesso, un calendario. Due `PopupControlExtenders` riunire le caselle di testo e finestra popup.

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample1.aspx)]

L'idea di base consiste nell'aggiungere un campo del form nascosto il &lt; `form` &gt; elemento che contiene la casella di testo quale avviata la finestra popup:

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample2.aspx)]

Quando la pagina viene caricata, il codice JavaScript aggiunge un gestore eventi per entrambe le caselle di testo: ogni volta che l'utente fa clic sulla casella di testo, il relativo nome viene scritto nel campo del form nascosto:

[!code-html[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample3.html)]

Nel codice lato server, è necessario leggere il valore del campo nascosto. Poiché sono semplici per modificare i campi modulo nascosti, è necessario un approccio di elenco elementi consentiti per la convalida del valore nascosto. Dopo aver individuata la casella di testo corretto, la data dal calendario viene scritta al suo interno.

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample4.aspx)]


[![Quando l'utente fa clic nella casella di testo viene visualizzato il calendario](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image1.png)

Il calendario viene visualizzato quando l'utente fa clic nella casella di testo ([fare clic per visualizzare l'immagine con dimensioni normali](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image3.png))


[![Facendo clic su una data lo inserisce nella casella di testo](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image4.png)

Facendo clic su una data lo inserisce nella casella di testo ([fare clic per visualizzare l'immagine con dimensioni normali](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image6.png))

> [!div class="step-by-step"]
> [Precedente](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb.md)
