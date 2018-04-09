---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-vb
title: Avviando una finestra Popup modale dal codice Server (VB) | Documenti Microsoft
author: wenz
description: Il controllo ModalPopup AJAX Control Toolkit offre un modo semplice per creare un popup modale tramite mezzi sul lato client. Tuttavia, alcuni scenari richiedono che t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 36ca81d7-906d-4db2-952b-add18a4ff421
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 46554dae60ad9cd13e97e5755e95cb2125d1fed9
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="launching-a-modal-popup-window-from-server-code-vb"></a><span data-ttu-id="c9c8e-104">Avviando una finestra Popup modale dal codice Server (VB)</span><span class="sxs-lookup"><span data-stu-id="c9c8e-104">Launching a Modal Popup Window from Server Code (VB)</span></span>
====================
<span data-ttu-id="c9c8e-105">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="c9c8e-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="c9c8e-106">[Scaricare codice](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup1.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="c9c8e-106">[Download Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup1VB.pdf)</span></span>

> <span data-ttu-id="c9c8e-107">Il controllo ModalPopup AJAX Control Toolkit offre un modo semplice per creare un popup modale tramite mezzi sul lato client.</span><span class="sxs-lookup"><span data-stu-id="c9c8e-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="c9c8e-108">Tuttavia alcuni scenari richiedono che l'apertura della finestra popup modale viene attivata sul lato server.</span><span class="sxs-lookup"><span data-stu-id="c9c8e-108">However some scenarios require that the opening of the modal popup is triggered on the server-side.</span></span>


## <a name="overview"></a><span data-ttu-id="c9c8e-109">Panoramica</span><span class="sxs-lookup"><span data-stu-id="c9c8e-109">Overview</span></span>

<span data-ttu-id="c9c8e-110">Il controllo ModalPopup AJAX Control Toolkit offre un modo semplice per creare un popup modale tramite mezzi sul lato client.</span><span class="sxs-lookup"><span data-stu-id="c9c8e-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="c9c8e-111">Tuttavia alcuni scenari richiedono che l'apertura della finestra popup modale viene attivata sul lato server.</span><span class="sxs-lookup"><span data-stu-id="c9c8e-111">However some scenarios require that the opening of the modal popup is triggered on the server-side.</span></span>

## <a name="steps"></a><span data-ttu-id="c9c8e-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="c9c8e-112">Steps</span></span>

<span data-ttu-id="c9c8e-113">Controllo pulsante di ASP.NET web prima di tutto, è necessario per illustrare il funzionamento di controllo ModalPopup.</span><span class="sxs-lookup"><span data-stu-id="c9c8e-113">First of all, an ASP.NET Button web control is required to demonstrate how the ModalPopup control works.</span></span> <span data-ttu-id="c9c8e-114">Aggiungere questi pulsanti all'interno di &lt;modulo&gt; elemento in una nuova pagina:</span><span class="sxs-lookup"><span data-stu-id="c9c8e-114">Add such a button within the &lt;form&gt; element on a new page:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample1.aspx)]

<span data-ttu-id="c9c8e-115">Quindi, è necessario il markup per il popup in cui che si desidera creare.</span><span class="sxs-lookup"><span data-stu-id="c9c8e-115">Then, you need the markup for the popup you want to create.</span></span> <span data-ttu-id="c9c8e-116">Come definire un `<asp:Panel>` controllo e assicurarsi che include un controllo pulsante.</span><span class="sxs-lookup"><span data-stu-id="c9c8e-116">Define it as an `<asp:Panel>` control and make sure that it includes a Button control.</span></span> <span data-ttu-id="c9c8e-117">Il controllo ModalPopup offre le funzionalità per rendere tali un pulsante per chiudere la finestra popup; in caso contrario non è facile per lasciare scompaiono.</span><span class="sxs-lookup"><span data-stu-id="c9c8e-117">The ModalPopup control offers the functionality to make such a button close the popup; otherwise there is no easy way to let it vanish.</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample2.aspx)]

<span data-ttu-id="c9c8e-118">Aggiungere il controllo ModalPopup di ASP.NET AJAX Toolkit alla pagina.</span><span class="sxs-lookup"><span data-stu-id="c9c8e-118">Next add the ModalPopup control from the ASP.NET AJAX Toolkit to the page.</span></span> <span data-ttu-id="c9c8e-119">Impostare le proprietà per il pulsante che consente di caricare il controllo pulsante che rende scompaiono e l'ID del popup effettivo.</span><span class="sxs-lookup"><span data-stu-id="c9c8e-119">Set properties for the button which loads the control, the button which makes it disappear, and the ID of the actual popup.</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample3.aspx)]

<span data-ttu-id="c9c8e-120">Come con tutte le pagine web basate su ASP.NET AJAX; il gestore di Script è necessaria per caricare le librerie JavaScript necessari per i browser di destinazione diverso:</span><span class="sxs-lookup"><span data-stu-id="c9c8e-120">As with all web pages based on ASP.NET AJAX; the Script Manager is required to load the necessary JavaScript libraries for the different target browsers:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample4.aspx)]

