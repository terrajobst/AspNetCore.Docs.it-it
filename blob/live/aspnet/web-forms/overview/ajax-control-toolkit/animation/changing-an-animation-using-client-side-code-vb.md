---
uid: web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-vb
title: La modifica di un'animazione con il codice lato Client (VB) | Documenti Microsoft
author: wenz
description: "Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework per aggiungere le animazioni a un controllo. L'animazione può inoltre..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: a7fe5de5-a964-4780-ae5e-70821dfb50a0
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 83d1a21fba37d8807be467d02b5550dc7d096e6c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="changing-an-animation-using-client-side-code-vb"></a><span data-ttu-id="f0db1-104">La modifica di un'animazione con il codice lato Client (VB)</span><span class="sxs-lookup"><span data-stu-id="f0db1-104">Changing an Animation Using Client-Side Code (VB)</span></span>
====================
<span data-ttu-id="f0db1-105">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="f0db1-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="f0db1-106">[Scaricare codice](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="f0db1-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11VB.pdf)</span></span>

> <span data-ttu-id="f0db1-107">Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework per aggiungere le animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="f0db1-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="f0db1-108">L'animazione può essere modificato utilizzando codice JavaScript sul lato client personalizzato.</span><span class="sxs-lookup"><span data-stu-id="f0db1-108">The animation can also be changed using custom client-side JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="f0db1-109">Panoramica</span><span class="sxs-lookup"><span data-stu-id="f0db1-109">Overview</span></span>

<span data-ttu-id="f0db1-110">Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework per aggiungere le animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="f0db1-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="f0db1-111">L'animazione può essere modificato utilizzando codice JavaScript sul lato client personalizzato.</span><span class="sxs-lookup"><span data-stu-id="f0db1-111">The animation can also be changed using custom client-side JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="f0db1-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="f0db1-112">Steps</span></span>

<span data-ttu-id="f0db1-113">Prima di tutto, includere il `ScriptManager` nella pagina; quindi, la libreria AJAX di ASP.NET viene caricata, che consente di usare il Toolkit di controllo:</span><span class="sxs-lookup"><span data-stu-id="f0db1-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample1.aspx)]

<span data-ttu-id="f0db1-114">Verrà applicata l'animazione a un riquadro del testo che presenta un aspetto simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="f0db1-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample2.aspx)]

<span data-ttu-id="f0db1-115">Nella classe CSS associata per il pannello, definire un colore di sfondo nice e inoltre impostare una larghezza fissa per il pannello:</span><span class="sxs-lookup"><span data-stu-id="f0db1-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](changing-an-animation-using-client-side-code-vb/samples/sample3.css)]

<span data-ttu-id="f0db1-116">Viene avviata l'animazione effettive per un pulsante HTML:</span><span class="sxs-lookup"><span data-stu-id="f0db1-116">The actual animation is launched by an HTML button:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample4.aspx)]

<span data-ttu-id="f0db1-117">Quindi, aggiungere il `AnimationExtender` alla pagina, fornendo un `ID`, `TargetControlID` attributo e l'obbligatoria `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="f0db1-117">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample5.aspx)]

<span data-ttu-id="f0db1-118">Si noti che non esiste alcun `<Animations>` nodo all'interno di `AnimationExtender` controllo.</span><span class="sxs-lookup"><span data-stu-id="f0db1-118">Note that there is no `<Animations>` node within the `AnimationExtender` control.</span></span> <span data-ttu-id="f0db1-119">Il codice JavaScript personalizzato viene utilizzato per fornire le animazioni da utilizzare con il controllo.</span><span class="sxs-lookup"><span data-stu-id="f0db1-119">Custom JavaScript code is used to provide the animations to be used with the control.</span></span>

