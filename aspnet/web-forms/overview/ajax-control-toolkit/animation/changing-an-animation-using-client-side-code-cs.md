---
uid: web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-cs
title: Modifica di un'animazione tramite codice lato Client (c#) | Microsoft Docs
author: wenz
description: Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo. L'animazione può inoltre...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 2bfbc5cc-f942-44b7-a62d-a29520f1da9a
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 253377ef6019a672680c6e819349357627ef111b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833638"
---
<a name="changing-an-animation-using-client-side-code-c"></a>Modifica di un'animazione tramite codice lato Client (c#)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare il codice](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11CS.pdf)

> Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo. L'animazione può essere modificata anche utilizzando codice JavaScript lato client personalizzato.


## <a name="overview"></a>Panoramica

Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo. L'animazione può essere modificata anche utilizzando codice JavaScript lato client personalizzato.

## <a name="steps"></a>Passaggi

Prima di tutto, includere il `ScriptManager` nella pagina; quindi, la libreria ASP.NET AJAX viene caricata, rendendo possibile usare il Toolkit di controllo:

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample1.aspx)]

Verrà applicata l'animazione a un pannello del testo che presenta un aspetto simile al seguente:

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample2.aspx)]

Nella classe CSS associata per il pannello, definire un colore di sfondo interessante e anche impostare una larghezza fissa per il pannello:

[!code-css[Main](changing-an-animation-using-client-side-code-cs/samples/sample3.css)]

L'animazione effettivo viene avviato da un pulsante HTML:

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample4.aspx)]

Quindi, aggiungere il `AnimationExtender` alla pagina, fornendo un' `ID`, il `TargetControlID` attributo e l'obbligatoria `runat="server"`:

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample5.aspx)]

Si noti che è presente alcun `<Animations>` nodo all'interno di `AnimationExtender` controllo. Il codice JavaScript personalizzato viene utilizzato per fornire le animazioni da utilizzare con il controllo.

Come con l'API del server di `AnimationExtender`, non esiste un modo semplice per assegnare un'animazione al dispositivo extender ancora. Tuttavia il dispositivo extender esporre diversi metodi per leggere e scrivere le animazioni registrato con i vari eventi (`OnClick`, `OnLoad`e così via). Ecco alcuni esempi:

- `get_OnClick()`
- `set_OnClick()`
- `get_OnLoad()`
- `set_OnLoad()`
- `...`

Il formato del valore restituito del `get_*()` funzioni e il formato dell'argomento per il `set_*()` funzioni è una stringa JSON, che fornisce una rappresentazione di oggetto di quello che sarebbe il markup XML. Attualmente, non è possibile passare un oggetto, ma è possibile leggere un oggetto da una determinata animazione (`get_OnXXXBehavior()` metodi).

Di seguito è una stringa JSON (senza le virgolette di delimitazione e formattate) che rappresenta un'animazione attivata dal pulsante, ma l'animazione del pannello riadattarlo e dissolvenza in uscita nello stesso momento:

[!code-json[Main](changing-an-animation-using-client-side-code-cs/samples/sample6.json)]

Il codice JavaScript seguente assegna questa descripting JSON per il `OnClick` animazione dell'oggetto extender corrente e lo esegue:

[!code-html[Main](changing-an-animation-using-client-side-code-cs/samples/sample7.html)]


[![L'animazione viene eseguita immediatamente, senza un clic del mouse (e con markup minimo)](changing-an-animation-using-client-side-code-cs/_static/image2.png)](changing-an-animation-using-client-side-code-cs/_static/image1.png)

L'animazione viene eseguita immediatamente, senza un clic del mouse (e con markup minimo) ([fare clic per visualizzare l'immagine con dimensioni normali](changing-an-animation-using-client-side-code-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](executing-animations-using-client-side-code-cs.md)
> [Successivo](animating-an-updatepanel-control-cs.md)
