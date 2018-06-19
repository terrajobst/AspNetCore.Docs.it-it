---
uid: web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-vb
title: Utilizzo di TextBoxWatermark con i controlli di convalida (VB) | Documenti Microsoft
author: wenz
description: Il controllo TextBoxWatermark AJAX Control Toolkit estende una casella di testo in modo che sia visualizzato all'interno della casella. Quando un utente fa clic nella casella, è possibile...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e6c2cb98-f745-4bc8-973a-813879c8a891
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: 0ca1a4af62af1d65525e59d0b7bc47245dd01476
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879231"
---
<a name="using-textboxwatermark-with-validation-controls-vb"></a><span data-ttu-id="49cfe-104">Utilizzo di TextBoxWatermark con i controlli di convalida (VB)</span><span class="sxs-lookup"><span data-stu-id="49cfe-104">Using TextBoxWatermark With Validation Controls (VB)</span></span>
====================
<span data-ttu-id="49cfe-105">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="49cfe-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="49cfe-106">[Scaricare codice](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark2.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="49cfe-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark2.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark2VB.pdf)</span></span>

> <span data-ttu-id="49cfe-107">Il controllo TextBoxWatermark AJAX Control Toolkit estende una casella di testo in modo che sia visualizzato all'interno della casella.</span><span class="sxs-lookup"><span data-stu-id="49cfe-107">The TextBoxWatermark control in the AJAX Control Toolkit extends a text box so that a text is displayed within the box.</span></span> <span data-ttu-id="49cfe-108">Quando un utente fa clic nella casella, viene svuotato.</span><span class="sxs-lookup"><span data-stu-id="49cfe-108">When a user clicks into the box, it is emptied.</span></span> <span data-ttu-id="49cfe-109">Se l'utente lascia la casella senza l'immissione di testo, viene nuovamente visualizzato il testo precompilato.</span><span class="sxs-lookup"><span data-stu-id="49cfe-109">If the user leaves the box without entering text, the prefilled text reappears.</span></span> <span data-ttu-id="49cfe-110">Ciò che siano in conflitto con i controlli di convalida ASP.NET nella stessa pagina, ma è possibile risolvere questi problemi.</span><span class="sxs-lookup"><span data-stu-id="49cfe-110">This may collide with ASP.NET Validation Controls on the same page, but these issues may be overcome.</span></span>


## <a name="overview"></a><span data-ttu-id="49cfe-111">Panoramica</span><span class="sxs-lookup"><span data-stu-id="49cfe-111">Overview</span></span>

<span data-ttu-id="49cfe-112">Il `TextBoxWatermark` controllo AJAX Control Toolkit estende una casella di testo in modo che sia visualizzato all'interno della casella.</span><span class="sxs-lookup"><span data-stu-id="49cfe-112">The `TextBoxWatermark` control in the AJAX Control Toolkit extends a text box so that a text is displayed within the box.</span></span> <span data-ttu-id="49cfe-113">Quando un utente fa clic nella casella, viene svuotato.</span><span class="sxs-lookup"><span data-stu-id="49cfe-113">When a user clicks into the box, it is emptied.</span></span> <span data-ttu-id="49cfe-114">Se l'utente lascia la casella senza l'immissione di testo, viene nuovamente visualizzato il testo precompilato.</span><span class="sxs-lookup"><span data-stu-id="49cfe-114">If the user leaves the box without entering text, the prefilled text reappears.</span></span> <span data-ttu-id="49cfe-115">Ciò che siano in conflitto con i controlli di convalida ASP.NET nella stessa pagina, ma è possibile risolvere questi problemi.</span><span class="sxs-lookup"><span data-stu-id="49cfe-115">This may collide with ASP.NET Validation Controls on the same page, but these issues may be overcome.</span></span>

## <a name="steps"></a><span data-ttu-id="49cfe-116">Passaggi</span><span class="sxs-lookup"><span data-stu-id="49cfe-116">Steps</span></span>

