---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
title: 'Parte 4: Elenco di prodotti | Documenti Microsoft'
author: JoeStagner
description: Questa serie di esercitazioni in dettaglio tutti i passaggi necessari per compilare l'applicazione di esempio Tailspin Spyworks. Parte 4 viene illustrato l'elenco di prodotti con Contr GridView....
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 4fab47d5-a6ec-4fdc-91f0-651a093a24b9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
msc.type: authoredcontent
ms.openlocfilehash: 69b26344e6dcdbf27e94da90ad5d6cd79f27ccd3
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
ms.locfileid: "30881064"
---
<a name="part-4-listing-products"></a><span data-ttu-id="013b3-104">Parte 4: Elenco di prodotti</span><span class="sxs-lookup"><span data-stu-id="013b3-104">Part 4: Listing Products</span></span>
====================
<span data-ttu-id="013b3-105">da [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="013b3-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="013b3-106">Tailspin Spyworks viene illustrato come particolarmente semplice è creare potenti applicazioni scalabili per la piattaforma .NET.</span><span class="sxs-lookup"><span data-stu-id="013b3-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="013b3-107">Illustra come usare le nuove caratteristiche in ASP.NET 4 per creare un archivio online, inclusi gli acquisti, estrazione e l'amministrazione.</span><span class="sxs-lookup"><span data-stu-id="013b3-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="013b3-108">Questa serie di esercitazioni in dettaglio tutti i passaggi necessari per compilare l'applicazione di esempio Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="013b3-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="013b3-109">Parte 4 viene illustrata l'elenco di prodotti con il controllo GridView.</span><span class="sxs-lookup"><span data-stu-id="013b3-109">Part 4 covers listing products with the GridView control.</span></span>


## <a id="_Toc260221670"></a>  <span data-ttu-id="013b3-110">Elenco di prodotti con il controllo GridView</span><span class="sxs-lookup"><span data-stu-id="013b3-110">Listing Products with the GridView Control</span></span>

<span data-ttu-id="013b3-111">Esaminiamo implementazione page ProductsList.aspx "Facendo clic" nella soluzione, selezionare "Aggiungi" e "New Item".</span><span class="sxs-lookup"><span data-stu-id="013b3-111">Let's begin implementing our ProductsList.aspx page by "Right Clicking" on our solution and selecting "Add" and "New Item".</span></span>

![](tailspin-spyworks-part-4/_static/image1.jpg)

<span data-ttu-id="013b3-112">Scegliere "Web Form mediante pagina Master" e immettere il nome di una pagina di ProductsList.aspx".</span><span class="sxs-lookup"><span data-stu-id="013b3-112">Choose "Web Form Using Master Page" and enter a page name of ProductsList.aspx".</span></span>

<span data-ttu-id="013b3-113">Fare clic su "Aggiungi".</span><span class="sxs-lookup"><span data-stu-id="013b3-113">Click "Add".</span></span>

![](tailspin-spyworks-part-4/_static/image2.jpg)

<span data-ttu-id="013b3-114">Successivamente, scegliere la cartella "Stili" in cui è stato inserito nella pagina Site. master e selezionarla nella finestra "Il contenuto della cartella".</span><span class="sxs-lookup"><span data-stu-id="013b3-114">Next choose the "Styles" folder where we placed the Site.Master page and select it from the "Contents of folder" window.</span></span>

![](tailspin-spyworks-part-4/_static/image3.jpg)

<span data-ttu-id="013b3-115">Fare clic su "Ok" per creare la pagina.</span><span class="sxs-lookup"><span data-stu-id="013b3-115">Click "Ok" to create the page.</span></span>

<span data-ttu-id="013b3-116">Il database viene popolato con i dati di prodotto come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="013b3-116">Our database is populated with product data as seen below.</span></span>

![](tailspin-spyworks-part-4/_static/image4.jpg)

<span data-ttu-id="013b3-117">Dopo aver creata la pagina nuovo verrà utilizzata un'origine dati di entità per accedere ai dati di prodotto, ma in questa istanza è necessario selezionare le entità del prodotto ed è necessario limitare gli elementi che vengono restituiti solo a quelli per la categoria selezionata.</span><span class="sxs-lookup"><span data-stu-id="013b3-117">After our page is created we'll again use an Entity Data Source to access that product data, but in this instance we need to select the Product Entities and we need to restrict the items that are returned to only those for the selected Category.</span></span>

<span data-ttu-id="013b3-118">A tale scopo c'EntityDataSource per generare automaticamente la clausola WHERE e si specificano il WhereParameter.</span><span class="sxs-lookup"><span data-stu-id="013b3-118">To accomplish this we'll tell the EntityDataSource to Auto Generate the WHERE clause and we'll specify the WhereParameter.</span></span>

<span data-ttu-id="013b3-119">Si ricordi che quando abbiamo creato le voci di Menu nel nostro "Menu categoria prodotto" sono compilate in modo dinamico il collegamento aggiungendo il CatagoryID per il parametro QueryString per ciascun collegamento.</span><span class="sxs-lookup"><span data-stu-id="013b3-119">You'll recall that when we created the Menu Items in our "Product Category Menu" we dynamically built the link by adding the CatagoryID to the QueryString for each link.</span></span> <span data-ttu-id="013b3-120">Microsoft comunicherà l'origine dati di entità da cui derivare il parametro in cui tale parametro di stringa di query.</span><span class="sxs-lookup"><span data-stu-id="013b3-120">We will tell the Entity Data Source to derive the WHERE parameter from that QueryString parameter.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample1.aspx)]

