---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-vb
title: Posizionamento di un controllo ModalPopup (VB) | Microsoft Docs
author: wenz
description: Il controllo ModalPopup in AJAX Control Toolkit offre un modo semplice per creare un popup modale utilizzano i canali lato client. Tuttavia il controllo non offre un...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 8a07210c-eb0e-485e-9ee8-82a101520e65
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-vb
msc.type: authoredcontent
ms.openlocfilehash: a507cb173b6dba96ca532a249fa9d495ded7d4e8
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838051"
---
<a name="positioning-a-modalpopup-vb"></a>Posizionamento di un controllo ModalPopup (VB)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare il codice](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4VB.pdf)

> Il controllo ModalPopup in AJAX Control Toolkit offre un modo semplice per creare un popup modale utilizzano i canali lato client. Tuttavia il controllo non offre una funzionalità incorporata per posizionare la finestra popup.


## <a name="overview"></a>Panoramica

Il controllo ModalPopup in AJAX Control Toolkit offre un modo semplice per creare un popup modale utilizzano i canali lato client. Tuttavia il controllo non offre una funzionalità incorporata per posizionare la finestra popup.

## <a name="steps"></a>Passaggi

Per attivare la funzionalità di ASP.NET AJAX e il Toolkit di controllo, il `ScriptManager`. controllo deve essere inserito in un punto qualsiasi della pagina (ma entro la `<form>` elemento):

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample1.aspx)]

Successivamente, aggiungere un pannello che funge da finestra popup modale. Viene usato un pulsante per chiudere la finestra popup:

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample2.aspx)]

Ogni volta che viene visualizzato la finestra popup, si deve essere posizionato in corrispondenza di un determinato punto della pagina. Per questa attività, viene creata una funzione di JavaScript lato client. Innanzitutto tenta di accedere al pannello. Caso di esito positivo, posizione del pannello è impostata con CSS e JavaScript (modifica la posizione del popup a verrà). Tuttavia il `ModalPopupExtender` controllo tenta anche di posizionare la finestra popup. Pertanto, il codice JavaScript ripetutamente il popup viene posizionato, ogni decimo di secondo.

[!code-html[Main](positioning-a-modalpopup-vb/samples/sample3.html)]

Come si può notare, il valore restituito del `setTimeout()` metodo JavaScript viene salvato in una variabile globale. In questo modo per arrestare il posizionamento ripetute del popup su richiesta, usando il `clearTimeout()` metodo:

[!code-javascript[Main](positioning-a-modalpopup-vb/samples/sample4.js)]

A questo punto tutto ciò che resta da fare è rendere il browser chiamare queste funzioni ogni volta che è appropriato. Il `movePanel()` funzione JavaScript deve essere chiamata quando si fa clic sul pulsante che attiva il pannello:

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample5.aspx)]

E il `stopMoving()` funzione entra in gioco quando il controllo popup viene chiusa questo può essere attivata nel `ModalPopupExtender` controllo:

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample6.aspx)]


[![Verrà visualizzata la finestra popup modale in corrispondenza della posizione designata](positioning-a-modalpopup-vb/_static/image2.png)](positioning-a-modalpopup-vb/_static/image1.png)

Verrà visualizzata la finestra popup modale in corrispondenza della posizione designata ([fare clic per visualizzare l'immagine con dimensioni normali](positioning-a-modalpopup-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](handling-postbacks-from-a-modalpopup-vb.md)
