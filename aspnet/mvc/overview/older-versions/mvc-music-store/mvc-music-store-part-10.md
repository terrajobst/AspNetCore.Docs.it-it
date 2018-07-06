---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
title: 'Parte 10: Aggiornamenti finali allo spostamento e struttura del sito, conclusioni | Microsoft Docs'
author: jongalloway
description: Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Music Store. Parte 10 copre aggiornamenti finali allo spostamento e S....
ms.author: aspnetcontent
ms.date: 04/21/2011
ms.assetid: 0c6e4c2f-fcdb-4978-9656-1990c6f15727
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
msc.type: authoredcontent
ms.openlocfilehash: 40ed7c337e097675199ab66229095bd3315d8c8d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/05/2018
ms.locfileid: "37836810"
---
<a name="part-10-final-updates-to-navigation-and-site-design-conclusion"></a><span data-ttu-id="a6871-104">Parte 10: Aggiornamenti finali allo spostamento e struttura del sito, conclusioni</span><span class="sxs-lookup"><span data-stu-id="a6871-104">Part 10: Final Updates to Navigation and Site Design, Conclusion</span></span>
====================
<span data-ttu-id="a6871-105">da [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="a6871-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="a6871-106">Store la musica MVC è un'applicazione di esercitazione introduce e dettagliata spiega come usare ASP.NET MVC e Visual Studio per lo sviluppo web.</span><span class="sxs-lookup"><span data-stu-id="a6871-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="a6871-107">La Store musica MVC è un'implementazione di archivio di esempio leggero che vende gli album musicali online e ne implementa amministrazione sito di base, accesso dell'utente e la funzionalità di carrello acquisti.</span><span class="sxs-lookup"><span data-stu-id="a6871-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="a6871-108">Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Music Store.</span><span class="sxs-lookup"><span data-stu-id="a6871-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="a6871-109">Parte 10 copre aggiornamenti finali allo spostamento e struttura del sito, conclusioni.</span><span class="sxs-lookup"><span data-stu-id="a6871-109">Part 10 covers Final Updates to Navigation and Site Design, Conclusion.</span></span>


<span data-ttu-id="a6871-110">Abbiamo completato tutte le funzionalità principali per il sito, ma sono ancora disponibili alcune funzionalità da aggiungere al riquadro di spostamento del sito, la home page e la pagina Sfoglia Store.</span><span class="sxs-lookup"><span data-stu-id="a6871-110">We've completed all the major functionality for our site, but we still have some features to add to the site navigation, the home page, and the Store Browse page.</span></span>

## <a name="creating-the-shopping-cart-summary-partial-view"></a><span data-ttu-id="a6871-111">Creazione della visualizzazione parziale riepilogo carrello acquisti</span><span class="sxs-lookup"><span data-stu-id="a6871-111">Creating the Shopping Cart Summary Partial View</span></span>

<span data-ttu-id="a6871-112">Si vuole esporre il numero di elementi nel carrello acquisti dell'utente nell'intero sito.</span><span class="sxs-lookup"><span data-stu-id="a6871-112">We want to expose the number of items in the user's shopping cart across the entire site.</span></span>

![](mvc-music-store-part-10/_static/image1.png)

<span data-ttu-id="a6871-113">È possibile implementare facilmente questo mediante la creazione di una visualizzazione parziale che viene aggiunto al nostro Site. master.</span><span class="sxs-lookup"><span data-stu-id="a6871-113">We can easily implement this by creating a partial view which is added to our Site.master.</span></span>

<span data-ttu-id="a6871-114">Come illustrato in precedenza, il controller ShoppingCart include un metodo di azione CartSummary che restituisce una visualizzazione parziale:</span><span class="sxs-lookup"><span data-stu-id="a6871-114">As shown previously, the ShoppingCart controller includes a CartSummary action method which returns a partial view:</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample1.cs)]

