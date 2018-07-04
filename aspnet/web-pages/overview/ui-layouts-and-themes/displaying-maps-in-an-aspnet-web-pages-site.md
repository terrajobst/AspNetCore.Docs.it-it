---
uid: web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
title: Visualizzazione di mappe in un Web ASP.NET le pagine del sito (Razor) | Microsoft Docs
author: tfitzmac
description: Questo articolo illustra come visualizzare le mappe interattive nelle pagine in un sito Web ASP.NET Web Pages (Razor) in base al mapping dei servizi forniti da Bing, Google, Ma...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: b5c268dd-ca6a-4562-b94c-a220fcf01f58
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 210cc37427024a4e8cae309634141900bb28b55e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368835"
---
<a name="displaying-maps-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="483c4-103">Visualizzazione di mappe in un sito di ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="483c4-103">Displaying Maps in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="483c4-104">da [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="483c4-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="483c4-105">Questo articolo illustra come visualizzare le mappe interattive nelle pagine in un sito Web ASP.NET Web Pages (Razor) in base al mapping dei servizi forniti da Bing, Google, MapQuest e Yahoo.</span><span class="sxs-lookup"><span data-stu-id="483c4-105">This article explains how to display interactive maps on pages in an ASP.NET Web Pages (Razor) website based on mapping services provided by Bing, Google, MapQuest, and Yahoo.</span></span>
> 
> <span data-ttu-id="483c4-106">Che cosa si apprenderà come:</span><span class="sxs-lookup"><span data-stu-id="483c4-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="483c4-107">Come generare una mappa in base all'indirizzo.</span><span class="sxs-lookup"><span data-stu-id="483c4-107">How to generate a map based on an address.</span></span>
> - <span data-ttu-id="483c4-108">Come generare una mappa in base alle coordinate di latitudine e longitudine.</span><span class="sxs-lookup"><span data-stu-id="483c4-108">How to generate a map based on latitude and longitude coordinates.</span></span>
> - <span data-ttu-id="483c4-109">Come registrare un Account per sviluppatore di Bing Maps e ottenere una chiave da usare con Bing Maps.</span><span class="sxs-lookup"><span data-stu-id="483c4-109">How to register a Bing Maps Developer Account and get a key to use with Bing Maps.</span></span>
> 
> <span data-ttu-id="483c4-110">Si tratta della funzionalità ASP.NET introdotta nell'articolo:</span><span class="sxs-lookup"><span data-stu-id="483c4-110">This is the ASP.NET feature introduced in the article:</span></span>
> 
> - <span data-ttu-id="483c4-111">Il `Maps` helper.</span><span class="sxs-lookup"><span data-stu-id="483c4-111">The `Maps` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="483c4-112">Versioni del software utilizzate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="483c4-112">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="483c4-113">ASP.NET Web Pages (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="483c4-113">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="483c4-114">WebMatrix 2</span><span class="sxs-lookup"><span data-stu-id="483c4-114">WebMatrix 2</span></span>
>   
> 
> <span data-ttu-id="483c4-115">Questa esercitazione si integra inoltre con WebMatrix 3.</span><span class="sxs-lookup"><span data-stu-id="483c4-115">This tutorial also works with WebMatrix 3.</span></span>


<span data-ttu-id="483c4-116">Nelle pagine Web, è possibile visualizzare le mappe in una pagina con `Maps` helper.</span><span class="sxs-lookup"><span data-stu-id="483c4-116">In Web Pages, you can display maps on a page by using `Maps` helper.</span></span> <span data-ttu-id="483c4-117">È possibile generare le mappe basate su un indirizzo o su un set di coordinate di longitudine e latitudine.</span><span class="sxs-lookup"><span data-stu-id="483c4-117">You can generate maps based either on an address or on a set of longitude and latitude coordinates.</span></span> <span data-ttu-id="483c4-118">Il `Maps` classe consente di chiamare nei motori di mappa più diffusi tra cui Bing, Google, MapQuest e Yahoo.</span><span class="sxs-lookup"><span data-stu-id="483c4-118">The `Maps` class lets you call into popular map engines including Bing, Google, MapQuest, and Yahoo.</span></span>

<span data-ttu-id="483c4-119">I passaggi per l'aggiunta di mapping a una pagina sono gli stessi indipendentemente da quale dei motori di mappa è chiamare.</span><span class="sxs-lookup"><span data-stu-id="483c4-119">The steps for adding mapping to a page are the same regardless of which of the map engines you call.</span></span> <span data-ttu-id="483c4-120">È sufficiente aggiungere un riferimento al file JavaScript che rende disponibili metodi per visualizzare la mappa e quindi si chiamano metodi del `Maps` helper.</span><span class="sxs-lookup"><span data-stu-id="483c4-120">You just add a JavaScript file reference that makes available methods to display the map, and then you call methods of the `Maps` helper.</span></span>

<span data-ttu-id="483c4-121">Si sceglie un servizio di mappa in base alle quali `Maps` metodo helper utilizzato.</span><span class="sxs-lookup"><span data-stu-id="483c4-121">You choose a map service based on which `Maps` helper method you use.</span></span> <span data-ttu-id="483c4-122">È possibile usare uno di questi:</span><span class="sxs-lookup"><span data-stu-id="483c4-122">You can use any of these:</span></span>

- `Maps.GetBingHtml`
- `Maps.GetGoogleHtml`
- `Maps.GetYahooHtml`
- `Maps.GetMapQuestHtml`

## <a name="installing-the-pieces-you-need"></a><span data-ttu-id="483c4-123">Installare le parti che necessarie</span><span class="sxs-lookup"><span data-stu-id="483c4-123">Installing the Pieces You Need</span></span>

<span data-ttu-id="483c4-124">Per visualizzare le mappe, sono necessari questi elementi:</span><span class="sxs-lookup"><span data-stu-id="483c4-124">To display maps, you need these pieces:</span></span>

- <span data-ttu-id="483c4-125">Il `Maps` helper.</span><span class="sxs-lookup"><span data-stu-id="483c4-125">The `Maps` helper.</span></span> <span data-ttu-id="483c4-126">Questo supporto è nella versione 2 di ASP.NET Web Helpers Library.</span><span class="sxs-lookup"><span data-stu-id="483c4-126">This helper is in version 2 of the ASP.NET Web Helpers Library.</span></span> <span data-ttu-id="483c4-127">Se è già stata aggiunta la libreria, è possibile installare nel sito come pacchetto NuGet.</span><span class="sxs-lookup"><span data-stu-id="483c4-127">If you haven't already added the library, you can install it in your site as a NuGet package.</span></span> <span data-ttu-id="483c4-128">Per informazioni dettagliate, vedere [installazione di helper in un sito con pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372).</span><span class="sxs-lookup"><span data-stu-id="483c4-128">For details, see [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372).</span></span> <span data-ttu-id="483c4-129">(Nella raccolta, cercare il `microsoft-web-helpers` package.)</span><span class="sxs-lookup"><span data-stu-id="483c4-129">(In the Gallery, search for the `microsoft-web-helpers` package.)</span></span>
- <span data-ttu-id="483c4-130">La libreria jQuery.</span><span class="sxs-lookup"><span data-stu-id="483c4-130">The jQuery library.</span></span> <span data-ttu-id="483c4-131">Molti dei modelli di sito WebMatrix include già le librerie jQuery nella loro *Script* cartelle.</span><span class="sxs-lookup"><span data-stu-id="483c4-131">Several of the WebMatrix site templates already include jQuery libraries in their *Script* folders.</span></span> <span data-ttu-id="483c4-132">Se non si dispone di queste librerie, è possibile scaricare la libreria jQuery più recente direttamente dai [jQuery.org](http://jQuery.org) sito.</span><span class="sxs-lookup"><span data-stu-id="483c4-132">If you do not have these libraries, you can download the latest jQuery library directly from the [jQuery.org](http://jQuery.org) site.</span></span> <span data-ttu-id="483c4-133">Oppure è possibile creare un nuovo sito usando un modello (ad esempio, il **Starter Site** modello) e quindi copiare i file jQuery da tale sito nel sito corrente.</span><span class="sxs-lookup"><span data-stu-id="483c4-133">Or you can create a new site using a template (for example, the **Starter Site** template) and then copy the jQuery files from that site to your current site.</span></span>

<span data-ttu-id="483c4-134">Infine, se si desidera usare le mappe di Bing, è necessario innanzitutto creare un account (gratuito) e ottenere una chiave.</span><span class="sxs-lookup"><span data-stu-id="483c4-134">Finally, if you want to use Bing maps, you must first create a (free) account and get a key.</span></span> <span data-ttu-id="483c4-135">Per ottenere una chiave, seguire questa procedura:</span><span class="sxs-lookup"><span data-stu-id="483c4-135">To get a key, follow these steps:</span></span>

1. <span data-ttu-id="483c4-136">Creare un account sul [Account per sviluppatore di Bing mappe](https://www.microsoft.com/maps/developers/web.aspx).</span><span class="sxs-lookup"><span data-stu-id="483c4-136">Create an account on the [Bing Maps Developer Account](https://www.microsoft.com/maps/developers/web.aspx).</span></span> <span data-ttu-id="483c4-137">È necessario un account Microsoft (Windows Live ID) nonché.</span><span class="sxs-lookup"><span data-stu-id="483c4-137">You must have a Microsoft account (Windows Live ID) as well.</span></span>

    <span data-ttu-id="483c4-138">È possibile specificare che si desidera usare la chiave per **valutazione e Test**.</span><span class="sxs-lookup"><span data-stu-id="483c4-138">You can specify that you want to use the key for **Evaluation/Test**.</span></span> <span data-ttu-id="483c4-139">Se si sta testando la funzione di mapping nel proprio computer mediante WebMatrix e IIS Express, visitare il **sito** dell'area di lavoro e annotare l'URL del sito (ad esempio, `http://localhost:50408`, anche se il numero di porta sarà probabilmente diverso).</span><span class="sxs-lookup"><span data-stu-id="483c4-139">If you are testing the mapping function on your own computer using WebMatrix and IIS Express, go the **Site** workspace and note the URL of your site (for example, `http://localhost:50408`, although your port number will probably be different).</span></span> <span data-ttu-id="483c4-140">È possibile usare questo *localhost* indirizzo del sito quando si registra.</span><span class="sxs-lookup"><span data-stu-id="483c4-140">You can use this *localhost* address as the site when you register.</span></span>
2. <span data-ttu-id="483c4-141">Dopo avere registrato per un account, passare al centro Account mappe di Bing e fare clic su **crea o Visualizza chiavi**:</span><span class="sxs-lookup"><span data-stu-id="483c4-141">After you have registered for an account, go to the Bing Maps Account Center and click **Create or view keys**:</span></span>

    ![mapping-2](displaying-maps-in-an-aspnet-web-pages-site/_static/image1.png)
3. <span data-ttu-id="483c4-143">Prendere nota della chiave che crea Bing.</span><span class="sxs-lookup"><span data-stu-id="483c4-143">Record the key that Bing creates.</span></span>

## <a name="creating-a-map-based-on-an-address-using-google"></a><span data-ttu-id="483c4-144">Creazione di una mappa in base all'indirizzo (mediante Google)</span><span class="sxs-lookup"><span data-stu-id="483c4-144">Creating a Map Based on an Address (Using Google)</span></span>

<span data-ttu-id="483c4-145">Nell'esempio seguente viene illustrato come creare una pagina che esegue il rendering di una mappa in base all'indirizzo.</span><span class="sxs-lookup"><span data-stu-id="483c4-145">The following example shows how to create a page that renders a map based on an address.</span></span> <span data-ttu-id="483c4-146">In questo esempio mostra come usare Google Maps.</span><span class="sxs-lookup"><span data-stu-id="483c4-146">This example shows how to use Google Maps.</span></span>

1. <span data-ttu-id="483c4-147">Creare un file denominato *MapAddress.cshtml* nella radice del sito.</span><span class="sxs-lookup"><span data-stu-id="483c4-147">Create a file named *MapAddress.cshtml* in the root of the site.</span></span> <span data-ttu-id="483c4-148">Questa pagina genera una mappa in base all'indirizzo che viene passato ad esso.</span><span class="sxs-lookup"><span data-stu-id="483c4-148">This page will generate a map based on an address that you pass to it.</span></span>
2. <span data-ttu-id="483c4-149">Copiare il codice seguente nel file, sovrascrivendo il contenuto esistente.</span><span class="sxs-lookup"><span data-stu-id="483c4-149">Copy the following code into the file, overwriting the existing content.</span></span>

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    <span data-ttu-id="483c4-150">Si noti che le funzionalità seguenti della pagina:</span><span class="sxs-lookup"><span data-stu-id="483c4-150">Notice the following features of the page:</span></span>

    - <span data-ttu-id="483c4-151">Il `<script>` elemento il `<head>` elemento.</span><span class="sxs-lookup"><span data-stu-id="483c4-151">The `<script>` element in the `<head>` element.</span></span> <span data-ttu-id="483c4-152">Nell'esempio, il `<script>` i riferimenti agli elementi di *jquery 1.6.4.min.js* file, ovvero una versione minimizzata (compressa) della libreria jQuery, versione 1.6.4.</span><span class="sxs-lookup"><span data-stu-id="483c4-152">In the example, the `<script>` element references the *jquery-1.6.4.min.js* file, which is a minified (compressed) version of the jQuery library, version 1.6.4.</span></span> <span data-ttu-id="483c4-153">Si noti che il riferimento si presuppone che il *js* file si trova nel *script* cartella del sito.</span><span class="sxs-lookup"><span data-stu-id="483c4-153">Note that the reference assumes that the *.js* file is in the *Scripts* folder of your site.</span></span> 

        > [!NOTE]
        > <span data-ttu-id="483c4-154">Se si usa una versione diversa della libreria jQuery, assicurarsi che sta puntando a tale versione correttamente.</span><span class="sxs-lookup"><span data-stu-id="483c4-154">If you're using a different version of the jQuery library, just make sure that you're pointing to that version correctly.</span></span>
    - <span data-ttu-id="483c4-155">La chiamata al `@Maps.GetGoogleHtml` nel corpo della pagina.</span><span class="sxs-lookup"><span data-stu-id="483c4-155">The call to the `@Maps.GetGoogleHtml` in the body of the page.</span></span> <span data-ttu-id="483c4-156">Per eseguire il mapping di un indirizzo, è necessario passare una stringa di indirizzo.</span><span class="sxs-lookup"><span data-stu-id="483c4-156">To map an address, you must pass an address string.</span></span> <span data-ttu-id="483c4-157">I metodi per altri motori di mappa funzionano in modo analogo (`@Maps.GetYahooHtml`, `@Maps.GetMapQuestHtml`).</span><span class="sxs-lookup"><span data-stu-id="483c4-157">The methods for the other map engines work in a similar way (`@Maps.GetYahooHtml`, `@Maps.GetMapQuestHtml`).</span></span>
3. <span data-ttu-id="483c4-158">Eseguire la pagina e immettere un indirizzo.</span><span class="sxs-lookup"><span data-stu-id="483c4-158">Run the page and enter an address.</span></span> <span data-ttu-id="483c4-159">La pagina viene visualizzata una mappa, basata sulle mappe Google, che mostra la posizione specificata.</span><span class="sxs-lookup"><span data-stu-id="483c4-159">The page displays a map, based on Google Maps, that shows the location that you specified.</span></span>

     ![1-mapping](displaying-maps-in-an-aspnet-web-pages-site/_static/image2.png)

## <a name="creating-a-map-based-on-latitude-and-longitude-coordinates-using-bing"></a><span data-ttu-id="483c4-161">Creazione di una mappa basata su latitudine e longitudine coordina (utilizzando Bing)</span><span class="sxs-lookup"><span data-stu-id="483c4-161">Creating a Map Based on Latitude and Longitude Coordinates (Using Bing)</span></span>

<span data-ttu-id="483c4-162">In questo esempio viene illustrato come creare una mappa in base alle coordinate.</span><span class="sxs-lookup"><span data-stu-id="483c4-162">This example shows how to create a map based on coordinates.</span></span> <span data-ttu-id="483c4-163">Questo esempio illustra come usare le mappe di Bing e su come includere la chiave di Bing.</span><span class="sxs-lookup"><span data-stu-id="483c4-163">This example shows how to use Bing maps and how to include your Bing key.</span></span> <span data-ttu-id="483c4-164">(È possibile creare una mappa in base alle coordinate con altri motori di mappa anche, senza usare una chiave di Bing).</span><span class="sxs-lookup"><span data-stu-id="483c4-164">(You can create a map based on coordinates using the other map engines also, without using a Bing key.)</span></span>

1. <span data-ttu-id="483c4-165">Creare un file denominato *MapCoordinates.cshtml* nella radice del sito e sostituire il contenuto esistente con il codice e il markup seguente:</span><span class="sxs-lookup"><span data-stu-id="483c4-165">Create a file named *MapCoordinates.cshtml* in the root of the site and replace the existing content with the following code and markup:</span></span>

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample2.cshtml)]
2. <span data-ttu-id="483c4-166">Sostituire `your-key-here` con la chiave di Bing Maps che è stato generato in precedenza.</span><span class="sxs-lookup"><span data-stu-id="483c4-166">Replace `your-key-here` with the Bing Maps key that you generated earlier.</span></span>
3. <span data-ttu-id="483c4-167">Eseguire la *MapCoordinates.cshtml* pagina, immettere le coordinate di latitudine e longitudine e quindi fare clic su di **Map It.**</span><span class="sxs-lookup"><span data-stu-id="483c4-167">Run the *MapCoordinates.cshtml* page, enter latitude and longitude coordinates, and then click the **Map It!**</span></span> <span data-ttu-id="483c4-168">immagini (...).</span><span class="sxs-lookup"><span data-stu-id="483c4-168">button.</span></span> <span data-ttu-id="483c4-169">(Se non si conoscono le coordinate, procedere come segue.</span><span class="sxs-lookup"><span data-stu-id="483c4-169">(If you don't know any coordinates, try the following.</span></span> <span data-ttu-id="483c4-170">Questo è un percorso all'interno del campus di Redmond Microsoft.)</span><span class="sxs-lookup"><span data-stu-id="483c4-170">This is a location on the Microsoft Redmond campus.)</span></span>

   - <span data-ttu-id="483c4-171">Latitudine: 47.6781005859375</span><span class="sxs-lookup"><span data-stu-id="483c4-171">Latitude: 47.6781005859375</span></span>
   - <span data-ttu-id="483c4-172">Longitudine:-122.158317565918</span><span class="sxs-lookup"><span data-stu-id="483c4-172">Longitude: -122.158317565918</span></span>

     <span data-ttu-id="483c4-173">Verrà visualizzata la pagina in base alle coordinate specificato.</span><span class="sxs-lookup"><span data-stu-id="483c4-173">The page is displayed using the coordinates that you specified.</span></span>

     ![mapping-3](displaying-maps-in-an-aspnet-web-pages-site/_static/image3.png)

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="483c4-175">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="483c4-175">Additional Resources</span></span>


[<span data-ttu-id="483c4-176">Riferimento all'API Microsoft.Maps</span><span class="sxs-lookup"><span data-stu-id="483c4-176">Microsoft.Maps API Reference</span></span>](https://msdn.microsoft.com/library/gg427611.aspx)
