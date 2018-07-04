---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-vb
title: Gestione dei postback da un controllo ModalPopup (VB) | Microsoft Docs
author: wenz
description: Il controllo ModalPopup in AJAX Control Toolkit offre un modo semplice per creare un popup modale utilizzano i canali lato client. È necessario prestare particolare attenzione quando un pos...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: f70ac2b3-900f-40fa-858f-ab057904506b
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-vb
msc.type: authoredcontent
ms.openlocfilehash: e37c26b149c14855574ea777eae49fc40ea83899
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37382666"
---
<a name="handling-postbacks-from-a-modalpopup-vb"></a>Gestione dei postback da un controllo ModalPopup (VB)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare il codice](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3VB.pdf)

> Il controllo ModalPopup in AJAX Control Toolkit offre un modo semplice per creare un popup modale utilizzano i canali lato client. È necessario prestare particolare attenzione quando si crea un postback dal entro la finestra popup.


## <a name="overview"></a>Panoramica

Il controllo ModalPopup in AJAX Control Toolkit offre un modo semplice per creare un popup modale utilizzano i canali lato client. È necessario prestare particolare attenzione quando si crea un postback dal entro la finestra popup.

## <a name="steps"></a>Passaggi

Per attivare la funzionalità di ASP.NET AJAX e il Toolkit di controllo, il `ScriptManager` controllo deve essere inserito in un punto qualsiasi della pagina (ma entro la `<form>` elemento):

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample1.aspx)]

Successivamente, aggiungere un pannello che funge da finestra popup modale. Non esiste, l'utente può immettere un nome e un indirizzo di posta elettronica. Un pulsante viene usato per chiudere la finestra popup e salvare le informazioni. Si noti che il `OnClick` attributo è impostato in modo che si verifica un postback quando viene fatto clic su questo pulsante:

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample2.aspx)]

La pagina stessa è costituito da due etichette esattamente le stesse informazioni: nome e l'indirizzo e-mail. Un pulsante viene usato per attivare la finestra popup modale:

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample3.aspx)]

Per visualizzare la finestra popup, aggiungere il `ModalPopupExtender` controllo. Impostare il `PopupControlID` dell'attributo ID del pannello e `TargetControlID` all'ID del pulsante:

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample4.aspx)]

Ora ogni volta che il `Save` entro la finestra popup modale è fatto clic sul pulsante, il server-side `SaveData()` metodo viene eseguito. Non esiste, è possibile salvare i dati immessi in un archivio dati. Per ragioni di semplicità, i nuovi dati viene restituiti solo nell'etichetta:

[!code-vb[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample5.vb)]

Inoltre, i controlli casella di testo all'interno del popup modale devono essere riempiti con il nome corrente e il messaggio di posta elettronica. Tuttavia questo è necessario solo quando si verifica alcun postback. Se si verifica un postback, la funzionalità di viewstate ASP.NET compilerà automaticamente le caselle di testo con i valori appropriati.

[!code-vb[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample6.vb)]


[![La finestra popup modale causa un postback](handling-postbacks-from-a-modalpopup-vb/_static/image2.png)](handling-postbacks-from-a-modalpopup-vb/_static/image1.png)

La finestra popup modale causa un postback ([fare clic per visualizzare l'immagine con dimensioni normali](handling-postbacks-from-a-modalpopup-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](using-modalpopup-with-a-repeater-control-vb.md)
> [Successivo](positioning-a-modalpopup-vb.md)
