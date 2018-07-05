---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-cs
title: Avvio di una finestra Popup modale dal codice Server (c#) | Microsoft Docs
author: wenz
description: Il controllo ModalPopup in AJAX Control Toolkit offre un modo semplice per creare un popup modale utilizzano i canali lato client. Tuttavia alcuni scenari richiedono che t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 2f67d8ef-73ca-447d-a0cc-6e3168431e6a
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-cs
msc.type: authoredcontent
ms.openlocfilehash: a13915f5748f8ad9b79fb787cc5e0682faa19388
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37374091"
---
<a name="launching-a-modal-popup-window-from-server-code-c"></a><span data-ttu-id="f3cff-104">Avvio di una finestra Popup modale dal codice Server (c#)</span><span class="sxs-lookup"><span data-stu-id="f3cff-104">Launching a Modal Popup Window from Server Code (C#)</span></span>
====================
<span data-ttu-id="f3cff-105">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="f3cff-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="f3cff-106">[Scaricare il codice](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup1.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="f3cff-106">[Download Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup1.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup1CS.pdf)</span></span>

> <span data-ttu-id="f3cff-107">Il controllo ModalPopup in AJAX Control Toolkit offre un modo semplice per creare un popup modale utilizzano i canali lato client.</span><span class="sxs-lookup"><span data-stu-id="f3cff-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="f3cff-108">Alcuni scenari richiedono tuttavia che l'apertura della finestra popup modale è attivato sul lato server.</span><span class="sxs-lookup"><span data-stu-id="f3cff-108">However some scenarios require that the opening of the modal popup is triggered on the server-side.</span></span>


## <a name="overview"></a><span data-ttu-id="f3cff-109">Panoramica</span><span class="sxs-lookup"><span data-stu-id="f3cff-109">Overview</span></span>

<span data-ttu-id="f3cff-110">Il controllo ModalPopup in AJAX Control Toolkit offre un modo semplice per creare un popup modale utilizzano i canali lato client.</span><span class="sxs-lookup"><span data-stu-id="f3cff-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="f3cff-111">Alcuni scenari richiedono tuttavia che l'apertura della finestra popup modale è attivato sul lato server.</span><span class="sxs-lookup"><span data-stu-id="f3cff-111">However some scenarios require that the opening of the modal popup is triggered on the server-side.</span></span>

## <a name="steps"></a><span data-ttu-id="f3cff-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="f3cff-112">Steps</span></span>

<span data-ttu-id="f3cff-113">Prima di tutto, per illustrare come funziona il controllo ModalPopup è necessario un controllo pulsante di ASP.NET di web.</span><span class="sxs-lookup"><span data-stu-id="f3cff-113">First of all, an ASP.NET Button web control is required to demonstrate how the ModalPopup control works.</span></span> <span data-ttu-id="f3cff-114">Aggiungere il pulsante di annullamento all'interno di &lt;form&gt; elemento in una nuova pagina:</span><span class="sxs-lookup"><span data-stu-id="f3cff-114">Add such a button within the &lt;form&gt; element on a new page:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample1.aspx)]

<span data-ttu-id="f3cff-115">Quindi, è necessario il markup per la finestra popup che si desidera creare.</span><span class="sxs-lookup"><span data-stu-id="f3cff-115">Then, you need the markup for the popup you want to create.</span></span> <span data-ttu-id="f3cff-116">Come definire un `<asp:Panel>` controllano e assicurarsi che include un controllo pulsante.</span><span class="sxs-lookup"><span data-stu-id="f3cff-116">Define it as an `<asp:Panel>` control and make sure that it includes a Button control.</span></span> <span data-ttu-id="f3cff-117">Il controllo ModalPopup offre le funzionalità per rendere tali un pulsante per chiudere la finestra popup; in caso contrario, non vi è alcun approccio facile per lasciare che scompaiono.</span><span class="sxs-lookup"><span data-stu-id="f3cff-117">The ModalPopup control offers the functionality to make such a button close the popup; otherwise there is no easy way to let it vanish.</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample2.aspx)]

<span data-ttu-id="f3cff-118">Successivamente, aggiungere il controllo ModalPopup di ASP.NET AJAX Toolkit alla pagina.</span><span class="sxs-lookup"><span data-stu-id="f3cff-118">Next add the ModalPopup control from the ASP.NET AJAX Toolkit to the page.</span></span> <span data-ttu-id="f3cff-119">Impostare le proprietà per il pulsante che consente di caricare il controllo pulsante che lo rende non vengono più visualizzati e l'ID della finestra popup effettivo.</span><span class="sxs-lookup"><span data-stu-id="f3cff-119">Set properties for the button which loads the control, the button which makes it disappear, and the ID of the actual popup.</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample3.aspx)]

<span data-ttu-id="f3cff-120">Come con tutte le pagine web basate su ASP.NET AJAX; la gestione di Script deve caricare le librerie JavaScript necessarie per i browser di destinazione diverso:</span><span class="sxs-lookup"><span data-stu-id="f3cff-120">As with all web pages based on ASP.NET AJAX; the Script Manager is required to load the necessary JavaScript libraries for the different target browsers:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample4.aspx)]

