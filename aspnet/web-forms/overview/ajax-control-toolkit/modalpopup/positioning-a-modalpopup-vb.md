---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-vb
title: Posizionamento di un ModalPopup (VB) | Documenti Microsoft
author: wenz
description: Il controllo ModalPopup AJAX Control Toolkit offre un modo semplice per creare un popup modale tramite mezzi sul lato client. Tuttavia il controllo non offre un...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 8a07210c-eb0e-485e-9ee8-82a101520e65
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-vb
msc.type: authoredcontent
ms.openlocfilehash: 2d20888674dfedee7a7af85efd8df118c8394c6c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="positioning-a-modalpopup-vb"></a>Posizionamento di un ModalPopup (VB)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare codice](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4VB.pdf)

> Il controllo ModalPopup AJAX Control Toolkit offre un modo semplice per creare un popup modale tramite mezzi sul lato client. Tuttavia il controllo non offre una funzionalità incorporata per posizionare la finestra popup.


## <a name="overview"></a>Panoramica

Il controllo ModalPopup AJAX Control Toolkit offre un modo semplice per creare un popup modale tramite mezzi sul lato client. Tuttavia il controllo non offre una funzionalità incorporata per posizionare la finestra popup.

## <a name="steps"></a>Passaggi

Per attivare le funzionalità di ASP.NET AJAX e il Toolkit di controllo, il `ScriptManager`. controllo deve essere inserito in un punto qualsiasi della pagina (ma entro il `<form>` elemento):

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample1.aspx)]

Successivamente, aggiungere un pannello che funge da finestra popup modale. Un pulsante viene utilizzato per chiudere la finestra popup:

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample2.aspx)]

Ogni volta che viene visualizzato la finestra popup, deve essere posizionato in un determinato punto della pagina. Per questa attività viene creata una funzione JavaScript sul lato client. Innanzitutto tenta di accedere al pannello. Se ha esito positivo, la posizione del pannello è impostata con CSS e JavaScript (modifica la posizione della finestra popup in verrà). Tuttavia il `ModalPopupExtender` controllo tenta anche di posizionare la finestra popup. Pertanto, il codice JavaScript ripetutamente il popup viene posizionato, ogni decimo di secondo.

[!code-html[Main](positioning-a-modalpopup-vb/samples/sample3.html)]

Come si può notare, il valore restituito di `setTimeout()` metodo JavaScript viene salvato in una variabile globale. Questo consente di arrestare il posizionamento ripetute del popup su richiesta, tramite il `clearTimeout()` metodo:

[!code-javascript[Main](positioning-a-modalpopup-vb/samples/sample4.js)]

Ora tutto ciò che resta da fare è per rendere il browser chiamare queste funzioni ogni volta che è appropriato. Il `movePanel()` funzione JavaScript deve essere chiamato quando viene scelto il pulsante che attiva il pannello:

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample5.aspx)]

E `stopMoving()` funzione si verifica quando la finestra popup viene chiusa può essere attivata nel `ModalPopupExtender` controllo:

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample6.aspx)]


[![Verrà visualizzata la finestra popup modale in corrispondenza della posizione designata](positioning-a-modalpopup-vb/_static/image2.png)](positioning-a-modalpopup-vb/_static/image1.png)

Il popup modale viene visualizzato in corrispondenza della posizione designata ([fare clic per visualizzare l'immagine ingrandita](positioning-a-modalpopup-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](handling-postbacks-from-a-modalpopup-vb.md)
