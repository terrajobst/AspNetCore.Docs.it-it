---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-vb
title: Esecuzione di animazioni tramite codice lato Client (VB) | Microsoft Docs
author: wenz
description: Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo. L'esecuzione di animazione...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: f7073f50-d765-456d-9957-926ce60f35f6
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 5408c51c1330a0394b89281e4ddce00df455a418
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371220"
---
<a name="executing-animations-using-client-side-code-vb"></a>Esecuzione di animazioni tramite codice lato Client (VB)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare il codice](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation10.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation10VB.pdf)

> Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo. L'esecuzione di animazione può essere attivato utilizzando codice JavaScript lato client personalizzato.


## <a name="overview"></a>Panoramica

Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo. L'esecuzione di animazione può essere attivato utilizzando codice JavaScript lato client personalizzato.

## <a name="steps"></a>Passaggi

Prima di tutto, includere il `ScriptManager` nella pagina; quindi, la libreria ASP.NET AJAX viene caricata, rendendo possibile usare il Toolkit di controllo:

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample1.aspx)]

Verrà applicata l'animazione a un pannello del testo che presenta un aspetto simile al seguente:

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample2.aspx)]

Nella classe CSS associata per il pannello, definire un colore di sfondo interessante e anche impostare una larghezza fissa per il pannello:

[!code-css[Main](executing-animations-using-client-side-code-vb/samples/sample3.css)]

Quindi, aggiungere il `AnimationExtender` alla pagina, fornendo un' `ID`, il `TargetControlID` attributo e l'obbligatoria `runat="server"`:

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample4.aspx)]

All'interno di `<Animations>` nodo, usare `<OnClick>` per eseguire le animazioni una volta l'utente fa clic sul pannello. Aggiungere due animazioni deve essere eseguito parallelly:

[!code-xml[Main](executing-animations-using-client-side-code-vb/samples/sample5.xml)]

Per fini dimostrativi, questa animazione (e qualsiasi altra animazione creato usando il Toolkit di controllo) viene eseguite tramite il codice JavaScript, una volta che viene eseguita la pagina. Prima di tutto è necessario accedere al `AnimationExtender` controllo. La libreria ASP.NET AJAX fornisce il `$find()` funzione per questa attività:

[!code-csharp[Main](executing-animations-using-client-side-code-vb/samples/sample6.cs)]

Il `AnimationExtender` controllo espone un'API completa, inclusi i metodi con nomi identici per i gestori eventi utilizzati nel markup XML: `OnClick()`, `OnLoad()`e così via. Ad esempio, una chiamata del `OnClick()` l'animazione in esecuzione del metodo il `<OnClick>` elemento del `AnimationExtender` controllo:

[!code-javascript[Main](executing-animations-using-client-side-code-vb/samples/sample7.js)]

Di seguito è riportato il codice completo di JavaScript lato client che emula il clic sul pannello dopo la pagina è stata caricata completamente si noti che il `pageLoad()` nome della funzione viene utilizzato il quale viene chiamato da ASP.NET AJAX una volta la pagina e incluse tutte le librerie sono state JavaScript caricato.

[!code-html[Main](executing-animations-using-client-side-code-vb/samples/sample8.html)]


[![L'animazione viene eseguita immediatamente, senza un clic del mouse](executing-animations-using-client-side-code-vb/_static/image2.png)](executing-animations-using-client-side-code-vb/_static/image1.png)

L'animazione viene eseguito immediatamente, senza un clic del mouse ([fare clic per visualizzare l'immagine con dimensioni normali](executing-animations-using-client-side-code-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](modifying-animations-from-the-server-side-vb.md)
> [Successivo](changing-an-animation-using-client-side-code-vb.md)
