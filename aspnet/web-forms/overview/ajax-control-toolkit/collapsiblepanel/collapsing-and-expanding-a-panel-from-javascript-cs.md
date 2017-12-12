---
uid: web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-cs
title: Compressione ed espansione di un pannello da JavaScript (c#) | Documenti Microsoft
author: wenz
description: "Il controllo CollapsiblePanel in ASP.NET AJAX Control Toolkit estende un pannello e fornisce la funzionalità per comprimere il contenuto ed espanderlo un..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: de5500be-75e5-461a-8064-b70ae52ea6a4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-cs
msc.type: authoredcontent
ms.openlocfilehash: 666f3e212ccdd5b26b466f4672134ce751dc5dd1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="collapsing-and-expanding-a-panel-from-javascript-c"></a>Compressione ed espansione di un pannello da JavaScript (c#)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare codice](http://download.microsoft.com/download/8/a/a/8aab3c3e-de6f-463f-805c-5fda567eef6e/CollapsiblePanel1.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/collapsiblepanel1CS.pdf)

> Il controllo CollapsiblePanel in ASP.NET AJAX Control Toolkit estende un pannello e fornisce la funzionalità per comprimere il contenuto ed espanderlo nuovamente. Queste due azioni possono anche essere attivate dal codice JavaScript personalizzato.


## <a name="overview"></a>Panoramica

Il controllo CollapsiblePanel in ASP.NET AJAX Control Toolkit estende un pannello e fornisce la funzionalità per comprimere il contenuto ed espanderlo nuovamente. Queste due azioni possono anche essere attivate dal codice JavaScript personalizzato.

## <a name="steps"></a>Passaggi

Prima di tutto, creare una nuova pagina ASP.NET e includere il `ScriptManager` rispetto a quello `<form>` elemento. Consente di caricare la libreria AJAX di ASP.NET che è necessario per il Toolkit di controllo.

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample1.aspx)]

Creare quindi un pannello con testo in modo da poter visualizzare l'effetto di compressione/espansione:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample2.aspx)]

Come si può notare, il pannello fa riferimento a una classe CSS che è illustrata di seguito (e definisce in sostanza un colore di sfondo e la larghezza del pannello):

[!code-css[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample3.css)]

Il `CollapsiblePanelExtender` controllo richiede il `TargetControlID` attributo in modo che il toolkit sappia quale pannello per comprimere o espandere su richiesta:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample4.aspx)]

Sfortunatamente, il programma di estensione attualmente non espone un'API specifica per comprimere o espandere il pannello, ma è alcuni metodi non documentate. In primo luogo, aggiungere tre pulsanti HTML della pagina che attiverà quindi il codice JavaScript sul lato client per comprimere o espandere il contenuto del pannello:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample5.aspx)]

Nel codice JavaScript sul lato client (introduttiva `<script type="text/javascript">`), il `$find()` metodo deve essere utilizzato per l'accesso di `CollapsiblePanelExtender`. `$find("cpe")`verrà restituito un riferimento a esso. Da qui in metodi specifici per risolvere l'attività in questione.

Il metodo di apertura (espansione) viene chiamato il pannello `_doOpen()`; il codice seguente implementa il `doOpen()` funzione chiamata quando viene selezionato il primo pulsante:

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample6.js)]

Per la chiusura o la compressione, il pannello di `_doClose()` (metodo) deve essere eseguito. Pertanto, quando l'utente fa clic sul secondo pulsante, viene chiamato il codice JavaScript seguente:

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample7.js)]

Il terzo pulsante Alterna lo stato del pannello: da compresso in espansa e viceversa. Il `CollapsiblePanelExtender` espone il `toggle()` metodo che ha esattamente questo: inverte lo stato del pannello. È tuttavia disponibile anche un altro approccio (che è utilizzato internamente dal `toggle()` (metodo)): il `get_Collapsed()` metodo il `CollapsiblePanelExtender()` indica se il pannello è compresso o non. A seconda del valore restituito di questa funzione, il pannello è quindi uno espanso (`_doOpen()` (metodo)) o compresso (`_doClose()`) metodo:

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample8.js)]


[![Il terzo pulsante Cambia lo stato del pannello: da compresso in espansi e nascosto](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image2.png)](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image1.png)

Il terzo pulsante Cambia lo stato del pannello: da compresso in espansi e nascosto ([fare clic per visualizzare l'immagine ingrandita](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image3.png))

>[!div class="step-by-step"]
[Successivo](collapsing-and-expanding-a-panel-from-javascript-vb.md)
