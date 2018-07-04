---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-cs
title: Gestione dei postback da un controllo Popup senza UpdatePanel (c#) | Microsoft Docs
author: wenz
description: Il dispositivo extender PopupControl in AJAX Control Toolkit offre un modo semplice per attivare una finestra popup quando viene attivato un qualsiasi altro controllo. Quando si verifica un postback in unità di streaming...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 25444121-5a72-4dac-8e50-ad2b7ac667af
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-cs
msc.type: authoredcontent
ms.openlocfilehash: d74a44a277bdcb460dc20b78bad3e5ed68445b4a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37370983"
---
<a name="handling-postbacks-from-a-popup-control-without-an-updatepanel-c"></a>Gestione dei postback da un controllo Popup senza UpdatePanel (c#)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare il codice](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3CS.pdf)

> Il dispositivo extender PopupControl in AJAX Control Toolkit offre un modo semplice per attivare una finestra popup quando viene attivato un qualsiasi altro controllo. Quando si verifica un postback in un pannello di questo tipo e sono presenti diversi pannelli nella pagina è difficile individuare il pannello è stato selezionato.


## <a name="overview"></a>Panoramica

Il dispositivo extender PopupControl in AJAX Control Toolkit offre un modo semplice per attivare una finestra popup quando viene attivato un qualsiasi altro controllo. Quando si verifica un postback in un pannello di questo tipo e sono presenti diversi pannelli nella pagina è difficile individuare il pannello è stato selezionato.

## <a name="steps"></a>Passaggi

Quando si usa un' `PopupControl` con un postback, ma senza che sia un `UpdatePanel` nella pagina, il Toolkit di controllo non offre un modo per determinare quale elemento client ha avviato la finestra popup che a sua volta ha causato il postback. Tuttavia un piccolo trucco fornisce una soluzione alternativa per questo scenario.

Prima di tutto, ecco la configurazione di base: due caselle di testo che attivano il popup stesso, un calendario. Due `PopupControlExtenders` riunire le caselle di testo e finestra popup.

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample1.aspx)]

L'idea di base consiste nell'aggiungere un campo del form nascosto il &lt; `form` &gt; elemento che contiene la casella di testo quale avviata la finestra popup:

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample2.aspx)]

Quando la pagina viene caricata, il codice JavaScript aggiunge un gestore eventi per entrambe le caselle di testo: ogni volta che l'utente fa clic sulla casella di testo, il relativo nome viene scritto nel campo del form nascosto:

[!code-html[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample3.html)]

Nel codice lato server, è necessario leggere il valore del campo nascosto. Poiché sono semplici per modificare i campi modulo nascosti, è necessario un approccio di elenco elementi consentiti per la convalida del valore nascosto. Dopo aver individuata la casella di testo corretto, la data dal calendario viene scritta al suo interno.

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample4.aspx)]


[![Quando l'utente fa clic nella casella di testo viene visualizzato il calendario](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image1.png)

Il calendario viene visualizzato quando l'utente fa clic nella casella di testo ([fare clic per visualizzare l'immagine con dimensioni normali](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image3.png))


[![Facendo clic su una data lo inserisce nella casella di testo](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image4.png)

Facendo clic su una data lo inserisce nella casella di testo ([fare clic per visualizzare l'immagine con dimensioni normali](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image6.png))

> [!div class="step-by-step"]
> [Precedente](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs.md)
> [Successivo](using-multiple-popup-controls-vb.md)
