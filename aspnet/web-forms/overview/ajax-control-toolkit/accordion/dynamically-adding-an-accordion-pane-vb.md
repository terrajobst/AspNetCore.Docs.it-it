---
uid: web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-vb
title: Aggiunta dinamica di un riquadro Accordion (VB) | Microsoft Docs
author: wenz
description: Il controllo Accordion in AJAX Control Toolkit fornisce più riquadri e consente all'utente di visualizzare uno di essi alla volta. I pannelli vengono dichiarati in genere w...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: fae968c9-1902-487d-b053-86a46dd52c3f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-vb
msc.type: authoredcontent
ms.openlocfilehash: 1fdb95ee1ee93bc011a257e4e21c876dbbc7d2a9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820026"
---
<a name="dynamically-adding-an-accordion-pane-vb"></a><span data-ttu-id="edaa1-104">Aggiunta dinamica di un riquadro Accordion (VB)</span><span class="sxs-lookup"><span data-stu-id="edaa1-104">Dynamically Adding An Accordion Pane (VB)</span></span>
====================
<span data-ttu-id="edaa1-105">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="edaa1-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="edaa1-106">[Scaricare il codice](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="edaa1-106">[Download Code](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2VB.pdf)</span></span>

> <span data-ttu-id="edaa1-107">Il controllo Accordion in AJAX Control Toolkit fornisce più riquadri e consente all'utente di visualizzare uno di essi alla volta.</span><span class="sxs-lookup"><span data-stu-id="edaa1-107">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="edaa1-108">I pannelli in genere vengono dichiarati all'interno della pagina stessa, ma il codice lato server può essere usato per ottenere lo stesso risultato.</span><span class="sxs-lookup"><span data-stu-id="edaa1-108">Panels are usually declared within the page itself, but server-side code can be used to achieve the same result.</span></span>


## <a name="overview"></a><span data-ttu-id="edaa1-109">Panoramica</span><span class="sxs-lookup"><span data-stu-id="edaa1-109">Overview</span></span>

<span data-ttu-id="edaa1-110">Il controllo Accordion in AJAX Control Toolkit fornisce più riquadri e consente all'utente di visualizzare uno di essi alla volta.</span><span class="sxs-lookup"><span data-stu-id="edaa1-110">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="edaa1-111">I pannelli in genere vengono dichiarati all'interno della pagina stessa, ma il codice lato server può essere usato per ottenere lo stesso risultato.</span><span class="sxs-lookup"><span data-stu-id="edaa1-111">Panels are usually declared within the page itself, but server-side code can be used to achieve the same result.</span></span>

## <a name="steps"></a><span data-ttu-id="edaa1-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="edaa1-112">Steps</span></span>

<span data-ttu-id="edaa1-113">Il controllo Accordion espone tutte le proprietà importanti al codice lato server.</span><span class="sxs-lookup"><span data-stu-id="edaa1-113">The Accordion control exposes all important properties to server-side code.</span></span> <span data-ttu-id="edaa1-114">Tra le altre cose, i `Panes` proprietà concede l'accesso alla raccolta di riquadri che costituiscono il controllo Accordion.</span><span class="sxs-lookup"><span data-stu-id="edaa1-114">Among other things, the `Panes` property grants access to the collection of panes that make up the Accordion.</span></span> <span data-ttu-id="edaa1-115">Ogni riquadro è di tipo `AccordionPane`.</span><span class="sxs-lookup"><span data-stu-id="edaa1-115">Every pane there is of type `AccordionPane`.</span></span> <span data-ttu-id="edaa1-116">È pertanto semplice per creare un riquadro di questo tipo:</span><span class="sxs-lookup"><span data-stu-id="edaa1-116">It is therefore trivial to create such a pane:</span></span>

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample1.vb)]

<span data-ttu-id="edaa1-117">Il `HeaderContainer` proprietà di `AccordionPane` fornisce l'accesso ai controlli ASP.NET all'interno della sezione di intestazione del riquadro; le `ContentContainer` proprietà della `AccordionPane` esegue la stessa operazione per la sezione contenuta del riquadro.</span><span class="sxs-lookup"><span data-stu-id="edaa1-117">The `HeaderContainer` property of `AccordionPane` provides access to the ASP.NET controls within the header section of the pane; the `ContentContainer` property of `AccordionPane` does the same for the content section of the pane.</span></span> <span data-ttu-id="edaa1-118">In questo modo il codice ASP.NET aggiungere contenuto nei riquadri di:</span><span class="sxs-lookup"><span data-stu-id="edaa1-118">This allows ASP.NET code to add content to the panes:</span></span>

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample2.vb)]

<span data-ttu-id="edaa1-119">Infine, è necessario aggiungere il riquadro per il `Panes` raccolta del controllo Accordion:</span><span class="sxs-lookup"><span data-stu-id="edaa1-119">Finally, the pane(s) must be added to the `Panes` collection of the Accordion:</span></span>

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample3.vb)]

<span data-ttu-id="edaa1-120">Ecco un codice lato server completo che consente di aggiungere due riquadri a un controllo Accordion:</span><span class="sxs-lookup"><span data-stu-id="edaa1-120">Here is a complete server-side code that adds two panes to an Accordion control:</span></span>

[!code-aspx[Main](dynamically-adding-an-accordion-pane-vb/samples/sample4.aspx)]

<span data-ttu-id="edaa1-121">L'unico elemento manca è il controllo Accordion stesso, che dipende dalla presenza di ASP.NET `ScriptManager` controllo:</span><span class="sxs-lookup"><span data-stu-id="edaa1-121">The only missing element is the Accordion itself, which depends on the presence of the ASP.NET `ScriptManager` control:</span></span>

[!code-aspx[Main](dynamically-adding-an-accordion-pane-vb/samples/sample5.aspx)]

<span data-ttu-id="edaa1-122">Per completare l'esempio, le due classi CSS a cui fa riferimento il controllo Accordion forniscono informazioni sullo stile per il browser:</span><span class="sxs-lookup"><span data-stu-id="edaa1-122">To finish the example, the two CSS classes referenced in the Accordion control provide style information for the browser:</span></span>

[!code-css[Main](dynamically-adding-an-accordion-pane-vb/samples/sample6.css)]


<span data-ttu-id="edaa1-123">[![I dati di controllo accordion è stato aggiunto in modo dinamico da codice lato server](dynamically-adding-an-accordion-pane-vb/_static/image2.png)](dynamically-adding-an-accordion-pane-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="edaa1-123">[![The data in the accordion was dynamically added by server-side code](dynamically-adding-an-accordion-pane-vb/_static/image2.png)](dynamically-adding-an-accordion-pane-vb/_static/image1.png)</span></span>

<span data-ttu-id="edaa1-124">I dati di controllo accordion è stato aggiunto in modo dinamico da codice lato server ([fare clic per visualizzare l'immagine con dimensioni normali](dynamically-adding-an-accordion-pane-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="edaa1-124">The data in the accordion was dynamically added by server-side code ([Click to view full-size image](dynamically-adding-an-accordion-pane-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="edaa1-125">Precedente</span><span class="sxs-lookup"><span data-stu-id="edaa1-125">Previous</span></span>](databinding-to-an-accordion-vb.md)
