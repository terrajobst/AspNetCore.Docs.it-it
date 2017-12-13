---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-cs
title: Posizionamento di un ModalPopup (c#) | Documenti Microsoft
author: wenz
description: Il controllo ModalPopup AJAX Control Toolkit offre un modo semplice per creare un popup modale tramite mezzi sul lato client. Tuttavia il controllo non offre un...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 1caac9d0-e21e-49d6-a8ff-e563a736d6ca
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-cs
msc.type: authoredcontent
ms.openlocfilehash: 8dcc4e20ac98cbbad1ea3e86b7f895d32c853d4a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="positioning-a-modalpopup-c"></a><span data-ttu-id="2e464-104">Posizionamento di un ModalPopup (c#)</span><span class="sxs-lookup"><span data-stu-id="2e464-104">Positioning a ModalPopup (C#)</span></span>
====================
<span data-ttu-id="2e464-105">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="2e464-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="2e464-106">[Scaricare codice](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="2e464-106">[Download Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4CS.pdf)</span></span>

> <span data-ttu-id="2e464-107">Il controllo ModalPopup AJAX Control Toolkit offre un modo semplice per creare un popup modale tramite mezzi sul lato client.</span><span class="sxs-lookup"><span data-stu-id="2e464-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="2e464-108">Tuttavia il controllo non offre una funzionalità incorporata per posizionare la finestra popup.</span><span class="sxs-lookup"><span data-stu-id="2e464-108">However the control does not offer a built-in functionality to position the popup.</span></span>


## <a name="overview"></a><span data-ttu-id="2e464-109">Panoramica</span><span class="sxs-lookup"><span data-stu-id="2e464-109">Overview</span></span>

<span data-ttu-id="2e464-110">Il controllo ModalPopup AJAX Control Toolkit offre un modo semplice per creare un popup modale tramite mezzi sul lato client.</span><span class="sxs-lookup"><span data-stu-id="2e464-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="2e464-111">Tuttavia il controllo non offre una funzionalità incorporata per posizionare la finestra popup.</span><span class="sxs-lookup"><span data-stu-id="2e464-111">However the control does not offer a built-in functionality to position the popup.</span></span>

## <a name="steps"></a><span data-ttu-id="2e464-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="2e464-112">Steps</span></span>

<span data-ttu-id="2e464-113">Per attivare le funzionalità di ASP.NET AJAX e il Toolkit di controllo, il `ScriptManager`.</span><span class="sxs-lookup"><span data-stu-id="2e464-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager`.</span></span> <span data-ttu-id="2e464-114">controllo deve essere inserito in un punto qualsiasi della pagina (ma entro il `<form>` elemento):</span><span class="sxs-lookup"><span data-stu-id="2e464-114">control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample1.aspx)]

<span data-ttu-id="2e464-115">Successivamente, aggiungere un pannello che funge da finestra popup modale.</span><span class="sxs-lookup"><span data-stu-id="2e464-115">Next, add a panel which serves as the modal popup.</span></span> <span data-ttu-id="2e464-116">Un pulsante viene utilizzato per chiudere la finestra popup:</span><span class="sxs-lookup"><span data-stu-id="2e464-116">A button is used to close the popup:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample2.aspx)]

