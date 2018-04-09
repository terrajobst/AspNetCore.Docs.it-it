---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
title: "Parte 5: Creazione di un'interfaccia utente dinamica con Knockout.js | Documenti Microsoft"
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 9d9cb3b0-f4a7-434e-a508-9fc0ad0eb813
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
msc.type: authoredcontent
ms.openlocfilehash: b63446d076fbb1143641dead788042967b996bf8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="part-5-creating-a-dynamic-ui-with-knockoutjs"></a><span data-ttu-id="37887-102">Parte 5: Creazione di un'interfaccia utente dinamica con Knockout.js</span><span class="sxs-lookup"><span data-stu-id="37887-102">Part 5: Creating a Dynamic UI with Knockout.js</span></span>
====================
<span data-ttu-id="37887-103">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="37887-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="37887-104">Scaricare il progetto completato</span><span class="sxs-lookup"><span data-stu-id="37887-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-a-dynamic-ui-with-knockoutjs"></a><span data-ttu-id="37887-105">Creazione di un'interfaccia utente dinamica con Knockout.js</span><span class="sxs-lookup"><span data-stu-id="37887-105">Creating a Dynamic UI with Knockout.js</span></span>

<span data-ttu-id="37887-106">In questa sezione, useremo Knockout.js per aggiungere funzionalità per la visualizzazione amministrazione.</span><span class="sxs-lookup"><span data-stu-id="37887-106">In this section, we'll use Knockout.js to add functionality to the Admin view.</span></span>

<span data-ttu-id="37887-107">[Knockout.js](http://knockoutjs.com/) è una libreria Javascript che rende più semplice associare i controlli HTML a dati.</span><span class="sxs-lookup"><span data-stu-id="37887-107">[Knockout.js](http://knockoutjs.com/) is a Javascript library that makes it easy to bind HTML controls to data.</span></span> <span data-ttu-id="37887-108">Knockout.js utilizza il modello Model-View-ViewModel (MVVM).</span><span class="sxs-lookup"><span data-stu-id="37887-108">Knockout.js uses the Model-View-ViewModel (MVVM) pattern.</span></span>

- <span data-ttu-id="37887-109">Il *modello* è la rappresentazione sul lato server dei dati del dominio di business (in questo caso, products e orders).</span><span class="sxs-lookup"><span data-stu-id="37887-109">The *model* is the server-side representation of the data in the business domain (in our case, products and orders).</span></span>
- <span data-ttu-id="37887-110">Il *visualizzazione* è il livello di presentazione (HTML).</span><span class="sxs-lookup"><span data-stu-id="37887-110">The *view* is the presentation layer (HTML).</span></span>
- <span data-ttu-id="37887-111">Il *modello di visualizzazione* è un oggetto Javascript che contiene i dati del modello.</span><span class="sxs-lookup"><span data-stu-id="37887-111">The *view-model* is a Javascript object that holds the model data.</span></span> <span data-ttu-id="37887-112">Il modello di visualizzazione è un'astrazione del codice dell'interfaccia utente.</span><span class="sxs-lookup"><span data-stu-id="37887-112">The view-model is a code abstraction of the UI.</span></span> <span data-ttu-id="37887-113">Ha alcuna conoscenza della rappresentazione HTML.</span><span class="sxs-lookup"><span data-stu-id="37887-113">It has no knowledge of the HTML representation.</span></span> <span data-ttu-id="37887-114">Rappresenta invece astratta funzionalità della visualizzazione, ad esempio "un elenco di elementi".</span><span class="sxs-lookup"><span data-stu-id="37887-114">Instead, it represents abstract features of the view, such as "a list of items".</span></span>

<span data-ttu-id="37887-115">La vista con associazione dati per il modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="37887-115">The view is data-bound to the view-model.</span></span> <span data-ttu-id="37887-116">Gli aggiornamenti al modello di visualizzazione vengono applicati automaticamente nella vista.</span><span class="sxs-lookup"><span data-stu-id="37887-116">Updates to the view-model are automatically reflected in the view.</span></span> <span data-ttu-id="37887-117">Il modello di visualizzazione, inoltre, ottiene gli eventi dalla visualizzazione, ad esempio pulsanti ed esegue operazioni sul modello, ad esempio la creazione di un ordine.</span><span class="sxs-lookup"><span data-stu-id="37887-117">The view-model also gets events from the view, such as button clicks, and performs operations on the model, such as creating an order.</span></span>

![](using-web-api-with-entity-framework-part-5/_static/image1.png)

<span data-ttu-id="37887-118">Prima di tutto definiamo il modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="37887-118">First we'll define the view-model.</span></span> <span data-ttu-id="37887-119">Successivamente, si verrà associato il markup HTML per il modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="37887-119">After that, we will bind the HTML markup to the view-model.</span></span>

<span data-ttu-id="37887-120">Aggiungere la seguente sezione Razor Admin.cshtml:</span><span class="sxs-lookup"><span data-stu-id="37887-120">Add the following Razor section to Admin.cshtml:</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample1.cshtml)]

