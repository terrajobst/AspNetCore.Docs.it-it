---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-cs
title: Modifica delle proprietà di DropShadow dal codice Client (c#) | Microsoft Docs
author: wenz
description: Personalizzazione dell'interfaccia di modifica di DataList
ms.author: riande
ms.date: 06/02/2008
ms.assetid: c83ca3e6-c0bf-4158-a166-40c1ab0f33da
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-cs
msc.type: authoredcontent
ms.openlocfilehash: f751e2378621a6ab74f9f15fe51fd18bdd8b4070
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830330"
---
<a name="manipulating-dropshadow-properties-from-client-code-c"></a><span data-ttu-id="4211d-103">Modifica delle proprietà di DropShadow dal codice Client (c#)</span><span class="sxs-lookup"><span data-stu-id="4211d-103">Manipulating DropShadow Properties from Client Code (C#)</span></span>
====================
<span data-ttu-id="4211d-104">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="4211d-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="4211d-105">[Scaricare il codice](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.cs.zip) o [Scarica il PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="4211d-105">[Download Code](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2CS.pdf)</span></span>

> <span data-ttu-id="4211d-106">Il controllo DropShadow in AJAX Control Toolkit estende un pannello con un'ombreggiatura.</span><span class="sxs-lookup"><span data-stu-id="4211d-106">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="4211d-107">Proprietà di questo tipo di estensione possono essere modificate anche con il codice JavaScript client.</span><span class="sxs-lookup"><span data-stu-id="4211d-107">Properties of this extender can also be changed using client JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="4211d-108">Panoramica</span><span class="sxs-lookup"><span data-stu-id="4211d-108">Overview</span></span>

<span data-ttu-id="4211d-109">Il controllo DropShadow in AJAX Control Toolkit estende un pannello con un'ombreggiatura.</span><span class="sxs-lookup"><span data-stu-id="4211d-109">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="4211d-110">Proprietà di questo tipo di estensione possono essere modificate anche con il codice JavaScript client.</span><span class="sxs-lookup"><span data-stu-id="4211d-110">Properties of this extender can also be changed using client JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="4211d-111">Passaggi</span><span class="sxs-lookup"><span data-stu-id="4211d-111">Steps</span></span>

<span data-ttu-id="4211d-112">Il codice inizia con un pannello che contiene alcune righe di testo:</span><span class="sxs-lookup"><span data-stu-id="4211d-112">The code starts with a panel containing some lines of text:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample1.aspx)]

<span data-ttu-id="4211d-113">La classe CSS associata offre il pannello di un colore di sfondo interessante:</span><span class="sxs-lookup"><span data-stu-id="4211d-113">The associated CSS class gives the panel a nice background color:</span></span>

[!code-css[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample2.css)]

<span data-ttu-id="4211d-114">Il `DropShadowExtender` viene aggiunto per estendere il pannello con un effetto di ombreggiatura, impostato su 50% di opacità:</span><span class="sxs-lookup"><span data-stu-id="4211d-114">The `DropShadowExtender` is added to extend the panel with a drop shadow effect, opacity set to 50%:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample3.aspx)]

<span data-ttu-id="4211d-115">Quindi, ASP.NET AJAX `ScriptManager` controllo Abilita il Toolkit di controllo lavorare:</span><span class="sxs-lookup"><span data-stu-id="4211d-115">Then, the ASP.NET AJAX `ScriptManager` control enables the Control Toolkit to work:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample4.aspx)]

<span data-ttu-id="4211d-116">Un altro pannello contiene due collegamenti di JavaScript per l'impostazione di opacità dell'ombreggiatura: il collegamento meno ridurre l'opacità dell'ombreggiatura, aumenta il collegamento di più.</span><span class="sxs-lookup"><span data-stu-id="4211d-116">Another panel contains two JavaScript links for setting the opacity of the drop shadow: the minus link decreases the shadow's opacity, the plus link increases it.</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample5.aspx)]

<span data-ttu-id="4211d-117">La funzione JavaScript `changeOpacity()` quindi necessario trovare prima il `DropShadowExtender` controllo nella pagina.</span><span class="sxs-lookup"><span data-stu-id="4211d-117">The JavaScript function `changeOpacity()` must then first find the `DropShadowExtender` control on the page.</span></span> <span data-ttu-id="4211d-118">ASP.NET AJAX definisce il `$find()` metodo per esattamente tale attività.</span><span class="sxs-lookup"><span data-stu-id="4211d-118">ASP.NET AJAX defines the `$find()` method for exactly that task.</span></span> <span data-ttu-id="4211d-119">Successivamente, il `get_Opacity()` che consente di recuperare l'opacità corrente, il `set_Opacity()` lo imposta metodo.</span><span class="sxs-lookup"><span data-stu-id="4211d-119">Then, the `get_Opacity()` method retrieves the current opacity, the `set_Opacity()` method sets it.</span></span> <span data-ttu-id="4211d-120">Il codice JavaScript inserisce quindi il valore di opacità nel `<label>` elemento:</span><span class="sxs-lookup"><span data-stu-id="4211d-120">The JavaScript code then puts the current opacity value in the `<label>` element:</span></span>

[!code-html[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample6.html)]


<span data-ttu-id="4211d-121">[![L'opacità viene modificato sul lato client](manipulating-dropshadow-properties-from-client-code-cs/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="4211d-121">[![The opacity is changed on the client side](manipulating-dropshadow-properties-from-client-code-cs/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-cs/_static/image1.png)</span></span>

<span data-ttu-id="4211d-122">L'opacità viene modificato sul lato client ([fare clic per visualizzare l'immagine con dimensioni normali](manipulating-dropshadow-properties-from-client-code-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="4211d-122">The opacity is changed on the client side ([Click to view full-size image](manipulating-dropshadow-properties-from-client-code-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="4211d-123">[Precedente](adjusting-the-z-index-of-a-dropshadow-cs.md)
> [Successivo](adjusting-the-z-index-of-a-dropshadow-vb.md)</span><span class="sxs-lookup"><span data-stu-id="4211d-123">[Previous](adjusting-the-z-index-of-a-dropshadow-cs.md)
[Next](adjusting-the-z-index-of-a-dropshadow-vb.md)</span></span>
