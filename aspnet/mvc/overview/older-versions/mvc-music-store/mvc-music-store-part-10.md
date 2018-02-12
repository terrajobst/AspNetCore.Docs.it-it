---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
title: 'Parte 10: Aggiornamenti finali allo spostamento e la struttura del sito, conclusione | Documenti Microsoft'
author: jongalloway
description: Questa serie di esercitazioni in dettaglio tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Negozio. Parte 10 copre finali aggiornamenti allo spostamento e S....
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 0c6e4c2f-fcdb-4978-9656-1990c6f15727
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
msc.type: authoredcontent
ms.openlocfilehash: 2a65e4b793b615c45cdf31166e0a000ae72ee534
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/12/2018
---
<a name="part-10-final-updates-to-navigation-and-site-design-conclusion"></a><span data-ttu-id="1bbc4-104">Parte 10: Aggiornamenti finali allo spostamento e la struttura del sito, conclusione</span><span class="sxs-lookup"><span data-stu-id="1bbc4-104">Part 10: Final Updates to Navigation and Site Design, Conclusion</span></span>
====================
<span data-ttu-id="1bbc4-105">da [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="1bbc4-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="1bbc4-106">L'archivio di musica MVC è un'applicazione di esercitazione che vengono presentati e dettagliate sull'utilizzo di MVC ASP.NET e Visual Studio per lo sviluppo web.</span><span class="sxs-lookup"><span data-stu-id="1bbc4-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="1bbc4-107">L'archivio di musica MVC è un'implementazione dell'archivio di esempio semplice che vende album musicali online e ne implementa amministrazione sito di base, account utente e funzionalità di carrello acquisti.</span><span class="sxs-lookup"><span data-stu-id="1bbc4-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="1bbc4-108">Questa serie di esercitazioni in dettaglio tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Negozio.</span><span class="sxs-lookup"><span data-stu-id="1bbc4-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="1bbc4-109">Parte 10 copre finali aggiornamenti allo spostamento e la struttura del sito, conclusione.</span><span class="sxs-lookup"><span data-stu-id="1bbc4-109">Part 10 covers Final Updates to Navigation and Site Design, Conclusion.</span></span>


<span data-ttu-id="1bbc4-110">Abbiamo completato tutte le funzionalità principali per il sito, ma si dispone ancora di alcune funzionalità da aggiungere per l'esplorazione del sito, la home page e la pagina Sfoglia archivio.</span><span class="sxs-lookup"><span data-stu-id="1bbc4-110">We've completed all the major functionality for our site, but we still have some features to add to the site navigation, the home page, and the Store Browse page.</span></span>

## <a name="creating-the-shopping-cart-summary-partial-view"></a><span data-ttu-id="1bbc4-111">Creazione della visualizzazione parziale riepilogo carrello acquisti</span><span class="sxs-lookup"><span data-stu-id="1bbc4-111">Creating the Shopping Cart Summary Partial View</span></span>

<span data-ttu-id="1bbc4-112">Si vuole esporre il numero di elementi nel carrello acquisti dell'utente in tutto il sito.</span><span class="sxs-lookup"><span data-stu-id="1bbc4-112">We want to expose the number of items in the user's shopping cart across the entire site.</span></span>

![](mvc-music-store-part-10/_static/image1.png)

<span data-ttu-id="1bbc4-113">È possibile implementare facilmente questo mediante la creazione di una visualizzazione parziale che viene aggiunto al nostro Site. master.</span><span class="sxs-lookup"><span data-stu-id="1bbc4-113">We can easily implement this by creating a partial view which is added to our Site.master.</span></span>

<span data-ttu-id="1bbc4-114">Come illustrato in precedenza, il controller ShoppingCart include un metodo di azione CartSummary che restituisce una visualizzazione parziale:</span><span class="sxs-lookup"><span data-stu-id="1bbc4-114">As shown previously, the ShoppingCart controller includes a CartSummary action method which returns a partial view:</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample1.cs)]

