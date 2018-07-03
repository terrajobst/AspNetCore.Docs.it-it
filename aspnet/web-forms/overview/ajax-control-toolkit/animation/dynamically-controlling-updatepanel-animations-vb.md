---
uid: web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-vb
title: Controllo dinamico delle animazioni UpdatePanel (VB) | Microsoft Docs
author: wenz
description: Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo. Per il contenuto di un...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: bea66072-59b6-42b4-98fa-211812f5925f
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-vb
msc.type: authoredcontent
ms.openlocfilehash: 79a256f5d5c62123184beffce3365ade0ac13e46
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365941"
---
<a name="dynamically-controlling-updatepanel-animations-vb"></a><span data-ttu-id="8b2a4-104">Controllo dinamico delle animazioni UpdatePanel (VB)</span><span class="sxs-lookup"><span data-stu-id="8b2a4-104">Dynamically Controlling UpdatePanel Animations (VB)</span></span>
====================
<span data-ttu-id="8b2a4-105">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="8b2a4-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="8b2a4-106">[Scaricare il codice](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation2.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="8b2a4-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation2.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation2VB.pdf)</span></span>

> <span data-ttu-id="8b2a4-107">Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="8b2a4-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="8b2a4-108">Per il contenuto di un UpdatePanel, è presente una speciale estensione che si basa principalmente sul framework di animazione: UpdatePanelAnimation.</span><span class="sxs-lookup"><span data-stu-id="8b2a4-108">For the contents of an UpdatePanel, a special extender exists that relies heavily on the animation framework: UpdatePanelAnimation.</span></span> <span data-ttu-id="8b2a4-109">Può inoltre funzionare insieme ai trigger UpdatePanel.</span><span class="sxs-lookup"><span data-stu-id="8b2a4-109">It can also work together with UpdatePanel triggers.</span></span>


## <a name="overview"></a><span data-ttu-id="8b2a4-110">Panoramica</span><span class="sxs-lookup"><span data-stu-id="8b2a4-110">Overview</span></span>

<span data-ttu-id="8b2a4-111">Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="8b2a4-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="8b2a4-112">Per il contenuto di un `UpdatePanel`, è presente una speciale estensione che si basa principalmente sul framework di animazione: `UpdatePanelAnimation`.</span><span class="sxs-lookup"><span data-stu-id="8b2a4-112">For the contents of an `UpdatePanel`, a special extender exists that relies heavily on the animation framework: `UpdatePanelAnimation`.</span></span> <span data-ttu-id="8b2a4-113">Anche possibile operare insieme `UpdatePanel` trigger.</span><span class="sxs-lookup"><span data-stu-id="8b2a4-113">It can also work together with `UpdatePanel` triggers.</span></span>

## <a name="steps"></a><span data-ttu-id="8b2a4-114">Passaggi</span><span class="sxs-lookup"><span data-stu-id="8b2a4-114">Steps</span></span>

<span data-ttu-id="8b2a4-115">Il primo passaggio consiste nel modo consueto per includere il `ScriptManager` nella pagina in modo che la libreria ASP.NET AJAX viene caricata e il Toolkit di controllo possono essere usato:</span><span class="sxs-lookup"><span data-stu-id="8b2a4-115">The first step is as usual to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample1.aspx)]

<span data-ttu-id="8b2a4-116">L'animazione in questo scenario verrà applicato a una visualizzazione dell'ora corrente.</span><span class="sxs-lookup"><span data-stu-id="8b2a4-116">The animation in this scenario will be applied to a display of the current time.</span></span> <span data-ttu-id="8b2a4-117">Queste informazioni possono essere scritti in un'etichetta usando il `Page_Load()` metodo, o (per motivi di semplicità) viene usato il codice inline seguente:</span><span class="sxs-lookup"><span data-stu-id="8b2a4-117">This information can be written into a label using the `Page_Load()` method, or (for the sake of simplicity) the following inline code is used:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample2.aspx)]

<span data-ttu-id="8b2a4-118">Viene creato anche un pulsante per attivare aggiornando l'ora:</span><span class="sxs-lookup"><span data-stu-id="8b2a4-118">Also, a button to trigger updating the time is created:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample3.aspx)]

