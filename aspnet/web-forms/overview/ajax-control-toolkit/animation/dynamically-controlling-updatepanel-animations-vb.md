---
uid: web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-vb
title: Controllare in modo dinamico le animazioni UpdatePanel (VB) | Documenti Microsoft
author: wenz
description: "Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework per aggiungere le animazioni a un controllo. Per il contenuto di un..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: bea66072-59b6-42b4-98fa-211812f5925f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-vb
msc.type: authoredcontent
ms.openlocfilehash: 75b1f169724d8d3f8bcbc46198d320eeb8e7260c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="dynamically-controlling-updatepanel-animations-vb"></a><span data-ttu-id="ea81f-104">Controllare in modo dinamico le animazioni UpdatePanel (VB)</span><span class="sxs-lookup"><span data-stu-id="ea81f-104">Dynamically Controlling UpdatePanel Animations (VB)</span></span>
====================
<span data-ttu-id="ea81f-105">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="ea81f-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="ea81f-106">[Scaricare codice](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation2.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="ea81f-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation2.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation2VB.pdf)</span></span>

> <span data-ttu-id="ea81f-107">Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework per aggiungere le animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="ea81f-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="ea81f-108">Per il contenuto dell'UpdatePanel, è presente una speciale estensione che si basa su framework animazione: UpdatePanelAnimation.</span><span class="sxs-lookup"><span data-stu-id="ea81f-108">For the contents of an UpdatePanel, a special extender exists that relies heavily on the animation framework: UpdatePanelAnimation.</span></span> <span data-ttu-id="ea81f-109">Può inoltre interagire con i trigger di UpdatePanel.</span><span class="sxs-lookup"><span data-stu-id="ea81f-109">It can also work together with UpdatePanel triggers.</span></span>


## <a name="overview"></a><span data-ttu-id="ea81f-110">Panoramica</span><span class="sxs-lookup"><span data-stu-id="ea81f-110">Overview</span></span>

<span data-ttu-id="ea81f-111">Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework per aggiungere le animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="ea81f-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="ea81f-112">Per il contenuto di un `UpdatePanel`, una speciale estensione esistente che si basa su framework animazione: `UpdatePanelAnimation`.</span><span class="sxs-lookup"><span data-stu-id="ea81f-112">For the contents of an `UpdatePanel`, a special extender exists that relies heavily on the animation framework: `UpdatePanelAnimation`.</span></span> <span data-ttu-id="ea81f-113">Anche possibile operare insieme `UpdatePanel` trigger.</span><span class="sxs-lookup"><span data-stu-id="ea81f-113">It can also work together with `UpdatePanel` triggers.</span></span>

## <a name="steps"></a><span data-ttu-id="ea81f-114">Passaggi</span><span class="sxs-lookup"><span data-stu-id="ea81f-114">Steps</span></span>

<span data-ttu-id="ea81f-115">Il primo passaggio consiste nel modo consueto per includere il `ScriptManager` nella pagina in modo che la libreria ASP.NET AJAX è caricata e può essere utilizzato il Toolkit di controllo:</span><span class="sxs-lookup"><span data-stu-id="ea81f-115">The first step is as usual to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample1.aspx)]

<span data-ttu-id="ea81f-116">In questo scenario l'animazione verrà applicato a una visualizzazione dell'ora corrente.</span><span class="sxs-lookup"><span data-stu-id="ea81f-116">The animation in this scenario will be applied to a display of the current time.</span></span> <span data-ttu-id="ea81f-117">Queste informazioni possono essere scritti in un'etichetta usando il `Page_Load()` metodo, o (per ragioni di semplicità) viene utilizzato il seguente codice inline:</span><span class="sxs-lookup"><span data-stu-id="ea81f-117">This information can be written into a label using the `Page_Load()` method, or (for the sake of simplicity) the following inline code is used:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample2.aspx)]

<span data-ttu-id="ea81f-118">Inoltre, viene creato un pulsante per attivare il tempo di aggiornamento:</span><span class="sxs-lookup"><span data-stu-id="ea81f-118">Also, a button to trigger updating the time is created:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample3.aspx)]

