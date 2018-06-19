---
uid: web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-vb
title: La modifica di un'animazione con il codice lato Client (VB) | Documenti Microsoft
author: wenz
description: Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework per aggiungere le animazioni a un controllo. L'animazione può inoltre...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: a7fe5de5-a964-4780-ae5e-70821dfb50a0
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 7f9b72576cc3a9e91827cfb40983821704621060
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879153"
---
<a name="changing-an-animation-using-client-side-code-vb"></a>La modifica di un'animazione con il codice lato Client (VB)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare codice](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11VB.pdf)

> Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework per aggiungere le animazioni a un controllo. L'animazione può essere modificato utilizzando codice JavaScript sul lato client personalizzato.


## <a name="overview"></a>Panoramica

Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework per aggiungere le animazioni a un controllo. L'animazione può essere modificato utilizzando codice JavaScript sul lato client personalizzato.

## <a name="steps"></a>Passaggi

Prima di tutto, includere il `ScriptManager` nella pagina; quindi, la libreria AJAX di ASP.NET viene caricata, che consente di usare il Toolkit di controllo:

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample1.aspx)]

Verrà applicata l'animazione a un riquadro del testo che presenta un aspetto simile al seguente:

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample2.aspx)]

Nella classe CSS associata per il pannello, definire un colore di sfondo nice e inoltre impostare una larghezza fissa per il pannello:

[!code-css[Main](changing-an-animation-using-client-side-code-vb/samples/sample3.css)]

Viene avviata l'animazione effettive per un pulsante HTML:

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample4.aspx)]

Quindi, aggiungere il `AnimationExtender` alla pagina, fornendo un `ID`, `TargetControlID` attributo e l'obbligatoria `runat="server"`:

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample5.aspx)]

Si noti che non esiste alcun `<Animations>` nodo all'interno di `AnimationExtender` controllo. Il codice JavaScript personalizzato viene utilizzato per fornire le animazioni da utilizzare con il controllo.

Come con l'API del server di `AnimationExtender`, non è possibile assegnare un'animazione a extender ancora facile. Tuttavia il programma di estensione espone diversi metodi per leggere e scrivere le animazioni registrato con i vari eventi (`OnClick`, `OnLoad`e così via). Ecco alcuni esempi:

- `get_OnClick()`
- `set_OnClick()`
- `get_OnLoad()`
- `set_OnLoad()`
- `...`

Il formato del valore restituito del `get_*()` funzioni e il formato dell'argomento per il `set_*()` funzioni è una stringa JSON, fornendo una rappresentazione dell'oggetto dei possibili il markup XML. Attualmente, non è possibile passare un oggetto, ma è possibile leggere un oggetto di una determinato animazione (`get_OnXXXBehavior()` metodi).

Di seguito è una stringa JSON (senza le virgolette di delimitazione e formattati correttamente) che rappresenta un'animazione attivata mediante il pulsante, ma l'animazione del pannello esegue il ridimensionamento e dissolvenza in uscita contemporaneamente:

[!code-json[Main](changing-an-animation-using-client-side-code-vb/samples/sample6.json)]

Il codice JavaScript seguente assegna questo descripting JSON per il `OnClick` animazione dell'oggetto di estensione corrente e lo esegue:

[!code-html[Main](changing-an-animation-using-client-side-code-vb/samples/sample7.html)]


[![L'animazione viene eseguita immediatamente, senza un clic del mouse (e con markup minimo)](changing-an-animation-using-client-side-code-vb/_static/image2.png)](changing-an-animation-using-client-side-code-vb/_static/image1.png)

L'animazione viene eseguita immediatamente, senza un clic del mouse (e con markup poco) ([fare clic per visualizzare l'immagine ingrandita](changing-an-animation-using-client-side-code-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](executing-animations-using-client-side-code-vb.md)
> [Successivo](animating-an-updatepanel-control-vb.md)
