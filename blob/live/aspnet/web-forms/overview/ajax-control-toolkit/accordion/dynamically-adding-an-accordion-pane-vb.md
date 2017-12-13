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
<a name="dynamically-adding-an-accordion-pane-vb"></a><span data-ttu-id="8dbd3-104">Aggiunta dinamica di un riquadro soffietto (VB)</span><span class="sxs-lookup"><span data-stu-id="8dbd3-104">Dynamically Adding An Accordion Pane (VB)</span></span>
====================
<span data-ttu-id="8dbd3-105">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="8dbd3-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="8dbd3-106">[Scaricare codice](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="8dbd3-106">[Download Code](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2VB.pdf)</span></span>

> <span data-ttu-id="8dbd3-107">Il controllo Accordion AJAX Control Toolkit fornisce più riquadri e consente all'utente di visualizzare uno di essi alla volta.</span><span class="sxs-lookup"><span data-stu-id="8dbd3-107">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="8dbd3-108">I pannelli vengono in genere dichiarati all'interno della pagina, ma il codice lato server consente di ottenere lo stesso risultato.</span><span class="sxs-lookup"><span data-stu-id="8dbd3-108">Panels are usually declared within the page itself, but server-side code can be used to achieve the same result.</span></span>


## <a name="overview"></a><span data-ttu-id="8dbd3-109">Panoramica</span><span class="sxs-lookup"><span data-stu-id="8dbd3-109">Overview</span></span>

<span data-ttu-id="8dbd3-110">Il controllo Accordion AJAX Control Toolkit fornisce più riquadri e consente all'utente di visualizzare uno di essi alla volta.</span><span class="sxs-lookup"><span data-stu-id="8dbd3-110">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="8dbd3-111">I pannelli vengono in genere dichiarati all'interno della pagina, ma il codice lato server consente di ottenere lo stesso risultato.</span><span class="sxs-lookup"><span data-stu-id="8dbd3-111">Panels are usually declared within the page itself, but server-side code can be used to achieve the same result.</span></span>

## <a name="steps"></a><span data-ttu-id="8dbd3-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="8dbd3-112">Steps</span></span>

<span data-ttu-id="8dbd3-113">Il controllo Accordion espone tutte le proprietà importanti per il codice lato server.</span><span class="sxs-lookup"><span data-stu-id="8dbd3-113">The Accordion control exposes all important properties to server-side code.</span></span> <span data-ttu-id="8dbd3-114">Tra le altre cose, la `Panes` proprietà concede l'accesso alla raccolta di riquadri che costituiscono il Accordion.</span><span class="sxs-lookup"><span data-stu-id="8dbd3-114">Among other things, the `Panes` property grants access to the collection of panes that make up the Accordion.</span></span> <span data-ttu-id="8dbd3-115">Ogni riquadro è di tipo `AccordionPane`.</span><span class="sxs-lookup"><span data-stu-id="8dbd3-115">Every pane there is of type `AccordionPane`.</span></span> <span data-ttu-id="8dbd3-116">È pertanto più semplice per creare un riquadro di questo tipo:</span><span class="sxs-lookup"><span data-stu-id="8dbd3-116">It is therefore trivial to create such a pane:</span></span>

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample1.vb)]

<span data-ttu-id="8dbd3-117">Il `HeaderContainer` proprietà di `AccordionPane` fornisce l'accesso ai controlli ASP.NET all'interno della sezione di intestazione del riquadro; il `ContentContainer` proprietà di `AccordionPane` esegue la stessa operazione per la sezione di contenuto del riquadro.</span><span class="sxs-lookup"><span data-stu-id="8dbd3-117">The `HeaderContainer` property of `AccordionPane` provides access to the ASP.NET controls within the header section of the pane; the `ContentContainer` property of `AccordionPane` does the same for the content section of the pane.</span></span> <span data-ttu-id="8dbd3-118">In questo modo il codice ASP.NET aggiungere contenuto ai riquadri:</span><span class="sxs-lookup"><span data-stu-id="8dbd3-118">This allows ASP.NET code to add content to the panes:</span></span>

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample2.vb)]

<span data-ttu-id="8dbd3-119">Infine, è necessario aggiungere il riquadro per il `Panes` insieme il Accordion:</span><span class="sxs-lookup"><span data-stu-id="8dbd3-119">Finally, the pane(s) must be added to the `Panes` collection of the Accordion:</span></span>

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample3.vb)]

<span data-ttu-id="8dbd3-120">Di seguito è riportato un codice lato server completo che consente di aggiungere due riquadri a un controllo Accordion:</span><span class="sxs-lookup"><span data-stu-id="8dbd3-120">Here is a complete server-side code that adds two panes to an Accordion control:</span></span>

[!code-aspx[Main](dynamically-adding-an-accordion-pane-vb/samples/sample4.aspx)]

<span data-ttu-id="8dbd3-121">L'unico elemento manca è Accordion, che dipende dalla presenza di ASP.NET `ScriptManager` controllo:</span><span class="sxs-lookup"><span data-stu-id="8dbd3-121">The only missing element is the Accordion itself, which depends on the presence of the ASP.NET `ScriptManager` control:</span></span>

[!code-aspx[Main](dynamically-adding-an-accordion-pane-vb/samples/sample5.aspx)]

<span data-ttu-id="8dbd3-122">Per completare l'esempio, le due classi CSS a cui fa riferimento nel controllo Accordion forniscono informazioni sullo stile per il browser:</span><span class="sxs-lookup"><span data-stu-id="8dbd3-122">To finish the example, the two CSS classes referenced in the Accordion control provide style information for the browser:</span></span>

[!code-css[Main](dynamically-adding-an-accordion-pane-vb/samples/sample6.css)]


<span data-ttu-id="8dbd3-123">[![I dati di accordion è stato aggiunto in modo dinamico dal codice lato server](dynamically-adding-an-accordion-pane-vb/_static/image2.png)](dynamically-adding-an-accordion-pane-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="8dbd3-123">[![The data in the accordion was dynamically added by server-side code](dynamically-adding-an-accordion-pane-vb/_static/image2.png)](dynamically-adding-an-accordion-pane-vb/_static/image1.png)</span></span>

<span data-ttu-id="8dbd3-124">I dati di accordion è stato aggiunto in modo dinamico dal codice lato server ([fare clic per visualizzare l'immagine ingrandita](dynamically-adding-an-accordion-pane-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="8dbd3-124">The data in the accordion was dynamically added by server-side code ([Click to view full-size image](dynamically-adding-an-accordion-pane-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="8dbd3-125">Precedente</span><span class="sxs-lookup"><span data-stu-id="8dbd3-125">Previous</span></span>](databinding-to-an-accordion-vb.md)