<span data-ttu-id="ea81f-119">Questo codice viene quindi inserito il `<ContentTemplate>` sezione di un `UpdatePanel` elemento.</span><span class="sxs-lookup"><span data-stu-id="ea81f-119">This code is then put into the `<ContentTemplate>` section of an `UpdatePanel` element.</span></span> <span data-ttu-id="ea81f-120">Il pannello `UpdateMode` attributo deve essere impostato su `"Conditional"`, poiché solo i trigger possono aggiornare il contenuto del pannello.</span><span class="sxs-lookup"><span data-stu-id="ea81f-120">The panel's `UpdateMode` attribute must be set to `"Conditional"`, since only triggers may update the panel's contents.</span></span> <span data-ttu-id="ea81f-121">Nel `<Triggers>` sezione del `UpdatePanel`, un trigger di postback asincrono viene creato e associato al `Click` evento del pulsante.</span><span class="sxs-lookup"><span data-stu-id="ea81f-121">In the `<Triggers>` section of the `UpdatePanel`, an asynchronous postback trigger is created and tied to the `Click` event of the button.</span></span> <span data-ttu-id="ea81f-122">Pertanto, se l'utente fa clic sul pulsante, il `UpdatePanel` viene aggiornato.</span><span class="sxs-lookup"><span data-stu-id="ea81f-122">Thus, if the user clicks on the button, the `UpdatePanel` is refreshed.</span></span> <span data-ttu-id="ea81f-123">Ecco il markup per il `UpdatePanel` controllo:</span><span class="sxs-lookup"><span data-stu-id="ea81f-123">Here is the markup for the `UpdatePanel` control:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample4.aspx)]

<span data-ttu-id="ea81f-124">Infine, il `UpdatePanelAnimationExtender` deve essere configurato: impostare il `TargetControlID` attributo per l'ID del pannello e definire un'animazione all'interno del programma di estensione.</span><span class="sxs-lookup"><span data-stu-id="ea81f-124">Finally, the `UpdatePanelAnimationExtender` must be configured: Set the `TargetControlID` attribute to the ID of the panel, and define an animation within the extender.</span></span> <span data-ttu-id="ea81f-125">Dissolvenza in permette senso, che consente di creare un particolare visual nice l'ora dell'aggiornamento.</span><span class="sxs-lookup"><span data-stu-id="ea81f-125">Fading in makes sense, which creates a nice visual emphasis on the updated time.</span></span> <span data-ttu-id="ea81f-126">Il markup di estensione può quindi simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="ea81f-126">Your extender markup may then look like this:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample5.aspx)]

<span data-ttu-id="ea81f-127">Eseguire il file nel browser.</span><span class="sxs-lookup"><span data-stu-id="ea81f-127">Run the file in the browser.</span></span> <span data-ttu-id="ea81f-128">Quando si fa clic sul pulsante, l'ora corrente viene visualizzata nel pannello, sempre la dissolvenza in entrata per la durata di un secondo.</span><span class="sxs-lookup"><span data-stu-id="ea81f-128">Whenever you click on the button, the current time is shown in the panel, always fading in for the duration of one second.</span></span>


<span data-ttu-id="ea81f-129">[![L'ora corrente è la dissolvenza in entrata](dynamically-controlling-updatepanel-animations-vb/_static/image2.png)](dynamically-controlling-updatepanel-animations-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="ea81f-129">[![The current time is fading in](dynamically-controlling-updatepanel-animations-vb/_static/image2.png)](dynamically-controlling-updatepanel-animations-vb/_static/image1.png)</span></span>

<span data-ttu-id="ea81f-130">L'ora corrente è la dissolvenza in entrata ([fare clic per visualizzare l'immagine ingrandita](dynamically-controlling-updatepanel-animations-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="ea81f-130">The current time is fading in ([Click to view full-size image](dynamically-controlling-updatepanel-animations-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="ea81f-131">Precedente</span><span class="sxs-lookup"><span data-stu-id="ea81f-131">Previous</span></span>](animating-an-updatepanel-control-vb.md)
