---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
title: 'Parte 4: Elenco di prodotti | Microsoft Docs'
author: JoeStagner
description: Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio Tailspin Spyworks. Parte 4 illustra l'elenco di prodotti con GridView Contr....
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 4fab47d5-a6ec-4fdc-91f0-651a093a24b9
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
msc.type: authoredcontent
ms.openlocfilehash: 929d88d747c25ca7c4f6f991421cb3aa9456aa45
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368809"
---
<a name="part-4-listing-products"></a><span data-ttu-id="9bd3b-104">Parte 4: Elenco di prodotti</span><span class="sxs-lookup"><span data-stu-id="9bd3b-104">Part 4: Listing Products</span></span>
====================
<span data-ttu-id="9bd3b-105">da [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="9bd3b-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="9bd3b-106">Tailspin Spyworks viene illustrato come saliente è davvero semplice per creare applicazioni potenti e scalabili per la piattaforma .NET.</span><span class="sxs-lookup"><span data-stu-id="9bd3b-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="9bd3b-107">Illustra come usare le nuove funzionalità in ASP.NET 4 per creare un negozio online, tra cui acquisti, estrazione e l'amministrazione.</span><span class="sxs-lookup"><span data-stu-id="9bd3b-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="9bd3b-108">Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="9bd3b-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="9bd3b-109">Parte 4 viene illustrato l'elenco di prodotti con il controllo GridView.</span><span class="sxs-lookup"><span data-stu-id="9bd3b-109">Part 4 covers listing products with the GridView control.</span></span>


## <a id="_Toc260221670"></a>  <span data-ttu-id="9bd3b-110">Elenco di prodotti con il controllo GridView</span><span class="sxs-lookup"><span data-stu-id="9bd3b-110">Listing Products with the GridView Control</span></span>

<span data-ttu-id="9bd3b-111">È possibile iniziare a implementare la pagina ProductsList.aspx facendo clic su"con il pulsante destro" nella nostra soluzione e selezionando "Aggiungi" e "Nuovo elemento".</span><span class="sxs-lookup"><span data-stu-id="9bd3b-111">Let's begin implementing our ProductsList.aspx page by "Right Clicking" on our solution and selecting "Add" and "New Item".</span></span>

![](tailspin-spyworks-part-4/_static/image1.jpg)

<span data-ttu-id="9bd3b-112">Scegliere "Web Form mediante pagina Master" e immettere un nome di pagina del ProductsList.aspx".</span><span class="sxs-lookup"><span data-stu-id="9bd3b-112">Choose "Web Form Using Master Page" and enter a page name of ProductsList.aspx".</span></span>

<span data-ttu-id="9bd3b-113">Fare clic su "Aggiungi".</span><span class="sxs-lookup"><span data-stu-id="9bd3b-113">Click "Add".</span></span>

![](tailspin-spyworks-part-4/_static/image2.jpg)

<span data-ttu-id="9bd3b-114">Quindi scegliere la cartella "Stili" in cui è stato inserito un pagina Site. master e selezionarlo dalla finestra "Contenuto della cartella".</span><span class="sxs-lookup"><span data-stu-id="9bd3b-114">Next choose the "Styles" folder where we placed the Site.Master page and select it from the "Contents of folder" window.</span></span>

![](tailspin-spyworks-part-4/_static/image3.jpg)

<span data-ttu-id="9bd3b-115">Fare clic su "Ok" per creare la pagina.</span><span class="sxs-lookup"><span data-stu-id="9bd3b-115">Click "Ok" to create the page.</span></span>

<span data-ttu-id="9bd3b-116">Il database viene popolato con dati del prodotto come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="9bd3b-116">Our database is populated with product data as seen below.</span></span>

![](tailspin-spyworks-part-4/_static/image4.jpg)

<span data-ttu-id="9bd3b-117">Dopo aver creata la pagina si userà anche in questo caso un'origine dati di entità per accedere ai dati del prodotto, ma in questo caso è necessario selezionare le entità Product e dobbiamo limitare gli elementi che vengono restituiti solo a quelli per la categoria selezionata.</span><span class="sxs-lookup"><span data-stu-id="9bd3b-117">After our page is created we'll again use an Entity Data Source to access that product data, but in this instance we need to select the Product Entities and we need to restrict the items that are returned to only those for the selected Category.</span></span>

<span data-ttu-id="9bd3b-118">A tale scopo possiamo dirti EntityDataSource per generare automaticamente la clausola WHERE e, specifichiamo il WhereParameter.</span><span class="sxs-lookup"><span data-stu-id="9bd3b-118">To accomplish this we'll tell the EntityDataSource to Auto Generate the WHERE clause and we'll specify the WhereParameter.</span></span>

<span data-ttu-id="9bd3b-119">Si ricordi che quando abbiamo creato le voci di Menu nel nostro "Menu categoria prodotto" viene compilata dinamicamente il collegamento aggiungendo il CatagoryID alla stringa di query per ogni collegamento.</span><span class="sxs-lookup"><span data-stu-id="9bd3b-119">You'll recall that when we created the Menu Items in our "Product Category Menu" we dynamically built the link by adding the CatagoryID to the QueryString for each link.</span></span> <span data-ttu-id="9bd3b-120">Microsoft comunicherà l'origine dati di entità da cui derivare il parametro in cui tale parametro di stringa di query.</span><span class="sxs-lookup"><span data-stu-id="9bd3b-120">We will tell the Entity Data Source to derive the WHERE parameter from that QueryString parameter.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample1.aspx)]

