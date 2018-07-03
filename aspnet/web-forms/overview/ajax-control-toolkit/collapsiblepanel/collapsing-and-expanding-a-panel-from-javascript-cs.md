---
uid: web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-cs
title: Compressione ed espansione di un pannello da JavaScript (c#) | Microsoft Docs
author: wenz
description: Il controllo CollapsiblePanel in ASP.NET AJAX Control Toolkit estende un pannello e fornisce funzionalità per comprimere il contenuto e per espanderlo in un...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: de5500be-75e5-461a-8064-b70ae52ea6a4
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-cs
msc.type: authoredcontent
ms.openlocfilehash: 85a9c6e6cc56cc563eefea6bc863950ba71149ba
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37402756"
---
<a name="collapsing-and-expanding-a-panel-from-javascript-c"></a><span data-ttu-id="66130-103">Compressione ed espansione di un pannello da JavaScript (c#)</span><span class="sxs-lookup"><span data-stu-id="66130-103">Collapsing and Expanding a Panel from JavaScript (C#)</span></span>
====================
<span data-ttu-id="66130-104">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="66130-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="66130-105">[Scaricare il codice](http://download.microsoft.com/download/8/a/a/8aab3c3e-de6f-463f-805c-5fda567eef6e/CollapsiblePanel1.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/collapsiblepanel1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="66130-105">[Download Code](http://download.microsoft.com/download/8/a/a/8aab3c3e-de6f-463f-805c-5fda567eef6e/CollapsiblePanel1.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/collapsiblepanel1CS.pdf)</span></span>

> <span data-ttu-id="66130-106">Il controllo CollapsiblePanel in ASP.NET AJAX Control Toolkit estende un pannello e lo inserisce in grado di comprimere il contenuto ed espanderla nuovamente.</span><span class="sxs-lookup"><span data-stu-id="66130-106">The CollapsiblePanel control in the ASP.NET AJAX Control Toolkit extends a panel and provides it with the capability to collapse its contents and expand it again.</span></span> <span data-ttu-id="66130-107">Queste due azioni possono essere attivate anche dal codice JavaScript personalizzato.</span><span class="sxs-lookup"><span data-stu-id="66130-107">These two actions can also be triggered from custom JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="66130-108">Panoramica</span><span class="sxs-lookup"><span data-stu-id="66130-108">Overview</span></span>

<span data-ttu-id="66130-109">Il controllo CollapsiblePanel in ASP.NET AJAX Control Toolkit estende un pannello e lo inserisce in grado di comprimere il contenuto ed espanderla nuovamente.</span><span class="sxs-lookup"><span data-stu-id="66130-109">The CollapsiblePanel control in the ASP.NET AJAX Control Toolkit extends a panel and provides it with the capability to collapse its contents and expand it again.</span></span> <span data-ttu-id="66130-110">Queste due azioni possono essere attivate anche dal codice JavaScript personalizzato.</span><span class="sxs-lookup"><span data-stu-id="66130-110">These two actions can also be triggered from custom JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="66130-111">Passaggi</span><span class="sxs-lookup"><span data-stu-id="66130-111">Steps</span></span>

<span data-ttu-id="66130-112">Prima di tutto, creare una nuova pagina ASP.NET e includere il `ScriptManager` all'interno di quella `<form>` elemento.</span><span class="sxs-lookup"><span data-stu-id="66130-112">First of all, create a new ASP.NET page and include the `ScriptManager` within the one `<form>` element.</span></span> <span data-ttu-id="66130-113">Consente di caricare la libreria ASP.NET AJAX che è necessario per il Toolkit di controllo:</span><span class="sxs-lookup"><span data-stu-id="66130-113">This loads the ASP.NET AJAX library which is required by the Control Toolkit:</span></span>

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample1.aspx)]

<span data-ttu-id="66130-114">Creare quindi un pannello con un testo in modo da poter visualizzare l'effetto di compressione/espansione:</span><span class="sxs-lookup"><span data-stu-id="66130-114">Then, create a panel with some text so that the collapse/expand effect can be seen:</span></span>

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample2.aspx)]

<span data-ttu-id="66130-115">Come può notare, il pannello fa riferimento a una classe CSS che è illustrata di seguito (e definisce in sostanza un colore di sfondo e la larghezza del pannello):</span><span class="sxs-lookup"><span data-stu-id="66130-115">As you can see, the panel references a CSS class which is shown here (and basically defines a background color and the panel's width):</span></span>

[!code-css[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample3.css)]

<span data-ttu-id="66130-116">Il `CollapsiblePanelExtender` controllo richiede la `TargetControlID` attributo in modo che il toolkit sappia quale pannello per comprimere o espandere su richiesta:</span><span class="sxs-lookup"><span data-stu-id="66130-116">The `CollapsiblePanelExtender` control requires the `TargetControlID` attribute so that the toolkit knows which panel to collapse or expand upon request:</span></span>

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample4.aspx)]

