---
uid: web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-vb
title: Creazione di un controllo di valutazione (VB) | Documenti Microsoft
author: wenz
description: Molti siti Web, di e-commerce ai siti delle community, offrono agli utenti di articoli di frequenza o elementi. Ciò in genere richiede alcuni impegno di codifica, ma non è disponibile il...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 6d0d70f4-725e-4258-8ae8-24a6ba1ddbf7
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-vb
msc.type: authoredcontent
ms.openlocfilehash: c19f36dfe1b72a3954db61ff1845e99c02e47c14
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="creating-a-rating-control-vb"></a><span data-ttu-id="11bd7-104">Creazione di un controllo di valutazione (VB)</span><span class="sxs-lookup"><span data-stu-id="11bd7-104">Creating a Rating Control (VB)</span></span>
====================
<span data-ttu-id="11bd7-105">da [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="11bd7-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="11bd7-106">[Scaricare codice](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/rating0.vb.zip) o [Scarica il PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/rating0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="11bd7-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/rating0.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/rating0VB.pdf)</span></span>

> <span data-ttu-id="11bd7-107">Molti siti Web, di e-commerce ai siti delle community, offrono agli utenti di articoli di frequenza o elementi.</span><span class="sxs-lookup"><span data-stu-id="11bd7-107">Many websites, from e-commerce to community sites, offer their users to rate articles or items.</span></span> <span data-ttu-id="11bd7-108">Questo in genere richiede alcuni impegno di codifica, ma abbiamo il Toolkit di controllo per l'eliminazione.</span><span class="sxs-lookup"><span data-stu-id="11bd7-108">This usually requires some coding effort, but we do have the Control Toolkit to our disposal.</span></span>


## <a name="overview"></a><span data-ttu-id="11bd7-109">Panoramica</span><span class="sxs-lookup"><span data-stu-id="11bd7-109">Overview</span></span>

<span data-ttu-id="11bd7-110">Molti siti Web, di e-commerce ai siti delle community, offrono agli utenti di articoli di frequenza o elementi.</span><span class="sxs-lookup"><span data-stu-id="11bd7-110">Many websites, from e-commerce to community sites, offer their users to rate articles or items.</span></span> <span data-ttu-id="11bd7-111">Questo in genere richiede alcuni impegno di codifica, ma abbiamo il Toolkit di controllo per l'eliminazione.</span><span class="sxs-lookup"><span data-stu-id="11bd7-111">This usually requires some coding effort, but we do have the Control Toolkit to our disposal.</span></span>

## <a name="steps"></a><span data-ttu-id="11bd7-112">Passaggi</span><span class="sxs-lookup"><span data-stu-id="11bd7-112">Steps</span></span>

<span data-ttu-id="11bd7-113">Prima di tutto, è necessario (almeno) due tipi di immagini: una per una piena elemento classificazione e uno per un elemento vuoto di classificazione.</span><span class="sxs-lookup"><span data-stu-id="11bd7-113">First of all, you need (at least) two kinds of images: one for a filled out rating item, and one for an empty rating item.</span></span> <span data-ttu-id="11bd7-114">Un elemento di classificazione è in genere una stella o un smile.</span><span class="sxs-lookup"><span data-stu-id="11bd7-114">A rating item is usually a star or a smiley.</span></span> <span data-ttu-id="11bd7-115">Per questo scenario, si trova tre file, smiley.png empty.png e smile done.png come parte dei download di codice sorgente per questa esercitazione.</span><span class="sxs-lookup"><span data-stu-id="11bd7-115">For this scenario, you find three files, smiley.png and empty.png and smiley-done.png as part of the source code downloads for this tutorial.</span></span>

<span data-ttu-id="11bd7-116">Quindi, creare un nuovo file ASP.NET e iniziare, aggiungere un `ScriptManager` controllo:</span><span class="sxs-lookup"><span data-stu-id="11bd7-116">Then, create a new ASP.NET file and start with adding a `ScriptManager` control to it:</span></span>

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample1.aspx)]

<span data-ttu-id="11bd7-117">Aggiungere quindi il `Rating` controllo di ASP.NET AJAX Control Toolkit.</span><span class="sxs-lookup"><span data-stu-id="11bd7-117">Then, add the `Rating` control from the ASP.NET AJAX Control Toolkit.</span></span> <span data-ttu-id="11bd7-118">È necessario impostare per questo esempio gli attributi seguenti:</span><span class="sxs-lookup"><span data-stu-id="11bd7-118">The following attributes need to be set for this example:</span></span>