<span data-ttu-id="1bbc4-115">Per creare la visualizzazione parziale CartSummary, fare clic sulla cartella viste/ShoppingCart e selezionare Aggiungi visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="1bbc4-115">To create the CartSummary partial view, right-click on the Views/ShoppingCart folder and select Add View.</span></span> <span data-ttu-id="1bbc4-116">Nome della vista CartSummary e controllare la casella di controllo "Crea una visualizzazione parziale", come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="1bbc4-116">Name the view CartSummary and check the "Create a partial view" checkbox as shown below.</span></span>

![](mvc-music-store-part-10/_static/image2.png)

<span data-ttu-id="1bbc4-117">La visualizzazione parziale CartSummary è molto semplice: è solo un collegamento alla visualizzazione che mostra il numero di elementi nel carrello ShoppingCart indice.</span><span class="sxs-lookup"><span data-stu-id="1bbc4-117">The CartSummary partial view is really simple - it's just a link to the ShoppingCart Index view which shows the number of items in the cart.</span></span> <span data-ttu-id="1bbc4-118">Il codice completo per CartSummary.cshtml è il seguente:</span><span class="sxs-lookup"><span data-stu-id="1bbc4-118">The complete code for CartSummary.cshtml is as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample2.cshtml)]

<span data-ttu-id="1bbc4-119">È possibile includere una visualizzazione parziale in qualsiasi pagina del sito, inclusi lo schema del sito, tramite il metodo Html.RenderAction.</span><span class="sxs-lookup"><span data-stu-id="1bbc4-119">We can include a partial view in any page in the site, including the Site master, by using the Html.RenderAction method.</span></span> <span data-ttu-id="1bbc4-120">RenderAction richiede di specificare il nome dell'azione ("CartSummary") e il nome del Controller ("ShoppingCart") come di seguito.</span><span class="sxs-lookup"><span data-stu-id="1bbc4-120">RenderAction requires us to specify the Action Name ("CartSummary") and the Controller Name ("ShoppingCart") as below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample3.cshtml)]

<span data-ttu-id="1bbc4-121">Prima di aggiungere questo Layout il sito, si creerà inoltre il genere di Menu per eseguire tutti gli aggiornamenti Site. master in una sola volta.</span><span class="sxs-lookup"><span data-stu-id="1bbc4-121">Before adding this to the site Layout, we will also create the Genre Menu so we can make all of our Site.master updates at one time.</span></span>

## <a name="creating-the-genre-menu-partial-view"></a><span data-ttu-id="1bbc4-122">Creazione della visualizzazione parziale di genere Menu</span><span class="sxs-lookup"><span data-stu-id="1bbc4-122">Creating the Genre Menu Partial View</span></span>

<span data-ttu-id="1bbc4-123">È possibile rendere molto più semplice per gli utenti per spostarsi tra l'archivio mediante l'aggiunta di un Menu di genere che elenca tutti i generi disponibili nel Negozio.</span><span class="sxs-lookup"><span data-stu-id="1bbc4-123">We can make it a lot easier for our users to navigate through the store by adding a Genre Menu which lists all the Genres available in our store.</span></span>

![](mvc-music-store-part-10/_static/image3.png)

<span data-ttu-id="1bbc4-124">Si seguirà la stessa procedura consente di creare anche una visualizzazione parziale GenreMenu e quindi è possibile aggiungere entrambi allo schema del sito.</span><span class="sxs-lookup"><span data-stu-id="1bbc4-124">We will follow the same steps also create a GenreMenu partial view, and then we can add them both to the Site master.</span></span> <span data-ttu-id="1bbc4-125">In primo luogo, aggiungere la seguente azione controller GenreMenu il StoreController:</span><span class="sxs-lookup"><span data-stu-id="1bbc4-125">First, add the following GenreMenu controller action to the StoreController:</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample4.cs)]

