---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-vb
title: Gestione dei postback da un ModalPopup (VB) | Documenti Microsoft
author: wenz
description: Il controllo ModalPopup AJAX Control Toolkit offre un modo semplice per creare un popup modale tramite mezzi sul lato client. È necessario prestare particolare attenzione quando un pos...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: f70ac2b3-900f-40fa-858f-ab057904506b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-vb
msc.type: authoredcontent
ms.openlocfilehash: 1aa2c42deb67015dd0b35edf4ba72d8d667ec88c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874210"
---
<a name="handling-postbacks-from-a-modalpopup-vb"></a><span data-ttu-id="e57d3-104">Gestione dei postback da un ModalPopup (VB)</span><span class="sxs-lookup"><span data-stu-id="e57d3-104">Handling Postbacks from a ModalPopup (VB)</span></span>
====================
<span data-ttu-id="e57d3-105">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="e57d3-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="e57d3-106">[Scaricare codice](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="e57d3-106">[Download Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3VB.pdf)</span></span>

> <span data-ttu-id="e57d3-107">Il controllo ModalPopup AJAX Control Toolkit offre un modo semplice per creare un popup modale tramite mezzi sul lato client.</span><span class="sxs-lookup"><span data-stu-id="e57d3-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="e57d3-108">È necessario prestare particolare attenzione quando viene creato un postback del popup.</span><span class="sxs-lookup"><span data-stu-id="e57d3-108">Special care must be taken when a postback is created from within the popup.</span></span>


## <a name="overview"></a><span data-ttu-id="e57d3-109">Panoramica</span><span class="sxs-lookup"><span data-stu-id="e57d3-109">Overview</span></span>

<span data-ttu-id="e57d3-110">Il controllo ModalPopup AJAX Control Toolkit offre un modo semplice per creare un popup modale tramite mezzi sul lato client.</span><span class="sxs-lookup"><span data-stu-id="e57d3-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="e57d3-111">È necessario prestare particolare attenzione quando viene creato un postback del popup.</span><span class="sxs-lookup"><span data-stu-id="e57d3-111">Special care must be taken when a postback is created from within the popup.</span></span>

## <a name="steps"></a><span data-ttu-id="e57d3-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="e57d3-112">Steps</span></span>

<span data-ttu-id="e57d3-113">Per attivare le funzionalità di ASP.NET AJAX e il Toolkit di controllo, il `ScriptManager` controllo deve essere inserito in un punto qualsiasi della pagina (ma entro il `<form>` elemento):</span><span class="sxs-lookup"><span data-stu-id="e57d3-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample1.aspx)]

<span data-ttu-id="e57d3-114">Successivamente, aggiungere un pannello che funge da finestra popup modale.</span><span class="sxs-lookup"><span data-stu-id="e57d3-114">Next, add a panel which serves as the modal popup.</span></span> <span data-ttu-id="e57d3-115">Non esiste, l'utente può immettere un nome e un indirizzo di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="e57d3-115">There, the user can enter a name and an email address.</span></span> <span data-ttu-id="e57d3-116">Un pulsante viene utilizzato per chiudere la finestra popup e salvare le informazioni.</span><span class="sxs-lookup"><span data-stu-id="e57d3-116">A button is used to close the popup and save the information.</span></span> <span data-ttu-id="e57d3-117">Si noti che il `OnClick` attributo è impostato in modo che si verifica un postback quando viene fatto clic su questo pulsante:</span><span class="sxs-lookup"><span data-stu-id="e57d3-117">Note that the `OnClick` attribute is set so that a postback occurs when this button is clicked:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample2.aspx)]

<span data-ttu-id="e57d3-118">La pagina stessa è costituito da due etichette esattamente le stesse informazioni: nome e l'indirizzo e-mail.</span><span class="sxs-lookup"><span data-stu-id="e57d3-118">The page itself consists of two labels for exactly the same information: name and email address.</span></span> <span data-ttu-id="e57d3-119">Un pulsante viene utilizzato per attivare il popup modale:</span><span class="sxs-lookup"><span data-stu-id="e57d3-119">A button is used to trigger the modal popup:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample3.aspx)]

<span data-ttu-id="e57d3-120">Per visualizzare la finestra popup, aggiungere il `ModalPopupExtender` controllo.</span><span class="sxs-lookup"><span data-stu-id="e57d3-120">In order to make the popup appear, add the `ModalPopupExtender` control.</span></span> <span data-ttu-id="e57d3-121">Impostare il `PopupControlID` attributo ID del pannello e `TargetControlID` all'ID del pulsante:</span><span class="sxs-lookup"><span data-stu-id="e57d3-121">Set the `PopupControlID` attribute to the panel's ID and `TargetControlID` to the button's ID:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample4.aspx)]

<span data-ttu-id="e57d3-122">Ora ogni volta che il `Save` del popup modale pulsante lato server `SaveData()` metodo viene eseguito.</span><span class="sxs-lookup"><span data-stu-id="e57d3-122">Now whenever the `Save` button within the modal popup is clicked, the server-side `SaveData()` method is executed.</span></span> <span data-ttu-id="e57d3-123">Non esiste, è stato possibile salvare i dati immessi in un archivio dati.</span><span class="sxs-lookup"><span data-stu-id="e57d3-123">There, you could save the entered data in a data store.</span></span> <span data-ttu-id="e57d3-124">Per ragioni di semplicità, i nuovi dati viene restituiti solo nell'etichetta:</span><span class="sxs-lookup"><span data-stu-id="e57d3-124">For the sake of simplicity, the new data is just output in the label:</span></span>

[!code-vb[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample5.vb)]

<span data-ttu-id="e57d3-125">Inoltre, i controlli casella di testo all'interno del popup modale devono essere riempiti con il nome corrente e un messaggio di posta elettronica.</span><span class="sxs-lookup"><span data-stu-id="e57d3-125">Also, the textbox controls within the modal popup should be filled with the current name and email.</span></span> <span data-ttu-id="e57d3-126">Tuttavia ciò è necessario solo quando si verifica alcun postback.</span><span class="sxs-lookup"><span data-stu-id="e57d3-126">However this is only necessary when no postback occurs.</span></span> <span data-ttu-id="e57d3-127">Se si verifica un postback, la funzionalità di viewstate ASP.NET verrà compilati automaticamente in caselle di testo con i valori appropriati.</span><span class="sxs-lookup"><span data-stu-id="e57d3-127">If there is a postback, the ASP.NET viewstate feature will automatically fill the textboxes with the appropriate values.</span></span>

[!code-vb[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample6.vb)]


<span data-ttu-id="e57d3-128">[![La finestra popup modale provoca un postback](handling-postbacks-from-a-modalpopup-vb/_static/image2.png)](handling-postbacks-from-a-modalpopup-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="e57d3-128">[![The modal popup causes a postback](handling-postbacks-from-a-modalpopup-vb/_static/image2.png)](handling-postbacks-from-a-modalpopup-vb/_static/image1.png)</span></span>

<span data-ttu-id="e57d3-129">Il popup modale provoca un postback ([fare clic per visualizzare l'immagine ingrandita](handling-postbacks-from-a-modalpopup-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="e57d3-129">The modal popup causes a postback ([Click to view full-size image](handling-postbacks-from-a-modalpopup-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e57d3-130">[Precedente](using-modalpopup-with-a-repeater-control-vb.md)
> [Successivo](positioning-a-modalpopup-vb.md)</span><span class="sxs-lookup"><span data-stu-id="e57d3-130">[Previous](using-modalpopup-with-a-repeater-control-vb.md)
[Next](positioning-a-modalpopup-vb.md)</span></span>
