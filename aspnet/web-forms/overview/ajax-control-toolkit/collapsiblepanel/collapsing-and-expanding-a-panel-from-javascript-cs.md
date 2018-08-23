---
uid: web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-cs
title: Compressione ed espansione di un pannello da JavaScript (c#) | Microsoft Docs
author: wenz
description: Il controllo CollapsiblePanel in ASP.NET AJAX Control Toolkit estende un pannello e fornisce funzionalità per comprimere il contenuto e per espanderlo in un...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: de5500be-75e5-461a-8064-b70ae52ea6a4
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-cs
msc.type: authoredcontent
ms.openlocfilehash: 325194fc1f980714ea2fc26cfaa13d86ae9d2f89
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835777"
---
<a name="collapsing-and-expanding-a-panel-from-javascript-c"></a>Compressione ed espansione di un pannello da JavaScript (c#)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare il codice](http://download.microsoft.com/download/8/a/a/8aab3c3e-de6f-463f-805c-5fda567eef6e/CollapsiblePanel1.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/collapsiblepanel1CS.pdf)

> Il controllo CollapsiblePanel in ASP.NET AJAX Control Toolkit estende un pannello e lo inserisce in grado di comprimere il contenuto ed espanderla nuovamente. Queste due azioni possono essere attivate anche dal codice JavaScript personalizzato.


## <a name="overview"></a>Panoramica

Il controllo CollapsiblePanel in ASP.NET AJAX Control Toolkit estende un pannello e lo inserisce in grado di comprimere il contenuto ed espanderla nuovamente. Queste due azioni possono essere attivate anche dal codice JavaScript personalizzato.

## <a name="steps"></a>Passaggi

Prima di tutto, creare una nuova pagina ASP.NET e includere il `ScriptManager` all'interno di quella `<form>` elemento. Consente di caricare la libreria ASP.NET AJAX che è necessario per il Toolkit di controllo:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample1.aspx)]

Creare quindi un pannello con un testo in modo da poter visualizzare l'effetto di compressione/espansione:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample2.aspx)]

Come può notare, il pannello fa riferimento a una classe CSS che è illustrata di seguito (e definisce in sostanza un colore di sfondo e la larghezza del pannello):

[!code-css[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample3.css)]

Il `CollapsiblePanelExtender` controllo richiede la `TargetControlID` attributo in modo che il toolkit sappia quale pannello per comprimere o espandere su richiesta:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample4.aspx)]

Sfortunatamente, il dispositivo extender attualmente non espone un'API specifica per comprimere o espandere il pannello, ma si eseguiranno alcuni metodi non documentate. Prima di tutto, aggiungere tre pulsanti HTML alla pagina che attiverà il codice JavaScript lato client per comprimere o espandere il contenuto del pannello:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample5.aspx)]

Nel codice JavaScript lato client (Introduzione `<script type="text/javascript">`), il `$find()` metodo deve essere utilizzato per l'accesso il `CollapsiblePanelExtender`. `$find("cpe")` restituirà un riferimento a esso. Da tale data, i metodi specifici risolverà l'attività in questione.

Il metodo per l'apertura (espansione) viene chiamato il pannello `_doOpen()`; il codice seguente implementa il `doOpen()` funzione chiamata quando si fa clic sul primo pulsante:

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample6.js)]

Per la chiusura o comprimere il pannello di `_doClose()` metodo deve essere eseguito. Pertanto, quando l'utente fa clic sul secondo pulsante, viene chiamato il codice JavaScript seguente:

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample7.js)]

Il terzo pulsante Attiva/disattiva lo stato del pannello: da compressa per espanso e viceversa. Il `CollapsiblePanelExtender` espone il `toggle()` metodo che è adibita: consente di invertire lo stato del pannello. È inoltre disponibile tuttavia un altro approccio (che viene usato internamente dal `toggle()` (metodo)): il `get_Collapsed()` metodo del `CollapsiblePanelExtender()` per indicare se il pannello viene compressa o non. A seconda del valore restituito di questa funzione, il pannello viene quindi espansa uno (`_doOpen()` metodo) o compresso (`_doClose()`) metodo:

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample8.js)]


[![Il terzo pulsante Modifica lo stato del pannello: da compresso espanso e indietro](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image2.png)](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image1.png)

Il terzo pulsante Modifica lo stato del pannello: da compresso espanso e back-([fare clic per visualizzare l'immagine con dimensioni normali](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image3.png))

> [!div class="step-by-step"]
> [avanti](collapsing-and-expanding-a-panel-from-javascript-vb.md)