<span data-ttu-id="1bbc4-126">Questa azione restituisce un elenco di generi che verrà visualizzato tramite la visualizzazione parziale, che verrà creato successivamente.</span><span class="sxs-lookup"><span data-stu-id="1bbc4-126">This action returns a list of Genres which will be displayed by the partial view, which we will create next.</span></span>

<span data-ttu-id="1bbc4-127">*Nota: È stato aggiunto l'attributo [ChildActionOnly] per questa azione del controller, che indica che si vuole inserire solo questa azione utilizzabile da una visualizzazione parziale. Questo attributo impedirà l'azione del controller venga eseguito selezionando /Store/GenreMenu. Questo non è necessario per le visualizzazioni parziali, ma è consigliabile, poiché è necessario assicurarsi che le azioni del controller vengono usate come si desidera. È inoltre stiamo restituzione PartialView anziché visualizzazione, che informa il motore di visualizzazione che consigliabile utilizzare il Layout per la visualizzazione, come includerlo nelle altre visualizzazioni.*</span><span class="sxs-lookup"><span data-stu-id="1bbc4-127">*Note: We have added the [ChildActionOnly] attribute to this controller action, which indicates that we only want this action to be used from a Partial View. This attribute will prevent the controller action from being executed by browsing to /Store/GenreMenu. This isn't required for partial views, but it is a good practice, since we want to make sure our controller actions are used as we intend. We are also returning PartialView rather than View, which lets the view engine know that it shouldn't use the Layout for this view, as it is being included in other views.*</span></span>

<span data-ttu-id="1bbc4-128">Fare clic su azione del controller GenreMenu e creare una visualizzazione parziale denominata GenreMenu fortemente tipizzato utilizzando la classe di dati di visualizzazione Genre, come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="1bbc4-128">Right-click on the GenreMenu controller action and create a partial view named GenreMenu which is strongly typed using the Genre view data class as shown below.</span></span>

![](mvc-music-store-part-10/_static/image4.png)

<span data-ttu-id="1bbc4-129">Aggiornare il codice di visualizzazione per la visualizzazione parziale GenreMenu visualizzare gli elementi utilizzando un elenco non ordinato nel modo seguente.</span><span class="sxs-lookup"><span data-stu-id="1bbc4-129">Update the view code for the GenreMenu partial view to display the items using an unordered list as follows.</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample5.cshtml)]

## <a name="updating-site-layout-to-display-our-partial-views"></a><span data-ttu-id="1bbc4-130">Aggiornamento del Layout del sito per visualizzare il nostro visualizzazioni parziali</span><span class="sxs-lookup"><span data-stu-id="1bbc4-130">Updating Site Layout to display our Partial Views</span></span>

<span data-ttu-id="1bbc4-131">È possibile aggiungere il nostro visualizzazioni parziali per il Layout del sito (/ / Shared/viste\_cshtml) chiamando Html.RenderAction().</span><span class="sxs-lookup"><span data-stu-id="1bbc4-131">We can add our partial views to the Site Layout (/Views/Shared/\_Layout.cshtml) by calling Html.RenderAction().</span></span> <span data-ttu-id="1bbc4-132">In entrambi, nonché alcuni tag aggiuntivi per visualizzarli, verrà aggiunto come illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="1bbc4-132">We'll add them both in, as well as some additional markup to display them, as shown below:</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample6.cshtml)]

<span data-ttu-id="1bbc4-133">A questo punto quando si esegue l'applicazione, verrà spiegato il genere nell'area di navigazione a sinistra e il riepilogo di carrello nella parte superiore.</span><span class="sxs-lookup"><span data-stu-id="1bbc4-133">Now when we run the application, we will see the Genre in the left navigation area and the Cart Summary at the top.</span></span>

## <a name="update-to-the-store-browse-page"></a><span data-ttu-id="1bbc4-134">Aggiornare la pagina Sfoglia archivio</span><span class="sxs-lookup"><span data-stu-id="1bbc4-134">Update to the Store Browse page</span></span>

