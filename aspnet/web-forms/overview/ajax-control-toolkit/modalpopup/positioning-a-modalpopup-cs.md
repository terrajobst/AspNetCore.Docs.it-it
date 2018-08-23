---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-cs
title: Posizionamento di un controllo ModalPopup (c#) | Microsoft Docs
author: wenz
description: Il controllo ModalPopup in AJAX Control Toolkit offre un modo semplice per creare un popup modale utilizzano i canali lato client. Tuttavia il controllo non offre un...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 1caac9d0-e21e-49d6-a8ff-e563a736d6ca
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-cs
msc.type: authoredcontent
ms.openlocfilehash: e767785f801b5110f0e9e915cd35c8a4425a9487
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828251"
---
<a name="positioning-a-modalpopup-c"></a>Posizionamento di un controllo ModalPopup (c#)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare il codice](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4CS.pdf)

> Il controllo ModalPopup in AJAX Control Toolkit offre un modo semplice per creare un popup modale utilizzano i canali lato client. Tuttavia il controllo non offre una funzionalità incorporata per posizionare la finestra popup.


## <a name="overview"></a>Panoramica

Il controllo ModalPopup in AJAX Control Toolkit offre un modo semplice per creare un popup modale utilizzano i canali lato client. Tuttavia il controllo non offre una funzionalità incorporata per posizionare la finestra popup.

## <a name="steps"></a>Passaggi

Per attivare la funzionalità di ASP.NET AJAX e il Toolkit di controllo, il `ScriptManager`. controllo deve essere inserito in un punto qualsiasi della pagina (ma entro la `<form>` elemento):

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample1.aspx)]

Successivamente, aggiungere un pannello che funge da finestra popup modale. Viene usato un pulsante per chiudere la finestra popup:

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample2.aspx)]

Ogni volta che viene visualizzato la finestra popup, si deve essere posizionato in corrispondenza di un determinato punto della pagina. Per questa attività, viene creata una funzione di JavaScript lato client. Innanzitutto tenta di accedere al pannello. Caso di esito positivo, posizione del pannello è impostata con CSS e JavaScript (modifica la posizione del popup a verrà). Tuttavia il `ModalPopupExtender` controllo tenta anche di posizionare la finestra popup. Pertanto, il codice JavaScript ripetutamente il popup viene posizionato, ogni decimo di secondo.

[!code-html[Main](positioning-a-modalpopup-cs/samples/sample3.html)]

Come si può notare, il valore restituito del `setTimeout()` metodo JavaScript viene salvato in una variabile globale. In questo modo per arrestare il posizionamento ripetute del popup su richiesta, usando il `clearTimeout()` metodo:

[!code-javascript[Main](positioning-a-modalpopup-cs/samples/sample4.js)]

A questo punto tutto ciò che resta da fare è rendere il browser chiamare queste funzioni ogni volta che è appropriato. Il `movePanel()` funzione JavaScript deve essere chiamata quando si fa clic sul pulsante che attiva il pannello:

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample5.aspx)]

E il `stopMoving()` funzione entra in gioco quando il controllo popup viene chiusa questo può essere attivata nel `ModalPopupExtender` controllo:

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample6.aspx)]


[![Verrà visualizzata la finestra popup modale in corrispondenza della posizione designata](positioning-a-modalpopup-cs/_static/image2.png)](positioning-a-modalpopup-cs/_static/image1.png)

Verrà visualizzata la finestra popup modale in corrispondenza della posizione designata ([fare clic per visualizzare l'immagine con dimensioni normali](positioning-a-modalpopup-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](handling-postbacks-from-a-modalpopup-cs.md)
> [Successivo](launching-a-modal-popup-window-from-server-code-vb.md)
