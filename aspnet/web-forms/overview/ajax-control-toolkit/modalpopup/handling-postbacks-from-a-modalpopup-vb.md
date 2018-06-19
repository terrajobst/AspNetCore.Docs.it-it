---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-vb
title: Gestione dei postback da un ModalPopup (VB) | Documenti Microsoft
author: wenz
description: Il controllo ModalPopup AJAX Control Toolkit offre un modo semplice per creare un popup modale tramite mezzi sul lato client. È necessario prestare particolare attenzione quando un pos...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: f70ac2b3-900f-40fa-858f-ab057904506b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-vb
msc.type: authoredcontent
ms.openlocfilehash: 1aa2c42deb67015dd0b35edf4ba72d8d667ec88c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874210"
---
<a name="handling-postbacks-from-a-modalpopup-vb"></a>Gestione dei postback da un ModalPopup (VB)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare codice](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3VB.pdf)

> Il controllo ModalPopup AJAX Control Toolkit offre un modo semplice per creare un popup modale tramite mezzi sul lato client. È necessario prestare particolare attenzione quando viene creato un postback del popup.


## <a name="overview"></a>Panoramica

Il controllo ModalPopup AJAX Control Toolkit offre un modo semplice per creare un popup modale tramite mezzi sul lato client. È necessario prestare particolare attenzione quando viene creato un postback del popup.

## <a name="steps"></a>Passaggi

Per attivare le funzionalità di ASP.NET AJAX e il Toolkit di controllo, il `ScriptManager` controllo deve essere inserito in un punto qualsiasi della pagina (ma entro il `<form>` elemento):

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample1.aspx)]

Successivamente, aggiungere un pannello che funge da finestra popup modale. Non esiste, l'utente può immettere un nome e un indirizzo di posta elettronica. Un pulsante viene utilizzato per chiudere la finestra popup e salvare le informazioni. Si noti che il `OnClick` attributo è impostato in modo che si verifica un postback quando viene fatto clic su questo pulsante:

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample2.aspx)]

La pagina stessa è costituito da due etichette esattamente le stesse informazioni: nome e l'indirizzo e-mail. Un pulsante viene utilizzato per attivare il popup modale:

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample3.aspx)]

Per visualizzare la finestra popup, aggiungere il `ModalPopupExtender` controllo. Impostare il `PopupControlID` attributo ID del pannello e `TargetControlID` all'ID del pulsante:

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample4.aspx)]

Ora ogni volta che il `Save` del popup modale pulsante lato server `SaveData()` metodo viene eseguito. Non esiste, è stato possibile salvare i dati immessi in un archivio dati. Per ragioni di semplicità, i nuovi dati viene restituiti solo nell'etichetta:

[!code-vb[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample5.vb)]

Inoltre, i controlli casella di testo all'interno del popup modale devono essere riempiti con il nome corrente e un messaggio di posta elettronica. Tuttavia ciò è necessario solo quando si verifica alcun postback. Se si verifica un postback, la funzionalità di viewstate ASP.NET verrà compilati automaticamente in caselle di testo con i valori appropriati.

[!code-vb[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample6.vb)]


[![La finestra popup modale provoca un postback](handling-postbacks-from-a-modalpopup-vb/_static/image2.png)](handling-postbacks-from-a-modalpopup-vb/_static/image1.png)

Il popup modale provoca un postback ([fare clic per visualizzare l'immagine ingrandita](handling-postbacks-from-a-modalpopup-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](using-modalpopup-with-a-repeater-control-vb.md)
> [Successivo](positioning-a-modalpopup-vb.md)