<span data-ttu-id="1bbc4-135">La pagina Sfoglia archivio funzionale, ma non è del tutto soddisfacente.</span><span class="sxs-lookup"><span data-stu-id="1bbc4-135">The Store Browse page is functional, but doesn't look very good.</span></span> <span data-ttu-id="1bbc4-136">È possibile aggiornare la pagina per visualizzare gli album in un layout ottimale, aggiornare il vista codice (presente in /Views/Store/Browse.cshtml) come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="1bbc4-136">We can update the page to show the albums in a better layout by updating the view code (found in /Views/Store/Browse.cshtml) as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample7.cshtml)]

<span data-ttu-id="1bbc4-137">Qui stiamo utilizzare Action anziché HTML. ActionLink in modo che è possibile applicare una formattazione speciale per il collegamento per includere il disegno album.</span><span class="sxs-lookup"><span data-stu-id="1bbc4-137">Here we are making use of Url.Action rather than Html.ActionLink so that we can apply special formatting to the link to include the album artwork.</span></span>

<span data-ttu-id="1bbc4-138">*Nota: Viene visualizzato una copertina generico per gli album. Queste informazioni vengono archiviate nel database e può essere modificato tramite la gestione di Store. Si è di aggiungere un'immagine personalizzata.*</span><span class="sxs-lookup"><span data-stu-id="1bbc4-138">*Note: We are displaying a generic album cover for these albums. This information is stored in the database and is editable via the Store Manager. You are welcome to add your own artwork.*</span></span>

<span data-ttu-id="1bbc4-139">Ora quando si passa a un genere, si visualizzeranno gli album visualizzati in una griglia con la grafica album.</span><span class="sxs-lookup"><span data-stu-id="1bbc4-139">Now when we browse to a Genre, we will see the albums shown in a grid with the album artwork.</span></span>

![](mvc-music-store-part-10/_static/image5.png)

## <a name="updating-the-home-page-to-show-top-selling-albums"></a><span data-ttu-id="1bbc4-140">Aggiornamento della pagina Home per visualizzare album vendite superiore</span><span class="sxs-lookup"><span data-stu-id="1bbc4-140">Updating the Home Page to show Top Selling Albums</span></span>

<span data-ttu-id="1bbc4-141">Si vuole funzionalità nostri venduti album nella home page per aumentare le vendite.</span><span class="sxs-lookup"><span data-stu-id="1bbc4-141">We want to feature our top selling albums on the home page to increase sales.</span></span> <span data-ttu-id="1bbc4-142">Alcuni aggiornamenti da apportare al nostro HomeController per gestire questa condizione e aggiungere in alcune altre immagini.</span><span class="sxs-lookup"><span data-stu-id="1bbc4-142">We'll make some updates to our HomeController to handle that, and add in some additional graphics as well.</span></span>

<span data-ttu-id="1bbc4-143">In primo luogo, una proprietà di navigazione verrà aggiunto alla classe Album in modo che sia in grado EntityFramework che essi è associati.</span><span class="sxs-lookup"><span data-stu-id="1bbc4-143">First, we'll add a navigation property to our Album class so that EntityFramework knows that they're associated.</span></span> <span data-ttu-id="1bbc4-144">Le ultime righe del nostro **Album** classe dovrebbe essere simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="1bbc4-144">The last few lines of our **Album** class should now look like this:</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample8.cs)]

<span data-ttu-id="1bbc4-145">*Nota: Questo richiede l'aggiunta di un utilizzo dell'istruzione di portare nello spazio dei nomi System.Collections.Generic.*</span><span class="sxs-lookup"><span data-stu-id="1bbc4-145">*Note: This will require adding a using statement to bring in the System.Collections.Generic namespace.*</span></span>

<span data-ttu-id="1bbc4-146">In primo luogo, verrà aggiunto un campo storeDB e il MvcMusicStore.Models utilizzando le istruzioni, come il nostro altri controller.</span><span class="sxs-lookup"><span data-stu-id="1bbc4-146">First, we'll add a storeDB field and the MvcMusicStore.Models using statements, as in our other controllers.</span></span> <span data-ttu-id="1bbc4-147">Successivamente, aggiungeremo il metodo seguente per la classe HomeController che esegue query di database per trovare album vendite in base alle OrderDetails.</span><span class="sxs-lookup"><span data-stu-id="1bbc4-147">Next, we'll add the following method to the HomeController which queries our database to find top selling albums according to OrderDetails.</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample9.cs)]

