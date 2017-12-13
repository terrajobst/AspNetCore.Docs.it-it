---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-vb
title: L'esecuzione di animazioni utilizzando codice lato Client (VB) | Documenti Microsoft
author: wenz
description: "Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework per aggiungere le animazioni a un controllo. L'esecuzione di animazione..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: f7073f50-d765-456d-9957-926ce60f35f6
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-vb
msc.type: authoredcontent
ms.openlocfilehash: c97ce87addd566ed1ba63ccdb81206c6449f49a2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="executing-animations-using-client-side-code-vb"></a><span data-ttu-id="70eeb-104">L'esecuzione di animazioni utilizzando codice lato Client (VB)</span><span class="sxs-lookup"><span data-stu-id="70eeb-104">Executing Animations Using Client-Side Code (VB)</span></span>
====================
<span data-ttu-id="70eeb-105">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="70eeb-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="70eeb-106">[Scaricare codice](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation10.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation10VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="70eeb-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation10.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation10VB.pdf)</span></span>

> <span data-ttu-id="70eeb-107">Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework per aggiungere le animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="70eeb-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="70eeb-108">L'esecuzione di animazione può essere attivato utilizzando codice JavaScript sul lato client personalizzato.</span><span class="sxs-lookup"><span data-stu-id="70eeb-108">The animation execution may also be triggered using custom client-side JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="70eeb-109">Panoramica</span><span class="sxs-lookup"><span data-stu-id="70eeb-109">Overview</span></span>

<span data-ttu-id="70eeb-110">Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework per aggiungere le animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="70eeb-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="70eeb-111">L'esecuzione di animazione può essere attivato utilizzando codice JavaScript sul lato client personalizzato.</span><span class="sxs-lookup"><span data-stu-id="70eeb-111">The animation execution may also be triggered using custom client-side JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="70eeb-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="70eeb-112">Steps</span></span>

<span data-ttu-id="70eeb-113">Prima di tutto, includere il `ScriptManager` nella pagina; quindi, la libreria AJAX di ASP.NET viene caricata, che consente di usare il Toolkit di controllo:</span><span class="sxs-lookup"><span data-stu-id="70eeb-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample1.aspx)]

<span data-ttu-id="70eeb-114">Verrà applicata l'animazione a un riquadro del testo che presenta un aspetto simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="70eeb-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample2.aspx)]

<span data-ttu-id="70eeb-115">Nella classe CSS associata per il pannello, definire un colore di sfondo nice e inoltre impostare una larghezza fissa per il pannello:</span><span class="sxs-lookup"><span data-stu-id="70eeb-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](executing-animations-using-client-side-code-vb/samples/sample3.css)]

<span data-ttu-id="70eeb-116">Quindi, aggiungere il `AnimationExtender` alla pagina, fornendo un `ID`, `TargetControlID` attributo e l'obbligatoria `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="70eeb-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample4.aspx)]

<span data-ttu-id="70eeb-117">All'interno di `<Animations>` nodo, utilizzare `<OnClick>` per eseguire le animazioni una sola volta, l'utente fa clic sul riquadro.</span><span class="sxs-lookup"><span data-stu-id="70eeb-117">Within the `<Animations>` node, use `<OnClick>` to run the animations once the user clicks on the panel.</span></span> <span data-ttu-id="70eeb-118">Aggiungere due animazioni deve essere eseguito parallelly:</span><span class="sxs-lookup"><span data-stu-id="70eeb-118">Add two animations to be executed parallelly:</span></span>

[!code-xml[Main](executing-animations-using-client-side-code-vb/samples/sample5.xml)]

<span data-ttu-id="70eeb-119">Ai fini di dimostrazione, questa animazione (e qualsiasi altra animazione creato con il Toolkit di controllo) viene eseguiti utilizzando codice JavaScript, dopo l'esecuzione della pagina.</span><span class="sxs-lookup"><span data-stu-id="70eeb-119">For the sake of demonstration, this animation (and any other animation created using the Control Toolkit) is executed using JavaScript code, once the page runs.</span></span> <span data-ttu-id="70eeb-120">Prima di tutto è necessario accedere al `AnimationExtender` controllo.</span><span class="sxs-lookup"><span data-stu-id="70eeb-120">First of all we need access to the `AnimationExtender` control.</span></span> <span data-ttu-id="70eeb-121">La libreria ASP.NET AJAX fornisce il `$find()` funzione per questa attività:</span><span class="sxs-lookup"><span data-stu-id="70eeb-121">The ASP.NET AJAX library provides the `$find()` function for this task:</span></span>

[!code-csharp[Main](executing-animations-using-client-side-code-vb/samples/sample6.cs)]

<span data-ttu-id="70eeb-122">Il `AnimationExtender` controllo espone un'API completa, inclusi i metodi con nomi identici ai gestori di eventi utilizzati nel markup XML: `OnClick()`, `OnLoad()`e così via.</span><span class="sxs-lookup"><span data-stu-id="70eeb-122">The `AnimationExtender` control exposes a rich API, including methods with names identical to the event handlers used in the XML markup: `OnClick()`, `OnLoad()`, and so on.</span></span> <span data-ttu-id="70eeb-123">Ad esempio, una chiamata del `OnClick()` metodo esegue l'animazione all'interno di `<OnClick>` elemento del `AnimationExtender` controllo:</span><span class="sxs-lookup"><span data-stu-id="70eeb-123">For instance, a call of the `OnClick()` method executes the animation within the `<OnClick>` element of the `AnimationExtender` control:</span></span>

[!code-javascript[Main](executing-animations-using-client-side-code-vb/samples/sample7.js)]

<span data-ttu-id="70eeb-124">Ecco il codice JavaScript sul lato client completo che emula il clic sul pannello dopo la pagina è stata caricata completamente si noti che il `pageLoad()` viene utilizzato il nome di funzione che viene chiamato da ASP.NET AJAX una volta la pagina e tutte incluse JavaScript sono state raccolte caricato.</span><span class="sxs-lookup"><span data-stu-id="70eeb-124">Here is the complete client-side JavaScript code that emulates the click on the panel once the page has been fully loaded note that the `pageLoad()` function name is used which is called by ASP.NET AJAX once the page and all included JavaScript libraries have been loaded.</span></span>

[!code-html[Main](executing-animations-using-client-side-code-vb/samples/sample8.html)]


<span data-ttu-id="70eeb-125">[![L'animazione viene eseguita immediatamente, senza un clic del mouse](executing-animations-using-client-side-code-vb/_static/image2.png)](executing-animations-using-client-side-code-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="70eeb-125">[![The animation runs immediately, without a mouse click](executing-animations-using-client-side-code-vb/_static/image2.png)](executing-animations-using-client-side-code-vb/_static/image1.png)</span></span>

<span data-ttu-id="70eeb-126">L'animazione viene eseguita immediatamente, senza un clic del mouse ([fare clic per visualizzare l'immagine ingrandita](executing-animations-using-client-side-code-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="70eeb-126">The animation runs immediately, without a mouse click ([Click to view full-size image](executing-animations-using-client-side-code-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="70eeb-127">[Precedente](modifying-animations-from-the-server-side-vb.md)
[Successivo](changing-an-animation-using-client-side-code-vb.md)</span><span class="sxs-lookup"><span data-stu-id="70eeb-127">[Previous](modifying-animations-from-the-server-side-vb.md)
[Next](changing-an-animation-using-client-side-code-vb.md)</span></span>
