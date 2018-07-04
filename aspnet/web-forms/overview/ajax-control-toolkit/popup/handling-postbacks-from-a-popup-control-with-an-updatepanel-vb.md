---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-vb
title: Gestione dei postback da un controllo Popup con UpdatePanel (VB) | Microsoft Docs
author: wenz
description: Il dispositivo extender PopupControl in AJAX Control Toolkit offre un modo semplice per attivare una finestra popup quando viene attivato un qualsiasi altro controllo. Deve essere adottata particolare attenzione...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: ec9db57c-9f68-402a-bf4c-0d63d5f6908e
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-vb
msc.type: authoredcontent
ms.openlocfilehash: cf17025219fbf24e749c172f2ddb47ec20980cdc
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37395903"
---
<a name="handling-postbacks-from-a-popup-control-with-an-updatepanel-vb"></a><span data-ttu-id="9affd-104">Gestione dei postback da un controllo Popup con UpdatePanel (VB)</span><span class="sxs-lookup"><span data-stu-id="9affd-104">Handling Postbacks from A Popup Control With an UpdatePanel (VB)</span></span>
====================
<span data-ttu-id="9affd-105">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="9affd-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="9affd-106">[Scaricare il codice](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="9affd-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2VB.pdf)</span></span>

> <span data-ttu-id="9affd-107">Il dispositivo extender PopupControl in AJAX Control Toolkit offre un modo semplice per attivare una finestra popup quando viene attivato un qualsiasi altro controllo.</span><span class="sxs-lookup"><span data-stu-id="9affd-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="9affd-108">Particolare attenzione è da eseguire quando si verifica un postback all'interno di questo tipo una finestra popup.</span><span class="sxs-lookup"><span data-stu-id="9affd-108">Special care has to be taken when a postback occurs within such a popup.</span></span>


## <a name="overview"></a><span data-ttu-id="9affd-109">Panoramica</span><span class="sxs-lookup"><span data-stu-id="9affd-109">Overview</span></span>

<span data-ttu-id="9affd-110">Il dispositivo extender PopupControl in AJAX Control Toolkit offre un modo semplice per attivare una finestra popup quando viene attivato un qualsiasi altro controllo.</span><span class="sxs-lookup"><span data-stu-id="9affd-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="9affd-111">Particolare attenzione è da eseguire quando si verifica un postback all'interno di questo tipo una finestra popup.</span><span class="sxs-lookup"><span data-stu-id="9affd-111">Special care has to be taken when a postback occurs within such a popup.</span></span>

## <a name="steps"></a><span data-ttu-id="9affd-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="9affd-112">Steps</span></span>

<span data-ttu-id="9affd-113">Quando si usa un' `PopupControl` con un postback, un `UpdatePanel` può impedire l'aggiornamento di pagina causato dal postback.</span><span class="sxs-lookup"><span data-stu-id="9affd-113">When using a `PopupControl` with a postback, an `UpdatePanel` can prevent the page refresh caused by the postback.</span></span> <span data-ttu-id="9affd-114">Il markup seguente definisce un paio di aspetti importanti:</span><span class="sxs-lookup"><span data-stu-id="9affd-114">The following markup defines a couple of important elements:</span></span>

- <span data-ttu-id="9affd-115">Oggetto `ScriptManager` controllare in modo che funzioni ASP.NET AJAX Control Toolkit</span><span class="sxs-lookup"><span data-stu-id="9affd-115">A `ScriptManager` control so that the ASP.NET AJAX Control Toolkit works</span></span>
- <span data-ttu-id="9affd-116">Due `TextBox` controlli che verranno attivata una finestra popup</span><span class="sxs-lookup"><span data-stu-id="9affd-116">Two `TextBox` controls which will both trigger a popup</span></span>
- <span data-ttu-id="9affd-117">Oggetto `Panel` controllo che servirà come il controllo popup</span><span class="sxs-lookup"><span data-stu-id="9affd-117">A `Panel` control that will serve as the popup</span></span>
- <span data-ttu-id="9affd-118">All'interno del pannello, una `Calendar` controllo incorporato all'interno di un `UpdatePanel` controllo</span><span class="sxs-lookup"><span data-stu-id="9affd-118">Within the panel, a `Calendar` control is embedded within an `UpdatePanel` control</span></span>
- <span data-ttu-id="9affd-119">Due `PopupControlExtender` controlli che assegnano il pannello per le caselle di testo</span><span class="sxs-lookup"><span data-stu-id="9affd-119">Two `PopupControlExtender` controls that assign the panel to the text boxes</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/samples/sample1.aspx)]

