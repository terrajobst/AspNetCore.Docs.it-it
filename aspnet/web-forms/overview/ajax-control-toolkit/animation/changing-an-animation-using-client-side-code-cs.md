---
uid: web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-cs
title: La modifica di un'animazione mediante il codice sul lato Client (c#) | Documenti Microsoft
author: wenz
description: "Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework per aggiungere le animazioni a un controllo. L'animazione può inoltre..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 2bfbc5cc-f942-44b7-a62d-a29520f1da9a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 6dcaeac073f54b0804fe3acf7ec22491b1cbbba5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="changing-an-animation-using-client-side-code-c"></a>La modifica di un'animazione mediante il codice sul lato Client (c#)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare codice](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11CS.pdf)

> Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework per aggiungere le animazioni a un controllo. L'animazione può essere modificato utilizzando codice JavaScript sul lato client personalizzato.


## <a name="overview"></a>Panoramica

Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework per aggiungere le animazioni a un controllo. L'animazione può essere modificato utilizzando codice JavaScript sul lato client personalizzato.

## <a name="steps"></a>Passaggi

Prima di tutto, includere il `ScriptManager` nella pagina; quindi, la libreria AJAX di ASP.NET viene caricata, che consente di usare il Toolkit di controllo:

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample1.aspx)]

Verrà applicata l'animazione a un riquadro del testo che presenta un aspetto simile al seguente:

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample2.aspx)]

Nella classe CSS associata per il pannello, definire un colore di sfondo nice e inoltre impostare una larghezza fissa per il pannello:

[!code-css[Main](changing-an-animation-using-client-side-code-cs/samples/sample3.css)]

Viene avviata l'animazione effettive per un pulsante HTML:

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample4.aspx)]

Quindi, aggiungere il `AnimationExtender` alla pagina, fornendo un `ID`, `TargetControlID` attributo e l'obbligatoria `runat="server"`:

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample5.aspx)]

Si noti che non esiste alcun `<Animations>` nodo all'interno di `AnimationExtender` controllo. Il codice JavaScript personalizzato viene utilizzato per fornire le animazioni da utilizzare con il controllo.

Come con l'API del server di `AnimationExtender`, non è possibile assegnare un'animazione a extender ancora facile. Tuttavia il programma di estensione espone diversi metodi per leggere e scrivere le animazioni registrato con i vari eventi (`OnClick`, `OnLoad`e così via). Ecco alcuni esempi:

- `get_OnClick()`
- `set_OnClick()`
- `get_OnLoad()`
- `set_OnLoad()`
- `...`

Il formato del valore restituito del `get_*()` funzioni e il formato dell'argomento per il `set_*()` funzioni è una stringa JSON, fornendo una rappresentazione dell'oggetto dei possibili il markup XML. Attualmente, non è possibile passare un oggetto, ma è possibile leggere un oggetto di una determinato animazione (`get_OnXXXBehavior()` metodi).

Di seguito è una stringa JSON (senza le virgolette di delimitazione e formattati correttamente) che rappresenta un'animazione attivata mediante il pulsante, ma l'animazione del pannello esegue il ridimensionamento e dissolvenza in uscita contemporaneamente:

[!code-json[Main](changing-an-animation-using-client-side-code-cs/samples/sample6.json)]

Il codice JavaScript seguente assegna questo descripting JSON per il `OnClick` animazione dell'oggetto di estensione corrente e lo esegue:

[!code-html[Main](changing-an-animation-using-client-side-code-cs/samples/sample7.html)]


[![L'animazione viene eseguita immediatamente, senza un clic del mouse (e con markup minimo)](changing-an-animation-using-client-side-code-cs/_static/image2.png)](changing-an-animation-using-client-side-code-cs/_static/image1.png)

L'animazione viene eseguita immediatamente, senza un clic del mouse (e con markup poco) ([fare clic per visualizzare l'immagine ingrandita](changing-an-animation-using-client-side-code-cs/_static/image3.png))

>[!div class="step-by-step"]
[Precedente](executing-animations-using-client-side-code-cs.md)
[Successivo](animating-an-updatepanel-control-cs.md)
