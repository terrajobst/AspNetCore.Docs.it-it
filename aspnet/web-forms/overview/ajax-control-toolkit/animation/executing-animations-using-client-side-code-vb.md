---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-vb
title: Esecuzione di animazioni tramite codice lato Client (VB) | Microsoft Docs
author: wenz
description: Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo. L'esecuzione di animazione...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: f7073f50-d765-456d-9957-926ce60f35f6
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 9b180ad17e0e3d2dffa6262d10a83a8353e0a1d0
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37811886"
---
<a name="executing-animations-using-client-side-code-vb"></a><span data-ttu-id="753ce-104">Esecuzione di animazioni tramite codice lato Client (VB)</span><span class="sxs-lookup"><span data-stu-id="753ce-104">Executing Animations Using Client-Side Code (VB)</span></span>
====================
<span data-ttu-id="753ce-105">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="753ce-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="753ce-106">[Scaricare il codice](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation10.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation10VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="753ce-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation10.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation10VB.pdf)</span></span>

> <span data-ttu-id="753ce-107">Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="753ce-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="753ce-108">L'esecuzione di animazione può essere attivato utilizzando codice JavaScript lato client personalizzato.</span><span class="sxs-lookup"><span data-stu-id="753ce-108">The animation execution may also be triggered using custom client-side JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="753ce-109">Panoramica</span><span class="sxs-lookup"><span data-stu-id="753ce-109">Overview</span></span>

<span data-ttu-id="753ce-110">Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="753ce-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="753ce-111">L'esecuzione di animazione può essere attivato utilizzando codice JavaScript lato client personalizzato.</span><span class="sxs-lookup"><span data-stu-id="753ce-111">The animation execution may also be triggered using custom client-side JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="753ce-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="753ce-112">Steps</span></span>

<span data-ttu-id="753ce-113">Prima di tutto, includere il `ScriptManager` nella pagina; quindi, la libreria ASP.NET AJAX viene caricata, rendendo possibile usare il Toolkit di controllo:</span><span class="sxs-lookup"><span data-stu-id="753ce-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample1.aspx)]

<span data-ttu-id="753ce-114">Verrà applicata l'animazione a un pannello del testo che presenta un aspetto simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="753ce-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample2.aspx)]

<span data-ttu-id="753ce-115">Nella classe CSS associata per il pannello, definire un colore di sfondo interessante e anche impostare una larghezza fissa per il pannello:</span><span class="sxs-lookup"><span data-stu-id="753ce-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](executing-animations-using-client-side-code-vb/samples/sample3.css)]

<span data-ttu-id="753ce-116">Quindi, aggiungere il `AnimationExtender` alla pagina, fornendo un' `ID`, il `TargetControlID` attributo e l'obbligatoria `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="753ce-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample4.aspx)]

<span data-ttu-id="753ce-117">All'interno di `<Animations>` nodo, usare `<OnClick>` per eseguire le animazioni una volta l'utente fa clic sul pannello.</span><span class="sxs-lookup"><span data-stu-id="753ce-117">Within the `<Animations>` node, use `<OnClick>` to run the animations once the user clicks on the panel.</span></span> <span data-ttu-id="753ce-118">Aggiungere due animazioni deve essere eseguito parallelly:</span><span class="sxs-lookup"><span data-stu-id="753ce-118">Add two animations to be executed parallelly:</span></span>

[!code-xml[Main](executing-animations-using-client-side-code-vb/samples/sample5.xml)]

<span data-ttu-id="753ce-119">Per fini dimostrativi, questa animazione (e qualsiasi altra animazione creato usando il Toolkit di controllo) viene eseguite tramite il codice JavaScript, una volta che viene eseguita la pagina.</span><span class="sxs-lookup"><span data-stu-id="753ce-119">For the sake of demonstration, this animation (and any other animation created using the Control Toolkit) is executed using JavaScript code, once the page runs.</span></span> <span data-ttu-id="753ce-120">Prima di tutto è necessario accedere al `AnimationExtender` controllo.</span><span class="sxs-lookup"><span data-stu-id="753ce-120">First of all we need access to the `AnimationExtender` control.</span></span> <span data-ttu-id="753ce-121">La libreria ASP.NET AJAX fornisce il `$find()` funzione per questa attività:</span><span class="sxs-lookup"><span data-stu-id="753ce-121">The ASP.NET AJAX library provides the `$find()` function for this task:</span></span>

[!code-csharp[Main](executing-animations-using-client-side-code-vb/samples/sample6.cs)]

<span data-ttu-id="753ce-122">Il `AnimationExtender` controllo espone un'API completa, inclusi i metodi con nomi identici per i gestori eventi utilizzati nel markup XML: `OnClick()`, `OnLoad()`e così via.</span><span class="sxs-lookup"><span data-stu-id="753ce-122">The `AnimationExtender` control exposes a rich API, including methods with names identical to the event handlers used in the XML markup: `OnClick()`, `OnLoad()`, and so on.</span></span> <span data-ttu-id="753ce-123">Ad esempio, una chiamata del `OnClick()` l'animazione in esecuzione del metodo il `<OnClick>` elemento del `AnimationExtender` controllo:</span><span class="sxs-lookup"><span data-stu-id="753ce-123">For instance, a call of the `OnClick()` method executes the animation within the `<OnClick>` element of the `AnimationExtender` control:</span></span>

[!code-javascript[Main](executing-animations-using-client-side-code-vb/samples/sample7.js)]

<span data-ttu-id="753ce-124">Di seguito è riportato il codice completo di JavaScript lato client che emula il clic sul pannello dopo la pagina è stata caricata completamente si noti che il `pageLoad()` nome della funzione viene utilizzato il quale viene chiamato da ASP.NET AJAX una volta la pagina e incluse tutte le librerie sono state JavaScript caricato.</span><span class="sxs-lookup"><span data-stu-id="753ce-124">Here is the complete client-side JavaScript code that emulates the click on the panel once the page has been fully loaded note that the `pageLoad()` function name is used which is called by ASP.NET AJAX once the page and all included JavaScript libraries have been loaded.</span></span>

[!code-html[Main](executing-animations-using-client-side-code-vb/samples/sample8.html)]


<span data-ttu-id="753ce-125">[![L'animazione viene eseguita immediatamente, senza un clic del mouse](executing-animations-using-client-side-code-vb/_static/image2.png)](executing-animations-using-client-side-code-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="753ce-125">[![The animation runs immediately, without a mouse click](executing-animations-using-client-side-code-vb/_static/image2.png)](executing-animations-using-client-side-code-vb/_static/image1.png)</span></span>

<span data-ttu-id="753ce-126">L'animazione viene eseguito immediatamente, senza un clic del mouse ([fare clic per visualizzare l'immagine con dimensioni normali](executing-animations-using-client-side-code-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="753ce-126">The animation runs immediately, without a mouse click ([Click to view full-size image](executing-animations-using-client-side-code-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="753ce-127">[Precedente](modifying-animations-from-the-server-side-vb.md)
> [Successivo](changing-an-animation-using-client-side-code-vb.md)</span><span class="sxs-lookup"><span data-stu-id="753ce-127">[Previous](modifying-animations-from-the-server-side-vb.md)
[Next](changing-an-animation-using-client-side-code-vb.md)</span></span>
