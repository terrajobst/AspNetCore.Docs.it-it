---
uid: web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-cs
title: Uso di più controlli Popup (c#) | Microsoft Docs
author: wenz
description: Il dispositivo extender PopupControl in AJAX Control Toolkit offre un modo semplice per attivare una finestra popup quando viene attivato un qualsiasi altro controllo. È anche possibile usare m...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 91511b0b-311d-481f-9e7c-73f07b813b79
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: 8cf7f929b696240e6805a74fb576ad56738491fa
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831569"
---
<a name="using-multiple-popup-controls-c"></a><span data-ttu-id="7087a-104">Uso di più controlli Popup (c#)</span><span class="sxs-lookup"><span data-stu-id="7087a-104">Using Multiple Popup Controls (C#)</span></span>
====================
<span data-ttu-id="7087a-105">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="7087a-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="7087a-106">[Scaricare il codice](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="7087a-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1CS.pdf)</span></span>

> <span data-ttu-id="7087a-107">Il dispositivo extender PopupControl in AJAX Control Toolkit offre un modo semplice per attivare una finestra popup quando viene attivato un qualsiasi altro controllo.</span><span class="sxs-lookup"><span data-stu-id="7087a-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="7087a-108">È anche possibile usare più di un controllo popup in un'unica pagina.</span><span class="sxs-lookup"><span data-stu-id="7087a-108">It is also possible to use more than one popup control on one page.</span></span>


## <a name="overview"></a><span data-ttu-id="7087a-109">Panoramica</span><span class="sxs-lookup"><span data-stu-id="7087a-109">Overview</span></span>

<span data-ttu-id="7087a-110">Il dispositivo extender PopupControl in AJAX Control Toolkit offre un modo semplice per attivare una finestra popup quando viene attivato un qualsiasi altro controllo.</span><span class="sxs-lookup"><span data-stu-id="7087a-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="7087a-111">È anche possibile usare più di un controllo popup in un'unica pagina.</span><span class="sxs-lookup"><span data-stu-id="7087a-111">It is also possible to use more than one popup control on one page.</span></span>

## <a name="steps"></a><span data-ttu-id="7087a-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="7087a-112">Steps</span></span>

<span data-ttu-id="7087a-113">Per attivare la funzionalità di ASP.NET AJAX e il Toolkit di controllo, il `ScriptManager` controllo deve essere inserito in un punto qualsiasi della pagina (ma entro la `<form>` elemento):</span><span class="sxs-lookup"><span data-stu-id="7087a-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample1.aspx)]

<span data-ttu-id="7087a-114">Successivamente, aggiungere un pannello che funge da finestra popup.</span><span class="sxs-lookup"><span data-stu-id="7087a-114">Next, add a panel which serves as the popup.</span></span> <span data-ttu-id="7087a-115">Nello scenario corrente, il pannello contiene un `Calendar` controllo.</span><span class="sxs-lookup"><span data-stu-id="7087a-115">In the current scenario, the panel contains a `Calendar` control.</span></span> <span data-ttu-id="7087a-116">Per evitare la pagina viene aggiornata causata dai postback del calendario, il pannello viene inserito all'interno di un `UpdatePanel` controllo:</span><span class="sxs-lookup"><span data-stu-id="7087a-116">In order to avoid the page refreshes caused by the Calendar's postbacks, the panel is put within an `UpdatePanel` control:</span></span>

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample2.aspx)]

<span data-ttu-id="7087a-117">La pagina contiene inoltre due caselle di testo.</span><span class="sxs-lookup"><span data-stu-id="7087a-117">The page also contains two text boxes.</span></span> <span data-ttu-id="7087a-118">Per ogni casella di testo, il calendario popup verranno visualizzati dopo aver attivata la casella di testo.</span><span class="sxs-lookup"><span data-stu-id="7087a-118">For each text box, the calendar popup shall appear once the text box is activated.</span></span>

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample3.aspx)]

<span data-ttu-id="7087a-119">A questo punto estendere ciascuna delle due caselle di testo con un `PopupControlExtender`.</span><span class="sxs-lookup"><span data-stu-id="7087a-119">Now extend each of the two text boxes with a `PopupControlExtender`.</span></span> <span data-ttu-id="7087a-120">Il `TargetControlID` attributo fornisce l'ID del controllo associato all'oggetto extender.</span><span class="sxs-lookup"><span data-stu-id="7087a-120">The `TargetControlID` attribute provides the ID of the control tied to the extender.</span></span> <span data-ttu-id="7087a-121">Il `PopupControlID` attributo contiene l'ID del pannello popup.</span><span class="sxs-lookup"><span data-stu-id="7087a-121">The `PopupControlID` attribute contains the ID of the popup panel.</span></span> <span data-ttu-id="7087a-122">In questo caso, entrambe le estensioni di visualizzare il pannello stesso, ma sono anche possibili, diversi pannelli.</span><span class="sxs-lookup"><span data-stu-id="7087a-122">In this case, both extenders show the same panel, but different panels are possible, as well.</span></span>

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample4.aspx)]

<span data-ttu-id="7087a-123">Ora ogni volta che si fa clic all'interno di un campo di testo, viene visualizzato un calendario sotto il campo, è possibile selezionare una data.</span><span class="sxs-lookup"><span data-stu-id="7087a-123">Now whenever you click within a text field, a calendar appears below the field, allowing you to select a date.</span></span> <span data-ttu-id="7087a-124">(Ricevendo la data selezionata nelle caselle di testo verrà trattato in un'esercitazione diversa.)</span><span class="sxs-lookup"><span data-stu-id="7087a-124">(Getting the selected date back into the text boxes will be covered in a different tutorial.)</span></span>


<span data-ttu-id="7087a-125">[![Quando l'utente fa clic nella casella di testo viene visualizzato il calendario](using-multiple-popup-controls-cs/_static/image2.png)](using-multiple-popup-controls-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="7087a-125">[![The Calendar appears when the user clicks into the textbox](using-multiple-popup-controls-cs/_static/image2.png)](using-multiple-popup-controls-cs/_static/image1.png)</span></span>

<span data-ttu-id="7087a-126">Il calendario viene visualizzato quando l'utente fa clic nella casella di testo ([fare clic per visualizzare l'immagine con dimensioni normali](using-multiple-popup-controls-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="7087a-126">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](using-multiple-popup-controls-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="7087a-127">avanti</span><span class="sxs-lookup"><span data-stu-id="7087a-127">Next</span></span>](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs.md)
