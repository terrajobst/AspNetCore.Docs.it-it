---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-cs
title: L'esecuzione di animazioni utilizzando codice lato Client (c#) | Documenti Microsoft
author: wenz
description: Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework per aggiungere le animazioni a un controllo. L'esecuzione di animazione...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 0270e0df-6fde-4a8f-a2cb-2cacc55143f2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-cs
msc.type: authoredcontent
ms.openlocfilehash: cfaed745b4c547d04033ee89d2c6549c5989cb73
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="executing-animations-using-client-side-code-c"></a>L'esecuzione di animazioni utilizzando codice lato Client (c#)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare codice](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation10.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation10CS.pdf)

> Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework per aggiungere le animazioni a un controllo. L'esecuzione di animazione può essere attivato utilizzando codice JavaScript sul lato client personalizzato.


## <a name="overview"></a>Panoramica

Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework per aggiungere le animazioni a un controllo. L'esecuzione di animazione può essere attivato utilizzando codice JavaScript sul lato client personalizzato.

## <a name="steps"></a>Passaggi

Prima di tutto, includere il `ScriptManager` nella pagina; quindi, la libreria AJAX di ASP.NET viene caricata, che consente di usare il Toolkit di controllo:

[!code-aspx[Main](executing-animations-using-client-side-code-cs/samples/sample1.aspx)]

Verrà applicata l'animazione a un riquadro del testo che presenta un aspetto simile al seguente:

[!code-aspx[Main](executing-animations-using-client-side-code-cs/samples/sample2.aspx)]

Nella classe CSS associata per il pannello, definire un colore di sfondo nice e inoltre impostare una larghezza fissa per il pannello:

[!code-css[Main](executing-animations-using-client-side-code-cs/samples/sample3.css)]

Quindi, aggiungere il `AnimationExtender` alla pagina, fornendo un `ID`, `TargetControlID` attributo e l'obbligatoria `runat="server"`:

[!code-aspx[Main](executing-animations-using-client-side-code-cs/samples/sample4.aspx)]

All'interno di `<Animations>` nodo, utilizzare `<OnClick>` per eseguire le animazioni una sola volta, l'utente fa clic sul riquadro. Aggiungere due animazioni deve essere eseguito parallelly:

[!code-xml[Main](executing-animations-using-client-side-code-cs/samples/sample5.xml)]

Ai fini di dimostrazione, questa animazione (e qualsiasi altra animazione creato con il Toolkit di controllo) viene eseguiti utilizzando codice JavaScript, dopo l'esecuzione della pagina. Prima di tutto è necessario accedere al `AnimationExtender` controllo. La libreria ASP.NET AJAX fornisce il `$find()` funzione per questa attività:

[!code-csharp[Main](executing-animations-using-client-side-code-cs/samples/sample6.cs)]

Il `AnimationExtender` controllo espone un'API completa, inclusi i metodi con nomi identici ai gestori di eventi utilizzati nel markup XML: `OnClick()`, `OnLoad()`e così via. Ad esempio, una chiamata del `OnClick()` metodo esegue l'animazione all'interno di `<OnClick>` elemento del `AnimationExtender` controllo:

[!code-javascript[Main](executing-animations-using-client-side-code-cs/samples/sample7.js)]

Ecco il codice JavaScript sul lato client completo che emula il clic sul pannello dopo la pagina è stata caricata completamente si noti che il `pageLoad()` viene utilizzato il nome di funzione che viene chiamato da ASP.NET AJAX una volta la pagina e tutte incluse JavaScript sono state raccolte caricato.

[!code-html[Main](executing-animations-using-client-side-code-cs/samples/sample8.html)]


[![L'animazione viene eseguita immediatamente, senza un clic del mouse](executing-animations-using-client-side-code-cs/_static/image2.png)](executing-animations-using-client-side-code-cs/_static/image1.png)

L'animazione viene eseguita immediatamente, senza un clic del mouse ([fare clic per visualizzare l'immagine ingrandita](executing-animations-using-client-side-code-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](modifying-animations-from-the-server-side-cs.md)
> [Successivo](changing-an-animation-using-client-side-code-cs.md)