<span data-ttu-id="013b3-121">Successivamente, è possibile configurare il controllo ListView per visualizzare un elenco di prodotti.</span><span class="sxs-lookup"><span data-stu-id="013b3-121">Next, we'll configure the ListView control to display a list of products.</span></span> <span data-ttu-id="013b3-122">Per creare un'esperienza ottimale di acquisto che è sarà compattare diverse funzionalità concisa ogni singolo prodotto visualizzato nel nostro ListVew.</span><span class="sxs-lookup"><span data-stu-id="013b3-122">To create an optimal shopping experience we'll compact several concise features into each individual product displayed in our ListVew.</span></span>

- <span data-ttu-id="013b3-123">Il nome del prodotto sarà un collegamento alla visualizzazione dettagli del prodotto.</span><span class="sxs-lookup"><span data-stu-id="013b3-123">The product name will be a link to the product's detail view.</span></span>
- <span data-ttu-id="013b3-124">Verrà visualizzato il prezzo del prodotto.</span><span class="sxs-lookup"><span data-stu-id="013b3-124">The product's price will be displayed.</span></span>
- <span data-ttu-id="013b3-125">Verrà visualizzata un'immagine del prodotto e si selezionerà in modo dinamico l'immagine da una directory di catalogo immagini nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="013b3-125">An image of the product will be displayed and we'll dynamically select the image from a catalog images directory in our application.</span></span>
- <span data-ttu-id="013b3-126">È incluso un collegamento per aggiungere subito il prodotto specifico al carrello.</span><span class="sxs-lookup"><span data-stu-id="013b3-126">We will include a link to immediately add the specific product to the shopping cart.</span></span>

<span data-ttu-id="013b3-127">Di seguito è riportato il markup per l'istanza del controllo ListView.</span><span class="sxs-lookup"><span data-stu-id="013b3-127">Here is the markup for our ListView control instance.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample2.aspx)]

<span data-ttu-id="013b3-128">Stiamo creando dinamicamente diversi collegamenti per ogni prodotto visualizzato.</span><span class="sxs-lookup"><span data-stu-id="013b3-128">We are dynamically building several links for each displayed product.</span></span>

<span data-ttu-id="013b3-129">Inoltre, prima che sia testare una nuova pagina è necessario creare la struttura di directory per il prodotto immagini catalogo come indicato di seguito.</span><span class="sxs-lookup"><span data-stu-id="013b3-129">Also, before we test own new page we need to create the directory structure for the product catalog images as follows.</span></span>

![](tailspin-spyworks-part-4/_static/image1.png)

<span data-ttu-id="013b3-130">Una volta le immagini di questo prodotto sono accessibili sia possibile testare la pagina di elenco del prodotto.</span><span class="sxs-lookup"><span data-stu-id="013b3-130">Once our product images are accessible we can test our product list page.</span></span>

![](tailspin-spyworks-part-4/_static/image5.jpg)

<span data-ttu-id="013b3-131">Dalla home page del sito, fare clic su uno dei collegamenti elenco categoria.</span><span class="sxs-lookup"><span data-stu-id="013b3-131">From the site's home page, click on one of the Category List Links.</span></span>

![](tailspin-spyworks-part-4/_static/image6.jpg)

<span data-ttu-id="013b3-132">È necessario implementare la pagina ProductDetials.apsx e la funzionalità AddToCart.</span><span class="sxs-lookup"><span data-stu-id="013b3-132">Now we need to implement the ProductDetials.apsx page and the AddToCart functionality.</span></span>