<span data-ttu-id="9affd-120">Si noti che il `OnSelectionChanged` attributo del `Calendar` NFS è impostata.</span><span class="sxs-lookup"><span data-stu-id="9affd-120">Note that the `OnSelectionChanged` attribute of the `Calendar` control is set.</span></span> <span data-ttu-id="9affd-121">In modo che quando l'utente seleziona una data all'interno del calendario, si verifica un postback e il metodo lato server `c1_SelectionChanged()` viene eseguita.</span><span class="sxs-lookup"><span data-stu-id="9affd-121">So when the user selects a date within the calendar, a postback occurs and the server-side method `c1_SelectionChanged()` is executed.</span></span> <span data-ttu-id="9affd-122">All'interno di tale metodo, la data corrente deve essere recuperata e il writeback per la casella di testo.</span><span class="sxs-lookup"><span data-stu-id="9affd-122">Within that method, the current date must be retrieved and written back to the textbox.</span></span>

<span data-ttu-id="9affd-123">La sintassi che è il seguente: prima di tutto, un proxy dell'oggetto per il `PopupControlExtender` nella pagina deve essere generato.</span><span class="sxs-lookup"><span data-stu-id="9affd-123">The syntax for that is as follows: First of all, a proxy object for the `PopupControlExtender` on the page must be generated.</span></span> <span data-ttu-id="9affd-124">ASP.NET AJAX Control Toolkit offre il `GetProxyForCurrentPopup()` (metodo).</span><span class="sxs-lookup"><span data-stu-id="9affd-124">The ASP.NET AJAX Control Toolkit offers the `GetProxyForCurrentPopup()` method.</span></span> <span data-ttu-id="9affd-125">L'oggetto restituisce questo metodo supporta il `Commit()` metodo che restituisce un valore al controllo che ha attivato il controllo popup (non il controllo che ha attivato la chiamata al metodo!).</span><span class="sxs-lookup"><span data-stu-id="9affd-125">The object this method returns supports the `Commit()` method which sends a value back to the control that triggered the popup (not the control that triggered the method call!).</span></span> <span data-ttu-id="9affd-126">Il codice seguente fornisce la data selezionata come argomento per il `Commit()` metodo causando il codice da scrivere nuovamente la data selezionata nella casella di testo:</span><span class="sxs-lookup"><span data-stu-id="9affd-126">The following code provides the selected date as the argument for the `Commit()` method, causing the code to write the selected date back to the text box:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/samples/sample2.aspx)]

<span data-ttu-id="9affd-127">Ora ogni volta che si fa clic su una data del calendario, la data selezionata viene visualizzato nella casella di testo associato, la creazione di un controllo selezione data che può attualmente disponibili in molti siti Web.</span><span class="sxs-lookup"><span data-stu-id="9affd-127">Now whenever you click on a calendar date, the selected date appears in the associated text box, creating a date picker control that can currently be found on many websites.</span></span>


<span data-ttu-id="9affd-128">[![Quando l'utente fa clic nella casella di testo viene visualizzato il calendario](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="9affd-128">[![The Calendar appears when the user clicks into the textbox](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image1.png)</span></span>

<span data-ttu-id="9affd-129">Il calendario viene visualizzato quando l'utente fa clic nella casella di testo ([fare clic per visualizzare l'immagine con dimensioni normali](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="9affd-129">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image3.png))</span></span>


<span data-ttu-id="9affd-130">[![Facendo clic su una data lo inserisce nella casella di testo](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="9affd-130">[![Clicking on a date puts it in the textbox](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image4.png)</span></span>

<span data-ttu-id="9affd-131">Facendo clic su una data lo inserisce nella casella di testo ([fare clic per visualizzare l'immagine con dimensioni normali](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="9affd-131">Clicking on a date puts it in the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="9affd-132">[Precedente](using-multiple-popup-controls-vb.md)
> [Successivo](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb.md)</span><span class="sxs-lookup"><span data-stu-id="9affd-132">[Previous](using-multiple-popup-controls-vb.md)
[Next](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb.md)</span></span>
