---
uid: web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-vb
title: Uso di più controlli Popup (VB) | Microsoft Docs
author: wenz
description: Il dispositivo extender PopupControl in AJAX Control Toolkit offre un modo semplice per attivare una finestra popup quando viene attivato un qualsiasi altro controllo. È anche possibile usare m...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 4da43d77-f6c4-43a8-9124-f1e8e1c8f0a2
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: 5a2be36ecf17a95d53e5e912ba0a90113f70f2fb
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37807635"
---
<a name="using-multiple-popup-controls-vb"></a><span data-ttu-id="579f1-104">Uso di più controlli Popup (VB)</span><span class="sxs-lookup"><span data-stu-id="579f1-104">Using Multiple Popup Controls (VB)</span></span>
====================
<span data-ttu-id="579f1-105">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="579f1-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="579f1-106">[Scaricare il codice](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="579f1-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1VB.pdf)</span></span>

> <span data-ttu-id="579f1-107">Il dispositivo extender PopupControl in AJAX Control Toolkit offre un modo semplice per attivare una finestra popup quando viene attivato un qualsiasi altro controllo.</span><span class="sxs-lookup"><span data-stu-id="579f1-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="579f1-108">È anche possibile usare più di un controllo popup in un'unica pagina.</span><span class="sxs-lookup"><span data-stu-id="579f1-108">It is also possible to use more than one popup control on one page.</span></span>


## <a name="overview"></a><span data-ttu-id="579f1-109">Panoramica</span><span class="sxs-lookup"><span data-stu-id="579f1-109">Overview</span></span>

<span data-ttu-id="579f1-110">Il dispositivo extender PopupControl in AJAX Control Toolkit offre un modo semplice per attivare una finestra popup quando viene attivato un qualsiasi altro controllo.</span><span class="sxs-lookup"><span data-stu-id="579f1-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="579f1-111">È anche possibile usare più di un controllo popup in un'unica pagina.</span><span class="sxs-lookup"><span data-stu-id="579f1-111">It is also possible to use more than one popup control on one page.</span></span>

## <a name="steps"></a><span data-ttu-id="579f1-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="579f1-112">Steps</span></span>

<span data-ttu-id="579f1-113">Per attivare la funzionalità di ASP.NET AJAX e il Toolkit di controllo, il `ScriptManager` controllo deve essere inserito in un punto qualsiasi della pagina (ma entro la `<form>` elemento):</span><span class="sxs-lookup"><span data-stu-id="579f1-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample1.aspx)]

<span data-ttu-id="579f1-114">Successivamente, aggiungere un pannello che funge da finestra popup.</span><span class="sxs-lookup"><span data-stu-id="579f1-114">Next, add a panel which serves as the popup.</span></span> <span data-ttu-id="579f1-115">Nello scenario corrente, il pannello contiene un `Calendar` controllo.</span><span class="sxs-lookup"><span data-stu-id="579f1-115">In the current scenario, the panel contains a `Calendar` control.</span></span> <span data-ttu-id="579f1-116">Per evitare la pagina viene aggiornata causata dai postback del calendario, il pannello viene inserito all'interno di un `UpdatePanel` controllo:</span><span class="sxs-lookup"><span data-stu-id="579f1-116">In order to avoid the page refreshes caused by the Calendar's postbacks, the panel is put within an `UpdatePanel` control:</span></span>

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample2.aspx)]

<span data-ttu-id="579f1-117">La pagina contiene inoltre due caselle di testo.</span><span class="sxs-lookup"><span data-stu-id="579f1-117">The page also contains two text boxes.</span></span> <span data-ttu-id="579f1-118">Per ogni casella di testo, il calendario popup verranno visualizzati dopo aver attivata la casella di testo.</span><span class="sxs-lookup"><span data-stu-id="579f1-118">For each text box, the calendar popup shall appear once the text box is activated.</span></span>

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample3.aspx)]

<span data-ttu-id="579f1-119">A questo punto estendere ciascuna delle due caselle di testo con un `PopupControlExtender`.</span><span class="sxs-lookup"><span data-stu-id="579f1-119">Now extend each of the two text boxes with a `PopupControlExtender`.</span></span> <span data-ttu-id="579f1-120">Il `TargetControlID` attributo fornisce l'ID del controllo associato all'oggetto extender.</span><span class="sxs-lookup"><span data-stu-id="579f1-120">The `TargetControlID` attribute provides the ID of the control tied to the extender.</span></span> <span data-ttu-id="579f1-121">Il `PopupControlID` attributo contiene l'ID del pannello popup.</span><span class="sxs-lookup"><span data-stu-id="579f1-121">The `PopupControlID` attribute contains the ID of the popup panel.</span></span> <span data-ttu-id="579f1-122">In questo caso, entrambe le estensioni di visualizzare il pannello stesso, ma sono anche possibili, diversi pannelli.</span><span class="sxs-lookup"><span data-stu-id="579f1-122">In this case, both extenders show the same panel, but different panels are possible, as well.</span></span>

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample4.aspx)]

<span data-ttu-id="579f1-123">Ora ogni volta che si fa clic all'interno di un campo di testo, viene visualizzato un calendario sotto il campo, è possibile selezionare una data.</span><span class="sxs-lookup"><span data-stu-id="579f1-123">Now whenever you click within a text field, a calendar appears below the field, allowing you to select a date.</span></span> <span data-ttu-id="579f1-124">(Ricevendo la data selezionata nelle caselle di testo verrà trattato in un'esercitazione diversa.)</span><span class="sxs-lookup"><span data-stu-id="579f1-124">(Getting the selected date back into the text boxes will be covered in a different tutorial.)</span></span>


<span data-ttu-id="579f1-125">[![Quando l'utente fa clic nella casella di testo viene visualizzato il calendario](using-multiple-popup-controls-vb/_static/image2.png)](using-multiple-popup-controls-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="579f1-125">[![The Calendar appears when the user clicks into the textbox](using-multiple-popup-controls-vb/_static/image2.png)](using-multiple-popup-controls-vb/_static/image1.png)</span></span>

<span data-ttu-id="579f1-126">Il calendario viene visualizzato quando l'utente fa clic nella casella di testo ([fare clic per visualizzare l'immagine con dimensioni normali](using-multiple-popup-controls-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="579f1-126">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](using-multiple-popup-controls-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="579f1-127">[Precedente](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)
> [Successivo](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb.md)</span><span class="sxs-lookup"><span data-stu-id="579f1-127">[Previous](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)
[Next](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb.md)</span></span>