<span data-ttu-id="013b3-133">Utilizzare File -&gt;nuovo per creare un nome di pagina ProductDetails.aspx tramite il sito pagina Master, come è stato fatto in precedenza.</span><span class="sxs-lookup"><span data-stu-id="013b3-133">Use File-&gt;New to create a page name ProductDetails.aspx using the site Master Page as we did previously.</span></span>

<span data-ttu-id="013b3-134">Si utilizzerà nuovamente un controllo EntityDataSource per accedere al record di prodotto specifico nel database e si utilizzerà un controllo FormView ASP.NET per visualizzare i dati di prodotto, come indicato di seguito.</span><span class="sxs-lookup"><span data-stu-id="013b3-134">We will again use an EntityDataSource control to access the specific product record in the database and we will use an ASP.NET FormView control to display the product data as follows.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample3.aspx)]

<span data-ttu-id="013b3-135">Non occorre preoccuparsi se la formattazione è un po' divertente all'utente.</span><span class="sxs-lookup"><span data-stu-id="013b3-135">Don't worry if the formatting looks a bit funny to you.</span></span> <span data-ttu-id="013b3-136">Il markup precedente lascia spazio nel layout di visualizzazione per un paio di funzionalità che verrà implementato in un secondo momento.</span><span class="sxs-lookup"><span data-stu-id="013b3-136">The markup above leaves room in the display layout for a couple of features we'll implement later on.</span></span>

<span data-ttu-id="013b3-137">Shopping Cart rappresenterà la logica più complessa nell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="013b3-137">The Shopping Cart will represent the more complex logic in our application.</span></span> <span data-ttu-id="013b3-138">Per iniziare a utilizzare File -&gt;nuovo per creare una pagina denominata MyShoppingCart.aspx.</span><span class="sxs-lookup"><span data-stu-id="013b3-138">To get started, use File-&gt;New to create a page called MyShoppingCart.aspx.</span></span>

<span data-ttu-id="013b3-139">Si noti che non si sta scelta ShoppingCart.aspx il nome.</span><span class="sxs-lookup"><span data-stu-id="013b3-139">Note that we are not choosing the name ShoppingCart.aspx.</span></span>

<span data-ttu-id="013b3-140">Il database contiene una tabella denominata "ShoppingCart".</span><span class="sxs-lookup"><span data-stu-id="013b3-140">Our database contains a table named "ShoppingCart".</span></span> <span data-ttu-id="013b3-141">Quando viene generato un Entity Data Model è stata creata una classe per ogni tabella nel database.</span><span class="sxs-lookup"><span data-stu-id="013b3-141">When we generated an Entity Data Model a class was created for each table in the database.</span></span> <span data-ttu-id="013b3-142">Pertanto, Entity Data Model generato una classe di entità denominata "ShoppingCart".</span><span class="sxs-lookup"><span data-stu-id="013b3-142">Therefore, the Entity Data Model generated an Entity Class named "ShoppingCart".</span></span> <span data-ttu-id="013b3-143">È possibile modificare il modello in modo che è possibile utilizzare il nome per l'implementazione di carrello acquisti o estenderlo per le esigenze specifiche, ma si è scelto invece a semplicemente selezionare un nome che verrà evitare il conflitto.</span><span class="sxs-lookup"><span data-stu-id="013b3-143">We could edit the model so that we could use that name for our shopping cart implementation or extend it for our needs, but we will opt instead to simply slect a name that will avoid the conflict.</span></span>

<span data-ttu-id="013b3-144">È inoltre importante notare che si creeranno un carrello semplice e incorporare la logica di carrello degli acquisti con la visualizzazione del carrello acquisti.</span><span class="sxs-lookup"><span data-stu-id="013b3-144">It's also worth noting that we will be creating a simple shopping cart and embedding the shopping cart logic with the shopping cart display.</span></span> <span data-ttu-id="013b3-145">È possibile anche scegliere di implementare nostro carrello acquisti in un livello di Business completamente separate.</span><span class="sxs-lookup"><span data-stu-id="013b3-145">We might also choose to implement our shopping cart in a completely separate Business Layer.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="013b3-146">[Precedente](tailspin-spyworks-part-3.md)
> [Successivo](tailspin-spyworks-part-5.md)</span><span class="sxs-lookup"><span data-stu-id="013b3-146">[Previous](tailspin-spyworks-part-3.md)
[Next](tailspin-spyworks-part-5.md)</span></span>
