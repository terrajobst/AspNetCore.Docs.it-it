---
uid: web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-vb
title: Creazione di controllo su/giù numerico con un back-end del servizio Web (VB) | Microsoft Docs
author: wenz
description: Anziché consentire a un utente di digitare un valore in una casella di controllo, un controllo (che è presente in Windows e altri sistemi operativi) su/giù numerico potrebbe rivelarsi man mano c...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: afa59dfa-fef1-43d3-8fdd-aea3be36ed3c
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/numericupdown/creating-a-numeric-up-down-control-with-a-web-service-backend-vb
msc.type: authoredcontent
ms.openlocfilehash: bebf70093dacb81331c009c6642c2f2d649b8567
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37805373"
---
<a name="creating-a-numeric-updown-control-with-a-web-service-backend-vb"></a><span data-ttu-id="721bc-103">Creazione di un controllo su/giù numerico con un back-end del servizio Web (VB)</span><span class="sxs-lookup"><span data-stu-id="721bc-103">Creating a Numeric Up/Down Control with a Web Service Backend (VB)</span></span>
====================
<span data-ttu-id="721bc-104">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="721bc-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="721bc-105">[Scaricare il codice](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/numericupdown1.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/numericupdown1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="721bc-105">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/numericupdown1.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/numericupdown1VB.pdf)</span></span>

> <span data-ttu-id="721bc-106">Anziché consentire a un utente di digitare un valore in una casella di controllo, un valore numerico su/giù controllo (che è presente in Windows e altri sistemi operativi) potrebbe rivelarsi man mano familiarità.</span><span class="sxs-lookup"><span data-stu-id="721bc-106">Instead of letting a user type a value into a check box, a numeric up/down control (that exists on Windows and other operating systems) could prove as more comfortable.</span></span> <span data-ttu-id="721bc-107">Per impostazione predefinita, il controllo NumericUpDown sempre aumenta o diminuisce di un valore di 1, ma un servizio web dimostra una maggiore flessibilità.</span><span class="sxs-lookup"><span data-stu-id="721bc-107">By default, the NumericUpDown control always increases or decreases a value by 1, but a web service proves more flexibility.</span></span>


## <a name="overview"></a><span data-ttu-id="721bc-108">Panoramica</span><span class="sxs-lookup"><span data-stu-id="721bc-108">Overview</span></span>

<span data-ttu-id="721bc-109">Anziché consentire a un utente di digitare un valore in una casella di controllo, un valore numerico su/giù controllo (che è presente in Windows e altri sistemi operativi) potrebbe rivelarsi man mano familiarità.</span><span class="sxs-lookup"><span data-stu-id="721bc-109">Instead of letting a user type a value into a check box, a numeric up/down control (that exists on Windows and other operating systems) could prove as more comfortable.</span></span> <span data-ttu-id="721bc-110">Per impostazione predefinita, il `NumericUpDown` controllo sempre aumenta o diminuisce di un valore di 1, ma un servizio web dimostra una maggiore flessibilità.</span><span class="sxs-lookup"><span data-stu-id="721bc-110">By default, the `NumericUpDown` control always increases or decreases a value by 1, but a web service proves more flexibility.</span></span>

## <a name="steps"></a><span data-ttu-id="721bc-111">Passaggi</span><span class="sxs-lookup"><span data-stu-id="721bc-111">Steps</span></span>

<span data-ttu-id="721bc-112">ASP.NET AJAX Control Toolkit contiene il `NumericUpDown` extender che aggiunge automaticamente due pulsanti a una casella di testo: una per l'aumento relativo valore, uno per ridurlo.</span><span class="sxs-lookup"><span data-stu-id="721bc-112">The ASP.NET AJAX Control Toolkit contains the `NumericUpDown` extender which automatically adds two buttons to a text box: One for increasing its value, one for decreasing it.</span></span> <span data-ttu-id="721bc-113">Tuttavia il controllo supporta anche una chiamata al servizio web (o chiamata al metodo pagina).</span><span class="sxs-lookup"><span data-stu-id="721bc-113">However the control also supports a web service call (or page method call).</span></span> <span data-ttu-id="721bc-114">Ogni volta che il viene selezionato verso l'alto o verso il basso sul pulsante, il codice JavaScript codice si connette al server web ed esegue un metodo non esiste.</span><span class="sxs-lookup"><span data-stu-id="721bc-114">Whenever the up or down button is clicked, the JavaScript code connects to the web server and executes a method there.</span></span> <span data-ttu-id="721bc-115">La firma del metodo è quello riportato di seguito:</span><span class="sxs-lookup"><span data-stu-id="721bc-115">The method signature is the following one:</span></span>

[!code-vb[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample1.vb)]

