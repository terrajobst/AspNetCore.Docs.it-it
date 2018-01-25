---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
title: 'Parte 7: Creazione principale pagina | Documenti Microsoft'
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: eb32a17b-626c-4373-9a7d-3387992f3c04
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
msc.type: authoredcontent
ms.openlocfilehash: 4d06e72bc664f707bbbe4603be41347158c58903
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/24/2018
---
<a name="part-7-creating-the-main-page"></a><span data-ttu-id="4cb05-102">Parte 7: Creazione principale pagina</span><span class="sxs-lookup"><span data-stu-id="4cb05-102">Part 7: Creating the Main Page</span></span>
====================
<span data-ttu-id="4cb05-103">da [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="4cb05-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="4cb05-104">Scaricare il progetto completato</span><span class="sxs-lookup"><span data-stu-id="4cb05-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-the-main-page"></a><span data-ttu-id="4cb05-105">Creazione di principale pagina</span><span class="sxs-lookup"><span data-stu-id="4cb05-105">Creating the Main Page</span></span>

<span data-ttu-id="4cb05-106">In questa sezione si creerà la pagina principale dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="4cb05-106">In this section, you will create the main application page.</span></span> <span data-ttu-id="4cb05-107">Questa pagina sarà più complessa rispetto alla pagina di amministrazione, in modo ci sarà approccio in diversi passaggi.</span><span class="sxs-lookup"><span data-stu-id="4cb05-107">This page will be more complex than the Admin page, so we'll approach it in several steps.</span></span> <span data-ttu-id="4cb05-108">Inoltre, si noterà alcune tecniche Knockout.js più avanzate.</span><span class="sxs-lookup"><span data-stu-id="4cb05-108">Along the way, you'll see some more advanced Knockout.js techniques.</span></span> <span data-ttu-id="4cb05-109">Di seguito è riportato il layout di base della pagina:</span><span class="sxs-lookup"><span data-stu-id="4cb05-109">Here is the basic layout of the page:</span></span>

![](using-web-api-with-entity-framework-part-7/_static/image1.png)

- <span data-ttu-id="4cb05-110">"Prodotti" contiene una matrice di prodotti.</span><span class="sxs-lookup"><span data-stu-id="4cb05-110">"Products" holds an array of products.</span></span>
- <span data-ttu-id="4cb05-111">"Carrello" contiene una matrice di prodotti con quantità.</span><span class="sxs-lookup"><span data-stu-id="4cb05-111">"Cart" holds an array of products with quantities.</span></span> <span data-ttu-id="4cb05-112">Fare clic su "Aggiungi al carrello" Aggiorna il carrello.</span><span class="sxs-lookup"><span data-stu-id="4cb05-112">Clicking "Add to Cart" updates the cart.</span></span>
- <span data-ttu-id="4cb05-113">"Orders" contiene una matrice di ID degli ordini.</span><span class="sxs-lookup"><span data-stu-id="4cb05-113">"Orders" holds an array of order IDs.</span></span>
- <span data-ttu-id="4cb05-114">"Dettagli" contiene un dettaglio di ordine, ovvero una matrice di elementi (prodotti con quantità)</span><span class="sxs-lookup"><span data-stu-id="4cb05-114">"Details" holds an order detail, which is an array of items (products with quantities)</span></span>

<span data-ttu-id="4cb05-115">Si inizierà definendo un layout di base in formato HTML, senza associazione a dati o script.</span><span class="sxs-lookup"><span data-stu-id="4cb05-115">We'll start by defining some basic layout in HTML, with no data binding or script.</span></span> <span data-ttu-id="4cb05-116">Aprire il file Views/Home/Index.cshtml e sostituire tutto il contenuto con quanto segue:</span><span class="sxs-lookup"><span data-stu-id="4cb05-116">Open the file Views/Home/Index.cshtml and replace all of the contents with the following:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample1.html)]

<span data-ttu-id="4cb05-117">Successivamente, aggiungere una sezione di script e creare un modello di visualizzazione vuoto:</span><span class="sxs-lookup"><span data-stu-id="4cb05-117">Next, add a Scripts section and create an empty view-model:</span></span>

[!code-cshtml[Main](using-web-api-with-entity-framework-part-7/samples/sample2.cshtml)]

<span data-ttu-id="4cb05-118">In base alla struttura sketch in precedenza, il modello di visualizzazione deve Observable per i prodotti, carrello, ordini e dettagli.</span><span class="sxs-lookup"><span data-stu-id="4cb05-118">Based on the design sketched earlier, our view model needs observables for products, cart, orders, and details.</span></span> <span data-ttu-id="4cb05-119">Aggiungere le seguenti variabili per il `AppViewModel` oggetto:</span><span class="sxs-lookup"><span data-stu-id="4cb05-119">Add the following variables to the `AppViewModel` object:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample3.js)]