<span data-ttu-id="9bd3b-121">Si configurerà quindi il controllo ListView per visualizzare un elenco di prodotti.</span><span class="sxs-lookup"><span data-stu-id="9bd3b-121">Next, we'll configure the ListView control to display a list of products.</span></span> <span data-ttu-id="9bd3b-122">Per creare un'esperienza di shopping ottimale che è sarà compact diverse funzionalità concisa in ogni singolo prodotto visualizzato nel nostro ListVew.</span><span class="sxs-lookup"><span data-stu-id="9bd3b-122">To create an optimal shopping experience we'll compact several concise features into each individual product displayed in our ListVew.</span></span>

- <span data-ttu-id="9bd3b-123">Il nome di prodotto è un collegamento alla visualizzazione di dettaglio del prodotto.</span><span class="sxs-lookup"><span data-stu-id="9bd3b-123">The product name will be a link to the product's detail view.</span></span>
- <span data-ttu-id="9bd3b-124">Verrà visualizzato il prezzo del prodotto.</span><span class="sxs-lookup"><span data-stu-id="9bd3b-124">The product's price will be displayed.</span></span>
- <span data-ttu-id="9bd3b-125">Verrà visualizzata un'immagine del prodotto e si selezionerà in modo dinamico l'immagine da una directory di immagini di catalogo nella nostra applicazione.</span><span class="sxs-lookup"><span data-stu-id="9bd3b-125">An image of the product will be displayed and we'll dynamically select the image from a catalog images directory in our application.</span></span>
- <span data-ttu-id="9bd3b-126">Verrà incluso un collegamento per aggiungere immediatamente il prodotto specifico al carrello acquisti.</span><span class="sxs-lookup"><span data-stu-id="9bd3b-126">We will include a link to immediately add the specific product to the shopping cart.</span></span>

<span data-ttu-id="9bd3b-127">Ecco il markup per l'istanza del controllo ListView.</span><span class="sxs-lookup"><span data-stu-id="9bd3b-127">Here is the markup for our ListView control instance.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample2.aspx)]

<span data-ttu-id="9bd3b-128">Stiamo creando dinamicamente diversi collegamenti per ogni prodotto visualizzato.</span><span class="sxs-lookup"><span data-stu-id="9bd3b-128">We are dynamically building several links for each displayed product.</span></span>

<span data-ttu-id="9bd3b-129">Inoltre, prima è proprio nuova pagina di test è necessario creare la struttura di directory per il prodotto catalogo immagini come indicato di seguito.</span><span class="sxs-lookup"><span data-stu-id="9bd3b-129">Also, before we test own new page we need to create the directory structure for the product catalog images as follows.</span></span>

![](tailspin-spyworks-part-4/_static/image1.png)

<span data-ttu-id="9bd3b-130">Dopo che sono accessibili le nostre immagini di prodotto è possibile testare la pagina di elenco del prodotto.</span><span class="sxs-lookup"><span data-stu-id="9bd3b-130">Once our product images are accessible we can test our product list page.</span></span>

![](tailspin-spyworks-part-4/_static/image5.jpg)

<span data-ttu-id="9bd3b-131">Dalla home page del sito, fare clic su uno dei collegamenti elenco categoria.</span><span class="sxs-lookup"><span data-stu-id="9bd3b-131">From the site's home page, click on one of the Category List Links.</span></span>

![](tailspin-spyworks-part-4/_static/image6.jpg)

<span data-ttu-id="9bd3b-132">Ora è necessario implementare la pagina ProductDetials.apsx e la funzionalità AddToCart.</span><span class="sxs-lookup"><span data-stu-id="9bd3b-132">Now we need to implement the ProductDetials.apsx page and the AddToCart functionality.</span></span>