<span data-ttu-id="a6871-115">Per creare la visualizzazione parziale CartSummary, pulsante destro del mouse sulla cartella visualizzazioni/ShoppingCart e selezionare Aggiungi visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="a6871-115">To create the CartSummary partial view, right-click on the Views/ShoppingCart folder and select Add View.</span></span> <span data-ttu-id="a6871-116">Nome della vista CartSummary e selezionare la casella di controllo "Crea una visualizzazione parziale" come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="a6871-116">Name the view CartSummary and check the "Create a partial view" checkbox as shown below.</span></span>

![](mvc-music-store-part-10/_static/image2.png)

<span data-ttu-id="a6871-117">La visualizzazione parziale CartSummary è davvero semplice: è solo un collegamento alla vista ShoppingCart Index che mostra il numero di elementi nel carrello.</span><span class="sxs-lookup"><span data-stu-id="a6871-117">The CartSummary partial view is really simple - it's just a link to the ShoppingCart Index view which shows the number of items in the cart.</span></span> <span data-ttu-id="a6871-118">Il codice completo per CartSummary.cshtml è come segue:</span><span class="sxs-lookup"><span data-stu-id="a6871-118">The complete code for CartSummary.cshtml is as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample2.cshtml)]

<span data-ttu-id="a6871-119">È possibile includere una visualizzazione parziale in qualsiasi pagina nel sito, incluso il database master del sito, usando il metodo Html.RenderAction.</span><span class="sxs-lookup"><span data-stu-id="a6871-119">We can include a partial view in any page in the site, including the Site master, by using the Html.RenderAction method.</span></span> <span data-ttu-id="a6871-120">RenderAction richiede di specificare il nome dell'azione ("CartSummary") e il nome del Controller ("ShoppingCart") come di seguito.</span><span class="sxs-lookup"><span data-stu-id="a6871-120">RenderAction requires us to specify the Action Name ("CartSummary") and the Controller Name ("ShoppingCart") as below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample3.cshtml)]

<span data-ttu-id="a6871-121">Prima di aggiungere questo sito di Layout, verranno create anche nel Menu Genre quindi faciliteremo tutti gli aggiornamenti Site. master in una sola volta.</span><span class="sxs-lookup"><span data-stu-id="a6871-121">Before adding this to the site Layout, we will also create the Genre Menu so we can make all of our Site.master updates at one time.</span></span>

## <a name="creating-the-genre-menu-partial-view"></a><span data-ttu-id="a6871-122">Creazione della visualizzazione parziale Menu al genere</span><span class="sxs-lookup"><span data-stu-id="a6871-122">Creating the Genre Menu Partial View</span></span>

<span data-ttu-id="a6871-123">È possibile rendere molto più semplice per gli utenti per spostarsi tra l'archivio mediante l'aggiunta di un Menu di genere che elenca tutti i generi disponibili nel nostro store.</span><span class="sxs-lookup"><span data-stu-id="a6871-123">We can make it a lot easier for our users to navigate through the store by adding a Genre Menu which lists all the Genres available in our store.</span></span>

![](mvc-music-store-part-10/_static/image3.png)

<span data-ttu-id="a6871-124">Si seguirà la stessa procedura crea anche una visualizzazione parziale GenreMenu e quindi è possibile aggiungere entrambi al master del sito.</span><span class="sxs-lookup"><span data-stu-id="a6871-124">We will follow the same steps also create a GenreMenu partial view, and then we can add them both to the Site master.</span></span> <span data-ttu-id="a6871-125">In primo luogo, aggiungere l'azione del controller seguente GenreMenu il StoreController:</span><span class="sxs-lookup"><span data-stu-id="a6871-125">First, add the following GenreMenu controller action to the StoreController:</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample4.cs)]

<span data-ttu-id="a6871-126">Questa azione restituisce un elenco dei generi che verrà visualizzato tramite la visualizzazione parziale, che verrà creato successivamente.</span><span class="sxs-lookup"><span data-stu-id="a6871-126">This action returns a list of Genres which will be displayed by the partial view, which we will create next.</span></span>

