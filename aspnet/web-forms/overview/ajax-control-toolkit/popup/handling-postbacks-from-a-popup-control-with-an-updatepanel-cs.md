---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-cs
title: Gestione dei postback da un controllo Popup con un UpdatePanel (c#) | Documenti Microsoft
author: wenz
description: L'estensione PopupControl AJAX Control Toolkit offre un modo semplice per attivare una finestra popup quando viene attivato un altro controllo. Particolare attenzione deve essere eseguita...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 1f68f59d-9c1e-4cf3-b304-c13ae6b7203e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-cs
msc.type: authoredcontent
ms.openlocfilehash: abedb5247f710b02752651a7bfb011ab63d32844
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879634"
---
<a name="handling-postbacks-from-a-popup-control-with-an-updatepanel-c"></a><span data-ttu-id="b8181-104">Gestione dei postback da un controllo Popup con un UpdatePanel (c#)</span><span class="sxs-lookup"><span data-stu-id="b8181-104">Handling Postbacks from A Popup Control With an UpdatePanel (C#)</span></span>
====================
<span data-ttu-id="b8181-105">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="b8181-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="b8181-106">[Scaricare codice](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="b8181-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2CS.pdf)</span></span>

> <span data-ttu-id="b8181-107">L'estensione PopupControl AJAX Control Toolkit offre un modo semplice per attivare una finestra popup quando viene attivato un altro controllo.</span><span class="sxs-lookup"><span data-stu-id="b8181-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="b8181-108">Particolare attenzione è da eseguire quando si verifica un postback all'interno di questo tipo una finestra popup.</span><span class="sxs-lookup"><span data-stu-id="b8181-108">Special care has to be taken when a postback occurs within such a popup.</span></span>


## <a name="overview"></a><span data-ttu-id="b8181-109">Panoramica</span><span class="sxs-lookup"><span data-stu-id="b8181-109">Overview</span></span>

<span data-ttu-id="b8181-110">L'estensione PopupControl AJAX Control Toolkit offre un modo semplice per attivare una finestra popup quando viene attivato un altro controllo.</span><span class="sxs-lookup"><span data-stu-id="b8181-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="b8181-111">Particolare attenzione è da eseguire quando si verifica un postback all'interno di questo tipo una finestra popup.</span><span class="sxs-lookup"><span data-stu-id="b8181-111">Special care has to be taken when a postback occurs within such a popup.</span></span>

## <a name="steps"></a><span data-ttu-id="b8181-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="b8181-112">Steps</span></span>

<span data-ttu-id="b8181-113">Quando si utilizza un `PopupControl` con un postback, un `UpdatePanel` può impedire l'aggiornamento della pagina causato dal postback.</span><span class="sxs-lookup"><span data-stu-id="b8181-113">When using a `PopupControl` with a postback, an `UpdatePanel` can prevent the page refresh caused by the postback.</span></span> <span data-ttu-id="b8181-114">Il markup seguente definisce un paio di aspetti importanti:</span><span class="sxs-lookup"><span data-stu-id="b8181-114">The following markup defines a couple of important elements:</span></span>

- <span data-ttu-id="b8181-115">Oggetto `ScriptManager` controllare in modo che il funzionamento di ASP.NET AJAX Control Toolkit</span><span class="sxs-lookup"><span data-stu-id="b8181-115">A `ScriptManager` control so that the ASP.NET AJAX Control Toolkit works</span></span>
- <span data-ttu-id="b8181-116">Due `TextBox` controlli che verranno attivata una finestra popup</span><span class="sxs-lookup"><span data-stu-id="b8181-116">Two `TextBox` controls which will both trigger a popup</span></span>
- <span data-ttu-id="b8181-117">Oggetto `Panel` controllo che verrà utilizzato come finestra popup</span><span class="sxs-lookup"><span data-stu-id="b8181-117">A `Panel` control that will serve as the popup</span></span>
- <span data-ttu-id="b8181-118">All'interno del pannello, un `Calendar` controllo incorporato all'interno di un `UpdatePanel` controllo</span><span class="sxs-lookup"><span data-stu-id="b8181-118">Within the panel, a `Calendar` control is embedded within an `UpdatePanel` control</span></span>
- <span data-ttu-id="b8181-119">Due `PopupControlExtender` i controlli che assegna il pannello alle caselle di testo</span><span class="sxs-lookup"><span data-stu-id="b8181-119">Two `PopupControlExtender` controls that assign the panel to the text boxes</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/samples/sample1.aspx)]

