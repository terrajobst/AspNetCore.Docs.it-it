---
uid: web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-vb
title: Utilizzo di più controlli Popup (VB) | Documenti Microsoft
author: wenz
description: L'estensione PopupControl AJAX Control Toolkit offre un modo semplice per attivare una finestra popup quando viene attivato un altro controllo. È inoltre possibile utilizzare m...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 4da43d77-f6c4-43a8-9124-f1e8e1c8f0a2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: 7c57aab3ecf2c02a8488b5ea4e3e0ed33ac5e7fe
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869322"
---
<a name="using-multiple-popup-controls-vb"></a><span data-ttu-id="7c15d-104">Utilizzo di più controlli Popup (VB)</span><span class="sxs-lookup"><span data-stu-id="7c15d-104">Using Multiple Popup Controls (VB)</span></span>
====================
<span data-ttu-id="7c15d-105">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="7c15d-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="7c15d-106">[Scaricare codice](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="7c15d-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1VB.pdf)</span></span>

> <span data-ttu-id="7c15d-107">L'estensione PopupControl AJAX Control Toolkit offre un modo semplice per attivare una finestra popup quando viene attivato un altro controllo.</span><span class="sxs-lookup"><span data-stu-id="7c15d-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="7c15d-108">È inoltre possibile utilizzare più di un controllo popup in un'unica pagina.</span><span class="sxs-lookup"><span data-stu-id="7c15d-108">It is also possible to use more than one popup control on one page.</span></span>


## <a name="overview"></a><span data-ttu-id="7c15d-109">Panoramica</span><span class="sxs-lookup"><span data-stu-id="7c15d-109">Overview</span></span>

<span data-ttu-id="7c15d-110">L'estensione PopupControl AJAX Control Toolkit offre un modo semplice per attivare una finestra popup quando viene attivato un altro controllo.</span><span class="sxs-lookup"><span data-stu-id="7c15d-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="7c15d-111">È inoltre possibile utilizzare più di un controllo popup in un'unica pagina.</span><span class="sxs-lookup"><span data-stu-id="7c15d-111">It is also possible to use more than one popup control on one page.</span></span>

## <a name="steps"></a><span data-ttu-id="7c15d-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="7c15d-112">Steps</span></span>

<span data-ttu-id="7c15d-113">Per attivare le funzionalità di ASP.NET AJAX e il Toolkit di controllo, il `ScriptManager` controllo deve essere inserito in un punto qualsiasi della pagina (ma entro il `<form>` elemento):</span><span class="sxs-lookup"><span data-stu-id="7c15d-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample1.aspx)]

<span data-ttu-id="7c15d-114">Successivamente, aggiungere un pannello che funge da finestra popup.</span><span class="sxs-lookup"><span data-stu-id="7c15d-114">Next, add a panel which serves as the popup.</span></span> <span data-ttu-id="7c15d-115">In questo scenario, il pannello contiene un `Calendar` controllo.</span><span class="sxs-lookup"><span data-stu-id="7c15d-115">In the current scenario, the panel contains a `Calendar` control.</span></span> <span data-ttu-id="7c15d-116">Per evitare la pagina viene aggiornata dovuta postback del calendario, il pannello viene inserito all'interno di un `UpdatePanel` controllo:</span><span class="sxs-lookup"><span data-stu-id="7c15d-116">In order to avoid the page refreshes caused by the Calendar's postbacks, the panel is put within an `UpdatePanel` control:</span></span>

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample2.aspx)]

<span data-ttu-id="7c15d-117">La pagina contiene inoltre due caselle di testo.</span><span class="sxs-lookup"><span data-stu-id="7c15d-117">The page also contains two text boxes.</span></span> <span data-ttu-id="7c15d-118">Per ogni casella di testo, appare la finestra popup di calendario dopo aver attivata la casella di testo.</span><span class="sxs-lookup"><span data-stu-id="7c15d-118">For each text box, the calendar popup shall appear once the text box is activated.</span></span>

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample3.aspx)]

<span data-ttu-id="7c15d-119">Ora estendere ciascuna delle due caselle di testo con un `PopupControlExtender`.</span><span class="sxs-lookup"><span data-stu-id="7c15d-119">Now extend each of the two text boxes with a `PopupControlExtender`.</span></span> <span data-ttu-id="7c15d-120">Il `TargetControlID` attributo fornisce l'ID del controllo associato all'oggetto di estensione.</span><span class="sxs-lookup"><span data-stu-id="7c15d-120">The `TargetControlID` attribute provides the ID of the control tied to the extender.</span></span> <span data-ttu-id="7c15d-121">Il `PopupControlID` attributo contiene l'ID del pannello del popup.</span><span class="sxs-lookup"><span data-stu-id="7c15d-121">The `PopupControlID` attribute contains the ID of the popup panel.</span></span> <span data-ttu-id="7c15d-122">In questo caso, entrambi Extender Mostra stesso riquadro, ma sono anche possibili, diversi pannelli.</span><span class="sxs-lookup"><span data-stu-id="7c15d-122">In this case, both extenders show the same panel, but different panels are possible, as well.</span></span>

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample4.aspx)]

<span data-ttu-id="7c15d-123">Ora ogni volta che si fa clic all'interno di un campo di testo, viene visualizzato un calendario sotto il campo, che consente di selezionare una data.</span><span class="sxs-lookup"><span data-stu-id="7c15d-123">Now whenever you click within a text field, a calendar appears below the field, allowing you to select a date.</span></span> <span data-ttu-id="7c15d-124">(Ottenere la data selezionata nelle caselle di testo verrà trattato in un'esercitazione diversa.)</span><span class="sxs-lookup"><span data-stu-id="7c15d-124">(Getting the selected date back into the text boxes will be covered in a different tutorial.)</span></span>


<span data-ttu-id="7c15d-125">[![Il calendario viene visualizzato quando l'utente fa clic nella casella di testo](using-multiple-popup-controls-vb/_static/image2.png)](using-multiple-popup-controls-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="7c15d-125">[![The Calendar appears when the user clicks into the textbox](using-multiple-popup-controls-vb/_static/image2.png)](using-multiple-popup-controls-vb/_static/image1.png)</span></span>

<span data-ttu-id="7c15d-126">Il calendario viene visualizzato quando l'utente fa clic nella casella di testo ([fare clic per visualizzare l'immagine ingrandita](using-multiple-popup-controls-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="7c15d-126">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](using-multiple-popup-controls-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="7c15d-127">[Precedente](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)
> [Successivo](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb.md)</span><span class="sxs-lookup"><span data-stu-id="7c15d-127">[Previous](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)
[Next](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb.md)</span></span>
