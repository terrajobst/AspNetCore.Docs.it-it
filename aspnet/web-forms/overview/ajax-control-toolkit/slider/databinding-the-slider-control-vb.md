---
uid: web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-vb
title: Data Binding del controllo Slider (VB) | Microsoft Docs
author: wenz
description: Il controllo dispositivo di scorrimento in AJAX Control Toolkit fornisce un dispositivo di scorrimento con interfaccia grafica che può essere controllata usando il mouse. È possibile associare la posizione corrente...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 4f3ba53f-d166-422d-b29c-403348057836
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-vb
msc.type: authoredcontent
ms.openlocfilehash: aeaca2ebf61f49a5c081a3a1df188aa1541192d9
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37376307"
---
<a name="databinding-the-slider-control-vb"></a><span data-ttu-id="5cf92-104">Data Binding del controllo Slider (VB)</span><span class="sxs-lookup"><span data-stu-id="5cf92-104">Databinding the Slider Control (VB)</span></span>
====================
<span data-ttu-id="5cf92-105">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="5cf92-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="5cf92-106">[Scaricare il codice](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="5cf92-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0VB.pdf)</span></span>

> <span data-ttu-id="5cf92-107">Il controllo dispositivo di scorrimento in AJAX Control Toolkit fornisce un dispositivo di scorrimento con interfaccia grafica che può essere controllata usando il mouse.</span><span class="sxs-lookup"><span data-stu-id="5cf92-107">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="5cf92-108">È possibile associare la posizione corrente del dispositivo di scorrimento su un altro controllo ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="5cf92-108">It is possible to bind the current position of the slider to another ASP.NET control.</span></span>


## <a name="overview"></a><span data-ttu-id="5cf92-109">Panoramica</span><span class="sxs-lookup"><span data-stu-id="5cf92-109">Overview</span></span>

<span data-ttu-id="5cf92-110">Il controllo dispositivo di scorrimento in AJAX Control Toolkit fornisce un dispositivo di scorrimento con interfaccia grafica che può essere controllata usando il mouse.</span><span class="sxs-lookup"><span data-stu-id="5cf92-110">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="5cf92-111">È possibile associare la posizione corrente del dispositivo di scorrimento su un altro controllo ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="5cf92-111">It is possible to bind the current position of the slider to another ASP.NET control.</span></span>

## <a name="steps"></a><span data-ttu-id="5cf92-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="5cf92-112">Steps</span></span>

<span data-ttu-id="5cf92-113">Per attivare la funzionalità di ASP.NET AJAX e il Toolkit di controllo, il `ScriptManager` controllo deve essere inserito in un punto qualsiasi della pagina (ma entro la `<form>` elemento):</span><span class="sxs-lookup"><span data-stu-id="5cf92-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](databinding-the-slider-control-vb/samples/sample1.aspx)]

<span data-ttu-id="5cf92-114">Successivamente, aggiungere due `TextBox` controlli alla pagina.</span><span class="sxs-lookup"><span data-stu-id="5cf92-114">Next, add two `TextBox` controls to the page.</span></span> <span data-ttu-id="5cf92-115">Uno verrà trasformato in un dispositivo di scorrimento grafico e un altro manterrà la posizione del dispositivo di scorrimento.</span><span class="sxs-lookup"><span data-stu-id="5cf92-115">One will be transformed into a graphical slider, and the other one will hold the position of the slider.</span></span>

[!code-aspx[Main](databinding-the-slider-control-vb/samples/sample2.aspx)]

<span data-ttu-id="5cf92-116">Il passaggio successivo è già il passaggio finale.</span><span class="sxs-lookup"><span data-stu-id="5cf92-116">The next step is already the final step.</span></span> <span data-ttu-id="5cf92-117">Il `SliderExtender` controllo da ASP.NET AJAX Control Toolkit rende un dispositivo di scorrimento dalla casella di testo prima e seconda casella di testo vengono aggiornati automaticamente quando il dispositivo di scorrimento posizionare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="5cf92-117">The `SliderExtender` control from the ASP.NET AJAX Control Toolkit makes a slider out of the first text box and automatically updates the second text box when the slider position changes.</span></span> <span data-ttu-id="5cf92-118">In modo che funzioni correttamente, il `SliderExtender`del `TargetControlID` attributo deve essere impostato sull'ID della prima casella di testo; il `BoundControlID` attributo deve essere impostato sull'ID della seconda casella di testo.</span><span class="sxs-lookup"><span data-stu-id="5cf92-118">In order for that to work, The `SliderExtender`'s `TargetControlID` attribute must be set to the ID of the first text box; the `BoundControlID` attribute must be set to the ID of the second text box.</span></span>

[!code-aspx[Main](databinding-the-slider-control-vb/samples/sample3.aspx)]

<span data-ttu-id="5cf92-119">Come può notare nel browser, il data binding funziona in entrambe le direzioni: digitando un nuovo valore nella casella di testo Aggiorna posizione del dispositivo di scorrimento.</span><span class="sxs-lookup"><span data-stu-id="5cf92-119">As you can see in the browser, the data binding works in both directions: entering a new value in the text box updates the slider's position.</span></span> <span data-ttu-id="5cf92-120">Se si apporta la seconda casella di testo di sola lettura, è possibile aggiungere un livello di protezione basso per il campo di testo in modo che sia più difficile per all'utente di aggiornare manualmente il valore in tale posizione.</span><span class="sxs-lookup"><span data-stu-id="5cf92-120">If you make the second text box read only, you may add a weak protection to the text field so that it is harder for the user to manually update the value in there.</span></span>


<span data-ttu-id="5cf92-121">[![Dispositivo di scorrimento e casella di testo sono sincronizzate](databinding-the-slider-control-vb/_static/image2.png)](databinding-the-slider-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="5cf92-121">[![Slider and text box are in sync](databinding-the-slider-control-vb/_static/image2.png)](databinding-the-slider-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="5cf92-122">Dispositivo di scorrimento e casella di testo vengono sincronizzati ([fare clic per visualizzare l'immagine con dimensioni normali](databinding-the-slider-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="5cf92-122">Slider and text box are in sync ([Click to view full-size image](databinding-the-slider-control-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="5cf92-123">Precedente</span><span class="sxs-lookup"><span data-stu-id="5cf92-123">Previous</span></span>](using-the-slider-control-with-auto-postback-vb.md)
