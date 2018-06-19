---
uid: web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-cs
title: Aggiunta dinamica di un riquadro soffietto (c#) | Documenti Microsoft
author: wenz
description: Il controllo Accordion AJAX Control Toolkit fornisce più riquadri e consente all'utente di visualizzare uno di essi alla volta. I pannelli vengono in genere dichiarati w...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 66d88cfa-f26f-46b1-ad52-1c9e03c04a48
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-cs
msc.type: authoredcontent
ms.openlocfilehash: ad2fc6ea3d527215c0226f3f594d781163d538b5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879465"
---
<a name="dynamically-adding-an-accordion-pane-c"></a>Aggiunta dinamica di un riquadro soffietto (c#)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare codice](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2CS.pdf)

> Il controllo Accordion AJAX Control Toolkit fornisce più riquadri e consente all'utente di visualizzare uno di essi alla volta. I pannelli vengono in genere dichiarati all'interno della pagina, ma il codice lato server consente di ottenere lo stesso risultato.


## <a name="overview"></a>Panoramica

Il controllo Accordion AJAX Control Toolkit fornisce più riquadri e consente all'utente di visualizzare uno di essi alla volta. I pannelli vengono in genere dichiarati all'interno della pagina, ma il codice lato server consente di ottenere lo stesso risultato.

## <a name="steps"></a>Passaggi

Il controllo Accordion espone tutte le proprietà importanti per il codice lato server. Tra le altre cose, la `Panes` proprietà concede l'accesso alla raccolta di riquadri che costituiscono il Accordion. Ogni riquadro è di tipo `AccordionPane`. È pertanto più semplice per creare un riquadro di questo tipo:

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample1.cs)]

Il `HeaderContainer` proprietà di `AccordionPane` fornisce l'accesso ai controlli ASP.NET all'interno della sezione di intestazione del riquadro; il `ContentContainer` proprietà di `AccordionPane` esegue la stessa operazione per la sezione di contenuto del riquadro. In questo modo il codice ASP.NET aggiungere contenuto ai riquadri:

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample2.cs)]

Infine, è necessario aggiungere il riquadro per il `Panes` insieme il Accordion:

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample3.cs)]

Di seguito è riportato un codice lato server completo che consente di aggiungere due riquadri a un controllo Accordion:

[!code-aspx[Main](dynamically-adding-an-accordion-pane-cs/samples/sample4.aspx)]

L'unico elemento manca è Accordion, che dipende dalla presenza di ASP.NET `ScriptManager` controllo:

[!code-aspx[Main](dynamically-adding-an-accordion-pane-cs/samples/sample5.aspx)]

Per completare l'esempio, le due classi CSS a cui fa riferimento nel controllo Accordion forniscono informazioni sullo stile per il browser:

[!code-css[Main](dynamically-adding-an-accordion-pane-cs/samples/sample6.css)]


[![I dati nella accordion è stato aggiunto in modo dinamico dal codice lato server](dynamically-adding-an-accordion-pane-cs/_static/image2.png)](dynamically-adding-an-accordion-pane-cs/_static/image1.png)

I dati di accordion è stato aggiunto in modo dinamico dal codice lato server ([fare clic per visualizzare l'immagine ingrandita](dynamically-adding-an-accordion-pane-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](databinding-to-an-accordion-cs.md)
> [Successivo](databinding-to-an-accordion-vb.md)