<span data-ttu-id="1bbc4-148">Si tratta di un metodo privato, poiché non si desidera renderlo disponibile come un'azione del controller.</span><span class="sxs-lookup"><span data-stu-id="1bbc4-148">This is a private method, since we don't want to make it available as a controller action.</span></span> <span data-ttu-id="1bbc4-149">Si incluso in HomeController per motivi di semplicità, ma vengono fornite informazioni utili per spostare la logica di business in classi di servizio separato come appropriato.</span><span class="sxs-lookup"><span data-stu-id="1bbc4-149">We are including it in the HomeController for simplicity, but you are encouraged to move your business logic into separate service classes as appropriate.</span></span>

<span data-ttu-id="1bbc4-150">A questo punto, è possibile aggiornare l'azione del controller di indice per eseguire query i primi 5 album di vendita e li tornare alla visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="1bbc4-150">With that in place, we can update the Index controller action to query the top 5 selling albums and return them to the view.</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample10.cs)]

<span data-ttu-id="1bbc4-151">Il codice completo per HomeController aggiornato è come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="1bbc4-151">The complete code for the updated HomeController is as shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample11.cs)]

<span data-ttu-id="1bbc4-152">Infine, è necessario aggiornare la visualizzazione dell'indice Home in modo che è possibile visualizzare un elenco di album l'aggiornamento del tipo di modello e aggiungendo l'elenco di album nella parte inferiore.</span><span class="sxs-lookup"><span data-stu-id="1bbc4-152">Finally, we'll need to update our Home Index view so that it can display a list of albums by updating the Model type and adding the album list to the bottom.</span></span> <span data-ttu-id="1bbc4-153">Si avrà l'opportunità di aggiungere anche un titolo e una sezione di innalzamento di livello alla pagina.</span><span class="sxs-lookup"><span data-stu-id="1bbc4-153">We will take this opportunity to also add a heading and a promotion section to the page.</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample12.cshtml)]

<span data-ttu-id="1bbc4-154">A questo punto quando si esegue l'applicazione, verranno esaminati aggiornato home page con album vendite e il messaggio promozionale.</span><span class="sxs-lookup"><span data-stu-id="1bbc4-154">Now when we run the application, we'll see our updated home page with top selling albums and our promotional message.</span></span>

![](mvc-music-store-part-10/_static/image1.jpg)

## <a name="conclusion"></a><span data-ttu-id="1bbc4-155">Conclusione</span><span class="sxs-lookup"><span data-stu-id="1bbc4-155">Conclusion</span></span>

<span data-ttu-id="1bbc4-156">Abbiamo visto che ASP.NET MVC rende più semplice per creare un sito Web complesse con accesso al database, l'appartenenza, AJAX, e così via.</span><span class="sxs-lookup"><span data-stu-id="1bbc4-156">We've seen that ASP.NET MVC makes it easy to create a sophisticated website with database access, membership, AJAX, etc.</span></span> <span data-ttu-id="1bbc4-157">abbastanza rapidamente.</span><span class="sxs-lookup"><span data-stu-id="1bbc4-157">pretty quickly.</span></span> <span data-ttu-id="1bbc4-158">Probabilmente questa esercitazione riceve gli strumenti che necessari per iniziare la creazione di applicazioni personalizzate ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="1bbc4-158">Hopefully this tutorial has given you the tools you need to get started building your own ASP.NET MVC applications!</span></span>


>[!div class="step-by-step"]
[<span data-ttu-id="1bbc4-159">Precedente</span><span class="sxs-lookup"><span data-stu-id="1bbc4-159">Previous</span></span>](mvc-music-store-part-9.md)