- <span data-ttu-id="11bd7-119">`CurrentRating` la valutazione iniziale da utilizzare</span><span class="sxs-lookup"><span data-stu-id="11bd7-119">`CurrentRating` the initial rating to be used</span></span>
- <span data-ttu-id="11bd7-120">`MaxRating` la classificazione massima</span><span class="sxs-lookup"><span data-stu-id="11bd7-120">`MaxRating` the maximum rating</span></span>
- <span data-ttu-id="11bd7-121">`EmptyStarCssClass` la classe CSS da utilizzare quando un elemento di classificazione (asterisco) è vuota</span><span class="sxs-lookup"><span data-stu-id="11bd7-121">`EmptyStarCssClass` the CSS class to use when a rating item ( star ) is empty</span></span>
- <span data-ttu-id="11bd7-122">`FilledStarCssClass` la classe CSS da utilizzare quando viene compilato un elemento di classificazione (asterisco)</span><span class="sxs-lookup"><span data-stu-id="11bd7-122">`FilledStarCssClass` the CSS class to use when a rating item ( star ) is filled out</span></span>
- <span data-ttu-id="11bd7-123">`StarCssClass` la classe CSS da usare per un stat visibile</span><span class="sxs-lookup"><span data-stu-id="11bd7-123">`StarCssClass` the CSS class to use for a visible stat</span></span>
- <span data-ttu-id="11bd7-124">`WaitingStarCssClass` la classe CSS da usare durante una classificazione a stelle viene inviata al server</span><span class="sxs-lookup"><span data-stu-id="11bd7-124">`WaitingStarCssClass` the CSS class to use while a star rating is sent back to the server</span></span>

<span data-ttu-id="11bd7-125">Di seguito è il markup che crea un controllo di classificazione con cinque elementi (smileys) di cui non viene riempita inizialmente:</span><span class="sxs-lookup"><span data-stu-id="11bd7-125">And here is the markup which creates a rating control with five items (smileys) of which none is filled out initially:</span></span>

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample2.aspx)]

<span data-ttu-id="11bd7-126">Le tre classi CSS a cui fa riferimento a questo punto necessario mostrare i file di immagine appropriata, ovvero semplice con CSS:</span><span class="sxs-lookup"><span data-stu-id="11bd7-126">The three referenced CSS classes now need to show the appropriate image files, which is easy to do using CSS:</span></span>

[!code-css[Main](creating-a-rating-control-vb/samples/sample3.css)]

<span data-ttu-id="11bd7-127">Assicurarsi di fornire la larghezza e l'altezza delle tre immagini, in caso contrario la visualizzazione potrebbe avere un aspetto leggermente messed backup.</span><span class="sxs-lookup"><span data-stu-id="11bd7-127">Make sure that you provide the width and height of the three images, otherwise the display may look a bit messed up.</span></span>

<span data-ttu-id="11bd7-128">Infine, il risultato della valutazione deve essere visualizzato all'utente (o almeno salvato in un database).</span><span class="sxs-lookup"><span data-stu-id="11bd7-128">Finally, the result of the rating should be displayed to the user (or, at least saved in a database).</span></span> <span data-ttu-id="11bd7-129">Aggiungere un'etichetta per l'output di un messaggio di testo e un pulsante di invio per inviare nuovamente il form di classificazione per il server:</span><span class="sxs-lookup"><span data-stu-id="11bd7-129">So add a label for the output of a text message and a submit button to post back the rating form to the server:</span></span>

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample4.aspx)]

<span data-ttu-id="11bd7-130">Nel codice sul lato server, accedere al controllo di valutazione tramite il relativo `ID` e quindi accedere ai relativi `CurrentRating` proprietà che è il numero di elementi di classificazione selezionati, in questo esempio un valore compreso tra 0 e 5.</span><span class="sxs-lookup"><span data-stu-id="11bd7-130">In the server-side code, access the Rating control via its `ID` and then access its `CurrentRating` property which is the number of the selected rating items, in our example a value between 0 and 5.</span></span>

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample5.aspx)]

<span data-ttu-id="11bd7-131">Salvare la pagina e caricarla nel browser.</span><span class="sxs-lookup"><span data-stu-id="11bd7-131">Save the page and load it into your browser.</span></span> <span data-ttu-id="11bd7-132">Quando si passa il mouse sugli elementi di classificazione (inizialmente vuota), si verifica un effetto JavaScript: le modifiche di classificazione.</span><span class="sxs-lookup"><span data-stu-id="11bd7-132">When you hover over the (initially empty) rating items, a JavaScript effect occurs: The rating changes.</span></span> <span data-ttu-id="11bd7-133">Quando si fa clic sul set di stelle, viene mantenuta la classificazione corrente.</span><span class="sxs-lookup"><span data-stu-id="11bd7-133">When you click on the set of stars, the current rating is retained.</span></span> <span data-ttu-id="11bd7-134">Infine, quando si invia il form, il codice lato server restituisce la classificazione selezionata.</span><span class="sxs-lookup"><span data-stu-id="11bd7-134">Finally, when you submit the form, the server-side code outputs the selected rating.</span></span>


<span data-ttu-id="11bd7-135">[![Creazione di un sistema di classificazione con quantità minima di codice](creating-a-rating-control-vb/_static/image2.png)](creating-a-rating-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="11bd7-135">[![Creating a rating system with minimal code](creating-a-rating-control-vb/_static/image2.png)](creating-a-rating-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="11bd7-136">Creazione di un sistema di classificazione con quantità minima di codice ([fare clic per visualizzare l'immagine ingrandita](creating-a-rating-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="11bd7-136">Creating a rating system with minimal code ([Click to view full-size image](creating-a-rating-control-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="11bd7-137">Precedente</span><span class="sxs-lookup"><span data-stu-id="11bd7-137">Previous</span></span>](creating-a-rating-control-cs.md)