<span data-ttu-id="49cfe-117">Configurazione di base dell'esempio è il seguente: un `TextBox` controllo è filigrana utilizzando un `TextBoxWatermarkExtender` controllo.</span><span class="sxs-lookup"><span data-stu-id="49cfe-117">The basic setup of the sample is the following: a `TextBox` control is watermarked using a `TextBoxWatermarkExtender` control.</span></span> <span data-ttu-id="49cfe-118">Un pulsante Attiva un postback e verrà successivamente utilizzato per attivare i controlli di convalida della pagina.</span><span class="sxs-lookup"><span data-stu-id="49cfe-118">A button triggers a postback and will later be used to trigger the validation controls on the page.</span></span> <span data-ttu-id="49cfe-119">Inoltre, un `ScriptManager` controllo è necessario inizializzare ASP.NET AJAX:</span><span class="sxs-lookup"><span data-stu-id="49cfe-119">Also, a `ScriptManager` control is required to initialize ASP.NET AJAX:</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-vb/samples/sample1.aspx)]

<span data-ttu-id="49cfe-120">A questo punto aggiungere un `RequiredFieldValidator` controllo che controlla se è il testo nel campo quando il form viene inviato.</span><span class="sxs-lookup"><span data-stu-id="49cfe-120">Now add a `RequiredFieldValidator` control that checks whether there is text in the field when the form is submitted.</span></span> <span data-ttu-id="49cfe-121">Il `InitialValue` impostata la proprietà del validator sullo stesso valore che viene utilizzata per il `TextBoxWatermarkExtender` controllo: quando il form viene inviato, il valore di una casella di testo non modificato è il valore limite all'interno di essa:</span><span class="sxs-lookup"><span data-stu-id="49cfe-121">The `InitialValue` property of the validator must be set to the same value that is used in the `TextBoxWatermarkExtender` control: When the form is submitted, the value of an unchanged textbox is the watermark value within it:</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-vb/samples/sample2.aspx)]

<span data-ttu-id="49cfe-122">È tuttavia un problema con questo approccio: se il client JavaScript, il campo di testo non è precompilato con il testo della filigrana, pertanto il `RequiredFieldValidator` non genera un messaggio di errore.</span><span class="sxs-lookup"><span data-stu-id="49cfe-122">However there is one problem with this approach: If the client disables JavaScript, the text field is not prefilled with the watermark text, therefore the `RequiredFieldValidator` does not trigger an error message.</span></span> <span data-ttu-id="49cfe-123">Pertanto, un secondo `RequiredFieldValidator` è necessario il controllo che verifica la presenza di una casella di testo (omettendo il `InitialValue` attributo).</span><span class="sxs-lookup"><span data-stu-id="49cfe-123">Therefore, a second `RequiredFieldValidator` control is required which checks for an empty text box (omitting the `InitialValue` attribute).</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-vb/samples/sample3.aspx)]

<span data-ttu-id="49cfe-124">Poiché entrambi validator utilizzare `Display` = `"Dynamic"`, l'utente finale non distingue dall'aspetto visivo di quale dei due validator è stato generato; in alternativa, sembra che si è verificato solo uno di essi.</span><span class="sxs-lookup"><span data-stu-id="49cfe-124">Since both validators use `Display`=`"Dynamic"`, the end user cannot distinguish from the visual appearance which of the two validators was fired; instead, it looks like there was only one of them.</span></span>

<span data-ttu-id="49cfe-125">Infine, aggiungere codice lato server per il testo nel campo di output se nessun validator emesso un messaggio di errore:</span><span class="sxs-lookup"><span data-stu-id="49cfe-125">Finally, add some server-side code to output the text in the field if no validator issued an error message:</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-vb/samples/sample4.aspx)]


<span data-ttu-id="49cfe-126">[![Il validator rileva che è disponibile alcun testo nel campo](using-textboxwatermark-with-validation-controls-vb/_static/image2.png)](using-textboxwatermark-with-validation-controls-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="49cfe-126">[![The validator complains that there is no text in the field](using-textboxwatermark-with-validation-controls-vb/_static/image2.png)](using-textboxwatermark-with-validation-controls-vb/_static/image1.png)</span></span>

<span data-ttu-id="49cfe-127">Il validator rileva che è disponibile alcun testo nel campo ([fare clic per visualizzare l'immagine ingrandita](using-textboxwatermark-with-validation-controls-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="49cfe-127">The validator complains that there is no text in the field ([Click to view full-size image](using-textboxwatermark-with-validation-controls-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="49cfe-128">Precedente</span><span class="sxs-lookup"><span data-stu-id="49cfe-128">Previous</span></span>](using-textboxwatermark-in-a-formview-vb.md)
