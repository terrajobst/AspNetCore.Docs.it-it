---
uid: web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-cs
title: Uso di TextBoxWatermark con i controlli di convalida (c#) | Microsoft Docs
author: wenz
description: Il controllo di TextBoxWatermark in AJAX Control Toolkit estende una casella di testo in modo che sia visualizzato all'interno della casella. Quando un utente fa clic nella casella, lo posso...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: d49940cb-d38c-456a-b800-5f0eb705d09f
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: fe28f169e5e714ba07d8a71bb33d0f62608d688b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37380691"
---
<a name="using-textboxwatermark-with-validation-controls-c"></a>Uso di TextBoxWatermark con i controlli di convalida (c#)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare il codice](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark2.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark2CS.pdf)

> Il controllo di TextBoxWatermark in AJAX Control Toolkit estende una casella di testo in modo che sia visualizzato all'interno della casella. Quando un utente fa clic nella casella, viene svuotato. Se l'utente lascia la casella senza dover immettere testo, viene nuovamente visualizzato il testo precompilato. Ciò siano in conflitto con i controlli di convalida ASP.NET nella stessa pagina, ma è possibile risolvere questi problemi.


## <a name="overview"></a>Panoramica

Il `TextBoxWatermark` controllo in AJAX Control Toolkit estende una casella di testo in modo che sia visualizzato all'interno della casella. Quando un utente fa clic nella casella, viene svuotato. Se l'utente lascia la casella senza dover immettere testo, viene nuovamente visualizzato il testo precompilato. Ciò siano in conflitto con i controlli di convalida ASP.NET nella stessa pagina, ma è possibile risolvere questi problemi.

## <a name="steps"></a>Passaggi

La configurazione di base dell'esempio è il seguente: una `TextBox` controllo è transmux usando un `TextBoxWatermarkExtender` controllo. Un pulsante Attiva un postback e verrà successivamente utilizzato per attivare i controlli di convalida della pagina. Inoltre, un `ScriptManager` controllo necessario per inizializzare ASP.NET AJAX:

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample1.aspx)]

Aggiungere ora un `RequiredFieldValidator` controllo che controlla se vi sia il testo nel campo quando il form viene inviato. Il `InitialValue` proprietà del validator deve essere impostata sullo stesso valore usato nel `TextBoxWatermarkExtender` controllo: quando viene inviato il modulo, il valore di una casella di testo non modificato è il valore del limite all'interno di esso:

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample2.aspx)]

Tuttavia si è verificato un problema con questo approccio: se il client viene disabilitato JavaScript, il campo di testo non è precompilato con il testo della filigrana, pertanto il `RequiredFieldValidator` non genererà un messaggio di errore. Pertanto un secondo `RequiredFieldValidator` è necessario il controllo che verifica la presenza di una casella di testo vuoto (omettendo il `InitialValue` attributo).

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample3.aspx)]

Perché usano entrambi i validator `Display` = `"Dynamic"`, l'utente finale non è possibile distinguere dall'aspetto visivo che le convalide due è stato attivato; al contrario, sembra che si è verificato solo uno di essi.

Infine, aggiungere codice lato server per il testo nel campo di output se nessun validator rilasciato un messaggio di errore:

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample4.aspx)]


[![Visualizza un errore di convalida che non vi sia alcun testo nel campo](using-textboxwatermark-with-validation-controls-cs/_static/image2.png)](using-textboxwatermark-with-validation-controls-cs/_static/image1.png)

Visualizza un errore di convalida che non vi sia alcun testo nel campo ([fare clic per visualizzare l'immagine con dimensioni normali](using-textboxwatermark-with-validation-controls-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](using-textboxwatermark-in-a-formview-cs.md)
> [Successivo](using-textboxwatermark-in-a-formview-vb.md)