<span data-ttu-id="2e464-117">Ogni volta che viene visualizzato la finestra popup, deve essere posizionato in un determinato punto della pagina.</span><span class="sxs-lookup"><span data-stu-id="2e464-117">Whenever the popup is shown, it shall be positioned at a certain place in the page.</span></span> <span data-ttu-id="2e464-118">Per questa attività viene creata una funzione JavaScript sul lato client.</span><span class="sxs-lookup"><span data-stu-id="2e464-118">For this task, a client-side JavaScript function is created.</span></span> <span data-ttu-id="2e464-119">Innanzitutto tenta di accedere al pannello.</span><span class="sxs-lookup"><span data-stu-id="2e464-119">It first tries to access the panel.</span></span> <span data-ttu-id="2e464-120">Se ha esito positivo, la posizione del pannello è impostata con CSS e JavaScript (modifica la posizione della finestra popup in verrà).</span><span class="sxs-lookup"><span data-stu-id="2e464-120">If it succeeds, the panel's position is set using CSS and JavaScript (change the position of the popup at will).</span></span> <span data-ttu-id="2e464-121">Tuttavia il `ModalPopupExtender` controllo tenta anche di posizionare la finestra popup.</span><span class="sxs-lookup"><span data-stu-id="2e464-121">However the `ModalPopupExtender` control also tries to position the popup.</span></span> <span data-ttu-id="2e464-122">Pertanto, il codice JavaScript ripetutamente il popup viene posizionato, ogni decimo di secondo.</span><span class="sxs-lookup"><span data-stu-id="2e464-122">Therefore, the JavaScript code repeatedly positions the popup, every tenth of a second.</span></span>

[!code-html[Main](positioning-a-modalpopup-cs/samples/sample3.html)]

<span data-ttu-id="2e464-123">Come si può notare, il valore restituito di `setTimeout()` metodo JavaScript viene salvato in una variabile globale.</span><span class="sxs-lookup"><span data-stu-id="2e464-123">As you can see, the return value of the `setTimeout()` JavaScript method is saved in a global variable.</span></span> <span data-ttu-id="2e464-124">Questo consente di arrestare il posizionamento ripetute del popup su richiesta, tramite il `clearTimeout()` metodo:</span><span class="sxs-lookup"><span data-stu-id="2e464-124">This allows to stop the repeated positioning of the popup on demand, using the `clearTimeout()` method:</span></span>

[!code-javascript[Main](positioning-a-modalpopup-cs/samples/sample4.js)]

<span data-ttu-id="2e464-125">Ora tutto ciò che resta da fare è per rendere il browser chiamare queste funzioni ogni volta che è appropriato.</span><span class="sxs-lookup"><span data-stu-id="2e464-125">Now all that is left to do is to make the browser call these functions whenever appropriate.</span></span> <span data-ttu-id="2e464-126">Il `movePanel()` funzione JavaScript deve essere chiamato quando viene scelto il pulsante che attiva il pannello:</span><span class="sxs-lookup"><span data-stu-id="2e464-126">The `movePanel()` JavaScript function must be called when the button is clicked that triggers the panel:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample5.aspx)]

<span data-ttu-id="2e464-127">E `stopMoving()` funzione si verifica quando la finestra popup viene chiusa può essere attivata nel `ModalPopupExtender` controllo:</span><span class="sxs-lookup"><span data-stu-id="2e464-127">And the `stopMoving()` function comes into play when the popup is closed this can be triggered in the `ModalPopupExtender` control:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample6.aspx)]


<span data-ttu-id="2e464-128">[![Il popup modale viene visualizzato in corrispondenza della posizione designata](positioning-a-modalpopup-cs/_static/image2.png)](positioning-a-modalpopup-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="2e464-128">[![The modal popup appears at the designated position](positioning-a-modalpopup-cs/_static/image2.png)](positioning-a-modalpopup-cs/_static/image1.png)</span></span>

<span data-ttu-id="2e464-129">Il popup modale viene visualizzato in corrispondenza della posizione designata ([fare clic per visualizzare l'immagine ingrandita](positioning-a-modalpopup-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="2e464-129">The modal popup appears at the designated position ([Click to view full-size image](positioning-a-modalpopup-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="2e464-130">[Precedente](handling-postbacks-from-a-modalpopup-cs.md)
[Successivo](launching-a-modal-popup-window-from-server-code-vb.md)</span><span class="sxs-lookup"><span data-stu-id="2e464-130">[Previous](handling-postbacks-from-a-modalpopup-cs.md)
[Next](launching-a-modal-popup-window-from-server-code-vb.md)</span></span>
