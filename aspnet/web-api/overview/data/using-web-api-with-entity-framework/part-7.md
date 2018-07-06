---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-7
title: Creare la vista (UI) | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 06/16/2014
ms.assetid: b2445062-a1fe-4133-8994-f510280f6d9a
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-7
msc.type: authoredcontent
ms.openlocfilehash: eeaa36ede45b82afd112a270acf68105d1ae6dbc
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820986"
---
<a name="create-the-view-ui"></a><span data-ttu-id="7bbe7-102">Creare la vista (UI)</span><span class="sxs-lookup"><span data-stu-id="7bbe7-102">Create the View (UI)</span></span>
====================
<span data-ttu-id="7bbe7-103">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="7bbe7-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="7bbe7-104">Download progetto completato</span><span class="sxs-lookup"><span data-stu-id="7bbe7-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="7bbe7-105">In questa sezione, si inizieranno a definire il codice HTML per l'app e aggiungere l'associazione dati tra il codice HTML e il modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="7bbe7-105">In this section, you will start to define the HTML for the app, and add data binding between the HTML and the view model.</span></span>

<span data-ttu-id="7bbe7-106">Aprire il file Views/Home/Index.cshtml.</span><span class="sxs-lookup"><span data-stu-id="7bbe7-106">Open the file Views/Home/Index.cshtml.</span></span> <span data-ttu-id="7bbe7-107">Sostituire l'intero contenuto del file con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="7bbe7-107">Replace the entire contents of that file with the following.</span></span>

[!code-cshtml[Main](part-7/samples/sample1.cshtml)]

<span data-ttu-id="7bbe7-108">La maggior parte delle `div` sono per gli elementi [Bootstrap](http://getbootstrap.com/) lo stile.</span><span class="sxs-lookup"><span data-stu-id="7bbe7-108">Most of the `div` elements are there for [Bootstrap](http://getbootstrap.com/) styling.</span></span> <span data-ttu-id="7bbe7-109">Gli elementi importanti sono quelli con `data-bind` attributi.</span><span class="sxs-lookup"><span data-stu-id="7bbe7-109">The important elements are the ones with `data-bind` attributes.</span></span> <span data-ttu-id="7bbe7-110">Questo attributo collega il codice HTML per il modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="7bbe7-110">This attribute links the HTML to the view model.</span></span>

<span data-ttu-id="7bbe7-111">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="7bbe7-111">For example:</span></span>

[!code-html[Main](part-7/samples/sample2.html)]

<span data-ttu-id="7bbe7-112">In questo esempio, il &quot; `text` &quot; associazione cause il `<p>` elemento per visualizzare il valore della `error` proprietà dal modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="7bbe7-112">In this example, the &quot;`text`&quot; binding causes the `<p>` element to show the value of the `error` property from the view model.</span></span> <span data-ttu-id="7bbe7-113">È importante ricordare che `error` è stato dichiarato come un `ko.observable`:</span><span class="sxs-lookup"><span data-stu-id="7bbe7-113">Recall that `error` was declared as a `ko.observable`:</span></span>

[!code-javascript[Main](part-7/samples/sample3.js)]

<span data-ttu-id="7bbe7-114">Ogni volta che un nuovo valore viene assegnato a `error`, Knockout aggiorna il testo di `<p>` elemento.</span><span class="sxs-lookup"><span data-stu-id="7bbe7-114">Whenever a new value is assigned to `error`, Knockout updates the text in the `<p>` element.</span></span>

<span data-ttu-id="7bbe7-115">Il `foreach` associazione indica Knockout per eseguire il contenuto del ciclo di `books` matrice.</span><span class="sxs-lookup"><span data-stu-id="7bbe7-115">The `foreach` binding tells Knockout to loop through the contents of the `books` array.</span></span> <span data-ttu-id="7bbe7-116">Per ogni elemento nella matrice, crea un nuovo Knockout &lt;li&gt; elemento.</span><span class="sxs-lookup"><span data-stu-id="7bbe7-116">For each item in the array, Knockout creates a new &lt;li&gt; element.</span></span> <span data-ttu-id="7bbe7-117">Le associazioni all'interno del contesto del `foreach` fare riferimento alle proprietà dell'elemento di matrice.</span><span class="sxs-lookup"><span data-stu-id="7bbe7-117">Bindings inside the context of the `foreach` refer to properties on the array item.</span></span> <span data-ttu-id="7bbe7-118">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="7bbe7-118">For example:</span></span>

[!code-html[Main](part-7/samples/sample4.html)]

<span data-ttu-id="7bbe7-119">In questo caso il `text` associazione legge la proprietà dell'autore di ogni libro.</span><span class="sxs-lookup"><span data-stu-id="7bbe7-119">Here the `text` binding reads the Author property of each book.</span></span>

<span data-ttu-id="7bbe7-120">Se si esegue l'applicazione a questo punto, dovrebbe essere simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="7bbe7-120">If you run the application now, it should look like this:</span></span>

![](part-7/_static/image1.png)

<span data-ttu-id="7bbe7-121">L'elenco di libri carica in modo asincrono, dopo il caricamento della pagina.</span><span class="sxs-lookup"><span data-stu-id="7bbe7-121">The list of books loads asynchronously, after the page loads.</span></span> <span data-ttu-id="7bbe7-122">Ora, il &quot;dettagli&quot; collegamenti non sono attivi.</span><span class="sxs-lookup"><span data-stu-id="7bbe7-122">Right now, the &quot;Details&quot; links are not functional.</span></span> <span data-ttu-id="7bbe7-123">Aggiungiamo questa funzionalità nella sezione successiva.</span><span class="sxs-lookup"><span data-stu-id="7bbe7-123">We'll add this functionality in the next section.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="7bbe7-124">[Precedente](part-6.md)
> [Successivo](part-8.md)</span><span class="sxs-lookup"><span data-stu-id="7bbe7-124">[Previous](part-6.md)
[Next](part-8.md)</span></span>