<span data-ttu-id="b8181-120">Si noti che il `OnSelectionChanged` attributo del `Calendar` NFS è impostata.</span><span class="sxs-lookup"><span data-stu-id="b8181-120">Note that the `OnSelectionChanged` attribute of the `Calendar` control is set.</span></span> <span data-ttu-id="b8181-121">Pertanto quando l'utente seleziona una data nel calendario, si verifica un postback e il metodo lato server `c1_SelectionChanged()` viene eseguita.</span><span class="sxs-lookup"><span data-stu-id="b8181-121">So when the user selects a date within the calendar, a postback occurs and the server-side method `c1_SelectionChanged()` is executed.</span></span> <span data-ttu-id="b8181-122">All'interno di tale metodo, è necessario recuperare la data corrente e writeback per la casella di testo.</span><span class="sxs-lookup"><span data-stu-id="b8181-122">Within that method, the current date must be retrieved and written back to the textbox.</span></span>

<span data-ttu-id="b8181-123">La sintassi necessaria è la seguente: prima di tutto, un proxy dell'oggetto per il `PopupControlExtender` della pagina deve essere generato.</span><span class="sxs-lookup"><span data-stu-id="b8181-123">The syntax for that is as follows: First of all, a proxy object for the `PopupControlExtender` on the page must be generated.</span></span> <span data-ttu-id="b8181-124">ASP.NET AJAX Control Toolkit offre il `GetProxyForCurrentPopup()` metodo.</span><span class="sxs-lookup"><span data-stu-id="b8181-124">The ASP.NET AJAX Control Toolkit offers the `GetProxyForCurrentPopup()` method.</span></span> <span data-ttu-id="b8181-125">Supporta l'oggetto di questo metodo restituisce il `Commit()` metodo che invia un valore al controllo che ha attivato il popup (non il controllo che ha attivato la chiamata al metodo!).</span><span class="sxs-lookup"><span data-stu-id="b8181-125">The object this method returns supports the `Commit()` method which sends a value back to the control that triggered the popup (not the control that triggered the method call!).</span></span> <span data-ttu-id="b8181-126">Il codice seguente fornisce la data selezionata come argomento per il `Commit()` metodo causando il codice per scrivere la data selezionata la casella di testo:</span><span class="sxs-lookup"><span data-stu-id="b8181-126">The following code provides the selected date as the argument for the `Commit()` method, causing the code to write the selected date back to the text box:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/samples/sample2.aspx)]

<span data-ttu-id="b8181-127">Ora ogni volta che si fa clic su una data di calendario, viene visualizzata la data selezionata nella casella di testo associato, la creazione di un controllo selezione data che attualmente sono disponibili in molti siti Web.</span><span class="sxs-lookup"><span data-stu-id="b8181-127">Now whenever you click on a calendar date, the selected date appears in the associated text box, creating a date picker control that can currently be found on many websites.</span></span>


<span data-ttu-id="b8181-128">[![Il calendario viene visualizzato quando l'utente fa clic nella casella di testo](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="b8181-128">[![The Calendar appears when the user clicks into the textbox](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image1.png)</span></span>

<span data-ttu-id="b8181-129">Il calendario viene visualizzato quando l'utente fa clic nella casella di testo ([fare clic per visualizzare l'immagine ingrandita](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="b8181-129">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image3.png))</span></span>


<span data-ttu-id="b8181-130">[![Facendo clic su una data lo inserisce nella casella di testo](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="b8181-130">[![Clicking on a date puts it in the textbox](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image4.png)</span></span>

<span data-ttu-id="b8181-131">Facendo clic su una data lo inserisce nella casella di testo ([fare clic per visualizzare l'immagine ingrandita](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="b8181-131">Clicking on a date puts it in the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="b8181-132">[Precedente](using-multiple-popup-controls-cs.md)
> [Successivo](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)</span><span class="sxs-lookup"><span data-stu-id="b8181-132">[Previous](using-multiple-popup-controls-cs.md)
[Next](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)</span></span>
