---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-7
title: Creare la vista (UI) | Documenti Microsoft
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: b2445062-a1fe-4133-8994-f510280f6d9a
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-7
msc.type: authoredcontent
ms.openlocfilehash: 8c5cc662e2e3c9cb07ca9e30ff57eb084d58e1bb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
---
<a name="create-the-view-ui"></a><span data-ttu-id="1713c-102">Creare la vista (UI)</span><span class="sxs-lookup"><span data-stu-id="1713c-102">Create the View (UI)</span></span>
====================
<span data-ttu-id="1713c-103">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="1713c-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="1713c-104">Scaricare il progetto completato</span><span class="sxs-lookup"><span data-stu-id="1713c-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="1713c-105">In questa sezione, si inizierà a definire il codice HTML per l'app e aggiungere un'associazione dati tra il codice HTML e il modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="1713c-105">In this section, you will start to define the HTML for the app, and add data binding between the HTML and the view model.</span></span>

<span data-ttu-id="1713c-106">Aprire il file Views/Home/Index.cshtml.</span><span class="sxs-lookup"><span data-stu-id="1713c-106">Open the file Views/Home/Index.cshtml.</span></span> <span data-ttu-id="1713c-107">Sostituire tutto il contenuto del file con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="1713c-107">Replace the entire contents of that file with the following.</span></span>

[!code-cshtml[Main](part-7/samples/sample1.cshtml)]

<span data-ttu-id="1713c-108">La maggior parte del `div` elementi esistono per [Bootstrap](http://getbootstrap.com/) stile.</span><span class="sxs-lookup"><span data-stu-id="1713c-108">Most of the `div` elements are there for [Bootstrap](http://getbootstrap.com/) styling.</span></span> <span data-ttu-id="1713c-109">Gli elementi importanti sono quelli con `data-bind` attributi.</span><span class="sxs-lookup"><span data-stu-id="1713c-109">The important elements are the ones with `data-bind` attributes.</span></span> <span data-ttu-id="1713c-110">Questo attributo collega il codice HTML per il modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="1713c-110">This attribute links the HTML to the view model.</span></span>

<span data-ttu-id="1713c-111">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="1713c-111">For example:</span></span>

[!code-html[Main](part-7/samples/sample2.html)]

<span data-ttu-id="1713c-112">In questo esempio, il &quot; `text` &quot; associazione cause il `<p>` elemento per visualizzare il valore di `error` proprietà dal modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="1713c-112">In this example, the &quot;`text`&quot; binding causes the `<p>` element to show the value of the `error` property from the view model.</span></span> <span data-ttu-id="1713c-113">Tenere presente che `error` è stato dichiarato come un `ko.observable`:</span><span class="sxs-lookup"><span data-stu-id="1713c-113">Recall that `error` was declared as a `ko.observable`:</span></span>

[!code-javascript[Main](part-7/samples/sample3.js)]

<span data-ttu-id="1713c-114">Ogni volta che viene assegnato un nuovo valore per `error`, Knockout aggiorna il testo di `<p>` elemento.</span><span class="sxs-lookup"><span data-stu-id="1713c-114">Whenever a new value is assigned to `error`, Knockout updates the text in the `<p>` element.</span></span>

<span data-ttu-id="1713c-115">Il `foreach` associazione indica Knockout per scorrere il contenuto del `books` matrice.</span><span class="sxs-lookup"><span data-stu-id="1713c-115">The `foreach` binding tells Knockout to loop through the contents of the `books` array.</span></span> <span data-ttu-id="1713c-116">Per ogni elemento nella matrice, crea un nuovo Knockout &lt;li&gt; elemento.</span><span class="sxs-lookup"><span data-stu-id="1713c-116">For each item in the array, Knockout creates a new &lt;li&gt; element.</span></span> <span data-ttu-id="1713c-117">Le associazioni all'interno del contesto del `foreach` fare riferimento alle proprietà dell'elemento di matrice.</span><span class="sxs-lookup"><span data-stu-id="1713c-117">Bindings inside the context of the `foreach` refer to properties on the array item.</span></span> <span data-ttu-id="1713c-118">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="1713c-118">For example:</span></span>

[!code-html[Main](part-7/samples/sample4.html)]

<span data-ttu-id="1713c-119">In questo caso il `text` associazione legge la proprietà autore di ogni libro.</span><span class="sxs-lookup"><span data-stu-id="1713c-119">Here the `text` binding reads the Author property of each book.</span></span>

<span data-ttu-id="1713c-120">Se si esegue ora l'applicazione, dovrebbe essere simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="1713c-120">If you run the application now, it should look like this:</span></span>

![](part-7/_static/image1.png)

<span data-ttu-id="1713c-121">L'elenco di libri carica in modo asincrono, dopo il caricamento della pagina.</span><span class="sxs-lookup"><span data-stu-id="1713c-121">The list of books loads asynchronously, after the page loads.</span></span> <span data-ttu-id="1713c-122">Ora, il &quot;dettagli&quot; collegamenti non sono funzionali.</span><span class="sxs-lookup"><span data-stu-id="1713c-122">Right now, the &quot;Details&quot; links are not functional.</span></span> <span data-ttu-id="1713c-123">Questa funzionalità verrà aggiunto nella sezione successiva.</span><span class="sxs-lookup"><span data-stu-id="1713c-123">We'll add this functionality in the next section.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="1713c-124">[Precedente](part-6.md)
[Successivo](part-8.md)</span><span class="sxs-lookup"><span data-stu-id="1713c-124">[Previous](part-6.md)
[Next](part-8.md)</span></span>