<span data-ttu-id="9bd3b-133">Utilizzare File -&gt;New per creare un nome di pagina ProductDetails.aspx usando Master Page del sito come in precedenza.</span><span class="sxs-lookup"><span data-stu-id="9bd3b-133">Use File-&gt;New to create a page name ProductDetails.aspx using the site Master Page as we did previously.</span></span>

<span data-ttu-id="9bd3b-134">Anche in questo caso si userà un controllo EntityDataSource per accedere al record di prodotto specifico nel database e si userà un controllo FormView ASP.NET per visualizzare i dati del prodotto come indicato di seguito.</span><span class="sxs-lookup"><span data-stu-id="9bd3b-134">We will again use an EntityDataSource control to access the specific product record in the database and we will use an ASP.NET FormView control to display the product data as follows.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample3.aspx)]

<span data-ttu-id="9bd3b-135">Non occorre preoccuparsi se la formattazione sembra un po' divertente all'utente.</span><span class="sxs-lookup"><span data-stu-id="9bd3b-135">Don't worry if the formatting looks a bit funny to you.</span></span> <span data-ttu-id="9bd3b-136">Il markup precedente lascia spazio nel layout di visualizzazione per un paio di funzionalità che verrà implementato in un secondo momento.</span><span class="sxs-lookup"><span data-stu-id="9bd3b-136">The markup above leaves room in the display layout for a couple of features we'll implement later on.</span></span>

<span data-ttu-id="9bd3b-137">Il carrello della spesa rappresenterà la logica più complessa nella nostra applicazione.</span><span class="sxs-lookup"><span data-stu-id="9bd3b-137">The Shopping Cart will represent the more complex logic in our application.</span></span> <span data-ttu-id="9bd3b-138">Per iniziare, usare File -&gt;nuovo per creare una pagina denominata MyShoppingCart.aspx.</span><span class="sxs-lookup"><span data-stu-id="9bd3b-138">To get started, use File-&gt;New to create a page called MyShoppingCart.aspx.</span></span>

<span data-ttu-id="9bd3b-139">Si noti che non è stato scelto il nome ShoppingCart.aspx.</span><span class="sxs-lookup"><span data-stu-id="9bd3b-139">Note that we are not choosing the name ShoppingCart.aspx.</span></span>

<span data-ttu-id="9bd3b-140">Il database contiene una tabella denominata "ShoppingCart".</span><span class="sxs-lookup"><span data-stu-id="9bd3b-140">Our database contains a table named "ShoppingCart".</span></span> <span data-ttu-id="9bd3b-141">Quando viene generato un Entity Data Model è stata creata una classe per ogni tabella nel database.</span><span class="sxs-lookup"><span data-stu-id="9bd3b-141">When we generated an Entity Data Model a class was created for each table in the database.</span></span> <span data-ttu-id="9bd3b-142">Pertanto, l'Entity Data Model generato una classe di entità denominato "ShoppingCart".</span><span class="sxs-lookup"><span data-stu-id="9bd3b-142">Therefore, the Entity Data Model generated an Entity Class named "ShoppingCart".</span></span> <span data-ttu-id="9bd3b-143">È stato possibile modificare il modello in modo che abbiamo potremmo utilizzare tale nome per l'implementazione di carrello degli acquisti o estenderla per le nostre esigenze, ma si opterà invece a è sufficiente selezionare un nome da evita il conflitto.</span><span class="sxs-lookup"><span data-stu-id="9bd3b-143">We could edit the model so that we could use that name for our shopping cart implementation or extend it for our needs, but we will opt instead to simply slect a name that will avoid the conflict.</span></span>

<span data-ttu-id="9bd3b-144">È inoltre importante sottolineare che creeremo un carrello acquisti semplice e incorporare la logica di carrello degli acquisti con la visualizzazione del carrello acquisti.</span><span class="sxs-lookup"><span data-stu-id="9bd3b-144">It's also worth noting that we will be creating a simple shopping cart and embedding the shopping cart logic with the shopping cart display.</span></span> <span data-ttu-id="9bd3b-145">Si potrebbe essere anche scegliere di implementare il carrello acquisti in un livello di Business completamente separate.</span><span class="sxs-lookup"><span data-stu-id="9bd3b-145">We might also choose to implement our shopping cart in a completely separate Business Layer.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="9bd3b-146">[Precedente](tailspin-spyworks-part-3.md)
> [Successivo](tailspin-spyworks-part-5.md)</span><span class="sxs-lookup"><span data-stu-id="9bd3b-146">[Previous](tailspin-spyworks-part-3.md)
[Next](tailspin-spyworks-part-5.md)</span></span>
