---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-cs
title: Gestione dei postback da un controllo Popup senza un UpdatePanel (c#) | Documenti Microsoft
author: wenz
description: L'estensione PopupControl AJAX Control Toolkit offre un modo semplice per attivare una finestra popup quando viene attivato un altro controllo. Quando si verifica un postback in su...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 25444121-5a72-4dac-8e50-ad2b7ac667af
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-cs
msc.type: authoredcontent
ms.openlocfilehash: df43052950b6186908fe1baf04808f40cb926f69
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="handling-postbacks-from-a-popup-control-without-an-updatepanel-c"></a><span data-ttu-id="6d765-104">Gestione dei postback da un controllo Popup senza un UpdatePanel (c#)</span><span class="sxs-lookup"><span data-stu-id="6d765-104">Handling Postbacks from A Popup Control Without an UpdatePanel (C#)</span></span>
====================
<span data-ttu-id="6d765-105">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="6d765-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="6d765-106">[Scaricare codice](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="6d765-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3CS.pdf)</span></span>

> <span data-ttu-id="6d765-107">L'estensione PopupControl AJAX Control Toolkit offre un modo semplice per attivare una finestra popup quando viene attivato un altro controllo.</span><span class="sxs-lookup"><span data-stu-id="6d765-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="6d765-108">Quando si verifica un postback in tali pannelli e sono presenti diversi riquadri della pagina è difficile determinare quale pannello è stato fatto clic.</span><span class="sxs-lookup"><span data-stu-id="6d765-108">When a postback occurs in such a panel and there are several panels on the page it is hard to determine which panel has been clicked.</span></span>


## <a name="overview"></a><span data-ttu-id="6d765-109">Panoramica</span><span class="sxs-lookup"><span data-stu-id="6d765-109">Overview</span></span>

<span data-ttu-id="6d765-110">L'estensione PopupControl AJAX Control Toolkit offre un modo semplice per attivare una finestra popup quando viene attivato un altro controllo.</span><span class="sxs-lookup"><span data-stu-id="6d765-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="6d765-111">Quando si verifica un postback in tali pannelli e sono presenti diversi riquadri della pagina è difficile determinare quale pannello è stato fatto clic.</span><span class="sxs-lookup"><span data-stu-id="6d765-111">When a postback occurs in such a panel and there are several panels on the page it is hard to determine which panel has been clicked.</span></span>

## <a name="steps"></a><span data-ttu-id="6d765-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="6d765-112">Steps</span></span>

<span data-ttu-id="6d765-113">Quando si utilizza un `PopupControl` con un postback, ma senza un `UpdatePanel` nella pagina, il Toolkit di controllo non offre un modo per determinare quale elemento client è attivata la finestra popup che a sua volta ha provocato il postback.</span><span class="sxs-lookup"><span data-stu-id="6d765-113">When using a `PopupControl` with a postback, but without having an `UpdatePanel` on the page, the Control Toolkit does not offer a way to determine which client element has triggered the popup which in turn caused the postback.</span></span> <span data-ttu-id="6d765-114">Un piccolo stratagemma tuttavia offre una soluzione alternativa per questo scenario.</span><span class="sxs-lookup"><span data-stu-id="6d765-114">However a small trick provides a workaround for this scenario.</span></span>

<span data-ttu-id="6d765-115">Ecco prima di tutto, il programma di installazione di base: due caselle di testo che attivano la stessa finestra popup, un calendario.</span><span class="sxs-lookup"><span data-stu-id="6d765-115">First of all, here is the basic setup: two text boxes which both trigger the same popup, a calendar.</span></span> <span data-ttu-id="6d765-116">Due `PopupControlExtenders` riunire le caselle di testo e popup.</span><span class="sxs-lookup"><span data-stu-id="6d765-116">Two `PopupControlExtenders` bring text boxes and popup together.</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample1.aspx)]

<span data-ttu-id="6d765-117">L'idea di base consiste nell'aggiungere un campo del form nascosto il &lt; `form` &gt; elemento che contiene la casella di testo quale avviata la finestra popup:</span><span class="sxs-lookup"><span data-stu-id="6d765-117">The basic idea is to add a hidden form field in the &lt;`form`&gt; element that holds the text box which launched the popup:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample2.aspx)]

<span data-ttu-id="6d765-118">Quando viene caricata la pagina, il codice JavaScript aggiunge un gestore eventi per entrambe le caselle di testo: ogni volta che l'utente fa clic su una casella di testo, il relativo nome viene scritto nel campo del form nascosto:</span><span class="sxs-lookup"><span data-stu-id="6d765-118">When the page is loaded, JavaScript code adds an event handler to both text boxes: Whenever the user clicks on a text box, its name is written into the hidden form field:</span></span>

[!code-html[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample3.html)]

<span data-ttu-id="6d765-119">Nel codice sul lato server, è necessario leggere il valore del campo nascosto.</span><span class="sxs-lookup"><span data-stu-id="6d765-119">In the server-side code, the value of the hidden field must be read.</span></span> <span data-ttu-id="6d765-120">Poiché i campi del form nascosto sono semplici da gestire, è necessario un approccio di whitelist per convalidare il valore hidden.</span><span class="sxs-lookup"><span data-stu-id="6d765-120">Since hidden form fields are trivial to manipulate, a whitelist approach to validate the hidden value is required.</span></span> <span data-ttu-id="6d765-121">Dopo aver individuata la casella di testo corretto, la data dal calendario viene scritto.</span><span class="sxs-lookup"><span data-stu-id="6d765-121">Once the correct text box has been identified, the date from the calendar is written into it.</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample4.aspx)]


<span data-ttu-id="6d765-122">[![Il calendario viene visualizzato quando l'utente fa clic nella casella di testo](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="6d765-122">[![The Calendar appears when the user clicks into the textbox](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image1.png)</span></span>

<span data-ttu-id="6d765-123">Il calendario viene visualizzato quando l'utente fa clic nella casella di testo ([fare clic per visualizzare l'immagine ingrandita](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="6d765-123">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image3.png))</span></span>


<span data-ttu-id="6d765-124">[![Facendo clic su una data lo inserisce nella casella di testo](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="6d765-124">[![Clicking on a date puts it in the textbox](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image4.png)</span></span>

<span data-ttu-id="6d765-125">Facendo clic su una data lo inserisce nella casella di testo ([fare clic per visualizzare l'immagine ingrandita](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="6d765-125">Clicking on a date puts it in the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image6.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="6d765-126">[Precedente](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs.md)
[Successivo](using-multiple-popup-controls-vb.md)</span><span class="sxs-lookup"><span data-stu-id="6d765-126">[Previous](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs.md)
[Next](using-multiple-popup-controls-vb.md)</span></span>
