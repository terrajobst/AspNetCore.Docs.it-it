---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-cs
title: Gestione dei postback da un controllo Popup senza UpdatePanel (c#) | Microsoft Docs
author: wenz
description: Il dispositivo extender PopupControl in AJAX Control Toolkit offre un modo semplice per attivare una finestra popup quando viene attivato un qualsiasi altro controllo. Quando si verifica un postback in unità di streaming...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 25444121-5a72-4dac-8e50-ad2b7ac667af
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-cs
msc.type: authoredcontent
ms.openlocfilehash: d74a44a277bdcb460dc20b78bad3e5ed68445b4a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37370983"
---
<a name="handling-postbacks-from-a-popup-control-without-an-updatepanel-c"></a><span data-ttu-id="0799c-104">Gestione dei postback da un controllo Popup senza UpdatePanel (c#)</span><span class="sxs-lookup"><span data-stu-id="0799c-104">Handling Postbacks from A Popup Control Without an UpdatePanel (C#)</span></span>
====================
<span data-ttu-id="0799c-105">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="0799c-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="0799c-106">[Scaricare il codice](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="0799c-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3CS.pdf)</span></span>

> <span data-ttu-id="0799c-107">Il dispositivo extender PopupControl in AJAX Control Toolkit offre un modo semplice per attivare una finestra popup quando viene attivato un qualsiasi altro controllo.</span><span class="sxs-lookup"><span data-stu-id="0799c-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="0799c-108">Quando si verifica un postback in un pannello di questo tipo e sono presenti diversi pannelli nella pagina è difficile individuare il pannello è stato selezionato.</span><span class="sxs-lookup"><span data-stu-id="0799c-108">When a postback occurs in such a panel and there are several panels on the page it is hard to determine which panel has been clicked.</span></span>


## <a name="overview"></a><span data-ttu-id="0799c-109">Panoramica</span><span class="sxs-lookup"><span data-stu-id="0799c-109">Overview</span></span>

<span data-ttu-id="0799c-110">Il dispositivo extender PopupControl in AJAX Control Toolkit offre un modo semplice per attivare una finestra popup quando viene attivato un qualsiasi altro controllo.</span><span class="sxs-lookup"><span data-stu-id="0799c-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="0799c-111">Quando si verifica un postback in un pannello di questo tipo e sono presenti diversi pannelli nella pagina è difficile individuare il pannello è stato selezionato.</span><span class="sxs-lookup"><span data-stu-id="0799c-111">When a postback occurs in such a panel and there are several panels on the page it is hard to determine which panel has been clicked.</span></span>

## <a name="steps"></a><span data-ttu-id="0799c-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="0799c-112">Steps</span></span>

<span data-ttu-id="0799c-113">Quando si usa un' `PopupControl` con un postback, ma senza che sia un `UpdatePanel` nella pagina, il Toolkit di controllo non offre un modo per determinare quale elemento client ha avviato la finestra popup che a sua volta ha causato il postback.</span><span class="sxs-lookup"><span data-stu-id="0799c-113">When using a `PopupControl` with a postback, but without having an `UpdatePanel` on the page, the Control Toolkit does not offer a way to determine which client element has triggered the popup which in turn caused the postback.</span></span> <span data-ttu-id="0799c-114">Tuttavia un piccolo trucco fornisce una soluzione alternativa per questo scenario.</span><span class="sxs-lookup"><span data-stu-id="0799c-114">However a small trick provides a workaround for this scenario.</span></span>

<span data-ttu-id="0799c-115">Prima di tutto, ecco la configurazione di base: due caselle di testo che attivano il popup stesso, un calendario.</span><span class="sxs-lookup"><span data-stu-id="0799c-115">First of all, here is the basic setup: two text boxes which both trigger the same popup, a calendar.</span></span> <span data-ttu-id="0799c-116">Due `PopupControlExtenders` riunire le caselle di testo e finestra popup.</span><span class="sxs-lookup"><span data-stu-id="0799c-116">Two `PopupControlExtenders` bring text boxes and popup together.</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample1.aspx)]

<span data-ttu-id="0799c-117">L'idea di base consiste nell'aggiungere un campo del form nascosto il &lt; `form` &gt; elemento che contiene la casella di testo quale avviata la finestra popup:</span><span class="sxs-lookup"><span data-stu-id="0799c-117">The basic idea is to add a hidden form field in the &lt;`form`&gt; element that holds the text box which launched the popup:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample2.aspx)]

<span data-ttu-id="0799c-118">Quando la pagina viene caricata, il codice JavaScript aggiunge un gestore eventi per entrambe le caselle di testo: ogni volta che l'utente fa clic sulla casella di testo, il relativo nome viene scritto nel campo del form nascosto:</span><span class="sxs-lookup"><span data-stu-id="0799c-118">When the page is loaded, JavaScript code adds an event handler to both text boxes: Whenever the user clicks on a text box, its name is written into the hidden form field:</span></span>

[!code-html[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample3.html)]

<span data-ttu-id="0799c-119">Nel codice lato server, è necessario leggere il valore del campo nascosto.</span><span class="sxs-lookup"><span data-stu-id="0799c-119">In the server-side code, the value of the hidden field must be read.</span></span> <span data-ttu-id="0799c-120">Poiché sono semplici per modificare i campi modulo nascosti, è necessario un approccio di elenco elementi consentiti per la convalida del valore nascosto.</span><span class="sxs-lookup"><span data-stu-id="0799c-120">Since hidden form fields are trivial to manipulate, a whitelist approach to validate the hidden value is required.</span></span> <span data-ttu-id="0799c-121">Dopo aver individuata la casella di testo corretto, la data dal calendario viene scritta al suo interno.</span><span class="sxs-lookup"><span data-stu-id="0799c-121">Once the correct text box has been identified, the date from the calendar is written into it.</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample4.aspx)]


<span data-ttu-id="0799c-122">[![Quando l'utente fa clic nella casella di testo viene visualizzato il calendario](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="0799c-122">[![The Calendar appears when the user clicks into the textbox](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image1.png)</span></span>

<span data-ttu-id="0799c-123">Il calendario viene visualizzato quando l'utente fa clic nella casella di testo ([fare clic per visualizzare l'immagine con dimensioni normali](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="0799c-123">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image3.png))</span></span>


<span data-ttu-id="0799c-124">[![Facendo clic su una data lo inserisce nella casella di testo](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="0799c-124">[![Clicking on a date puts it in the textbox](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image4.png)</span></span>

<span data-ttu-id="0799c-125">Facendo clic su una data lo inserisce nella casella di testo ([fare clic per visualizzare l'immagine con dimensioni normali](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="0799c-125">Clicking on a date puts it in the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="0799c-126">[Precedente](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs.md)
> [Successivo](using-multiple-popup-controls-vb.md)</span><span class="sxs-lookup"><span data-stu-id="0799c-126">[Previous](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs.md)
[Next](using-multiple-popup-controls-vb.md)</span></span>