<span data-ttu-id="4cb05-120">Gli utenti possono aggiungere elementi nell'elenco di prodotti nel carrello e rimuovere elementi dal carrello.</span><span class="sxs-lookup"><span data-stu-id="4cb05-120">Users can add items from the products list into the cart, and remove items from the cart.</span></span> <span data-ttu-id="4cb05-121">Per incapsulare queste funzioni, si creerà un'altra classe di modello di visualizzazione che rappresenta un prodotto.</span><span class="sxs-lookup"><span data-stu-id="4cb05-121">To encapsulate these functions, we'll create another view-model class that represents a product.</span></span> <span data-ttu-id="4cb05-122">Aggiungi il seguente codice a `AppViewModel`.</span><span class="sxs-lookup"><span data-stu-id="4cb05-122">Add the following code to `AppViewModel`:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample4.js?highlight=4)]

<span data-ttu-id="4cb05-123">Il `ProductViewModel` classe contiene due funzioni che consentono di spostare il prodotto da e verso il carrello: `addItemToCart` aggiunge un'unità del prodotto al carrello, e `removeAllFromCart` rimuove tutte le quantità del prodotto.</span><span class="sxs-lookup"><span data-stu-id="4cb05-123">The `ProductViewModel` class contains two functions that are used to move the product to and from the cart: `addItemToCart` adds one unit of the product to the cart, and `removeAllFromCart` removes all quantities of the product.</span></span>

<span data-ttu-id="4cb05-124">Gli utenti possono selezionare un ordine esistente e ottenere i dettagli dell'ordine.</span><span class="sxs-lookup"><span data-stu-id="4cb05-124">Users can select an existing order and get the order details.</span></span> <span data-ttu-id="4cb05-125">È possibile incapsulare questa funzionalità in un altro modello di visualizzazione:</span><span class="sxs-lookup"><span data-stu-id="4cb05-125">We'll encapsulate this functionality into another view-model:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample5.js?highlight=4)]

<span data-ttu-id="4cb05-126">Il `OrderDetailsViewModel` viene inizializzata con un ordine, e vengono recuperati i dettagli dell'ordine inviando una richiesta AJAX al server.</span><span class="sxs-lookup"><span data-stu-id="4cb05-126">The `OrderDetailsViewModel` is initialized with an order, and it fetches the order details by sending an AJAX request to the server.</span></span>

<span data-ttu-id="4cb05-127">Si noti inoltre il `total` proprietà il `OrderDetailsViewModel`.</span><span class="sxs-lookup"><span data-stu-id="4cb05-127">Also, notice the `total` property on the `OrderDetailsViewModel`.</span></span> <span data-ttu-id="4cb05-128">Questa proprietà è un tipo speciale di observable chiamato un [calcolata observable](http://knockoutjs.com/documentation/computedObservables.html).</span><span class="sxs-lookup"><span data-stu-id="4cb05-128">This property is a special kind of observable called a [computed observable](http://knockoutjs.com/documentation/computedObservables.html).</span></span> <span data-ttu-id="4cb05-129">Come suggerisce il nome, un observable calcolata consente di associare dati a un valore calcolato &#8212; in questo caso, il costo totale dell'ordine.</span><span class="sxs-lookup"><span data-stu-id="4cb05-129">As the name implies, a computed observable lets you data bind to a computed value&#8212;in this case, the total cost of the order.</span></span>

<span data-ttu-id="4cb05-130">Successivamente, aggiungere queste funzioni `AppViewModel`:</span><span class="sxs-lookup"><span data-stu-id="4cb05-130">Next, add these functions to `AppViewModel`:</span></span>

- <span data-ttu-id="4cb05-131">`resetCart`Rimuove tutti gli elementi dal carrello.</span><span class="sxs-lookup"><span data-stu-id="4cb05-131">`resetCart` removes all items from the cart.</span></span>
- <span data-ttu-id="4cb05-132">`getDetails`Ottiene i dettagli per un ordine (da pGrazie a un nuovo `OrderDetailsViewModel` sul `details` elenco).</span><span class="sxs-lookup"><span data-stu-id="4cb05-132">`getDetails` gets the details for an order (by pusing a new `OrderDetailsViewModel` onto the `details` list).</span></span>
- <span data-ttu-id="4cb05-133">`createOrder`Crea un nuovo ordine e svuota il carrello.</span><span class="sxs-lookup"><span data-stu-id="4cb05-133">`createOrder` creates a new order and empties the cart.</span></span>


[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample6.js?highlight=4)]

