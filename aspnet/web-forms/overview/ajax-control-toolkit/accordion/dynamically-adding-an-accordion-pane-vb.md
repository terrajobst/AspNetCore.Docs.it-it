---
uid: web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-vb
title: Aggiunta dinamica di un riquadro Accordion (VB) | Microsoft Docs
author: wenz
description: Il controllo Accordion in AJAX Control Toolkit fornisce più riquadri e consente all'utente di visualizzare uno di essi alla volta. I pannelli vengono dichiarati in genere w...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: fae968c9-1902-487d-b053-86a46dd52c3f
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-vb
msc.type: authoredcontent
ms.openlocfilehash: 3dd82fab03e06aa5dd3baba7dd24734fa964b350
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37378962"
---
<a name="dynamically-adding-an-accordion-pane-vb"></a>Aggiunta dinamica di un riquadro Accordion (VB)
====================
da [Christian Wenz](https://github.com/wenz)

[Scaricare il codice](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2VB.pdf)

> Il controllo Accordion in AJAX Control Toolkit fornisce più riquadri e consente all'utente di visualizzare uno di essi alla volta. I pannelli in genere vengono dichiarati all'interno della pagina stessa, ma il codice lato server può essere usato per ottenere lo stesso risultato.


## <a name="overview"></a>Panoramica

Il controllo Accordion in AJAX Control Toolkit fornisce più riquadri e consente all'utente di visualizzare uno di essi alla volta. I pannelli in genere vengono dichiarati all'interno della pagina stessa, ma il codice lato server può essere usato per ottenere lo stesso risultato.

## <a name="steps"></a>Passaggi

Il controllo Accordion espone tutte le proprietà importanti al codice lato server. Tra le altre cose, i `Panes` proprietà concede l'accesso alla raccolta di riquadri che costituiscono il controllo Accordion. Ogni riquadro è di tipo `AccordionPane`. È pertanto semplice per creare un riquadro di questo tipo:

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample1.vb)]

Il `HeaderContainer` proprietà di `AccordionPane` fornisce l'accesso ai controlli ASP.NET all'interno della sezione di intestazione del riquadro; le `ContentContainer` proprietà della `AccordionPane` esegue la stessa operazione per la sezione contenuta del riquadro. In questo modo il codice ASP.NET aggiungere contenuto nei riquadri di:

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample2.vb)]

Infine, è necessario aggiungere il riquadro per il `Panes` raccolta del controllo Accordion:

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample3.vb)]

Ecco un codice lato server completo che consente di aggiungere due riquadri a un controllo Accordion:

[!code-aspx[Main](dynamically-adding-an-accordion-pane-vb/samples/sample4.aspx)]

L'unico elemento manca è il controllo Accordion stesso, che dipende dalla presenza di ASP.NET `ScriptManager` controllo:

[!code-aspx[Main](dynamically-adding-an-accordion-pane-vb/samples/sample5.aspx)]

Per completare l'esempio, le due classi CSS a cui fa riferimento il controllo Accordion forniscono informazioni sullo stile per il browser:

[!code-css[Main](dynamically-adding-an-accordion-pane-vb/samples/sample6.css)]


[![I dati di controllo accordion è stato aggiunto in modo dinamico da codice lato server](dynamically-adding-an-accordion-pane-vb/_static/image2.png)](dynamically-adding-an-accordion-pane-vb/_static/image1.png)

I dati di controllo accordion è stato aggiunto in modo dinamico da codice lato server ([fare clic per visualizzare l'immagine con dimensioni normali](dynamically-adding-an-accordion-pane-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Precedente](databinding-to-an-accordion-vb.md)
