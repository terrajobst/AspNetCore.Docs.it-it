---
uid: web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-vb
title: Associazione dati controllo dispositivo di scorrimento (VB) | Documenti Microsoft
author: wenz
description: Il controllo dispositivo di scorrimento in AJAX Control Toolkit fornisce un dispositivo di scorrimento con interfaccia grafica che può essere controllata utilizzando il mouse. È possibile associare la posizione corrente...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 4f3ba53f-d166-422d-b29c-403348057836
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 3ecd8598cd7fdcbbb4812e501bb30fa1f563a054
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879140"
---
<a name="databinding-the-slider-control-vb"></a><span data-ttu-id="7cc37-104">Associazione dati controllo dispositivo di scorrimento (VB)</span><span class="sxs-lookup"><span data-stu-id="7cc37-104">Databinding the Slider Control (VB)</span></span>
====================
<span data-ttu-id="7cc37-105">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="7cc37-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="7cc37-106">[Scaricare codice](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="7cc37-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0VB.pdf)</span></span>

> <span data-ttu-id="7cc37-107">Il controllo dispositivo di scorrimento in AJAX Control Toolkit fornisce un dispositivo di scorrimento con interfaccia grafica che può essere controllata utilizzando il mouse.</span><span class="sxs-lookup"><span data-stu-id="7cc37-107">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="7cc37-108">È possibile associare la posizione corrente del dispositivo di scorrimento a un altro controllo ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="7cc37-108">It is possible to bind the current position of the slider to another ASP.NET control.</span></span>


## <a name="overview"></a><span data-ttu-id="7cc37-109">Panoramica</span><span class="sxs-lookup"><span data-stu-id="7cc37-109">Overview</span></span>

<span data-ttu-id="7cc37-110">Il controllo dispositivo di scorrimento in AJAX Control Toolkit fornisce un dispositivo di scorrimento con interfaccia grafica che può essere controllata utilizzando il mouse.</span><span class="sxs-lookup"><span data-stu-id="7cc37-110">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="7cc37-111">È possibile associare la posizione corrente del dispositivo di scorrimento a un altro controllo ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="7cc37-111">It is possible to bind the current position of the slider to another ASP.NET control.</span></span>

## <a name="steps"></a><span data-ttu-id="7cc37-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="7cc37-112">Steps</span></span>

<span data-ttu-id="7cc37-113">Per attivare le funzionalità di ASP.NET AJAX e il Toolkit di controllo, il `ScriptManager` controllo deve essere inserito in un punto qualsiasi della pagina (ma entro il `<form>` elemento):</span><span class="sxs-lookup"><span data-stu-id="7cc37-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](databinding-the-slider-control-vb/samples/sample1.aspx)]

<span data-ttu-id="7cc37-114">Successivamente, aggiungere due `TextBox` controlli alla pagina.</span><span class="sxs-lookup"><span data-stu-id="7cc37-114">Next, add two `TextBox` controls to the page.</span></span> <span data-ttu-id="7cc37-115">Uno verrà trasformato in un dispositivo di scorrimento con interfaccia grafica e l'altro conterrà la posizione del dispositivo di scorrimento.</span><span class="sxs-lookup"><span data-stu-id="7cc37-115">One will be transformed into a graphical slider, and the other one will hold the position of the slider.</span></span>

[!code-aspx[Main](databinding-the-slider-control-vb/samples/sample2.aspx)]

<span data-ttu-id="7cc37-116">Il passaggio successivo è già il passaggio finale.</span><span class="sxs-lookup"><span data-stu-id="7cc37-116">The next step is already the final step.</span></span> <span data-ttu-id="7cc37-117">Il `SliderExtender` controllo di ASP.NET AJAX Control Toolkit rende un dispositivo di scorrimento dalla casella di testo prima e seconda casella di testo vengono aggiornati automaticamente quando il dispositivo di scorrimento posizionare le modifiche.</span><span class="sxs-lookup"><span data-stu-id="7cc37-117">The `SliderExtender` control from the ASP.NET AJAX Control Toolkit makes a slider out of the first text box and automatically updates the second text box when the slider position changes.</span></span> <span data-ttu-id="7cc37-118">In modo che funzioni correttamente, il `SliderExtender`del `TargetControlID` attributo deve essere impostato per l'ID della prima casella di testo; il `BoundControlID` attributo deve essere impostato per l'ID della seconda casella di testo.</span><span class="sxs-lookup"><span data-stu-id="7cc37-118">In order for that to work, The `SliderExtender`'s `TargetControlID` attribute must be set to the ID of the first text box; the `BoundControlID` attribute must be set to the ID of the second text box.</span></span>

[!code-aspx[Main](databinding-the-slider-control-vb/samples/sample3.aspx)]

<span data-ttu-id="7cc37-119">Come è possibile visualizzare nel browser, l'associazione dati funziona in entrambe le direzioni: immettendo un nuovo valore nella casella di testo Aggiorna la posizione del dispositivo di scorrimento.</span><span class="sxs-lookup"><span data-stu-id="7cc37-119">As you can see in the browser, the data binding works in both directions: entering a new value in the text box updates the slider's position.</span></span> <span data-ttu-id="7cc37-120">Se si apportano seconda casella di testo di sola lettura, è possibile aggiungere un livello di protezione basso per il campo di testo in modo che sia più difficile per l'utente aggiornare manualmente il valore in questa posizione.</span><span class="sxs-lookup"><span data-stu-id="7cc37-120">If you make the second text box read only, you may add a weak protection to the text field so that it is harder for the user to manually update the value in there.</span></span>


<span data-ttu-id="7cc37-121">[![Dispositivo di scorrimento e casella di testo sono sincronizzati](databinding-the-slider-control-vb/_static/image2.png)](databinding-the-slider-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="7cc37-121">[![Slider and text box are in sync](databinding-the-slider-control-vb/_static/image2.png)](databinding-the-slider-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="7cc37-122">Dispositivo di scorrimento e casella di testo sono sincronizzati ([fare clic per visualizzare l'immagine ingrandita](databinding-the-slider-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="7cc37-122">Slider and text box are in sync ([Click to view full-size image](databinding-the-slider-control-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="7cc37-123">Precedente</span><span class="sxs-lookup"><span data-stu-id="7cc37-123">Previous</span></span>](using-the-slider-control-with-auto-postback-vb.md)