<span data-ttu-id="f3cff-121">Eseguire l'esempio nel browser.</span><span class="sxs-lookup"><span data-stu-id="f3cff-121">Run the example in the browser.</span></span> <span data-ttu-id="f3cff-122">Quando si fa clic sul pulsante, viene visualizzata la finestra popup modale.</span><span class="sxs-lookup"><span data-stu-id="f3cff-122">When you click on the button, the modal popup appears.</span></span> <span data-ttu-id="f3cff-123">Per ottenere lo stesso effetto tramite codice lato server, è necessario un nuovo pulsante:</span><span class="sxs-lookup"><span data-stu-id="f3cff-123">In order to achieve the same effect using server-side code, a new button is required:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample5.aspx)]

<span data-ttu-id="f3cff-124">Come può notare, un clic sul pulsante genera un postback ed esegue il `ServerButton_Click()` metodo nel server.</span><span class="sxs-lookup"><span data-stu-id="f3cff-124">As you can see, a click on the button generates a postback and executes the `ServerButton_Click()` method on the server.</span></span> <span data-ttu-id="f3cff-125">In questo metodo, una funzione JavaScript denominata `launchModal()` viene eseguita per essere precisi, la funzione JavaScript verrà eseguita dopo la pagina è stata caricata:</span><span class="sxs-lookup"><span data-stu-id="f3cff-125">In this method, a JavaScript function called `launchModal()` is executed to be exact, the JavaScript function will be executed once the page has been loaded:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample6.aspx)]

<span data-ttu-id="f3cff-126">Il compito di `launchModal()` consiste nel visualizzare il controllo ModalPopup.</span><span class="sxs-lookup"><span data-stu-id="f3cff-126">The job of `launchModal()` is to display the ModalPopup.</span></span> <span data-ttu-id="f3cff-127">Il `launchModal()` funzione viene eseguita una volta caricata la pagina HTML completata.</span><span class="sxs-lookup"><span data-stu-id="f3cff-127">The `launchModal()` function is executed once the complete HTML page has been loaded.</span></span> <span data-ttu-id="f3cff-128">In quel momento, tuttavia, il framework ASP.NET AJAX non è stato caricato completamente ancora.</span><span class="sxs-lookup"><span data-stu-id="f3cff-128">At that moment, however, the ASP.NET AJAX framework has not been fully loaded yet.</span></span> <span data-ttu-id="f3cff-129">Pertanto, il `launchModal()` funzione limita a impostare una variabile che il controllo ModalPopup deve essere visualizzato in un secondo momento:</span><span class="sxs-lookup"><span data-stu-id="f3cff-129">Therefore, the `launchModal()` function just sets a variable that the ModalPopup control must be shown later on:</span></span>

[!code-html[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample7.html)]

<span data-ttu-id="f3cff-130">Il `pageLoad()` funzione JavaScript è una funzione speciale che viene eseguita una volta che ASP.NET AJAX è stato caricato completamente.</span><span class="sxs-lookup"><span data-stu-id="f3cff-130">The `pageLoad()` JavaScript function is a special function that gets executed once ASP.NET AJAX has been fully loaded.</span></span> <span data-ttu-id="f3cff-131">Di conseguenza è aggiungere codice a questa funzione per visualizzare il controllo ModalPopup, ma solo se `launchModal()` è stato chiamato prima di:</span><span class="sxs-lookup"><span data-stu-id="f3cff-131">Therefore we add code to this function to show the ModalPopup control, but only if `launchModal()` has been called before:</span></span>

[!code-javascript[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample8.js)]

<span data-ttu-id="f3cff-132">Il `$find()` funzione esegue la ricerca di un elemento denominato nella pagina e previsto l'ID lato server come un parametro.</span><span class="sxs-lookup"><span data-stu-id="f3cff-132">The `$find()` function is looking for a named element on the page and expects the server-side ID as a parameter.</span></span> <span data-ttu-id="f3cff-133">Pertanto `$find("mpe")` restituisce la rappresentazione di client del controllo ModalPopup; relativo `show()` metodo consente la finestra popup visualizzata.</span><span class="sxs-lookup"><span data-stu-id="f3cff-133">Therefore, `$find("mpe")` returns the client representation of the ModalPopup control; its `show()` method lets the popup appear.</span></span>


<span data-ttu-id="f3cff-134">[![Viene visualizzata la finestra popup modale quando viene fatto clic su uno dei pulsanti](launching-a-modal-popup-window-from-server-code-cs/_static/image2.png)](launching-a-modal-popup-window-from-server-code-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="f3cff-134">[![The modal popup appears when either of the buttons is clicked](launching-a-modal-popup-window-from-server-code-cs/_static/image2.png)](launching-a-modal-popup-window-from-server-code-cs/_static/image1.png)</span></span>

<span data-ttu-id="f3cff-135">Viene visualizzata la finestra popup modale quando viene fatto clic su uno dei pulsanti ([fare clic per visualizzare l'immagine con dimensioni normali](launching-a-modal-popup-window-from-server-code-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="f3cff-135">The modal popup appears when either of the buttons is clicked ([Click to view full-size image](launching-a-modal-popup-window-from-server-code-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="f3cff-136">avanti</span><span class="sxs-lookup"><span data-stu-id="f3cff-136">Next</span></span>](using-modalpopup-with-a-repeater-control-cs.md)
