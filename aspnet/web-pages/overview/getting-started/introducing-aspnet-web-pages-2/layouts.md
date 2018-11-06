---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
title: 'Introduzione a ASP.NET Web Pages: creazione di un Layout coerente | Microsoft Docs'
author: Rick-Anderson
description: Questa esercitazione illustra come usare i layout per creare un aspetto coerente per le pagine in un sito che usa ASP.NET Web Pages. Si presuppone di aver completato il...
ms.author: riande
ms.date: 05/28/2015
ms.assetid: c85ec591-f8d7-4882-b763-de6ab9f3df7a
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/layouts
msc.type: authoredcontent
ms.openlocfilehash: a6a007678d58547e9987ebda46bd08ae8aea66f7
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021179"
---
<a name="introducing-aspnet-web-pages---creating-a-consistent-layout"></a><span data-ttu-id="7d18c-104">Introduzione a pagine Web ASP.NET - creazione di un Layout coerente</span><span class="sxs-lookup"><span data-stu-id="7d18c-104">Introducing ASP.NET Web Pages - Creating a Consistent Layout</span></span>
====================
<span data-ttu-id="7d18c-105">da [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="7d18c-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="7d18c-106">Questa esercitazione illustra come usare *layout* per creare un aspetto coerente per le pagine in un sito che usa ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="7d18c-106">This tutorial shows you how to use *layouts* to create a consistent look for the pages on a site that uses ASP.NET Web Pages.</span></span> <span data-ttu-id="7d18c-107">Si presuppone di aver completato la serie attraverso [l'eliminazione dei dati di un Database in ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251584).</span><span class="sxs-lookup"><span data-stu-id="7d18c-107">It assumes you have completed the series through [Deleting Database Data in ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251584).</span></span>
> 
> <span data-ttu-id="7d18c-108">Che cosa si apprenderà come:</span><span class="sxs-lookup"><span data-stu-id="7d18c-108">What you'll learn:</span></span>
> 
> - <span data-ttu-id="7d18c-109">È una pagina di layout.</span><span class="sxs-lookup"><span data-stu-id="7d18c-109">What a layout page is.</span></span>
> - <span data-ttu-id="7d18c-110">Come combinare le pagine di layout con contenuto dinamico.</span><span class="sxs-lookup"><span data-stu-id="7d18c-110">How to combine layout pages with dynamic content.</span></span>
> - <span data-ttu-id="7d18c-111">Come passare valori a una pagina di layout.</span><span class="sxs-lookup"><span data-stu-id="7d18c-111">How to pass values to a layout page.</span></span>


## <a name="about-layouts"></a><span data-ttu-id="7d18c-112">Informazioni sui layout</span><span class="sxs-lookup"><span data-stu-id="7d18c-112">About Layouts</span></span>

<span data-ttu-id="7d18c-113">Le pagine create finora sono stati tutti complete, pagine autonome.</span><span class="sxs-lookup"><span data-stu-id="7d18c-113">The pages you've created so far have all been complete, standalone pages.</span></span> <span data-ttu-id="7d18c-114">Appartengono allo stesso sito, ma non hanno un aspetto standard o tutti gli elementi comuni.</span><span class="sxs-lookup"><span data-stu-id="7d18c-114">They all belong to the same site, but they don't have any common elements or a standard look.</span></span>

<span data-ttu-id="7d18c-115">La maggior parte dei siti hanno un aspetto coerente e layout.</span><span class="sxs-lookup"><span data-stu-id="7d18c-115">Most sites do have a consistent look and layout.</span></span> <span data-ttu-id="7d18c-116">Ad esempio, se si passa ad il [Microsoft.com/web](https://www.microsoft.com/web/) del sito e Guardatevi intorno, noterete che tutte le pagine siano conformi a un layout generale e a un tema di visual:</span><span class="sxs-lookup"><span data-stu-id="7d18c-116">For example, if you go to the [Microsoft.com/web](https://www.microsoft.com/web/) site and look around, you see that the pages all adhere to an overall layout and to a visual theme:</span></span>

![Pagina del sito Web Microsoft.com/ che mostra il layout dell'intestazione, dell'area di navigazione, area di contenuto e il piè di pagina](layouts/_static/image1.png)

<span data-ttu-id="7d18c-118">Un' *inefficiente* per creare questo layout sarebbe definire un'intestazione, barra di spostamento e il piè di pagina separatamente in ognuna delle pagine.</span><span class="sxs-lookup"><span data-stu-id="7d18c-118">An *inefficient* way to create this layout would be to define a header, navigation bar, and footer separately on each of your pages.</span></span> <span data-ttu-id="7d18c-119">È necessario duplicare lo stesso markup ogni volta.</span><span class="sxs-lookup"><span data-stu-id="7d18c-119">You'd be duplicating the same markup each time.</span></span> <span data-ttu-id="7d18c-120">Se si desidera apportare delle modifiche (ad esempio, aggiornare il piè di pagina), sarebbe necessario modificare separatamente ogni pagina.</span><span class="sxs-lookup"><span data-stu-id="7d18c-120">If you wanted to change something (for example, update the footer), you'd have to change each page separately.</span></span>

<span data-ttu-id="7d18c-121">In questi casi *pagine di layout* sono disponibili in.</span><span class="sxs-lookup"><span data-stu-id="7d18c-121">That's where *layout pages* come in.</span></span> <span data-ttu-id="7d18c-122">In ASP.NET Web Pages, è possibile definire una pagina di layout che fornisce un contenitore complessivo per le pagine nel sito.</span><span class="sxs-lookup"><span data-stu-id="7d18c-122">In ASP.NET Web Pages, you can define a layout page that provides an overall container for pages on your site.</span></span> <span data-ttu-id="7d18c-123">Pagina di layout, ad esempio, può contenere l'intestazione dell'area di navigazione e nel piè di pagina.</span><span class="sxs-lookup"><span data-stu-id="7d18c-123">For example, the layout page can contain the header, navigation area, and footer.</span></span> <span data-ttu-id="7d18c-124">La pagina di layout include un segnaposto in cui passa il contenuto principale.</span><span class="sxs-lookup"><span data-stu-id="7d18c-124">The layout page includes a placeholder where the main content goes.</span></span>

<span data-ttu-id="7d18c-125">È quindi possibile definire singole pagine di contenuto che contengono il markup e il codice solo per tale pagina.</span><span class="sxs-lookup"><span data-stu-id="7d18c-125">You can then define individual content pages that contain the markup and the code for only that page.</span></span> <span data-ttu-id="7d18c-126">Le pagine di contenuto non sono necessario essere pagine HTML complete; non ancora devono avere un `<body>` elemento.</span><span class="sxs-lookup"><span data-stu-id="7d18c-126">Content pages don't have to be complete HTML pages; they don't even have to have a `<body>` element.</span></span> <span data-ttu-id="7d18c-127">Hanno anche una riga di codice che indica quale pagina di layout che si desidera visualizzare il contenuto di ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="7d18c-127">They also have a line of code that tells ASP.NET what layout page you want to display the content in.</span></span> <span data-ttu-id="7d18c-128">Ecco un'immagine che mostra all'incirca il funzionamento di questa relazione:</span><span class="sxs-lookup"><span data-stu-id="7d18c-128">Here's a picture that shows roughly how this relationship works:</span></span>

![Diagramma concettuale che mostra due pagine di contenuto e una pagina di layout in cui si adatta](layouts/_static/image2.png)

<span data-ttu-id="7d18c-130">Questa interazione è facile da comprendere quando vederla in azione.</span><span class="sxs-lookup"><span data-stu-id="7d18c-130">This interaction is easy to understand when you see it in action.</span></span> <span data-ttu-id="7d18c-131">In questa esercitazione, sarà necessario modificare le pagine di film per usare un layout.</span><span class="sxs-lookup"><span data-stu-id="7d18c-131">In this tutorial, you'll change your movies pages to use a layout.</span></span>

## <a name="adding-a-layout-page"></a><span data-ttu-id="7d18c-132">Aggiunta di una pagina Layout</span><span class="sxs-lookup"><span data-stu-id="7d18c-132">Adding a Layout Page</span></span>

<span data-ttu-id="7d18c-133">Inizierà creando una pagina di layout che consente di definire un layout di pagina tipiche con un'intestazione, piè di pagina e un'area per il contenuto principale.</span><span class="sxs-lookup"><span data-stu-id="7d18c-133">You'll start by creating a layout page that defines a typical page layout with a header, footer, and an area for the main content.</span></span> <span data-ttu-id="7d18c-134">Nel sito WebPagesMovies, aggiungere una pagina con estensione CSHTML denominata  *\_layout. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="7d18c-134">In the WebPagesMovies site, add a CSHTML page named *\_Layout.cshtml*.</span></span>

<span data-ttu-id="7d18c-135">Il carattere di sottolineatura iniziale ( `_` ) carattere è significativo.</span><span class="sxs-lookup"><span data-stu-id="7d18c-135">The leading underscore ( `_` ) character is significant.</span></span> <span data-ttu-id="7d18c-136">Se il nome di una pagina inizia con un carattere di sottolineatura, ASP.NET non invierà direttamente tale pagina nel browser.</span><span class="sxs-lookup"><span data-stu-id="7d18c-136">If a page's name starts with an underscore, ASP.NET won't directly send that page to the browser.</span></span> <span data-ttu-id="7d18c-137">Questa convenzione consente di definire le pagine che sono necessarie per il sito, ma che gli utenti non devono essere in grado di richiedere direttamente.</span><span class="sxs-lookup"><span data-stu-id="7d18c-137">This convention lets you define pages that are required for your site but that users shouldn't be able to request directly.</span></span>

<span data-ttu-id="7d18c-138">Sostituire il contenuto della pagina con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="7d18c-138">Replace the content in the page with the following:</span></span>

[!code-html[Main](layouts/samples/sample1.html)]

<span data-ttu-id="7d18c-139">Come si può notare, questo markup è semplicemente codice HTML di cui viene utilizzata `<div>` elementi per definire più di tre sezioni nella pagina più uno `<div>` elemento per contenere le tre sezioni.</span><span class="sxs-lookup"><span data-stu-id="7d18c-139">As you can see, this markup is just HTML that uses `<div>` elements to define three sections in the page plus one more `<div>` element to hold the three sections.</span></span> <span data-ttu-id="7d18c-140">Il piè di pagina contiene un frammento di codice Razor: `@DateTime.Now.Year`, che verrà eseguito il rendering dell'anno corrente nel percorso specificato nella pagina.</span><span class="sxs-lookup"><span data-stu-id="7d18c-140">The footer contains a bit of Razor code: `@DateTime.Now.Year`, which will render the current year at that location in the page.</span></span>

<span data-ttu-id="7d18c-141">Si noti che è presente un collegamento a un foglio di stile denominato *Movies.css*.</span><span class="sxs-lookup"><span data-stu-id="7d18c-141">Notice that there's a link to a style sheet named *Movies.css*.</span></span> <span data-ttu-id="7d18c-142">Il foglio di stile è dove verranno definiti i dettagli del layout fisico degli elementi.</span><span class="sxs-lookup"><span data-stu-id="7d18c-142">The style sheet is where the details of the physical layout of the elements will be defined.</span></span> <span data-ttu-id="7d18c-143">Che verrà creata più avanti.</span><span class="sxs-lookup"><span data-stu-id="7d18c-143">You'll create that in a moment.</span></span>

<span data-ttu-id="7d18c-144">La funzionalità uniche in questo  *\_layout. cshtml* pagina è il `@Render.Body()` riga.</span><span class="sxs-lookup"><span data-stu-id="7d18c-144">The only unusual feature in this *\_Layout.cshtml* page is the `@Render.Body()` line.</span></span> <span data-ttu-id="7d18c-145">Che è un segnaposto dove verrà salvato il contenuto quando questo layout è unito a un'altra pagina.</span><span class="sxs-lookup"><span data-stu-id="7d18c-145">That's the placeholder where the content will go when this layout is merged with another page.</span></span>

## <a name="adding-a-css-file"></a><span data-ttu-id="7d18c-146">Aggiunta di un File con estensione CSS</span><span class="sxs-lookup"><span data-stu-id="7d18c-146">Adding a .css File</span></span>

<span data-ttu-id="7d18c-147">Il modo migliore per definire la disposizione effettiva (vale a dire, aspetto) degli elementi nella pagina è usare le regole di fogli di stile.</span><span class="sxs-lookup"><span data-stu-id="7d18c-147">The preferred way to define the actual arrangement (that is, appearance) of elements on the page is to use cascading style sheet (CSS) rules.</span></span> <span data-ttu-id="7d18c-148">Quindi si creerà una *CSS* file con le regole per il nuovo layout.</span><span class="sxs-lookup"><span data-stu-id="7d18c-148">So you'll create a *.css* file that has the rules for your new layout.</span></span>

<span data-ttu-id="7d18c-149">In WebMatrix, selezionare la radice del sito.</span><span class="sxs-lookup"><span data-stu-id="7d18c-149">In WebMatrix, select the root of your site.</span></span> <span data-ttu-id="7d18c-150">Quindi nel **file** della scheda della barra multifunzione, fare clic sulla freccia sotto il **New** pulsante e quindi fare clic su **nuova cartella**.</span><span class="sxs-lookup"><span data-stu-id="7d18c-150">Then in the **Files** tab of the ribbon, click the arrow under the **New** button and then click **New Folder**.</span></span>

![L'opzione "Nuova cartella" in nuovi nella barra multifunzione.](layouts/_static/image3.png)

<span data-ttu-id="7d18c-152">Denominare la nuova cartella *stili*.</span><span class="sxs-lookup"><span data-stu-id="7d18c-152">Name the new folder *Styles*.</span></span>

![Denominazione nella nuova cartella 'Stili'](layouts/_static/image4.png)

<span data-ttu-id="7d18c-154">All'interno di nuove *stili* cartella, creare un file denominato *Movies.css*.</span><span class="sxs-lookup"><span data-stu-id="7d18c-154">Inside the new *Styles* folder, create a file named *Movies.css*.</span></span>

![Creazione di un nuovo file Movies.css](layouts/_static/image5.png)

<span data-ttu-id="7d18c-156">Sostituire il contenuto della nuova *CSS* file con il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="7d18c-156">Replace the contents of the new *.css* file with the following:</span></span>

[!code-css[Main](layouts/samples/sample2.css)]

<span data-ttu-id="7d18c-157">Non parliamo di gran parte di queste regole CSS, tranne a considerare due aspetti.</span><span class="sxs-lookup"><span data-stu-id="7d18c-157">We won't say much about these CSS rules, except to note two things.</span></span> <span data-ttu-id="7d18c-158">Una è che oltre a impostare i tipi di carattere e dimensioni, le regole di usano il posizionamento assoluto per stabilire il percorso di area del contenuto principale, intestazione, piè di pagina.</span><span class="sxs-lookup"><span data-stu-id="7d18c-158">One is that in addition to setting fonts and sizes, the rules use absolute positioning to establish the location of the header, footer, and main content area.</span></span> <span data-ttu-id="7d18c-159">Se si ha familiarità con il posizionamento in CSS, è possibile leggere il [posizionamento CSS](http://www.w3schools.com/css/css_positioning.asp) esercitazioni nel sito W3Schools.</span><span class="sxs-lookup"><span data-stu-id="7d18c-159">If you're new to positioning in CSS, you can read the [CSS Positioning](http://www.w3schools.com/css/css_positioning.asp) tutorial at the W3Schools site.</span></span>

<span data-ttu-id="7d18c-160">L'altra cosa da notare è che nella parte inferiore, è stato copiato le regole di stile che facevano originariamente definito singolarmente tramite il *Movies.cshtml* file.</span><span class="sxs-lookup"><span data-stu-id="7d18c-160">The other thing to note is that at the bottom, we've copied the style rules that were originally defined individually in the *Movies.cshtml* file.</span></span> <span data-ttu-id="7d18c-161">Queste regole sono state usate nel [Introduzione alla visualizzazione dei dati da utilizzando ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251580) esercitazione per rendere il `WebGrid` helper il rendering del markup che l'aggiunta di strisce alla tabella.</span><span class="sxs-lookup"><span data-stu-id="7d18c-161">These rules were used in the [Introduction to Displaying Data by Using ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251580) tutorial to make the `WebGrid` helper render markup that added stripes to the table.</span></span> <span data-ttu-id="7d18c-162">(Se si intende usare un *CSS* file per le definizioni di stile, è possibile inserire anche le regole di stile per l'intero sito in essa.)</span><span class="sxs-lookup"><span data-stu-id="7d18c-162">(If you're going to use a *.css* file for style definitions, you might as well put the style rules for the whole site in it.)</span></span>

## <a name="updating-the-movies-file-to-use-the-layout"></a><span data-ttu-id="7d18c-163">L'aggiornamento del File di film per l'uso del Layout</span><span class="sxs-lookup"><span data-stu-id="7d18c-163">Updating the Movies File to Use the Layout</span></span>

<span data-ttu-id="7d18c-164">A questo punto è possibile aggiornare i file esistenti nel sito per usare il nuovo layout.</span><span class="sxs-lookup"><span data-stu-id="7d18c-164">Now you can update the existing files in your site to use the new layout.</span></span> <span data-ttu-id="7d18c-165">Aprire il *Movies.cshtml* file.</span><span class="sxs-lookup"><span data-stu-id="7d18c-165">Open the *Movies.cshtml* file.</span></span> <span data-ttu-id="7d18c-166">Nella parte superiore, come la prima riga del codice, aggiungere quanto segue:</span><span class="sxs-lookup"><span data-stu-id="7d18c-166">At the top, as the first line of code, add the following:</span></span>

[!code-csharp[Main](layouts/samples/sample3.cs)]

<span data-ttu-id="7d18c-167">In questo modo inizia ora la pagina:</span><span class="sxs-lookup"><span data-stu-id="7d18c-167">The page now starts out this way:</span></span>

[!code-cshtml[Main](layouts/samples/sample4.cshtml?highlight=2)]

<span data-ttu-id="7d18c-168">Questa riga di codice indica ad ASP.NET che, quando la *film* pagina è in esecuzione, deve essere unito con la  *\_layout. cshtml* file.</span><span class="sxs-lookup"><span data-stu-id="7d18c-168">This one line of code tells ASP.NET that when the *Movies* page runs, it should be merged with the *\_Layout.cshtml* file.</span></span>

<span data-ttu-id="7d18c-169">Poiché il *Movies.cshtml* file Usa ora una pagina di layout, è possibile rimuovere il markup dalle *Movies.cshtml* pagina che ha preso in considerazione dal  *\_layout. cshtml*file.</span><span class="sxs-lookup"><span data-stu-id="7d18c-169">Since the *Movies.cshtml* file now uses a layout page, you can remove the markup from the *Movies.cshtml* page that's taken care of by the *\_Layout.cshtml* file.</span></span> <span data-ttu-id="7d18c-170">Estrarre il `<!DOCTYPE>`, `<html>`, e `<body>` di apertura e chiusura di tag.</span><span class="sxs-lookup"><span data-stu-id="7d18c-170">Take out the `<!DOCTYPE>`, `<html>`, and `<body>` opening and closing tags.</span></span> <span data-ttu-id="7d18c-171">Interessi l'intera `<head>` elemento e il relativo contenuto, che include le regole di stile per la griglia, poiché tali regole è ora disponibile in un *CSS* file.</span><span class="sxs-lookup"><span data-stu-id="7d18c-171">Take out the entire `<head>` element and its contents, which includes the style rules for the grid, since you've now got those rules in a *.css* file.</span></span> <span data-ttu-id="7d18c-172">Anche se siete, modificare l'oggetto esistente `<h1>` elemento da un' `<h2>` elemento; è necessario un `<h1>` elemento nella pagina di layout già.</span><span class="sxs-lookup"><span data-stu-id="7d18c-172">While you're at it, change the existing `<h1>` element to an `<h2>` element; you have an `<h1>` element in the layout page already.</span></span> <span data-ttu-id="7d18c-173">Modifica il `<h2>` testo per "Elenco di film".</span><span class="sxs-lookup"><span data-stu-id="7d18c-173">Change the `<h2>` text to "List Movies".</span></span>

<span data-ttu-id="7d18c-174">È normalmente non sarebbe necessario rendere questi tipi di modifiche in una pagina di contenuto.</span><span class="sxs-lookup"><span data-stu-id="7d18c-174">Normally you wouldn't have to make these sorts of changes in a content page.</span></span> <span data-ttu-id="7d18c-175">Quando il sito iniziare con una pagina di layout, si creano le pagine di contenuto senza tutti questi elementi per iniziare.</span><span class="sxs-lookup"><span data-stu-id="7d18c-175">When you start your site out with a layout page, you create content pages without all these elements to begin with.</span></span> <span data-ttu-id="7d18c-176">In questo caso, tuttavia, si sta convertendo una pagina autonoma in uno che utilizza un layout, pertanto non c'è un po' di pulizia.</span><span class="sxs-lookup"><span data-stu-id="7d18c-176">In this case, though, you're converting a standalone page to one that uses a layout, so there's a bit of cleanup.</span></span>

<span data-ttu-id="7d18c-177">Quando hai finito, il *Movies.cshtml* pagina avrà un aspetto simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="7d18c-177">When you're finished, the *Movies.cshtml* page will look like the following:</span></span>

[!code-cshtml[Main](layouts/samples/sample5.cshtml)]

### <a name="testing-the-layout"></a><span data-ttu-id="7d18c-178">Il Layout di test</span><span class="sxs-lookup"><span data-stu-id="7d18c-178">Testing the Layout</span></span>

<span data-ttu-id="7d18c-179">A questo punto è possibile visualizzare il layout di un aspetto analogo.</span><span class="sxs-lookup"><span data-stu-id="7d18c-179">Now you can see what the layout looks like.</span></span> <span data-ttu-id="7d18c-180">In WebMatrix, fare doppio clic il *Movies.cshtml* pagina e selezionare **Avvia nel browser**.</span><span class="sxs-lookup"><span data-stu-id="7d18c-180">In WebMatrix, right-click the *Movies.cshtml* page and select **Launch in browser**.</span></span> <span data-ttu-id="7d18c-181">Il browser visualizza la pagina, si differenzia da questa pagina:</span><span class="sxs-lookup"><span data-stu-id="7d18c-181">When the browser displays the page, it looks like this page:</span></span>

![Pagina Movies sottoposta a rendering utilizzando un layout](layouts/_static/image6.png)

<span data-ttu-id="7d18c-183">ASP.NET ha unito il contenuto della pagina Movies.cshtml nel  *\_layout. cshtml* pagina a destra dove il `RenderBody` metodo è.</span><span class="sxs-lookup"><span data-stu-id="7d18c-183">ASP.NET has merged the content of the Movies.cshtml page into the *\_Layout.cshtml* page right where the `RenderBody` method is.</span></span> <span data-ttu-id="7d18c-184">E ovviamente il  *\_layout. cshtml* pagina riferimenti a un *CSS* file che definisce l'aspetto della pagina.</span><span class="sxs-lookup"><span data-stu-id="7d18c-184">And of course the *\_Layout.cshtml* page references a *.css* file that defines the look of the page.</span></span>

## <a name="updating-the-addmovie-page-to-use-the-layout"></a><span data-ttu-id="7d18c-185">Aggiornamento della pagina AddMovie per l'uso del Layout</span><span class="sxs-lookup"><span data-stu-id="7d18c-185">Updating the AddMovie Page to Use the Layout</span></span>

<span data-ttu-id="7d18c-186">Il reale vantaggio di layout è che è possibile usarli per tutte le pagine nel sito.</span><span class="sxs-lookup"><span data-stu-id="7d18c-186">The real benefit of layouts is that you can use them for all the pages in your site.</span></span> <span data-ttu-id="7d18c-187">Aprire il *AddMovie.cshtml* pagina.</span><span class="sxs-lookup"><span data-stu-id="7d18c-187">Open the *AddMovie.cshtml* page.</span></span>

<span data-ttu-id="7d18c-188">Si ricorderà che il *AddMovie.cshtml* pagina originariamente rappresentava alcune regole CSS in esso per definire l'aspetto dei messaggi di errore di convalida.</span><span class="sxs-lookup"><span data-stu-id="7d18c-188">You might remember that the *AddMovie.cshtml* page originally had some CSS rules in it to define the look of validation error messages.</span></span> <span data-ttu-id="7d18c-189">Poiché è necessario un *CSS* file per ora il sito, è possibile spostare tali regole per il *CSS* file.</span><span class="sxs-lookup"><span data-stu-id="7d18c-189">Since you have a *.css* file for your site now, you can move those rules to the *.css* file.</span></span> <span data-ttu-id="7d18c-190">Li rimuove dal *AddMovie.cshtml* del file e aggiungerli alla fine della *Movies.css* file.</span><span class="sxs-lookup"><span data-stu-id="7d18c-190">Remove them from the *AddMovie.cshtml* file and add them to the bottom of the *Movies.css* file.</span></span> <span data-ttu-id="7d18c-191">Si siano spostando le regole seguenti:</span><span class="sxs-lookup"><span data-stu-id="7d18c-191">You are moving the following rules:</span></span>

[!code-css[Main](layouts/samples/sample6.css)]

<span data-ttu-id="7d18c-192">Ora apportare la stessa natura delle modifiche in *AddMovie.cshtml* che è stato fatto per *Movies.cshtml* , ovvero aggiungere `Layout="~/_Layout.cshtml;` e rimuovere il markup HTML che si trova ora estraneo.</span><span class="sxs-lookup"><span data-stu-id="7d18c-192">Now make the same sorts of changes in *AddMovie.cshtml* that you did for *Movies.cshtml* — add `Layout="~/_Layout.cshtml;` and remove the HTML markup that's now extraneous.</span></span> <span data-ttu-id="7d18c-193">Modificare l'elemento `<h1>` in `<h2>`.</span><span class="sxs-lookup"><span data-stu-id="7d18c-193">Change the `<h1>` element to `<h2>`.</span></span> <span data-ttu-id="7d18c-194">Al termine, la pagina avrà un aspetto simile all'esempio:</span><span class="sxs-lookup"><span data-stu-id="7d18c-194">When you're done, the page will look like this example:</span></span>

[!code-cshtml[Main](layouts/samples/sample7.cshtml)]

<span data-ttu-id="7d18c-195">Eseguire la pagina.</span><span class="sxs-lookup"><span data-stu-id="7d18c-195">Run the page.</span></span> <span data-ttu-id="7d18c-196">A questo punto sembra questa illustrazione:</span><span class="sxs-lookup"><span data-stu-id="7d18c-196">Now it looks like this illustration:</span></span>

!['Aggiungi film' pagina sottoposta a rendering utilizzando un layout](layouts/_static/image7.png)

<span data-ttu-id="7d18c-198">Si desidera apportare modifiche analoghe alle pagine del sito, ovvero *EditMovie.cshtml* e *DeleteMovie.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="7d18c-198">You want to make similar changes to the pages in the site — *EditMovie.cshtml* and *DeleteMovie.cshtml*.</span></span> <span data-ttu-id="7d18c-199">Tuttavia, prima di procedere, è possibile apportare un'altra modifica al layout che rende un po' più flessibile.</span><span class="sxs-lookup"><span data-stu-id="7d18c-199">However, before you do, you can make another change to the layout that makes it a little more flexible.</span></span>

## <a name="passing-title-information-to-the-layout-page"></a><span data-ttu-id="7d18c-200">Informazioni sui titoli di passare alla pagina di Layout</span><span class="sxs-lookup"><span data-stu-id="7d18c-200">Passing Title Information to the Layout Page</span></span>

<span data-ttu-id="7d18c-201">Il  *\_layout. cshtml* dispone di pagina in cui è stato creato un `<title>` elemento che è impostato su "Movie area sito personale".</span><span class="sxs-lookup"><span data-stu-id="7d18c-201">The *\_Layout.cshtml* page that you created has a `<title>` element that's set to "My Movie Site".</span></span> <span data-ttu-id="7d18c-202">La maggior parte dei browser di visualizzare il contenuto di questo elemento come testo in una scheda:</span><span class="sxs-lookup"><span data-stu-id="7d18c-202">Most browsers display the content of this element as the text on a tab:</span></span>

![La pagina &lt;titolo&gt; elemento visualizzato in una scheda del browser](layouts/_static/image8.png)

<span data-ttu-id="7d18c-204">Queste informazioni di titolo sono generiche.</span><span class="sxs-lookup"><span data-stu-id="7d18c-204">This title information is generic.</span></span> <span data-ttu-id="7d18c-205">Si supponga che il testo del titolo per essere più specifici per la pagina corrente.</span><span class="sxs-lookup"><span data-stu-id="7d18c-205">Suppose that you want the title text to be more specific to the current page.</span></span> <span data-ttu-id="7d18c-206">(Il testo del titolo viene usato anche dai motori di ricerca per stabilire quale sia la pagina sulle.) È possibile passare informazioni da una pagina contenuto, ad esempio *Movies.cshtml* oppure *AddMovie.cshtml* alla pagina di layout e quindi usare tali informazioni per personalizzare quali la pagina di layout esegue il rendering.</span><span class="sxs-lookup"><span data-stu-id="7d18c-206">(The title text is also used by search engines to determine what your page is about.) You can pass information from a content page like *Movies.cshtml* or *AddMovie.cshtml* to the layout page, and then use that information to customize what the layout page renders.</span></span>

<span data-ttu-id="7d18c-207">Aprire il *Movies.cshtml* pagina nuovamente.</span><span class="sxs-lookup"><span data-stu-id="7d18c-207">Open the *Movies.cshtml* page again.</span></span> <span data-ttu-id="7d18c-208">Nel codice nella parte superiore, aggiungere la riga seguente:</span><span class="sxs-lookup"><span data-stu-id="7d18c-208">In the code at the top, add the following line:</span></span>

[!code-csharp[Main](layouts/samples/sample8.cs)]

<span data-ttu-id="7d18c-209">Il `Page` l'oggetto è disponibile in tutti i *cshtml* pagine ed è per questo scopo, vale a dire per condividere informazioni tra una pagina e il relativo layout.</span><span class="sxs-lookup"><span data-stu-id="7d18c-209">The `Page` object is available on all *.cshtml* pages and is for this purpose, namely to share information between a page and its layout.</span></span>

<span data-ttu-id="7d18c-210">Aprire il<em>\_layout. cshtml</em> pagina.</span><span class="sxs-lookup"><span data-stu-id="7d18c-210">Open the<em>\_Layout.cshtml</em> page.</span></span> <span data-ttu-id="7d18c-211">Modifica il `<title>` elemento in modo che l'it è simile a questo markup:</span><span class="sxs-lookup"><span data-stu-id="7d18c-211">Change the `<title>` element so that it looks like this markup:</span></span>

[!code-html[Main](layouts/samples/sample9.html)]

<span data-ttu-id="7d18c-212">Questo codice esegue il rendering di qualsiasi oggetto presente nel `Page.Title` proprietà a destra nel percorso specificato nella pagina.</span><span class="sxs-lookup"><span data-stu-id="7d18c-212">This code renders whatever is in the `Page.Title` property right at that location in the page.</span></span>

<span data-ttu-id="7d18c-213">Eseguire la *Movies.cshtml* pagina.</span><span class="sxs-lookup"><span data-stu-id="7d18c-213">Run the *Movies.cshtml* page.</span></span> <span data-ttu-id="7d18c-214">Questa volta la scheda del browser mostra ciò che è stato passato come valore di `Page.Title`:</span><span class="sxs-lookup"><span data-stu-id="7d18c-214">This time the browser tab shows what you passed as the value of `Page.Title`:</span></span>

![Una scheda del browser che mostra il titolo creato in modo dinamico](layouts/_static/image9.png)

<span data-ttu-id="7d18c-216">Se si desidera, consente di visualizzare il codice sorgente della pagina nel browser.</span><span class="sxs-lookup"><span data-stu-id="7d18c-216">If you want, view the page source in the browser.</span></span> <span data-ttu-id="7d18c-217">È possibile notare che il `<title>` elemento viene sottoposto a rendering come `<title>List Movies</title>`.</span><span class="sxs-lookup"><span data-stu-id="7d18c-217">You can see that the `<title>` element is rendered as `<title>List Movies</title>`.</span></span>

> [!TIP] 
> 
> <span data-ttu-id="7d18c-218">**Oggetto Page**</span><span class="sxs-lookup"><span data-stu-id="7d18c-218">**The Page Object**</span></span>
> 
> <span data-ttu-id="7d18c-219">Una funzionalità utile dei `Page` è costituito da un oggetto dinamico, ovvero il `Title` proprietà non è un nome fisso o riservato.</span><span class="sxs-lookup"><span data-stu-id="7d18c-219">A useful feature of `Page` is that it's a dynamic object — the `Title` property is not a fixed or reserved name.</span></span> <span data-ttu-id="7d18c-220">È possibile usare *eventuali* nome per il valore di `Page` oggetto.</span><span class="sxs-lookup"><span data-stu-id="7d18c-220">You can use *any* name for a value of the `Page` object.</span></span> <span data-ttu-id="7d18c-221">Ad esempio, si può facilmente aver passato il titolo usando una proprietà denominata `Page.CurrentName` o `Page.MyPage`.</span><span class="sxs-lookup"><span data-stu-id="7d18c-221">For example, you could as easily have passed the title by using a property named `Page.CurrentName` or `Page.MyPage`.</span></span> <span data-ttu-id="7d18c-222">L'unica restrizione è che il nome deve seguire le regole normali per le proprietà che possono essere denominate.</span><span class="sxs-lookup"><span data-stu-id="7d18c-222">The only restriction is that the name has to follow the normal rules for what properties can be named.</span></span> <span data-ttu-id="7d18c-223">(Ad esempio, il nome non può contenere uno spazio).</span><span class="sxs-lookup"><span data-stu-id="7d18c-223">(For example, the name can't contain a space.)</span></span>
> 
> <span data-ttu-id="7d18c-224">È possibile passare qualsiasi numero di valori usando il `Page` oggetto.</span><span class="sxs-lookup"><span data-stu-id="7d18c-224">You can pass any number of values by using the `Page` object.</span></span> <span data-ttu-id="7d18c-225">Se si desidera passare informazioni relative al filmato alla pagina di layout, è possibile passare i valori usando ad esempio `Page.MovieTitle` e `Page.Genre` e `Page.MovieYear`.</span><span class="sxs-lookup"><span data-stu-id="7d18c-225">If you wanted to pass movie information to the layout page, you could pass values by using something like `Page.MovieTitle` and `Page.Genre` and `Page.MovieYear`.</span></span> <span data-ttu-id="7d18c-226">In alternativa, tutti gli altri nomi che è ideato per archiviare le informazioni. L'unico requisito, ovvero che è probabilmente ovvio, è che è necessario usare gli stessi nomi nella pagina di contenuto e nella pagina di layout.</span><span class="sxs-lookup"><span data-stu-id="7d18c-226">(Or any other names that you invented to store the information.) The only requirement — which is probably obvious — is that you have to use the same names in the content page and the layout page.</span></span>
> 
> <span data-ttu-id="7d18c-227">Le informazioni passate tramite il `Page` oggetto non è limitato al solo con il testo da visualizzare nella pagina di layout.</span><span class="sxs-lookup"><span data-stu-id="7d18c-227">The information you pass by using the `Page` object isn't limited to just text to display on the layout page.</span></span> <span data-ttu-id="7d18c-228">È possibile passare un valore alla pagina di layout, e quindi codice nella pagina di layout può usare il valore per decidere se visualizzare una sezione della pagina, quali *CSS* per l'uso di file e così via.</span><span class="sxs-lookup"><span data-stu-id="7d18c-228">You can pass a value to the layout page, and then code in the layout page can use the value to decide whether to display a section of the page, what *.css* file to use, and so on.</span></span> <span data-ttu-id="7d18c-229">I valori passati nella `Page` oggetto sono, ad esempio eventuali altri valori che verrà usata nel codice.</span><span class="sxs-lookup"><span data-stu-id="7d18c-229">The values you pass in the `Page` object are like any other values that you use in code.</span></span> <span data-ttu-id="7d18c-230">È sufficiente che i valori hanno origine nella pagina di contenuto e vengono passati alla pagina di layout.</span><span class="sxs-lookup"><span data-stu-id="7d18c-230">It's just that the values originate in the content page and are passed to the layout page.</span></span>


<span data-ttu-id="7d18c-231">Aprire il *AddMovie.cshtml* pagina e aggiungere una riga all'inizio del codice che fornisce un titolo per il *AddMovie.cshtml* pagina:</span><span class="sxs-lookup"><span data-stu-id="7d18c-231">Open the *AddMovie.cshtml* page and add a line to the top of the code that provides a title for the *AddMovie.cshtml* page:</span></span>

[!code-csharp[Main](layouts/samples/sample10.cs)]

<span data-ttu-id="7d18c-232">Eseguire la *AddMovie.cshtml* pagina.</span><span class="sxs-lookup"><span data-stu-id="7d18c-232">Run the *AddMovie.cshtml* page.</span></span> <span data-ttu-id="7d18c-233">Viene visualizzato il nuovo titolo:</span><span class="sxs-lookup"><span data-stu-id="7d18c-233">You see the new title there:</span></span>

![Una scheda del browser che mostra il titolo 'Aggiungi film' creato in modo dinamico](layouts/_static/image10.png)

## <a name="updating-the-remaining-pages-to-use-the-layout"></a><span data-ttu-id="7d18c-235">Aggiornamento delle pagine rimanenti per l'uso del Layout</span><span class="sxs-lookup"><span data-stu-id="7d18c-235">Updating the Remaining Pages to Use the Layout</span></span>

<span data-ttu-id="7d18c-236">A questo punto sarà possibile completare le pagine rimanenti presenti nel sito in modo che utilizzino il nuovo layout.</span><span class="sxs-lookup"><span data-stu-id="7d18c-236">Now you can finish the remaining pages in your site so that they use the new layout.</span></span> <span data-ttu-id="7d18c-237">Aprire *EditMovie.cshtml* e *DeleteMovie.cshtml* a sua volta e apportare le stesse modifiche in ognuna.</span><span class="sxs-lookup"><span data-stu-id="7d18c-237">Open *EditMovie.cshtml* and *DeleteMovie.cshtml* in turn and make the same changes in each.</span></span>

<span data-ttu-id="7d18c-238">Aggiungere la riga di codice che si collega alla pagina di layout:</span><span class="sxs-lookup"><span data-stu-id="7d18c-238">Add the line of code that links to the layout page:</span></span>

[!code-csharp[Main](layouts/samples/sample11.cs)]

<span data-ttu-id="7d18c-239">Aggiungere una riga per impostare il titolo della pagina:</span><span class="sxs-lookup"><span data-stu-id="7d18c-239">Add a line to set the title of the page:</span></span>

[!code-csharp[Main](layouts/samples/sample12.cs)]

<span data-ttu-id="7d18c-240">oppure:</span><span class="sxs-lookup"><span data-stu-id="7d18c-240">or:</span></span>

[!code-csharp[Main](layouts/samples/sample13.cs)]

<span data-ttu-id="7d18c-241">Rimuovere tutto il markup HTML estraneo, lasciare in pratica, solo i bit che si trovano all'interno di `<body>` elemento (più il blocco di codice nella parte superiore).</span><span class="sxs-lookup"><span data-stu-id="7d18c-241">Remove all the extraneous HTML markup — basically, leave only the bits that are inside the `<body>` element (plus the code block at the top).</span></span>

<span data-ttu-id="7d18c-242">Modifica il `<h1>` elemento sia una `<h2>` elemento.</span><span class="sxs-lookup"><span data-stu-id="7d18c-242">Change the `<h1>` element to be an `<h2>` element.</span></span>

<span data-ttu-id="7d18c-243">Dopo avere apportato queste modifiche, il test di ogni e assicurarsi che vengano visualizzate correttamente e che il titolo sia corretto.</span><span class="sxs-lookup"><span data-stu-id="7d18c-243">When you've made these changes, test each and make sure that it's displaying properly and that the title is correct.</span></span>

## <a name="parting-thoughts-about-layout-pages"></a><span data-ttu-id="7d18c-244">PROD143 idee sulle pagine di Layout</span><span class="sxs-lookup"><span data-stu-id="7d18c-244">Parting Thoughts About Layout Pages</span></span>

<span data-ttu-id="7d18c-245">In questa esercitazione è stata creata una  *\_layout. cshtml* pagina e usato la `RenderBody` metodo per unire il contenuto da un'altra pagina.</span><span class="sxs-lookup"><span data-stu-id="7d18c-245">In this tutorial you created a *\_Layout.cshtml* page and used the `RenderBody` method to merge content from another page.</span></span> <span data-ttu-id="7d18c-246">Che è il modello di base per l'uso di layout nelle pagine Web.</span><span class="sxs-lookup"><span data-stu-id="7d18c-246">That's the basic pattern for using layouts in Web Pages.</span></span>

<span data-ttu-id="7d18c-247">Pagine di layout hanno funzionalità aggiuntive che non sono stati trattati qui.</span><span class="sxs-lookup"><span data-stu-id="7d18c-247">Layout pages have additional features that we didn't cover here.</span></span> <span data-ttu-id="7d18c-248">Ad esempio, è possibile annidare le pagine di layout, ovvero una pagina di layout può a sua volta fare riferimento a un altro.</span><span class="sxs-lookup"><span data-stu-id="7d18c-248">For example, you can nest layout pages — one layout page can in turn reference another.</span></span> <span data-ttu-id="7d18c-249">Layout annidati può essere utile se si lavora con le sezioni di un sito che richiedono layout diversi.</span><span class="sxs-lookup"><span data-stu-id="7d18c-249">Nested layouts can be useful if you're working with subsections of a site that require different layouts.</span></span> <span data-ttu-id="7d18c-250">È anche possibile usare metodi aggiuntivi (ad esempio, `RenderSection`) per configurare denominato sezioni nella pagina di layout.</span><span class="sxs-lookup"><span data-stu-id="7d18c-250">You can also use additional methods (for example, `RenderSection`) to set up named sections in the layout page.</span></span>

<span data-ttu-id="7d18c-251">La combinazione di pagine di layout e *CSS* file è potente.</span><span class="sxs-lookup"><span data-stu-id="7d18c-251">The combination of layout pages and *.css* files is powerful.</span></span> <span data-ttu-id="7d18c-252">Come si vedrà nella serie di esercitazioni in avanti, in WebMatrix è possibile creare un sito basato su un *modello*, che offre un sito dotato di funzionalità precompilate in esso.</span><span class="sxs-lookup"><span data-stu-id="7d18c-252">As you'll see in the next tutorial series, in WebMatrix you can create a site based on a *template*, which gives you a site that has prebuilt functionality in it.</span></span> <span data-ttu-id="7d18c-253">I modelli usano buona pagine di layout e CSS per creare siti che aspetto accattivante e che dispongono di funzionalità come i menu.</span><span class="sxs-lookup"><span data-stu-id="7d18c-253">The templates make good use of layout pages and CSS to create sites that look great and that have features like menus.</span></span> <span data-ttu-id="7d18c-254">Ecco uno screenshot della pagina iniziale da un sito basato su un modello, che mostra le funzionalità che utilizzano pagine di layout e CSS:</span><span class="sxs-lookup"><span data-stu-id="7d18c-254">Here's a screenshot of the home page from a site based on a template, showing features that use layout pages and CSS:</span></span>

![Layout creata dal modello di sito WebMatrix che mostra l'intestazione dell'area di navigazione, area di contenuto, sezione facoltativa e i collegamenti di accesso](layouts/_static/image11.png)

## <a name="complete-listing-for-movie-page-updated-to-use-a-layout-page"></a><span data-ttu-id="7d18c-256">Elenco completo per la pagina di film (aggiornata per usare una pagina di Layout)</span><span class="sxs-lookup"><span data-stu-id="7d18c-256">Complete Listing for Movie Page (Updated to Use a Layout Page)</span></span>

[!code-cshtml[Main](layouts/samples/sample14.cshtml)]

## <a name="complete-page-listing-for-add-movie-page-updated-for-layout"></a><span data-ttu-id="7d18c-257">Pagina completata con l'elenco per aggiungere pagina film (aggiornata per il Layout)</span><span class="sxs-lookup"><span data-stu-id="7d18c-257">Complete Page Listing for Add Movie Page (Updated for Layout)</span></span>

[!code-cshtml[Main](layouts/samples/sample15.cshtml)]

## <a name="complete-page-listing-for-delete-movie-page-updated-for-layout"></a><span data-ttu-id="7d18c-258">Completare l'elenco di pagina per pagina film Delete (aggiornata per il Layout)</span><span class="sxs-lookup"><span data-stu-id="7d18c-258">Complete Page Listing for Delete Movie Page (Updated for Layout)</span></span>

[!code-cshtml[Main](layouts/samples/sample16.cshtml)]

## <a name="complete-page-listing-for-edit-movie-page-updated-for-layout"></a><span data-ttu-id="7d18c-259">Completare l'elenco di pagina per pagina film modifica (aggiornata per il Layout)</span><span class="sxs-lookup"><span data-stu-id="7d18c-259">Complete Page Listing for Edit Movie Page (Updated for Layout)</span></span>

[!code-cshtml[Main](layouts/samples/sample17.cshtml)]

## <a name="coming-up-next"></a><span data-ttu-id="7d18c-260">In arrivo</span><span class="sxs-lookup"><span data-stu-id="7d18c-260">Coming Up Next</span></span>

<span data-ttu-id="7d18c-261">Nella prossima esercitazione, si apprenderà come pubblicare il sito Internet in modo che tutti gli utenti possono visualizzarlo.</span><span class="sxs-lookup"><span data-stu-id="7d18c-261">In the next tutorial, you'll learn how to publish your site to the Internet so everyone can see it.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7d18c-262">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="7d18c-262">Additional Resources</span></span>

- <span data-ttu-id="7d18c-263">[Creazione di un aspetto coerente](https://go.microsoft.com/fwlink/?LinkID=202891) , ovvero un articolo che fornisce maggiori dettagli sull'uso dei layout.</span><span class="sxs-lookup"><span data-stu-id="7d18c-263">[Creating a Consistent Look](https://go.microsoft.com/fwlink/?LinkID=202891) — An article that provides some more detail on working with layouts.</span></span> <span data-ttu-id="7d18c-264">Viene inoltre descritto come passare un valore a una pagina di layout che mostra o nasconde la parte del contenuto.</span><span class="sxs-lookup"><span data-stu-id="7d18c-264">It also describes how to pass a value to a layout page that shows or hides some of the content.</span></span>
- <span data-ttu-id="7d18c-265">[Annidati pagine di Layout con Razor](http://www.mikesdotnetting.com/Article/164/Nested-Layout-Pages-with-Razor) , ovvero il blog di Mike Brind un esempio di come annidare le pagine di layout.</span><span class="sxs-lookup"><span data-stu-id="7d18c-265">[Nested Layout Pages with Razor](http://www.mikesdotnetting.com/Article/164/Nested-Layout-Pages-with-Razor) — Mike Brind blogs an example of how to nest layout pages.</span></span> <span data-ttu-id="7d18c-266">(Include il download delle pagine).</span><span class="sxs-lookup"><span data-stu-id="7d18c-266">(Includes a download of the pages.)</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="7d18c-267">[Precedente](deleting-data.md)
> [Successivo](publishing.md)</span><span class="sxs-lookup"><span data-stu-id="7d18c-267">[Previous](deleting-data.md)
[Next](publishing.md)</span></span>