<span data-ttu-id="8b2a4-119">Questo codice viene inserito quindi nel `<ContentTemplate>` sezione di un `UpdatePanel` elemento.</span><span class="sxs-lookup"><span data-stu-id="8b2a4-119">This code is then put into the `<ContentTemplate>` section of an `UpdatePanel` element.</span></span> <span data-ttu-id="8b2a4-120">Il pannello `UpdateMode` attributo deve essere impostato su `"Conditional"`, dal momento che solo i trigger possono aggiornare il contenuto del pannello.</span><span class="sxs-lookup"><span data-stu-id="8b2a4-120">The panel's `UpdateMode` attribute must be set to `"Conditional"`, since only triggers may update the panel's contents.</span></span> <span data-ttu-id="8b2a4-121">Nel `<Triggers>` sezione del `UpdatePanel`, viene creato e associato a un trigger di postback asincrono di `Click` evento del pulsante.</span><span class="sxs-lookup"><span data-stu-id="8b2a4-121">In the `<Triggers>` section of the `UpdatePanel`, an asynchronous postback trigger is created and tied to the `Click` event of the button.</span></span> <span data-ttu-id="8b2a4-122">In questo modo, se l'utente fa clic sul pulsante, il `UpdatePanel` viene aggiornato.</span><span class="sxs-lookup"><span data-stu-id="8b2a4-122">Thus, if the user clicks on the button, the `UpdatePanel` is refreshed.</span></span> <span data-ttu-id="8b2a4-123">Ecco il markup per il `UpdatePanel` controllo:</span><span class="sxs-lookup"><span data-stu-id="8b2a4-123">Here is the markup for the `UpdatePanel` control:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample4.aspx)]

<span data-ttu-id="8b2a4-124">Infine, il `UpdatePanelAnimationExtender` deve essere configurato: impostare il `TargetControlID` attributo per l'ID del pannello e definire un'animazione entro il dispositivo extender.</span><span class="sxs-lookup"><span data-stu-id="8b2a4-124">Finally, the `UpdatePanelAnimationExtender` must be configured: Set the `TargetControlID` attribute to the ID of the panel, and define an animation within the extender.</span></span> <span data-ttu-id="8b2a4-125">La dissolvenza in rende senso, che crea un risalto visivo interessante sull'ora dell'aggiornamento.</span><span class="sxs-lookup"><span data-stu-id="8b2a4-125">Fading in makes sense, which creates a nice visual emphasis on the updated time.</span></span> <span data-ttu-id="8b2a4-126">Il markup di estensione potrebbero quindi simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="8b2a4-126">Your extender markup may then look like this:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample5.aspx)]

<span data-ttu-id="8b2a4-127">Eseguire il file nel browser.</span><span class="sxs-lookup"><span data-stu-id="8b2a4-127">Run the file in the browser.</span></span> <span data-ttu-id="8b2a4-128">Quando si fa clic sul pulsante, l'ora corrente viene visualizzata nel pannello dei sempre la dissolvenza in entrata per la durata di un secondo.</span><span class="sxs-lookup"><span data-stu-id="8b2a4-128">Whenever you click on the button, the current time is shown in the panel, always fading in for the duration of one second.</span></span>


<span data-ttu-id="8b2a4-129">[![L'ora corrente è la dissolvenza in entrata](dynamically-controlling-updatepanel-animations-vb/_static/image2.png)](dynamically-controlling-updatepanel-animations-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="8b2a4-129">[![The current time is fading in](dynamically-controlling-updatepanel-animations-vb/_static/image2.png)](dynamically-controlling-updatepanel-animations-vb/_static/image1.png)</span></span>

<span data-ttu-id="8b2a4-130">L'ora corrente è la dissolvenza in entrata ([fare clic per visualizzare l'immagine con dimensioni normali](dynamically-controlling-updatepanel-animations-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="8b2a4-130">The current time is fading in ([Click to view full-size image](dynamically-controlling-updatepanel-animations-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="8b2a4-131">Precedente</span><span class="sxs-lookup"><span data-stu-id="8b2a4-131">Previous</span></span>](animating-an-updatepanel-control-vb.md)
