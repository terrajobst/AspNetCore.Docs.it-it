---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-cs
title: La modifica delle proprietà di DropShadow dal codice Client (c#) | Documenti Microsoft
author: wenz
description: Personalizzazione dell'interfaccia di modifica del controllo DataList
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: c83ca3e6-c0bf-4158-a166-40c1ab0f33da
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 37a7784e1d42477e31938e1d15495993ac86fc56
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="manipulating-dropshadow-properties-from-client-code-c"></a><span data-ttu-id="213c6-103">La modifica delle proprietà di DropShadow dal codice Client (c#)</span><span class="sxs-lookup"><span data-stu-id="213c6-103">Manipulating DropShadow Properties from Client Code (C#)</span></span>
====================
<span data-ttu-id="213c6-104">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="213c6-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="213c6-105">[Scaricare codice](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="213c6-105">[Download Code](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2CS.pdf)</span></span>

> <span data-ttu-id="213c6-106">Il controllo DropShadow AJAX Control Toolkit estende un pannello con un'ombreggiatura.</span><span class="sxs-lookup"><span data-stu-id="213c6-106">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="213c6-107">Proprietà di questo tipo di estensione possono essere modificate anche utilizzando il codice client JavaScript.</span><span class="sxs-lookup"><span data-stu-id="213c6-107">Properties of this extender can also be changed using client JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="213c6-108">Panoramica</span><span class="sxs-lookup"><span data-stu-id="213c6-108">Overview</span></span>

<span data-ttu-id="213c6-109">Il controllo DropShadow AJAX Control Toolkit estende un pannello con un'ombreggiatura.</span><span class="sxs-lookup"><span data-stu-id="213c6-109">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="213c6-110">Proprietà di questo tipo di estensione possono essere modificate anche utilizzando il codice client JavaScript.</span><span class="sxs-lookup"><span data-stu-id="213c6-110">Properties of this extender can also be changed using client JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="213c6-111">Passaggi</span><span class="sxs-lookup"><span data-stu-id="213c6-111">Steps</span></span>

<span data-ttu-id="213c6-112">Il codice inizia con un pannello contenente alcune righe di testo:</span><span class="sxs-lookup"><span data-stu-id="213c6-112">The code starts with a panel containing some lines of text:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample1.aspx)]

<span data-ttu-id="213c6-113">La classe CSS associata offre il pannello di un colore di sfondo nice:</span><span class="sxs-lookup"><span data-stu-id="213c6-113">The associated CSS class gives the panel a nice background color:</span></span>

[!code-css[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample2.css)]

<span data-ttu-id="213c6-114">Il `DropShadowExtender` viene aggiunto al riquadro con un effetto di ombreggiatura, impostare su 50% di opacità estendere:</span><span class="sxs-lookup"><span data-stu-id="213c6-114">The `DropShadowExtender` is added to extend the panel with a drop shadow effect, opacity set to 50%:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample3.aspx)]

<span data-ttu-id="213c6-115">Quindi, ASP.NET AJAX `ScriptManager` controllo Abilita il Toolkit di controllo utilizzare:</span><span class="sxs-lookup"><span data-stu-id="213c6-115">Then, the ASP.NET AJAX `ScriptManager` control enables the Control Toolkit to work:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample4.aspx)]

<span data-ttu-id="213c6-116">Un altro pannello contiene due collegamenti di JavaScript per l'impostazione dell'opacità dell'ombreggiatura: il collegamento meno ridurre l'opacità dell'ombreggiatura, il collegamento più aumenta.</span><span class="sxs-lookup"><span data-stu-id="213c6-116">Another panel contains two JavaScript links for setting the opacity of the drop shadow: the minus link decreases the shadow's opacity, the plus link increases it.</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample5.aspx)]

<span data-ttu-id="213c6-117">La funzione JavaScript `changeOpacity()` quindi necessario trovare il `DropShadowExtender` controllo nella pagina.</span><span class="sxs-lookup"><span data-stu-id="213c6-117">The JavaScript function `changeOpacity()` must then first find the `DropShadowExtender` control on the page.</span></span> <span data-ttu-id="213c6-118">ASP.NET AJAX definisce il `$find()` metodo per esattamente tale attività.</span><span class="sxs-lookup"><span data-stu-id="213c6-118">ASP.NET AJAX defines the `$find()` method for exactly that task.</span></span> <span data-ttu-id="213c6-119">Quindi, `get_Opacity()` che consente di recuperare l'opacità corrente, il `set_Opacity()` viene impostato dal metodo.</span><span class="sxs-lookup"><span data-stu-id="213c6-119">Then, the `get_Opacity()` method retrieves the current opacity, the `set_Opacity()` method sets it.</span></span> <span data-ttu-id="213c6-120">Il valore di opacità del codice JavaScript viene posizionato nel `<label>` elemento:</span><span class="sxs-lookup"><span data-stu-id="213c6-120">The JavaScript code then puts the current opacity value in the `<label>` element:</span></span>

[!code-html[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample6.html)]


<span data-ttu-id="213c6-121">[![L'opacità viene modificato sul lato client](manipulating-dropshadow-properties-from-client-code-cs/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="213c6-121">[![The opacity is changed on the client side](manipulating-dropshadow-properties-from-client-code-cs/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-cs/_static/image1.png)</span></span>

<span data-ttu-id="213c6-122">L'opacità viene modificato sul lato client ([fare clic per visualizzare l'immagine ingrandita](manipulating-dropshadow-properties-from-client-code-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="213c6-122">The opacity is changed on the client side ([Click to view full-size image](manipulating-dropshadow-properties-from-client-code-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="213c6-123">[Precedente](adjusting-the-z-index-of-a-dropshadow-cs.md)
> [Successivo](adjusting-the-z-index-of-a-dropshadow-vb.md)</span><span class="sxs-lookup"><span data-stu-id="213c6-123">[Previous](adjusting-the-z-index-of-a-dropshadow-cs.md)
[Next](adjusting-the-z-index-of-a-dropshadow-vb.md)</span></span>