<span data-ttu-id="a6871-127">*Nota: È stato aggiunto l'attributo [ChildActionOnly] all'azione del controller, che indica che si desidera solo questa azione per essere usata da una visualizzazione parziale. Questo attributo eviterà l'azione del controller da in esecuzione passando a /Store/GenreMenu. Non è obbligatorio per le visualizzazioni parziali, ma è buona norma, poiché si vogliono verificare che le azioni del controller vengono usate come Microsoft prevede di rendere. È inoltre sta restituendo PartialView invece di visualizzazione, che consente il motore di visualizzazione di sapere che non devono usare il Layout per questa visualizzazione, come verrà incluso nelle altre visualizzazioni.*</span><span class="sxs-lookup"><span data-stu-id="a6871-127">*Note: We have added the [ChildActionOnly] attribute to this controller action, which indicates that we only want this action to be used from a Partial View. This attribute will prevent the controller action from being executed by browsing to /Store/GenreMenu. This isn't required for partial views, but it is a good practice, since we want to make sure our controller actions are used as we intend. We are also returning PartialView rather than View, which lets the view engine know that it shouldn't use the Layout for this view, as it is being included in other views.*</span></span>

<span data-ttu-id="a6871-128">Fare clic su azione del controller GenreMenu e creare una visualizzazione parziale denominata GenreMenu che è fortemente tipizzata utilizzando la classe di dati di visualizzazione Genre, come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="a6871-128">Right-click on the GenreMenu controller action and create a partial view named GenreMenu which is strongly typed using the Genre view data class as shown below.</span></span>

![](mvc-music-store-part-10/_static/image4.png)

<span data-ttu-id="a6871-129">Aggiornare il codice di visualizzazione per la visualizzazione parziale GenreMenu visualizzare gli elementi usando un elenco non ordinato nel modo seguente.</span><span class="sxs-lookup"><span data-stu-id="a6871-129">Update the view code for the GenreMenu partial view to display the items using an unordered list as follows.</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample5.cshtml)]

## <a name="updating-site-layout-to-display-our-partial-views"></a><span data-ttu-id="a6871-130">Aggiornamento del Layout del sito per visualizzare viste parziali</span><span class="sxs-lookup"><span data-stu-id="a6871-130">Updating Site Layout to display our Partial Views</span></span>

<span data-ttu-id="a6871-131">È possibile aggiungere viste parziali al Layout del sito (/viste/Shared/\_layout. cshtml) chiamando Html.RenderAction().</span><span class="sxs-lookup"><span data-stu-id="a6871-131">We can add our partial views to the Site Layout (/Views/Shared/\_Layout.cshtml) by calling Html.RenderAction().</span></span> <span data-ttu-id="a6871-132">Viene aggiunto in entrambi in, nonché alcuni tag aggiuntivi per visualizzarli, come illustrato di seguito:</span><span class="sxs-lookup"><span data-stu-id="a6871-132">We'll add them both in, as well as some additional markup to display them, as shown below:</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample6.cshtml)]

<span data-ttu-id="a6871-133">A questo punto quando si esegue l'applicazione, si vedrà il genere nell'area di navigazione a sinistra e il riepilogo di carrello nella parte superiore.</span><span class="sxs-lookup"><span data-stu-id="a6871-133">Now when we run the application, we will see the Genre in the left navigation area and the Cart Summary at the top.</span></span>

## <a name="update-to-the-store-browse-page"></a><span data-ttu-id="a6871-134">Aggiornare la pagina Sfoglia Store</span><span class="sxs-lookup"><span data-stu-id="a6871-134">Update to the Store Browse page</span></span>