<span data-ttu-id="37887-121">È possibile aggiungere un punto qualsiasi in questa sezione nel file.</span><span class="sxs-lookup"><span data-stu-id="37887-121">You can add this section anywhere in the file.</span></span> <span data-ttu-id="37887-122">Quando il rendering, la sezione viene visualizzata nella parte inferiore della pagina HTML destro del mouse prima della chiusura &lt;/body&gt; tag.</span><span class="sxs-lookup"><span data-stu-id="37887-122">When the view is rendered, the section appears at the bottom of the HTML page, right before the closing &lt;/body&gt; tag.</span></span>

<span data-ttu-id="37887-123">Tutti gli script di per questa pagina verrà inviata all'interno del tag di script indicato dal commento:</span><span class="sxs-lookup"><span data-stu-id="37887-123">All of the script for this page will go inside the script tag indicated by the comment:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample2.html)]

<span data-ttu-id="37887-124">Prima di definire una classe di modello di visualizzazione:</span><span class="sxs-lookup"><span data-stu-id="37887-124">First, define a view-model class:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample3.js)]

<span data-ttu-id="37887-125">**ko.observableArray** è un tipo speciale di oggetto in Knockout, chiamato un' *observable*.</span><span class="sxs-lookup"><span data-stu-id="37887-125">**ko.observableArray** is a special kind of object in Knockout, called an *observable*.</span></span> <span data-ttu-id="37887-126">Dal [Knockout.js documentazione](http://knockoutjs.com/documentation/observables.html): observable è un "oggetto JavaScript che può inviare una notifica di sottoscrittori sulle modifiche."</span><span class="sxs-lookup"><span data-stu-id="37887-126">From the [Knockout.js documentation](http://knockoutjs.com/documentation/observables.html): An observable is a "JavaScript object that can notify subscribers about changes."</span></span> <span data-ttu-id="37887-127">Quando il contenuto di un oggetto osservabile cambia, la vista viene aggiornata automaticamente in modo che corrisponda.</span><span class="sxs-lookup"><span data-stu-id="37887-127">When the contents of an observable change, the view is automatically updated to match.</span></span>

<span data-ttu-id="37887-128">Per popolare il `products` matrice, effettuare una richiesta AJAX all'API web.</span><span class="sxs-lookup"><span data-stu-id="37887-128">To populate the `products` array, make an AJAX request to the web API.</span></span> <span data-ttu-id="37887-129">Tenere presente che è archiviato l'URI di base per l'API nel contenitore delle visualizzazioni (vedere [parte 4](using-web-api-with-entity-framework-part-4.md) dell'esercitazione).</span><span class="sxs-lookup"><span data-stu-id="37887-129">Recall that we stored the base URI for the API in the view bag (see [Part 4](using-web-api-with-entity-framework-part-4.md) of the tutorial).</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample4.js?highlight=5)]

<span data-ttu-id="37887-130">Successivamente, aggiungere le funzioni per il modello di visualizzazione per creare, aggiornare ed eliminare i prodotti.</span><span class="sxs-lookup"><span data-stu-id="37887-130">Next, add functions to the view-model to create, update, and delete products.</span></span> <span data-ttu-id="37887-131">Queste funzioni inviare chiamate AJAX all'API web e utilizzano i risultati per aggiornare il modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="37887-131">These functions submit AJAX calls to the web API and use the results to update the view-model.</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample5.js?highlight=7)]

<span data-ttu-id="37887-132">Ora la parte più importante: quando DOM è fulled chiamata caricato, il **ko.applyBindings** funzione e passare una nuova istanza del `ProductsViewModel`:</span><span class="sxs-lookup"><span data-stu-id="37887-132">Now the most important part: When the DOM is fulled loaded, call the **ko.applyBindings** function and pass in a new instance of the `ProductsViewModel`:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample6.js)]

<span data-ttu-id="37887-133">Il **ko.applyBindings** metodo attiva Knockout e collega il modello di visualizzazione per la visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="37887-133">The **ko.applyBindings** method activates Knockout and wires up the view-model to the view.</span></span>

<span data-ttu-id="37887-134">Ora che è disponibile un modello di visualizzazione, è possibile creare le associazioni.</span><span class="sxs-lookup"><span data-stu-id="37887-134">Now that we have a view-model, we can create the bindings.</span></span> <span data-ttu-id="37887-135">In Knockout.js, questo scopo, aggiungere `data-bind` attributi agli elementi HTML.</span><span class="sxs-lookup"><span data-stu-id="37887-135">In Knockout.js, you do this by adding `data-bind` attributes to HTML elements.</span></span> <span data-ttu-id="37887-136">Ad esempio, per associare un elenco HTML in una matrice, utilizzare il `foreach` associazione:</span><span class="sxs-lookup"><span data-stu-id="37887-136">For example, to bind an HTML list to an array, use the `foreach` binding:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample7.html?highlight=1)]