<span data-ttu-id="66130-117">Sfortunatamente, il dispositivo extender attualmente non espone un'API specifica per comprimere o espandere il pannello, ma si eseguiranno alcuni metodi non documentate.</span><span class="sxs-lookup"><span data-stu-id="66130-117">Unfortunately, the extender currently does not expose a specific API for collapsing or expanding the panel, but some undocumented methods will do.</span></span> <span data-ttu-id="66130-118">Prima di tutto, aggiungere tre pulsanti HTML alla pagina che attiverà il codice JavaScript lato client per comprimere o espandere il contenuto del pannello:</span><span class="sxs-lookup"><span data-stu-id="66130-118">First of all, add three HTML buttons to the page which will then trigger the client-side JavaScript to collapse or expand the panel's contents:</span></span>

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample5.aspx)]

<span data-ttu-id="66130-119">Nel codice JavaScript lato client (Introduzione `<script type="text/javascript">`), il `$find()` metodo deve essere utilizzato per l'accesso il `CollapsiblePanelExtender`.</span><span class="sxs-lookup"><span data-stu-id="66130-119">In the client-side JavaScript code (started with `<script type="text/javascript">`), the `$find()` method needs to be used to access the `CollapsiblePanelExtender`.</span></span> <span data-ttu-id="66130-120">`$find("cpe")` restituirà un riferimento a esso.</span><span class="sxs-lookup"><span data-stu-id="66130-120">`$find("cpe")` will return a reference to it.</span></span> <span data-ttu-id="66130-121">Da tale data, i metodi specifici risolverà l'attività in questione.</span><span class="sxs-lookup"><span data-stu-id="66130-121">From there on, specific methods will solve the task at hand.</span></span>

<span data-ttu-id="66130-122">Il metodo per l'apertura (espansione) viene chiamato il pannello `_doOpen()`; il codice seguente implementa il `doOpen()` funzione chiamata quando si fa clic sul primo pulsante:</span><span class="sxs-lookup"><span data-stu-id="66130-122">The method for opening (expanding) the panel is called `_doOpen()`; the following code implements the `doOpen()` function called when the first button is clicked:</span></span>

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample6.js)]

<span data-ttu-id="66130-123">Per la chiusura o comprimere il pannello di `_doClose()` metodo deve essere eseguito.</span><span class="sxs-lookup"><span data-stu-id="66130-123">For closing, or collapsing the panel, the `_doClose()` method needs to be executed.</span></span> <span data-ttu-id="66130-124">Pertanto, quando l'utente fa clic sul secondo pulsante, viene chiamato il codice JavaScript seguente:</span><span class="sxs-lookup"><span data-stu-id="66130-124">So when the user clicks on the second button, the following JavaScript code is called:</span></span>

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample7.js)]

<span data-ttu-id="66130-125">Il terzo pulsante Attiva/disattiva lo stato del pannello: da compressa per espanso e viceversa.</span><span class="sxs-lookup"><span data-stu-id="66130-125">The third button toggles the state of the panel: from collapsed to expanded, and vice versa.</span></span> <span data-ttu-id="66130-126">Il `CollapsiblePanelExtender` espone il `toggle()` metodo che è adibita: consente di invertire lo stato del pannello.</span><span class="sxs-lookup"><span data-stu-id="66130-126">The `CollapsiblePanelExtender` exposes the `toggle()` method which does exactly that: reverses the state of the panel.</span></span> <span data-ttu-id="66130-127">È inoltre disponibile tuttavia un altro approccio (che viene usato internamente dal `toggle()` (metodo)): il `get_Collapsed()` metodo del `CollapsiblePanelExtender()` per indicare se il pannello viene compressa o non.</span><span class="sxs-lookup"><span data-stu-id="66130-127">However there is also another approach (which is internally used by the `toggle()` method): The `get_Collapsed()` method of the `CollapsiblePanelExtender()` tells us whether the panel is collapsed or not.</span></span> <span data-ttu-id="66130-128">A seconda del valore restituito di questa funzione, il pannello viene quindi espansa uno (`_doOpen()` metodo) o compresso (`_doClose()`) metodo:</span><span class="sxs-lookup"><span data-stu-id="66130-128">Depending on the return value of this function, the panel is then either expanded (`_doOpen()` method) or collapsed (`_doClose()`) method:</span></span>

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample8.js)]


<span data-ttu-id="66130-129">[![Il terzo pulsante Modifica lo stato del pannello: da compresso espanso e indietro](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image2.png)](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="66130-129">[![The third button changes the state of the panel: from collapsed to expanded and back](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image2.png)](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image1.png)</span></span>

<span data-ttu-id="66130-130">Il terzo pulsante Modifica lo stato del pannello: da compresso espanso e back-([fare clic per visualizzare l'immagine con dimensioni normali](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="66130-130">The third button changes the state of the panel: from collapsed to expanded and back ([Click to view full-size image](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="66130-131">avanti</span><span class="sxs-lookup"><span data-stu-id="66130-131">Next</span></span>](collapsing-and-expanding-a-panel-from-javascript-vb.md)