<span data-ttu-id="f0db1-120">Come con l'API del server di `AnimationExtender`, non è possibile assegnare un'animazione a extender ancora facile.</span><span class="sxs-lookup"><span data-stu-id="f0db1-120">As with the server API of `AnimationExtender`, there is no easy way to assign an animation to the extender yet.</span></span> <span data-ttu-id="f0db1-121">Tuttavia il programma di estensione espone diversi metodi per leggere e scrivere le animazioni registrato con i vari eventi (`OnClick`, `OnLoad`e così via).</span><span class="sxs-lookup"><span data-stu-id="f0db1-121">However the extender does expose several methods to read and write animations registered with the various events (`OnClick`, `OnLoad`, and so on).</span></span> <span data-ttu-id="f0db1-122">Ecco alcuni esempi:</span><span class="sxs-lookup"><span data-stu-id="f0db1-122">Here are some examples:</span></span>

- `get_OnClick()`
- `set_OnClick()`
- `get_OnLoad()`
- `set_OnLoad()`
- `...`

<span data-ttu-id="f0db1-123">Il formato del valore restituito del `get_*()` funzioni e il formato dell'argomento per il `set_*()` funzioni è una stringa JSON, fornendo una rappresentazione dell'oggetto dei possibili il markup XML.</span><span class="sxs-lookup"><span data-stu-id="f0db1-123">The format of the return value of the `get_*()` functions and the format of the argument for the `set_*()` functions is a JSON string, providing an object representation of what the XML markup would be.</span></span> <span data-ttu-id="f0db1-124">Attualmente, non è possibile passare un oggetto, ma è possibile leggere un oggetto di una determinato animazione (`get_OnXXXBehavior()` metodi).</span><span class="sxs-lookup"><span data-stu-id="f0db1-124">Currently, there is no way to pass an object in, but it is possible to read an object from a given animation (`get_OnXXXBehavior()` methods).</span></span>

<span data-ttu-id="f0db1-125">Di seguito è una stringa JSON (senza le virgolette di delimitazione e formattati correttamente) che rappresenta un'animazione attivata mediante il pulsante, ma l'animazione del pannello esegue il ridimensionamento e dissolvenza in uscita contemporaneamente:</span><span class="sxs-lookup"><span data-stu-id="f0db1-125">Here is a JSON string (without the delimiting quotes and formatted nicely) representing an animation triggered by the button, but animating the panel by resizing it and fading it out at the same time:</span></span>

[!code-json[Main](changing-an-animation-using-client-side-code-vb/samples/sample6.json)]

<span data-ttu-id="f0db1-126">Il codice JavaScript seguente assegna questo descripting JSON per il `OnClick` animazione dell'oggetto di estensione corrente e lo esegue:</span><span class="sxs-lookup"><span data-stu-id="f0db1-126">The following JavaScript code assigns this JSON descripting to the `OnClick` animation of the current extender and runs it:</span></span>

[!code-html[Main](changing-an-animation-using-client-side-code-vb/samples/sample7.html)]


<span data-ttu-id="f0db1-127">[![L'animazione viene eseguita immediatamente, senza un clic del mouse (e con markup minimo)](changing-an-animation-using-client-side-code-vb/_static/image2.png)](changing-an-animation-using-client-side-code-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="f0db1-127">[![The animation runs immediately, without a mouse click (and with very little markup)](changing-an-animation-using-client-side-code-vb/_static/image2.png)](changing-an-animation-using-client-side-code-vb/_static/image1.png)</span></span>

<span data-ttu-id="f0db1-128">L'animazione viene eseguita immediatamente, senza un clic del mouse (e con markup poco) ([fare clic per visualizzare l'immagine ingrandita](changing-an-animation-using-client-side-code-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="f0db1-128">The animation runs immediately, without a mouse click (and with very little markup) ([Click to view full-size image](changing-an-animation-using-client-side-code-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="f0db1-129">[Precedente](executing-animations-using-client-side-code-vb.md)
[Successivo](animating-an-updatepanel-control-vb.md)</span><span class="sxs-lookup"><span data-stu-id="f0db1-129">[Previous](executing-animations-using-client-side-code-vb.md)
[Next](animating-an-updatepanel-control-vb.md)</span></span>