<span data-ttu-id="37887-137">Il `foreach` associazione, scorre la matrice e crea figlio di elementi per ogni oggetto nella matrice.</span><span class="sxs-lookup"><span data-stu-id="37887-137">The `foreach` binding iterates through the array and creates child elements for each object in the array.</span></span> <span data-ttu-id="37887-138">Le associazioni sugli elementi figlio possono fare riferimento alle proprietà di oggetti di matrice.</span><span class="sxs-lookup"><span data-stu-id="37887-138">Bindings on the child elements can refer to properties on the array objects.</span></span>

<span data-ttu-id="37887-139">Aggiungere associazioni seguenti all'elenco "-i prodotti di aggiornamento":</span><span class="sxs-lookup"><span data-stu-id="37887-139">Add the following bindings to the "update-products" list:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample8.html)]

<span data-ttu-id="37887-140">Il `<li>` elemento presente nell'ambito del **foreach** associazione.</span><span class="sxs-lookup"><span data-stu-id="37887-140">The `<li>` element occurs within the scope of the **foreach** binding.</span></span> <span data-ttu-id="37887-141">Che verrà Knockout indica il rendering dell'elemento di una volta per ogni prodotto nel `products` matrice.</span><span class="sxs-lookup"><span data-stu-id="37887-141">That means Knockout will render the element once for each product in the `products` array.</span></span> <span data-ttu-id="37887-142">Tutte le associazioni all'interno di `<li>` elemento fare riferimento a tale istanza di un prodotto.</span><span class="sxs-lookup"><span data-stu-id="37887-142">All of the bindings within the `<li>` element refer to that product instance.</span></span> <span data-ttu-id="37887-143">Ad esempio, `$data.Name` si intende la `Name` proprietà del prodotto.</span><span class="sxs-lookup"><span data-stu-id="37887-143">For example, `$data.Name` refers to the `Name` property on the product.</span></span>

<span data-ttu-id="37887-144">Per impostare i valori di input di testo, utilizzare il `value` associazione.</span><span class="sxs-lookup"><span data-stu-id="37887-144">To set the values of the text inputs, use the `value` binding.</span></span> <span data-ttu-id="37887-145">I pulsanti sono associati a funzioni-vista del modello utilizzando il `click` associazione.</span><span class="sxs-lookup"><span data-stu-id="37887-145">The buttons are bound to functions on the model-view, using the `click` binding.</span></span> <span data-ttu-id="37887-146">L'istanza di un prodotto viene passato come parametro per ogni funzione.</span><span class="sxs-lookup"><span data-stu-id="37887-146">The product instance is passed as a parameter to each function.</span></span> <span data-ttu-id="37887-147">Per ulteriori informazioni, il [Knockout.js documentazione](http://knockoutjs.com/documentation/observables.html) ha buona descrizioni delle associazioni diverse.</span><span class="sxs-lookup"><span data-stu-id="37887-147">For more information, the [Knockout.js documentation](http://knockoutjs.com/documentation/observables.html) has good descriptions of the various bindings.</span></span>

<span data-ttu-id="37887-148">Successivamente, aggiungere un'associazione per il **inviare** eventi del modulo aggiungere prodotto:</span><span class="sxs-lookup"><span data-stu-id="37887-148">Next, add a binding for the **submit** event on the Add Product form:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample9.html)]

<span data-ttu-id="37887-149">Questa associazione chiama il `create` funzione nel modello di visualizzazione per creare un nuovo prodotto.</span><span class="sxs-lookup"><span data-stu-id="37887-149">This binding calls the `create` function on the view-model to create a new product.</span></span>

<span data-ttu-id="37887-150">Ecco il codice completo per la visualizzazione di amministrazione:</span><span class="sxs-lookup"><span data-stu-id="37887-150">Here is the complete code for the Admin view:</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample10.cshtml)]

<span data-ttu-id="37887-151">Eseguire l'applicazione, accedere con l'account amministratore e fare clic sul collegamento "Admin".</span><span class="sxs-lookup"><span data-stu-id="37887-151">Run the application, log in with the Administrator account, and click the "Admin" link.</span></span> <span data-ttu-id="37887-152">Vedere l'elenco dei prodotti e in grado di creare, aggiornare o eliminare i prodotti.</span><span class="sxs-lookup"><span data-stu-id="37887-152">You should see the list of products, and be able to create, update, or delete products.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="37887-153">[Precedente](using-web-api-with-entity-framework-part-4.md)
> [Successivo](using-web-api-with-entity-framework-part-6.md)</span><span class="sxs-lookup"><span data-stu-id="37887-153">[Previous](using-web-api-with-entity-framework-part-4.md)
[Next](using-web-api-with-entity-framework-part-6.md)</span></span>