<span data-ttu-id="a6871-135">La pagina Sfoglia Store è funzionale, ma non molto bene.</span><span class="sxs-lookup"><span data-stu-id="a6871-135">The Store Browse page is functional, but doesn't look very good.</span></span> <span data-ttu-id="a6871-136">È possibile aggiornare la pagina per visualizzare album in un layout ottimale, aggiornare il visualizzazione codice (disponibile in /Views/Store/Browse.cshtml) come indicato di seguito:</span><span class="sxs-lookup"><span data-stu-id="a6871-136">We can update the page to show the albums in a better layout by updating the view code (found in /Views/Store/Browse.cshtml) as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample7.cshtml)]

<span data-ttu-id="a6871-137">Qui seguito all'utilizzo di Action anziché HTML. ActionLink in modo che è possibile applicare una formattazione speciale per il collegamento per includere la grafica di album.</span><span class="sxs-lookup"><span data-stu-id="a6871-137">Here we are making use of Url.Action rather than Html.ActionLink so that we can apply special formatting to the link to include the album artwork.</span></span>

<span data-ttu-id="a6871-138">*Nota: Si sta visualizzando una copertina album generico per questi album. Queste informazioni vengono archiviate nel database e può essere modificate tramite la gestione di Store. Si è possibile aggiungere un'immagine personalizzata.*</span><span class="sxs-lookup"><span data-stu-id="a6871-138">*Note: We are displaying a generic album cover for these albums. This information is stored in the database and is editable via the Store Manager. You are welcome to add your own artwork.*</span></span>

<span data-ttu-id="a6871-139">A questo punto quando si passa a un genere, si vedrà album visualizzato in una griglia con la grafica di album.</span><span class="sxs-lookup"><span data-stu-id="a6871-139">Now when we browse to a Genre, we will see the albums shown in a grid with the album artwork.</span></span>

![](mvc-music-store-part-10/_static/image5.png)

## <a name="updating-the-home-page-to-show-top-selling-albums"></a><span data-ttu-id="a6871-140">Aggiornare la Home Page per visualizzare album vendita Top</span><span class="sxs-lookup"><span data-stu-id="a6871-140">Updating the Home Page to show Top Selling Albums</span></span>

<span data-ttu-id="a6871-141">Vogliamo principali album nella home page per incrementare le vendite di vendita di funzionalità.</span><span class="sxs-lookup"><span data-stu-id="a6871-141">We want to feature our top selling albums on the home page to increase sales.</span></span> <span data-ttu-id="a6871-142">Apportare alcuni aggiornamenti al nostro HomeController all'handle che e aggiungere anche alcuni grafici aggiuntivi.</span><span class="sxs-lookup"><span data-stu-id="a6871-142">We'll make some updates to our HomeController to handle that, and add in some additional graphics as well.</span></span>

<span data-ttu-id="a6871-143">In primo luogo, si aggiungerà una proprietà di navigazione per la classe Album in modo che Entity Framework sappia che sono associate.</span><span class="sxs-lookup"><span data-stu-id="a6871-143">First, we'll add a navigation property to our Album class so that EntityFramework knows that they're associated.</span></span> <span data-ttu-id="a6871-144">Le ultime righe del nostro **Album** classe dovrebbe ora essere simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="a6871-144">The last few lines of our **Album** class should now look like this:</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample8.cs)]

<span data-ttu-id="a6871-145">*Nota: Questo richiederà l'aggiunta di un utilizzo portare nello spazio dei nomi System.Collections.Generic dell'istruzione.*</span><span class="sxs-lookup"><span data-stu-id="a6871-145">*Note: This will require adding a using statement to bring in the System.Collections.Generic namespace.*</span></span>

<span data-ttu-id="a6871-146">In primo luogo, si aggiungerà un campo storeDB e il MvcMusicStore.Models usando le istruzioni, come nel nostro altri controller.</span><span class="sxs-lookup"><span data-stu-id="a6871-146">First, we'll add a storeDB field and the MvcMusicStore.Models using statements, as in our other controllers.</span></span> <span data-ttu-id="a6871-147">Successivamente, si aggiungerà il metodo seguente per la classe HomeController che esegue una query dal database per trovare superiore gli album delle vendite in base a OrderDetails.</span><span class="sxs-lookup"><span data-stu-id="a6871-147">Next, we'll add the following method to the HomeController which queries our database to find top selling albums according to OrderDetails.</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample9.cs)]

