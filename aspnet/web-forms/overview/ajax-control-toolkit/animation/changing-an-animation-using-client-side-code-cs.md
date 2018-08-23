---
uid: web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-cs
title: Modifica di un'animazione tramite codice lato Client (c#) | Microsoft Docs
author: wenz
description: Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo. L'animazione può inoltre...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 2bfbc5cc-f942-44b7-a62d-a29520f1da9a
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 253377ef6019a672680c6e819349357627ef111b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833638"
---
<a name="changing-an-animation-using-client-side-code-c"></a><span data-ttu-id="a1cf9-104">Modifica di un'animazione tramite codice lato Client (c#)</span><span class="sxs-lookup"><span data-stu-id="a1cf9-104">Changing an Animation Using Client-Side Code (C#)</span></span>
====================
<span data-ttu-id="a1cf9-105">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="a1cf9-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="a1cf9-106">[Scaricare il codice](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="a1cf9-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11CS.pdf)</span></span>

> <span data-ttu-id="a1cf9-107">Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="a1cf9-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="a1cf9-108">L'animazione può essere modificata anche utilizzando codice JavaScript lato client personalizzato.</span><span class="sxs-lookup"><span data-stu-id="a1cf9-108">The animation can also be changed using custom client-side JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="a1cf9-109">Panoramica</span><span class="sxs-lookup"><span data-stu-id="a1cf9-109">Overview</span></span>

<span data-ttu-id="a1cf9-110">Il controllo di animazione in ASP.NET AJAX Control Toolkit non è semplicemente un controllo, ma un intero framework aggiungere animazioni a un controllo.</span><span class="sxs-lookup"><span data-stu-id="a1cf9-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="a1cf9-111">L'animazione può essere modificata anche utilizzando codice JavaScript lato client personalizzato.</span><span class="sxs-lookup"><span data-stu-id="a1cf9-111">The animation can also be changed using custom client-side JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="a1cf9-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="a1cf9-112">Steps</span></span>

<span data-ttu-id="a1cf9-113">Prima di tutto, includere il `ScriptManager` nella pagina; quindi, la libreria ASP.NET AJAX viene caricata, rendendo possibile usare il Toolkit di controllo:</span><span class="sxs-lookup"><span data-stu-id="a1cf9-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample1.aspx)]

<span data-ttu-id="a1cf9-114">Verrà applicata l'animazione a un pannello del testo che presenta un aspetto simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="a1cf9-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample2.aspx)]

<span data-ttu-id="a1cf9-115">Nella classe CSS associata per il pannello, definire un colore di sfondo interessante e anche impostare una larghezza fissa per il pannello:</span><span class="sxs-lookup"><span data-stu-id="a1cf9-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](changing-an-animation-using-client-side-code-cs/samples/sample3.css)]

<span data-ttu-id="a1cf9-116">L'animazione effettivo viene avviato da un pulsante HTML:</span><span class="sxs-lookup"><span data-stu-id="a1cf9-116">The actual animation is launched by an HTML button:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample4.aspx)]