<span data-ttu-id="721bc-116">Il `current` argomento è il valore corrente nella casella di testo; la `tag` attributo sia i dati di contesto aggiuntive che è possibile impostare come proprietà del `NumericUpDown` extender (ma non obbligatorio).</span><span class="sxs-lookup"><span data-stu-id="721bc-116">The `current` argument is the current value in the text box; the `tag` attribute is additional context data that can be set as a property of the `NumericUpDown` extender (but is not required).</span></span>

<span data-ttu-id="721bc-117">Per questo esempio, il valore numerico controllo su/giù deve consentire solo i valori che sono potenze di due: 1, 2, 4, 8, 16, 32, 64 e così via.</span><span class="sxs-lookup"><span data-stu-id="721bc-117">For this sample, the numeric up/down control shall only allow values that are powers of two: 1, 2, 4, 8, 16, 32, 64, and so on.</span></span> <span data-ttu-id="721bc-118">Di conseguenza, il metodo eseguito quando l'utente vuole aumentare il valore necessario raddoppiare il valore precedente. l'altro metodo deve dividere valore per due.</span><span class="sxs-lookup"><span data-stu-id="721bc-118">Therefore, the method executed when the user wants to increase the value must double the old value; the other method must divide value by two.</span></span> <span data-ttu-id="721bc-119">Quindi, ecco il servizio web completo:</span><span class="sxs-lookup"><span data-stu-id="721bc-119">So here is the complete web service:</span></span>

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample2.aspx)]

<span data-ttu-id="721bc-120">Infine, creare una nuova pagina ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="721bc-120">Finally, create a new ASP.NET page.</span></span> <span data-ttu-id="721bc-121">Come di consueto, è necessario un `ScriptManager` (controllo), una `TextBox` controllo e un `NumericUpDownExtender` controllo.</span><span class="sxs-lookup"><span data-stu-id="721bc-121">As usual, you need a `ScriptManager` control, a `TextBox` control and a `NumericUpDownExtender` control.</span></span> <span data-ttu-id="721bc-122">Nel secondo caso, è necessario fornire le informazioni sul servizio web:</span><span class="sxs-lookup"><span data-stu-id="721bc-122">For the latter, you have to provide the web service information:</span></span>

- <span data-ttu-id="721bc-123">`ServiceDownMethod` nome di scorrimento metodo web o pagina (metodo)</span><span class="sxs-lookup"><span data-stu-id="721bc-123">`ServiceDownMethod` name of the down web method or page method</span></span>
- <span data-ttu-id="721bc-124">`ServiceDownPath` percorso del servizio web con il metodo di servizio verso il basso. omettere se si usa un metodo di pagina</span><span class="sxs-lookup"><span data-stu-id="721bc-124">`ServiceDownPath` path to the web service with the down service method; omit if you are using a page method</span></span>
- <span data-ttu-id="721bc-125">`ServiceUpMethod` nome della freccia metodo web o metodo di pagina</span><span class="sxs-lookup"><span data-stu-id="721bc-125">`ServiceUpMethod` name of the up web method or page method</span></span>
- <span data-ttu-id="721bc-126">`ServiceUpPath` percorso del servizio web con il metodo del servizio backup; omettere se si usa un metodo di pagina</span><span class="sxs-lookup"><span data-stu-id="721bc-126">`ServiceUpPath` path to the web service with the up service method; omit if you are using a page method</span></span>

<span data-ttu-id="721bc-127">Ecco il markup completo per la pagina:</span><span class="sxs-lookup"><span data-stu-id="721bc-127">Here is the complete markup for the page:</span></span>

[!code-aspx[Main](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/samples/sample3.aspx)]

<span data-ttu-id="721bc-128">Se si esegue la pagina, si noti come il valore nella casella di testo raddoppia sempre quando si fa clic sul pulsante superiore e viene dimezzato quando si fa clic sul pulsante inferiore.</span><span class="sxs-lookup"><span data-stu-id="721bc-128">If you run the page, notice how the value in the text box always doubles when you click on the upper button, and is halved when you click on the lower button.</span></span>


<span data-ttu-id="721bc-129">[![Vengono visualizzati solo i numeri che sono una potenza di 2](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image2.png)](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="721bc-129">[![Only numbers that are a power of 2 appear](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image2.png)](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image1.png)</span></span>

<span data-ttu-id="721bc-130">Vengono visualizzati solo i numeri che sono una potenza di 2 ([fare clic per visualizzare l'immagine con dimensioni normali](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="721bc-130">Only numbers that are a power of 2 appear ([Click to view full-size image](creating-a-numeric-up-down-control-with-a-web-service-backend-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="721bc-131">Precedente</span><span class="sxs-lookup"><span data-stu-id="721bc-131">Previous</span></span>](creating-a-numeric-up-down-control-with-a-web-service-backend-cs.md)