<span data-ttu-id="4cb05-134">Infine, inizializzare il modello di visualizzazione eseguendo le richieste AJAX per i prodotti e gli ordini:</span><span class="sxs-lookup"><span data-stu-id="4cb05-134">Finally, initialize the view model by making AJAX requests for the products and orders:</span></span>

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample7.js)]

<span data-ttu-id="4cb05-135">OK, che è la quantità di codice, ma è stato creato il backup dettagliate, ci auguriamo che la progettazione è crittografata.</span><span class="sxs-lookup"><span data-stu-id="4cb05-135">OK, that's a lot of code, but we built it up step-by-step, so hopefully the design is clear.</span></span> <span data-ttu-id="4cb05-136">È ora possibile aggiungere alcune associazioni Knockout.js al formato HTML.</span><span class="sxs-lookup"><span data-stu-id="4cb05-136">Now we can add some Knockout.js bindings to the HTML.</span></span>

<span data-ttu-id="4cb05-137">**Prodotti**</span><span class="sxs-lookup"><span data-stu-id="4cb05-137">**Products**</span></span>

<span data-ttu-id="4cb05-138">Ecco i binding per l'elenco dei prodotti:</span><span class="sxs-lookup"><span data-stu-id="4cb05-138">Here are the bindings for the product list:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample8.html)]

<span data-ttu-id="4cb05-139">Si scorre la matrice di prodotti e visualizza il nome e il prezzo.</span><span class="sxs-lookup"><span data-stu-id="4cb05-139">This iterates over the products array and displays the name and price.</span></span> <span data-ttu-id="4cb05-140">Il pulsante "Aggiungi e Order" è visibile solo quando l'utente è connesso.</span><span class="sxs-lookup"><span data-stu-id="4cb05-140">The "Add to Order" button is visible only when the user is logged in.</span></span>

<span data-ttu-id="4cb05-141">Le chiamate del pulsante "Aggiungi a ordine" `addItemToCart` sul `ProductViewModel` istanza per il prodotto.</span><span class="sxs-lookup"><span data-stu-id="4cb05-141">The "Add to Order" button calls `addItemToCart` on the `ProductViewModel` instance for the product.</span></span> <span data-ttu-id="4cb05-142">Ciò dimostra una funzionalità interessante di Knockout.js: quando un modello di visualizzazione contiene altri modelli di visualizzazione, è possibile applicare le associazioni a modello interno.</span><span class="sxs-lookup"><span data-stu-id="4cb05-142">This demonstrates a nice feature of Knockout.js: When a view-model contains other view-models, you can apply the bindings to the inner model.</span></span> <span data-ttu-id="4cb05-143">In questo esempio, le associazioni all'interno di `foreach` vengono applicati a ogni il `ProductViewModel` istanze.</span><span class="sxs-lookup"><span data-stu-id="4cb05-143">In this example, the bindings within the `foreach` are applied to each of the `ProductViewModel` instances.</span></span> <span data-ttu-id="4cb05-144">Questo approccio è molto più chiara di tutte le funzionalità di inserimento in un singolo modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="4cb05-144">This approach is much cleaner than putting all of the functionality into a single view-model.</span></span>

<span data-ttu-id="4cb05-145">**Carrello**</span><span class="sxs-lookup"><span data-stu-id="4cb05-145">**Cart**</span></span>

<span data-ttu-id="4cb05-146">Ecco le associazioni per il carrello:</span><span class="sxs-lookup"><span data-stu-id="4cb05-146">Here are the bindings for the cart:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample9.html)]