<span data-ttu-id="a6871-148">Si tratta di un metodo privato, poiché non vogliamo per renderlo disponibile come un'azione del controller.</span><span class="sxs-lookup"><span data-stu-id="a6871-148">This is a private method, since we don't want to make it available as a controller action.</span></span> <span data-ttu-id="a6871-149">Si sta includerlo in HomeController per motivi di semplicità, ma sono invitati a spostare la logica di business in classi di servizio separato come appropriato.</span><span class="sxs-lookup"><span data-stu-id="a6871-149">We are including it in the HomeController for simplicity, but you are encouraged to move your business logic into separate service classes as appropriate.</span></span>

<span data-ttu-id="a6871-150">A questo punto, è possibile aggiornare l'azione del controller dell'indice per eseguire query i primi 5 vendere gli album e li tornare alla visualizzazione.</span><span class="sxs-lookup"><span data-stu-id="a6871-150">With that in place, we can update the Index controller action to query the top 5 selling albums and return them to the view.</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample10.cs)]

<span data-ttu-id="a6871-151">Il codice completo per la classe HomeController aggiornato è come illustrato di seguito.</span><span class="sxs-lookup"><span data-stu-id="a6871-151">The complete code for the updated HomeController is as shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample11.cs)]

<span data-ttu-id="a6871-152">Infine, è necessario aggiornare la visualizzazione dell'indice Home in modo che sia possibile visualizzare un elenco di album aggiornando il tipo di modello e aggiungendo l'elenco di album alla fine.</span><span class="sxs-lookup"><span data-stu-id="a6871-152">Finally, we'll need to update our Home Index view so that it can display a list of albums by updating the Model type and adding the album list to the bottom.</span></span> <span data-ttu-id="a6871-153">Verrà usato l'opportunità di aggiungere anche un'intestazione e una sezione di innalzamento di livello alla pagina.</span><span class="sxs-lookup"><span data-stu-id="a6871-153">We will take this opportunity to also add a heading and a promotion section to the page.</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample12.cshtml)]

<span data-ttu-id="a6871-154">A questo punto quando si esegue l'applicazione, si vedrà la home page aggiornata con album di vendita superiore e il nostro messaggio promozionale.</span><span class="sxs-lookup"><span data-stu-id="a6871-154">Now when we run the application, we'll see our updated home page with top selling albums and our promotional message.</span></span>

![](mvc-music-store-part-10/_static/image1.jpg)

## <a name="conclusion"></a><span data-ttu-id="a6871-155">Conclusione</span><span class="sxs-lookup"><span data-stu-id="a6871-155">Conclusion</span></span>

<span data-ttu-id="a6871-156">Abbiamo visto che ASP.NET MVC rende più semplice per creare un sito Web sofisticate con accesso al database, l'appartenenza, AJAX, e così via.</span><span class="sxs-lookup"><span data-stu-id="a6871-156">We've seen that ASP.NET MVC makes it easy to create a sophisticated website with database access, membership, AJAX, etc.</span></span> <span data-ttu-id="a6871-157">abbastanza rapidamente.</span><span class="sxs-lookup"><span data-stu-id="a6871-157">pretty quickly.</span></span> <span data-ttu-id="a6871-158">Si spera in questa esercitazione è stata fornita gli strumenti che necessari per iniziare a compilare applicazioni personalizzati ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="a6871-158">Hopefully this tutorial has given you the tools you need to get started building your own ASP.NET MVC applications!</span></span>


> [!div class="step-by-step"]
> [<span data-ttu-id="a6871-159">Precedente</span><span class="sxs-lookup"><span data-stu-id="a6871-159">Previous</span></span>](mvc-music-store-part-9.md)
