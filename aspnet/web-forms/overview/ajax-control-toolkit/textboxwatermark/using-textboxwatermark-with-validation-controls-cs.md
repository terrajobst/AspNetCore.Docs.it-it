---
uid: web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-cs
title: Utilizzo di TextBoxWatermark con i controlli di convalida (c#) | Documenti Microsoft
author: wenz
description: Il controllo TextBoxWatermark AJAX Control Toolkit estende una casella di testo in modo che sia visualizzato all'interno della casella. Quando un utente fa clic nella casella, è possibile...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: d49940cb-d38c-456a-b800-5f0eb705d09f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: b5cc7974f3444b54770cba54b991aab7b103f753
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="using-textboxwatermark-with-validation-controls-c"></a>Utilizzo di TextBoxWatermark con i controlli di convalida (c#)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare codice](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark2.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark2CS.pdf)

> Il controllo TextBoxWatermark AJAX Control Toolkit estende una casella di testo in modo che sia visualizzato all'interno della casella. Quando un utente fa clic nella casella, viene svuotato. Se l'utente lascia la casella senza l'immissione di testo, viene nuovamente visualizzato il testo precompilato. Ciò che siano in conflitto con i controlli di convalida ASP.NET nella stessa pagina, ma è possibile risolvere questi problemi.


## <a name="overview"></a>Panoramica

Il `TextBoxWatermark` controllo AJAX Control Toolkit estende una casella di testo in modo che sia visualizzato all'interno della casella. Quando un utente fa clic nella casella, viene svuotato. Se l'utente lascia la casella senza l'immissione di testo, viene nuovamente visualizzato il testo precompilato. Ciò che siano in conflitto con i controlli di convalida ASP.NET nella stessa pagina, ma è possibile risolvere questi problemi.

## <a name="steps"></a>Passaggi

Configurazione di base dell'esempio è il seguente: un `TextBox` controllo è filigrana utilizzando un `TextBoxWatermarkExtender` controllo. Un pulsante Attiva un postback e verrà successivamente utilizzato per attivare i controlli di convalida della pagina. Inoltre, un `ScriptManager` controllo è necessario inizializzare ASP.NET AJAX:

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample1.aspx)]

A questo punto aggiungere un `RequiredFieldValidator` controllo che controlla se è il testo nel campo quando il form viene inviato. Il `InitialValue` impostata la proprietà del validator sullo stesso valore che viene utilizzata per il `TextBoxWatermarkExtender` controllo: quando il form viene inviato, il valore di una casella di testo non modificato è il valore limite all'interno di essa:

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample2.aspx)]

È tuttavia un problema con questo approccio: se il client JavaScript, il campo di testo non è precompilato con il testo della filigrana, pertanto il `RequiredFieldValidator` non genera un messaggio di errore. Pertanto, un secondo `RequiredFieldValidator` è necessario il controllo che verifica la presenza di una casella di testo (omettendo il `InitialValue` attributo).

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample3.aspx)]

Poiché entrambi validator utilizzare `Display` = `"Dynamic"`, l'utente finale non distingue dall'aspetto visivo di quale dei due validator è stato generato; in alternativa, sembra che si è verificato solo uno di essi.

Infine, aggiungere codice lato server per il testo nel campo di output se nessun validator emesso un messaggio di errore:

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample4.aspx)]


[![Il validator rileva che è disponibile alcun testo nel campo](using-textboxwatermark-with-validation-controls-cs/_static/image2.png)](using-textboxwatermark-with-validation-controls-cs/_static/image1.png)

Il validator rileva che è disponibile alcun testo nel campo ([fare clic per visualizzare l'immagine ingrandita](using-textboxwatermark-with-validation-controls-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](using-textboxwatermark-in-a-formview-cs.md)
> [Successivo](using-textboxwatermark-in-a-formview-vb.md)