<span data-ttu-id="4cb05-147">Questo si scorre la matrice del carrello e visualizza il nome, prezzo e quantità.</span><span class="sxs-lookup"><span data-stu-id="4cb05-147">This iterates over the cart array and displays the name, price, and quantity.</span></span> <span data-ttu-id="4cb05-148">Si noti che il collegamento "Rimuovi" e il pulsante "Creazione dell'ordine" sono associati a funzioni di modello di visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="4cb05-148">Note that the "Remove" link and the "Create Order" button are bound to view-model functions.</span></span>

<span data-ttu-id="4cb05-149">**Ordini**</span><span class="sxs-lookup"><span data-stu-id="4cb05-149">**Orders**</span></span>

<span data-ttu-id="4cb05-150">Ecco i binding per l'elenco di ordini:</span><span class="sxs-lookup"><span data-stu-id="4cb05-150">Here are the bindings for the orders list:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample10.html)]

<span data-ttu-id="4cb05-151">Questo scorre gli ordini e Mostra l'ID ordine.</span><span class="sxs-lookup"><span data-stu-id="4cb05-151">This iterates over the orders and shows the order ID.</span></span> <span data-ttu-id="4cb05-152">L'evento fare clic sul collegamento è associato il `getDetails` (funzione).</span><span class="sxs-lookup"><span data-stu-id="4cb05-152">The click event on the link is bound to the `getDetails` function.</span></span>

<span data-ttu-id="4cb05-153">**Dettagli dell'ordine**</span><span class="sxs-lookup"><span data-stu-id="4cb05-153">**Order Details**</span></span>

<span data-ttu-id="4cb05-154">Ecco le associazioni per i dettagli dell'ordine:</span><span class="sxs-lookup"><span data-stu-id="4cb05-154">Here are the bindings for the order details:</span></span>

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample11.html)]

<span data-ttu-id="4cb05-155">Questo scorre gli elementi nell'ordine e consente di visualizzare il prodotto, prezzo e quantità.</span><span class="sxs-lookup"><span data-stu-id="4cb05-155">This iterates over the items in the order and displays the product, price, and quanity.</span></span> <span data-ttu-id="4cb05-156">Tag div circostante è visibile solo se la matrice di dettagli contiene uno o più elementi.</span><span class="sxs-lookup"><span data-stu-id="4cb05-156">The surrounding div is visible only if the details array contains one or more items.</span></span>

## <a name="conclusion"></a><span data-ttu-id="4cb05-157">Conclusione</span><span class="sxs-lookup"><span data-stu-id="4cb05-157">Conclusion</span></span>

<span data-ttu-id="4cb05-158">In questa esercitazione è creata un'applicazione che usa Entity Framework per comunicare con il database e l'API Web ASP.NET per fornire un'interfaccia pubblico nel primo livello di dati.</span><span class="sxs-lookup"><span data-stu-id="4cb05-158">In this tutorial, you created an application that uses Entity Framework to communicate with the database, and ASP.NET Web API to provide a public-facing interface on top of the data layer.</span></span> <span data-ttu-id="4cb05-159">Utilizziamo ASP.NET MVC 4 per il rendering delle pagine HTML e Knockout.js più jQuery per fornire interazioni dinamiche senza caricamento pagina.</span><span class="sxs-lookup"><span data-stu-id="4cb05-159">We use ASP.NET MVC 4 to render the HTML pages, and Knockout.js plus jQuery to provide dynamic interactions without page reloads.</span></span>

<span data-ttu-id="4cb05-160">Risorse aggiuntive:</span><span class="sxs-lookup"><span data-stu-id="4cb05-160">Additional resources:</span></span>

- [<span data-ttu-id="4cb05-161">Mappa del contenuto di accesso ai dati ASP.NET</span><span class="sxs-lookup"><span data-stu-id="4cb05-161">ASP.NET Data Access Content Map</span></span>](https://msdn.microsoft.com/library/6759sth4.aspx)
- [<span data-ttu-id="4cb05-162">Centro per sviluppatori di Entity Framework</span><span class="sxs-lookup"><span data-stu-id="4cb05-162">Entity Framework Developer Center</span></span>](https://msdn.microsoft.com/data/ef)

>[!div class="step-by-step"]
[<span data-ttu-id="4cb05-163">Precedente</span><span class="sxs-lookup"><span data-stu-id="4cb05-163">Previous</span></span>](using-web-api-with-entity-framework-part-6.md)