<span data-ttu-id="c9c8e-121">Eseguire l'esempio nel browser.</span><span class="sxs-lookup"><span data-stu-id="c9c8e-121">Run the example in the browser.</span></span> <span data-ttu-id="c9c8e-122">Quando si fa clic sul pulsante, viene visualizzata il finestra popup modale.</span><span class="sxs-lookup"><span data-stu-id="c9c8e-122">When you click on the button, the modal popup appears.</span></span> <span data-ttu-id="c9c8e-123">Per ottenere lo stesso effetto utilizzando codice lato server, è necessario un nuovo pulsante:</span><span class="sxs-lookup"><span data-stu-id="c9c8e-123">In order to achieve the same effect using server-side code, a new button is required:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample5.aspx)]

<span data-ttu-id="c9c8e-124">Come si può notare, un clic sul pulsante genera un postback ed esegue il `ServerButton_Click()` metodo sul server.</span><span class="sxs-lookup"><span data-stu-id="c9c8e-124">As you can see, a click on the button generates a postback and executes the `ServerButton_Click()` method on the server.</span></span> <span data-ttu-id="c9c8e-125">In questo metodo, è stata chiamata una funzione JavaScript `launchModal()` viene eseguita per essere esatto, la funzione JavaScript verrà eseguita dopo che è stata caricata la pagina:</span><span class="sxs-lookup"><span data-stu-id="c9c8e-125">In this method, a JavaScript function called `launchModal()` is executed to be exact, the JavaScript function will be executed once the page has been loaded:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample6.aspx)]

<span data-ttu-id="c9c8e-126">Il processo di `launchModal()` viene visualizzato il ModalPopup.</span><span class="sxs-lookup"><span data-stu-id="c9c8e-126">The job of `launchModal()` is to display the ModalPopup.</span></span> <span data-ttu-id="c9c8e-127">Il `launchModal()` funzione viene eseguita una volta caricata nella pagina HTML.</span><span class="sxs-lookup"><span data-stu-id="c9c8e-127">The `launchModal()` function is executed once the complete HTML page has been loaded.</span></span> <span data-ttu-id="c9c8e-128">In quel momento, tuttavia, il framework ASP.NET AJAX non è stato caricato completamente ancora.</span><span class="sxs-lookup"><span data-stu-id="c9c8e-128">At that moment, however, the ASP.NET AJAX framework has not been fully loaded yet.</span></span> <span data-ttu-id="c9c8e-129">Pertanto, il `launchModal()` funzione si limita a impostare una variabile che il controllo ModalPopup deve essere visualizzato in un secondo momento:</span><span class="sxs-lookup"><span data-stu-id="c9c8e-129">Therefore, the `launchModal()` function just sets a variable that the ModalPopup control must be shown later on:</span></span>

[!code-html[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample7.html)]

<span data-ttu-id="c9c8e-130">Il `pageLoad()` JavaScript è una funzione speciale che viene eseguita una volta ASP.NET AJAX è stato caricato completamente.</span><span class="sxs-lookup"><span data-stu-id="c9c8e-130">The `pageLoad()` JavaScript function is a special function that gets executed once ASP.NET AJAX has been fully loaded.</span></span> <span data-ttu-id="c9c8e-131">Pertanto è aggiungere il codice a questa funzione per visualizzare il controllo ModalPopup, ma solo se `launchModal()` è stato chiamato prima di:</span><span class="sxs-lookup"><span data-stu-id="c9c8e-131">Therefore we add code to this function to show the ModalPopup control, but only if `launchModal()` has been called before:</span></span>

[!code-javascript[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample8.js)]

<span data-ttu-id="c9c8e-132">Il `$find()` funzione esegue la ricerca di un elemento denominato nella pagina e non prevede l'ID lato server come parametro.</span><span class="sxs-lookup"><span data-stu-id="c9c8e-132">The `$find()` function is looking for a named element on the page and expects the server-side ID as a parameter.</span></span> <span data-ttu-id="c9c8e-133">Pertanto, `$find("mpe")` restituisce la rappresentazione di client del controllo ModalPopup; relativo `show()` metodo consente la finestra popup visualizzata.</span><span class="sxs-lookup"><span data-stu-id="c9c8e-133">Therefore, `$find("mpe")` returns the client representation of the ModalPopup control; its `show()` method lets the popup appear.</span></span>


<span data-ttu-id="c9c8e-134">[![Viene visualizzata la finestra popup modale quando si fa clic su uno dei pulsanti](launching-a-modal-popup-window-from-server-code-vb/_static/image2.png)](launching-a-modal-popup-window-from-server-code-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="c9c8e-134">[![The modal popup appears when either of the buttons is clicked](launching-a-modal-popup-window-from-server-code-vb/_static/image2.png)](launching-a-modal-popup-window-from-server-code-vb/_static/image1.png)</span></span>

<span data-ttu-id="c9c8e-135">Il popup modale viene visualizzato quando si fa clic su uno dei pulsanti ([fare clic per visualizzare l'immagine ingrandita](launching-a-modal-popup-window-from-server-code-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="c9c8e-135">The modal popup appears when either of the buttons is clicked ([Click to view full-size image](launching-a-modal-popup-window-from-server-code-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="c9c8e-136">[Precedente](positioning-a-modalpopup-cs.md)
> [Successivo](using-modalpopup-with-a-repeater-control-vb.md)</span><span class="sxs-lookup"><span data-stu-id="c9c8e-136">[Previous](positioning-a-modalpopup-cs.md)
[Next](using-modalpopup-with-a-repeater-control-vb.md)</span></span>
