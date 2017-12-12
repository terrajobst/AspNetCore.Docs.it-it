---
uid: web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-vb
title: Aggiunta dinamica di un riquadro soffietto (VB) | Documenti Microsoft
author: wenz
description: "Il controllo Accordion AJAX Control Toolkit fornisce più riquadri e consente all'utente di visualizzare uno di essi alla volta. I pannelli vengono in genere dichiarati w..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: fae968c9-1902-487d-b053-86a46dd52c3f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-vb
msc.type: authoredcontent
ms.openlocfilehash: 6adc0b7985e5ba3da1684b645ae1b71b5112594a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="dynamically-adding-an-accordion-pane-vb"></a>Aggiunta dinamica di un riquadro soffietto (VB)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare codice](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2VB.pdf)

> Il controllo Accordion AJAX Control Toolkit fornisce più riquadri e consente all'utente di visualizzare uno di essi alla volta. I pannelli vengono in genere dichiarati all'interno della pagina, ma il codice lato server consente di ottenere lo stesso risultato.


## <a name="overview"></a>Panoramica

Il controllo Accordion AJAX Control Toolkit fornisce più riquadri e consente all'utente di visualizzare uno di essi alla volta. I pannelli vengono in genere dichiarati all'interno della pagina, ma il codice lato server consente di ottenere lo stesso risultato.

## <a name="steps"></a>Passaggi

Il controllo Accordion espone tutte le proprietà importanti per il codice lato server. Tra le altre cose, la `Panes` proprietà concede l'accesso alla raccolta di riquadri che costituiscono il Accordion. Ogni riquadro è di tipo `AccordionPane`. È pertanto più semplice per creare un riquadro di questo tipo:

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample1.vb)]

Il `HeaderContainer` proprietà di `AccordionPane` fornisce l'accesso ai controlli ASP.NET all'interno della sezione di intestazione del riquadro; il `ContentContainer` proprietà di `AccordionPane` esegue la stessa operazione per la sezione di contenuto del riquadro. In questo modo il codice ASP.NET aggiungere contenuto ai riquadri:

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample2.vb)]

Infine, è necessario aggiungere il riquadro per il `Panes` insieme il Accordion:

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample3.vb)]

Di seguito è riportato un codice lato server completo che consente di aggiungere due riquadri a un controllo Accordion:

[!code-aspx[Main](dynamically-adding-an-accordion-pane-vb/samples/sample4.aspx)]

L'unico elemento manca è Accordion, che dipende dalla presenza di ASP.NET `ScriptManager` controllo:

[!code-aspx[Main](dynamically-adding-an-accordion-pane-vb/samples/sample5.aspx)]

Per completare l'esempio, le due classi CSS a cui fa riferimento nel controllo Accordion forniscono informazioni sullo stile per il browser:

[!code-css[Main](dynamically-adding-an-accordion-pane-vb/samples/sample6.css)]


[![I dati di accordion è stato aggiunto in modo dinamico dal codice lato server](dynamically-adding-an-accordion-pane-vb/_static/image2.png)](dynamically-adding-an-accordion-pane-vb/_static/image1.png)

I dati di accordion è stato aggiunto in modo dinamico dal codice lato server ([fare clic per visualizzare l'immagine ingrandita](dynamically-adding-an-accordion-pane-vb/_static/image3.png))

>[!div class="step-by-step"]
[Precedente](databinding-to-an-accordion-vb.md)
