---
uid: web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
title: Visualizzazione di mappe in un Web ASP.NET pagine del sito (Razor) | Documenti Microsoft
author: tfitzmac
description: In questo articolo viene illustrato come visualizzare le mappe interattive nelle pagine in un sito Web ASP.NET Web Pages (Razor) basato sulla corrispondenza tra servizi forniti da Bing, Google, Ma...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: b5c268dd-ca6a-4562-b94c-a220fcf01f58
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 608dab8760bad7b877ab6fd4f89b21e980f5b1db
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
<a name="displaying-maps-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="a1812-103">Visualizzazione di mappe in un sito Web di ASP.NET di pagine (Razor)</span><span class="sxs-lookup"><span data-stu-id="a1812-103">Displaying Maps in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="a1812-104">da [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="a1812-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="a1812-105">In questo articolo viene illustrato come visualizzare le mappe interattive nelle pagine in un sito Web ASP.NET Web Pages (Razor) basato sulla corrispondenza tra servizi forniti da Bing, Google, ricerca mappa e Yahoo.</span><span class="sxs-lookup"><span data-stu-id="a1812-105">This article explains how to display interactive maps on pages in an ASP.NET Web Pages (Razor) website based on mapping services provided by Bing, Google, MapQuest, and Yahoo.</span></span>
> 
> <span data-ttu-id="a1812-106">Illustra quanto segue:</span><span class="sxs-lookup"><span data-stu-id="a1812-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="a1812-107">Come generare una mappa in base all'indirizzo.</span><span class="sxs-lookup"><span data-stu-id="a1812-107">How to generate a map based on an address.</span></span>
> - <span data-ttu-id="a1812-108">Come generare una mappa in base alle coordinate di latitudine e longitudine.</span><span class="sxs-lookup"><span data-stu-id="a1812-108">How to generate a map based on latitude and longitude coordinates.</span></span>
> - <span data-ttu-id="a1812-109">Come registrare un Account sviluppatore di Bing Maps e ottenere una chiave da usare con Bing Maps.</span><span class="sxs-lookup"><span data-stu-id="a1812-109">How to register a Bing Maps Developer Account and get a key to use with Bing Maps.</span></span>
> 
> <span data-ttu-id="a1812-110">Si tratta della funzionalità ASP.NET introdotta nell'articolo:</span><span class="sxs-lookup"><span data-stu-id="a1812-110">This is the ASP.NET feature introduced in the article:</span></span>
> 
> - <span data-ttu-id="a1812-111">Il `Maps` helper.</span><span class="sxs-lookup"><span data-stu-id="a1812-111">The `Maps` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="a1812-112">Versioni del software utilizzate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="a1812-112">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="a1812-113">Pagine Web ASP.NET (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="a1812-113">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="a1812-114">WebMatrix 2</span><span class="sxs-lookup"><span data-stu-id="a1812-114">WebMatrix 2</span></span>
>   
> 
> <span data-ttu-id="a1812-115">In questa esercitazione si integra inoltre con 3 di WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="a1812-115">This tutorial also works with WebMatrix 3.</span></span>


<span data-ttu-id="a1812-116">Nelle pagine Web, è possibile visualizzare le mappe in una pagina utilizzando `Maps` helper.</span><span class="sxs-lookup"><span data-stu-id="a1812-116">In Web Pages, you can display maps on a page by using `Maps` helper.</span></span> <span data-ttu-id="a1812-117">È possibile generare mappe in base a un indirizzo o su un set di coordinate di longitudine e latitudine.</span><span class="sxs-lookup"><span data-stu-id="a1812-117">You can generate maps based either on an address or on a set of longitude and latitude coordinates.</span></span> <span data-ttu-id="a1812-118">La `Maps` classe consente di chiamare in moduli di gestione di mappa più diffusi tra Bing, Google, ricerca mappa e Yahoo.</span><span class="sxs-lookup"><span data-stu-id="a1812-118">The `Maps` class lets you call into popular map engines including Bing, Google, MapQuest, and Yahoo.</span></span>

<span data-ttu-id="a1812-119">I passaggi per aggiungere il mapping a una pagina sono gli stessi indipendentemente da quale dei motori di mappa è chiamare il metodo.</span><span class="sxs-lookup"><span data-stu-id="a1812-119">The steps for adding mapping to a page are the same regardless of which of the map engines you call.</span></span> <span data-ttu-id="a1812-120">È sufficiente aggiungere un riferimento al file JavaScript che rende i metodi disponibili per visualizzare la mappa e quindi si chiamano metodi del `Maps` helper.</span><span class="sxs-lookup"><span data-stu-id="a1812-120">You just add a JavaScript file reference that makes available methods to display the map, and then you call methods of the `Maps` helper.</span></span>

<span data-ttu-id="a1812-121">Si sceglie un servizio di mappa in base al quale `Maps` metodo helper utilizzato.</span><span class="sxs-lookup"><span data-stu-id="a1812-121">You choose a map service based on which `Maps` helper method you use.</span></span> <span data-ttu-id="a1812-122">È possibile utilizzare una di queste:</span><span class="sxs-lookup"><span data-stu-id="a1812-122">You can use any of these:</span></span>

- `Maps.GetBingHtml`
- `Maps.GetGoogleHtml`
- `Maps.GetYahooHtml`
- `Maps.GetMapQuestHtml`

## <a name="installing-the-pieces-you-need"></a><span data-ttu-id="a1812-123">Installare le parti che necessarie</span><span class="sxs-lookup"><span data-stu-id="a1812-123">Installing the Pieces You Need</span></span>

<span data-ttu-id="a1812-124">Per visualizzare le mappe, è necessario queste parti:</span><span class="sxs-lookup"><span data-stu-id="a1812-124">To display maps, you need these pieces:</span></span>

- <span data-ttu-id="a1812-125">Il `Maps` helper.</span><span class="sxs-lookup"><span data-stu-id="a1812-125">The `Maps` helper.</span></span> <span data-ttu-id="a1812-126">Questo supporto è in versione 2 di ASP.NET Web Helpers Library.</span><span class="sxs-lookup"><span data-stu-id="a1812-126">This helper is in version 2 of the ASP.NET Web Helpers Library.</span></span> <span data-ttu-id="a1812-127">Se non si già aggiunti la libreria, è possibile installare nel sito come pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="a1812-127">If you haven't already added the library, you can install it in your site as a NuGet package.</span></span> <span data-ttu-id="a1812-128">Per informazioni dettagliate, vedere [helper per l'installazione in un sito di pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372).</span><span class="sxs-lookup"><span data-stu-id="a1812-128">For details, see [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372).</span></span> <span data-ttu-id="a1812-129">(Nella raccolta, cercare il `microsoft-web-helpers` pacchetto.)</span><span class="sxs-lookup"><span data-stu-id="a1812-129">(In the Gallery, search for the `microsoft-web-helpers` package.)</span></span>
- <span data-ttu-id="a1812-130">La libreria jQuery.</span><span class="sxs-lookup"><span data-stu-id="a1812-130">The jQuery library.</span></span> <span data-ttu-id="a1812-131">Molti dei modelli di sito WebMatrix include già le librerie jQuery in loro *Script* cartelle.</span><span class="sxs-lookup"><span data-stu-id="a1812-131">Several of the WebMatrix site templates already include jQuery libraries in their *Script* folders.</span></span> <span data-ttu-id="a1812-132">Se non si dispone di queste librerie, è possibile scaricare la libreria jQuery più recente direttamente il [jQuery.org](http://jQuery.org) sito.</span><span class="sxs-lookup"><span data-stu-id="a1812-132">If you do not have these libraries, you can download the latest jQuery library directly from the [jQuery.org](http://jQuery.org) site.</span></span> <span data-ttu-id="a1812-133">Oppure è possibile creare un nuovo sito usando un modello (ad esempio, il **Starter Site** modello) e quindi copiare i file di jQuery da tale sito al sito corrente.</span><span class="sxs-lookup"><span data-stu-id="a1812-133">Or you can create a new site using a template (for example, the **Starter Site** template) and then copy the jQuery files from that site to your current site.</span></span>

<span data-ttu-id="a1812-134">Infine, se si desidera usare le mappe di Bing, è necessario innanzitutto creare un account (gratuito) e ottenere una chiave.</span><span class="sxs-lookup"><span data-stu-id="a1812-134">Finally, if you want to use Bing maps, you must first create a (free) account and get a key.</span></span> <span data-ttu-id="a1812-135">Per ottenere una chiave, seguire questi passaggi:</span><span class="sxs-lookup"><span data-stu-id="a1812-135">To get a key, follow these steps:</span></span>

1. <span data-ttu-id="a1812-136">Creare un account sul [Account sviluppatore di Bing mappe](https://www.microsoft.com/maps/developers/web.aspx).</span><span class="sxs-lookup"><span data-stu-id="a1812-136">Create an account on the [Bing Maps Developer Account](https://www.microsoft.com/maps/developers/web.aspx).</span></span> <span data-ttu-id="a1812-137">È necessario disporre di un account Microsoft (Windows Live ID) nonché.</span><span class="sxs-lookup"><span data-stu-id="a1812-137">You must have a Microsoft account (Windows Live ID) as well.</span></span>

    <span data-ttu-id="a1812-138">È possibile specificare che si desidera utilizzare la chiave per **valutazione/Test**.</span><span class="sxs-lookup"><span data-stu-id="a1812-138">You can specify that you want to use the key for **Evaluation/Test**.</span></span> <span data-ttu-id="a1812-139">Se si sta testando la funzione di mapping nel proprio computer mediante WebMatrix e IIS Express, visitare il **sito** dell'area di lavoro e annotare l'URL del sito (ad esempio, `http://localhost:50408`, anche se il numero di porta sarà probabilmente diverso).</span><span class="sxs-lookup"><span data-stu-id="a1812-139">If you are testing the mapping function on your own computer using WebMatrix and IIS Express, go the **Site** workspace and note the URL of your site (for example, `http://localhost:50408`, although your port number will probably be different).</span></span> <span data-ttu-id="a1812-140">È possibile utilizzare questo *localhost* indirizzo del sito quando si registra.</span><span class="sxs-lookup"><span data-stu-id="a1812-140">You can use this *localhost* address as the site when you register.</span></span>
2. <span data-ttu-id="a1812-141">Dopo avere registrato per un account, il centro Account di Bing Maps, fare clic su **le chiavi Create o visualizzazione**:</span><span class="sxs-lookup"><span data-stu-id="a1812-141">After you have registered for an account, go to the Bing Maps Account Center and click **Create or view keys**:</span></span>

    ![mapping-2](displaying-maps-in-an-aspnet-web-pages-site/_static/image1.png)
3. <span data-ttu-id="a1812-143">Prendere nota della chiave che crea Bing.</span><span class="sxs-lookup"><span data-stu-id="a1812-143">Record the key that Bing creates.</span></span>

## <a name="creating-a-map-based-on-an-address-using-google"></a><span data-ttu-id="a1812-144">Creazione di una mappa in base all'indirizzo (tramite Google)</span><span class="sxs-lookup"><span data-stu-id="a1812-144">Creating a Map Based on an Address (Using Google)</span></span>

<span data-ttu-id="a1812-145">Nell'esempio seguente viene illustrato come creare una pagina che esegue il rendering di una mappa in base all'indirizzo.</span><span class="sxs-lookup"><span data-stu-id="a1812-145">The following example shows how to create a page that renders a map based on an address.</span></span> <span data-ttu-id="a1812-146">In questo esempio viene illustrato come usare le mappe di Google.</span><span class="sxs-lookup"><span data-stu-id="a1812-146">This example shows how to use Google Maps.</span></span>

1. <span data-ttu-id="a1812-147">Creare un file denominato *MapAddress.cshtml* nella radice del sito.</span><span class="sxs-lookup"><span data-stu-id="a1812-147">Create a file named *MapAddress.cshtml* in the root of the site.</span></span> <span data-ttu-id="a1812-148">Questa pagina genererà una mappa in base all'indirizzo passato a esso.</span><span class="sxs-lookup"><span data-stu-id="a1812-148">This page will generate a map based on an address that you pass to it.</span></span>
2. <span data-ttu-id="a1812-149">Copiare il codice seguente nel file, sovrascrivendo il contenuto esistente.</span><span class="sxs-lookup"><span data-stu-id="a1812-149">Copy the following code into the file, overwriting the existing content.</span></span>

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    <span data-ttu-id="a1812-150">Tenere presente le seguenti funzionalità della pagina:</span><span class="sxs-lookup"><span data-stu-id="a1812-150">Notice the following features of the page:</span></span>

    - <span data-ttu-id="a1812-151">Il `<script>` elemento il `<head>` elemento.</span><span class="sxs-lookup"><span data-stu-id="a1812-151">The `<script>` element in the `<head>` element.</span></span> <span data-ttu-id="a1812-152">Nell'esempio di `<script>` riferimenti a elementi di *jquery 1.6.4.min.js* file, che è una versione minimizzata (compressa) della libreria jQuery, versione 1.6.4.</span><span class="sxs-lookup"><span data-stu-id="a1812-152">In the example, the `<script>` element references the *jquery-1.6.4.min.js* file, which is a minified (compressed) version of the jQuery library, version 1.6.4.</span></span> <span data-ttu-id="a1812-153">Si noti che il riferimento si presuppone che il *. js* file è il *script* cartella del sito.</span><span class="sxs-lookup"><span data-stu-id="a1812-153">Note that the reference assumes that the *.js* file is in the *Scripts* folder of your site.</span></span> 

        > [!NOTE]
        > <span data-ttu-id="a1812-154">Se si utilizza una versione diversa della libreria jQuery, assicurarsi che sta correttamente puntano a tale versione.</span><span class="sxs-lookup"><span data-stu-id="a1812-154">If you're using a different version of the jQuery library, just make sure that you're pointing to that version correctly.</span></span>
    - <span data-ttu-id="a1812-155">La chiamata al `@Maps.GetGoogleHtml` nel corpo della pagina.</span><span class="sxs-lookup"><span data-stu-id="a1812-155">The call to the `@Maps.GetGoogleHtml` in the body of the page.</span></span> <span data-ttu-id="a1812-156">Per eseguire il mapping di un indirizzo, è necessario passare una stringa di indirizzo.</span><span class="sxs-lookup"><span data-stu-id="a1812-156">To map an address, you must pass an address string.</span></span> <span data-ttu-id="a1812-157">I metodi per i motori di mappa funzionano in modo analogo (`@Maps.GetYahooHtml`, `@Maps.GetMapQuestHtml`).</span><span class="sxs-lookup"><span data-stu-id="a1812-157">The methods for the other map engines work in a similar way (`@Maps.GetYahooHtml`, `@Maps.GetMapQuestHtml`).</span></span>
3. <span data-ttu-id="a1812-158">Eseguire la pagina e immettere un indirizzo.</span><span class="sxs-lookup"><span data-stu-id="a1812-158">Run the page and enter an address.</span></span> <span data-ttu-id="a1812-159">La pagina Visualizza una mappa, in base alle mappe di Google, che mostra il percorso specificato.</span><span class="sxs-lookup"><span data-stu-id="a1812-159">The page displays a map, based on Google Maps, that shows the location that you specified.</span></span>

     ![mapping di-1](displaying-maps-in-an-aspnet-web-pages-site/_static/image2.png)

## <a name="creating-a-map-based-on-latitude-and-longitude-coordinates-using-bing"></a><span data-ttu-id="a1812-161">Creazione di una mappa in base a latitudine e longitudine coordinate (tramite Bing)</span><span class="sxs-lookup"><span data-stu-id="a1812-161">Creating a Map Based on Latitude and Longitude Coordinates (Using Bing)</span></span>

<span data-ttu-id="a1812-162">In questo esempio viene illustrato come creare una mappa in base alle coordinate.</span><span class="sxs-lookup"><span data-stu-id="a1812-162">This example shows how to create a map based on coordinates.</span></span> <span data-ttu-id="a1812-163">In questo esempio viene illustrato come utilizzare Bing maps e su come includere la chiave di Bing.</span><span class="sxs-lookup"><span data-stu-id="a1812-163">This example shows how to use Bing maps and how to include your Bing key.</span></span> <span data-ttu-id="a1812-164">(È possibile creare una mappa in base alle coordinate utilizzando altri motori di mappa, inoltre, senza utilizzare una chiave di Bing).</span><span class="sxs-lookup"><span data-stu-id="a1812-164">(You can create a map based on coordinates using the other map engines also, without using a Bing key.)</span></span>

1. <span data-ttu-id="a1812-165">Creare un file denominato *MapCoordinates.cshtml* nella radice del sito e sostituire il contenuto esistente con il codice e markup seguenti:</span><span class="sxs-lookup"><span data-stu-id="a1812-165">Create a file named *MapCoordinates.cshtml* in the root of the site and replace the existing content with the following code and markup:</span></span>

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample2.cshtml)]
2. <span data-ttu-id="a1812-166">Sostituire `your-key-here` con la chiave di Bing Maps generato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="a1812-166">Replace `your-key-here` with the Bing Maps key that you generated earlier.</span></span>
3. <span data-ttu-id="a1812-167">Eseguire la *MapCoordinates.cshtml* immettere le coordinate di latitudine e longitudine e quindi fare clic sul **Map It!**</span><span class="sxs-lookup"><span data-stu-id="a1812-167">Run the *MapCoordinates.cshtml* page, enter latitude and longitude coordinates, and then click the **Map It!**</span></span> <span data-ttu-id="a1812-168">immagini (...).</span><span class="sxs-lookup"><span data-stu-id="a1812-168">button.</span></span> <span data-ttu-id="a1812-169">(Se non si conoscono tutte le coordinate, procedere come segue.</span><span class="sxs-lookup"><span data-stu-id="a1812-169">(If you don't know any coordinates, try the following.</span></span> <span data-ttu-id="a1812-170">Questo è un percorso nel campus Microsoft Redmond.)</span><span class="sxs-lookup"><span data-stu-id="a1812-170">This is a location on the Microsoft Redmond campus.)</span></span>

   - <span data-ttu-id="a1812-171">Latitude: 47.6781005859375</span><span class="sxs-lookup"><span data-stu-id="a1812-171">Latitude: 47.6781005859375</span></span>
   - <span data-ttu-id="a1812-172">Longitudine:-122.158317565918</span><span class="sxs-lookup"><span data-stu-id="a1812-172">Longitude: -122.158317565918</span></span>

     <span data-ttu-id="a1812-173">Verrà visualizzata la pagina in base alle coordinate specificate.</span><span class="sxs-lookup"><span data-stu-id="a1812-173">The page is displayed using the coordinates that you specified.</span></span>

     ![mapping-3](displaying-maps-in-an-aspnet-web-pages-site/_static/image3.png)

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="a1812-175">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="a1812-175">Additional Resources</span></span>


[<span data-ttu-id="a1812-176">Riferimento all'API Microsoft.Maps</span><span class="sxs-lookup"><span data-stu-id="a1812-176">Microsoft.Maps API Reference</span></span>](https://msdn.microsoft.com/library/gg427611.aspx)