<span data-ttu-id="a1cf9-117">Quindi, aggiungere il `AnimationExtender` alla pagina, fornendo un' `ID`, il `TargetControlID` attributo e l'obbligatoria `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="a1cf9-117">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample5.aspx)]

<span data-ttu-id="a1cf9-118">Si noti che è presente alcun `<Animations>` nodo all'interno di `AnimationExtender` controllo.</span><span class="sxs-lookup"><span data-stu-id="a1cf9-118">Note that there is no `<Animations>` node within the `AnimationExtender` control.</span></span> <span data-ttu-id="a1cf9-119">Il codice JavaScript personalizzato viene utilizzato per fornire le animazioni da utilizzare con il controllo.</span><span class="sxs-lookup"><span data-stu-id="a1cf9-119">Custom JavaScript code is used to provide the animations to be used with the control.</span></span>

<span data-ttu-id="a1cf9-120">Come con l'API del server di `AnimationExtender`, non esiste un modo semplice per assegnare un'animazione al dispositivo extender ancora.</span><span class="sxs-lookup"><span data-stu-id="a1cf9-120">As with the server API of `AnimationExtender`, there is no easy way to assign an animation to the extender yet.</span></span> <span data-ttu-id="a1cf9-121">Tuttavia il dispositivo extender esporre diversi metodi per leggere e scrivere le animazioni registrato con i vari eventi (`OnClick`, `OnLoad`e così via).</span><span class="sxs-lookup"><span data-stu-id="a1cf9-121">However the extender does expose several methods to read and write animations registered with the various events (`OnClick`, `OnLoad`, and so on).</span></span> <span data-ttu-id="a1cf9-122">Ecco alcuni esempi:</span><span class="sxs-lookup"><span data-stu-id="a1cf9-122">Here are some examples:</span></span>

- `get_OnClick()`
- `set_OnClick()`
- `get_OnLoad()`
- `set_OnLoad()`
- `...`

<span data-ttu-id="a1cf9-123">Il formato del valore restituito del `get_*()` funzioni e il formato dell'argomento per il `set_*()` funzioni è una stringa JSON, che fornisce una rappresentazione di oggetto di quello che sarebbe il markup XML.</span><span class="sxs-lookup"><span data-stu-id="a1cf9-123">The format of the return value of the `get_*()` functions and the format of the argument for the `set_*()` functions is a JSON string, providing an object representation of what the XML markup would be.</span></span> <span data-ttu-id="a1cf9-124">Attualmente, non è possibile passare un oggetto, ma è possibile leggere un oggetto da una determinata animazione (`get_OnXXXBehavior()` metodi).</span><span class="sxs-lookup"><span data-stu-id="a1cf9-124">Currently, there is no way to pass an object in, but it is possible to read an object from a given animation (`get_OnXXXBehavior()` methods).</span></span>

<span data-ttu-id="a1cf9-125">Di seguito è una stringa JSON (senza le virgolette di delimitazione e formattate) che rappresenta un'animazione attivata dal pulsante, ma l'animazione del pannello riadattarlo e dissolvenza in uscita nello stesso momento:</span><span class="sxs-lookup"><span data-stu-id="a1cf9-125">Here is a JSON string (without the delimiting quotes and formatted nicely) representing an animation triggered by the button, but animating the panel by resizing it and fading it out at the same time:</span></span>

[!code-json[Main](changing-an-animation-using-client-side-code-cs/samples/sample6.json)]

<span data-ttu-id="a1cf9-126">Il codice JavaScript seguente assegna questa descripting JSON per il `OnClick` animazione dell'oggetto extender corrente e lo esegue:</span><span class="sxs-lookup"><span data-stu-id="a1cf9-126">The following JavaScript code assigns this JSON descripting to the `OnClick` animation of the current extender and runs it:</span></span>

[!code-html[Main](changing-an-animation-using-client-side-code-cs/samples/sample7.html)]


<span data-ttu-id="a1cf9-127">[![L'animazione viene eseguita immediatamente, senza un clic del mouse (e con markup minimo)](changing-an-animation-using-client-side-code-cs/_static/image2.png)](changing-an-animation-using-client-side-code-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="a1cf9-127">[![The animation runs immediately, without a mouse click (and with very little markup)](changing-an-animation-using-client-side-code-cs/_static/image2.png)](changing-an-animation-using-client-side-code-cs/_static/image1.png)</span></span>

<span data-ttu-id="a1cf9-128">L'animazione viene eseguita immediatamente, senza un clic del mouse (e con markup minimo) ([fare clic per visualizzare l'immagine con dimensioni normali](changing-an-animation-using-client-side-code-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="a1cf9-128">The animation runs immediately, without a mouse click (and with very little markup) ([Click to view full-size image](changing-an-animation-using-client-side-code-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a1cf9-129">[Precedente](executing-animations-using-client-side-code-cs.md)
> [Successivo](animating-an-updatepanel-control-cs.md)</span><span class="sxs-lookup"><span data-stu-id="a1cf9-129">[Previous](executing-animations-using-client-side-code-cs.md)
[Next](animating-an-updatepanel-control-cs.md)</span></span>
