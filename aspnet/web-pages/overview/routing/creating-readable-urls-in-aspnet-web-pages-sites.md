---
uid: web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
title: Creazione di URL leggibili in ASP.NET Web Pages (Razor) siti | Microsoft Docs
author: tfitzmac
description: Questo articolo descrive il routing in un sito Web di ASP.NET Web Pages (Razor) e come è possibile utilizzare gli URL che risultano più leggibili e migliori per SEO. Che cosa si imposterà un...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: a8aac1ac-89de-4415-afe0-97a41c6423d2
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: d027be2924f24c1080deb4ee2deacc235bbd1001
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2018
ms.locfileid: "37385748"
---
<a name="creating-readable-urls-in-aspnet-web-pages-razor-sites"></a><span data-ttu-id="0e05e-104">Creazione di URL leggibili nei siti di ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="0e05e-104">Creating Readable URLs in ASP.NET Web Pages (Razor) Sites</span></span>
====================
<span data-ttu-id="0e05e-105">da [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="0e05e-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="0e05e-106">Questo articolo descrive il routing in un sito Web di ASP.NET Web Pages (Razor) e come è possibile utilizzare gli URL che risultano più leggibili e migliori per SEO.</span><span class="sxs-lookup"><span data-stu-id="0e05e-106">This article describes routing in an ASP.NET Web Pages (Razor) website, and how this lets you use URLs that are more readable and better for SEO.</span></span>
> 
> <span data-ttu-id="0e05e-107">Che cosa si apprenderà come:</span><span class="sxs-lookup"><span data-stu-id="0e05e-107">What you'll learn:</span></span>
> 
> - <span data-ttu-id="0e05e-108">Come ASP.NET utilizza il routing per l'utilizzo di più URL leggibili e disponibili per la ricerca.</span><span class="sxs-lookup"><span data-stu-id="0e05e-108">How ASP.NET uses routing to let you use more readable and searchable URLs.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="0e05e-109">Versioni del software utilizzate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="0e05e-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="0e05e-110">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="0e05e-110">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="0e05e-111">Questa esercitazione si integra inoltre con ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="0e05e-111">This tutorial also works with ASP.NET Web Pages 2.</span></span>


## <a name="about-routing"></a><span data-ttu-id="0e05e-112">Informazioni sul Routing</span><span class="sxs-lookup"><span data-stu-id="0e05e-112">About Routing</span></span>

<span data-ttu-id="0e05e-113">Gli URL per le pagine del sito possono avere un impatto su come funziona il sito.</span><span class="sxs-lookup"><span data-stu-id="0e05e-113">The URLs for the pages in your site can have an impact on how well the site works.</span></span> <span data-ttu-id="0e05e-114">Un URL Ecco &quot;descrittivo&quot; rendono più semplice per gli utenti a utilizzare il sito.</span><span class="sxs-lookup"><span data-stu-id="0e05e-114">A URL that's &quot;friendly&quot; can make it easier for people to use the site.</span></span> <span data-ttu-id="0e05e-115">Inoltre possibile facilitare ottimizzazione motore di ricerca (SEO) per il sito.</span><span class="sxs-lookup"><span data-stu-id="0e05e-115">It can also help with search-engine optimization (SEO) for the site.</span></span> <span data-ttu-id="0e05e-116">Siti Web ASP.NET includono la possibilità di utilizzare automaticamente gli URL brevi.</span><span class="sxs-lookup"><span data-stu-id="0e05e-116">ASP.NET websites include the ability to use friendly URLs automatically.</span></span>

<span data-ttu-id="0e05e-117">ASP.NET consente di creare URL significativo che descrivono le azioni dell'utente anziché semplicemente riferimento a un file nel server.</span><span class="sxs-lookup"><span data-stu-id="0e05e-117">ASP.NET lets you create meaningful URLs that describe user actions instead of just pointing to a file on the server.</span></span> <span data-ttu-id="0e05e-118">Prendere in considerazione questi URL per un blog fittizio:</span><span class="sxs-lookup"><span data-stu-id="0e05e-118">Consider these URLs for a fictional blog:</span></span>

- `http://www.contoso.com/Blog/blog.cshtml?categories=hardware`
- `http://www.contoso.com//Blog/blog.cshtml?startdate=2009-11-01&enddate=2009-11-30`

<span data-ttu-id="0e05e-119">Confrontare questi URL per i seguenti:</span><span class="sxs-lookup"><span data-stu-id="0e05e-119">Compare those URLs to the following ones:</span></span>

- `http://www.contoso.com/Blog/categories/hardware/`
- `http://www.contoso.com/Blog/2009/November`

<span data-ttu-id="0e05e-120">Nella coppia primo, un utente deve sapere che il blog viene visualizzato utilizzando il *blog.cshtml* pagina e sarà necessario costruire una stringa di query che ottiene gli intervalli di categoria e data a destra.</span><span class="sxs-lookup"><span data-stu-id="0e05e-120">In the first pair, a user would have to know that the blog is displayed using the *blog.cshtml* page, and would then have to construct a query string that gets the right category or date range.</span></span> <span data-ttu-id="0e05e-121">È molto più semplice da comprendere e creare il secondo set di esempi.</span><span class="sxs-lookup"><span data-stu-id="0e05e-121">The second set of examples is much easier to comprehend and create.</span></span>

<span data-ttu-id="0e05e-122">Gli URL per il primo esempio anche puntino direttamente a un file specifico (*blog.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="0e05e-122">The URLs for the first example also point directly to a specific file (*blog.cshtml*).</span></span> <span data-ttu-id="0e05e-123">Se per qualche motivo i blog sono state spostate in un'altra cartella nel server o se i blog sono state riscritte per utilizzare un'altra pagina, i collegamenti avresti torto.</span><span class="sxs-lookup"><span data-stu-id="0e05e-123">If for some reason the blog were moved to another folder on the server, or if the blog were rewritten to use a different page, the links would be wrong.</span></span> <span data-ttu-id="0e05e-124">Il secondo set di URL non punta a una pagina specifica, pertanto anche se viene modificata l'implementazione di blog o località, l'URL può essere ancora adottato.</span><span class="sxs-lookup"><span data-stu-id="0e05e-124">The second set of URLs doesn't point to a specific page, so even if the blog implementation or location changes, the URLs would still be valid.</span></span>

<span data-ttu-id="0e05e-125">In ASP.NET Web Pages, è possibile creare un URL più descrittivo, ad esempio quelli negli esempi precedenti poiché in ASP.NET viene utilizzato *routing*.</span><span class="sxs-lookup"><span data-stu-id="0e05e-125">In ASP.NET Web Pages, you can create friendlier URLs like those in the above examples because ASP.NET uses *routing*.</span></span> <span data-ttu-id="0e05e-126">Routing crea mapping logico da un URL a una pagina (o pagine) che può soddisfare la richiesta.</span><span class="sxs-lookup"><span data-stu-id="0e05e-126">Routing creates logical mapping from a URL to a page (or pages) that can fulfill the request.</span></span> <span data-ttu-id="0e05e-127">Poiché il mapping è logico (non fisico, a un file specifico), routing offre una notevole flessibilità nel modo in cui si definiscono l'URL per il sito.</span><span class="sxs-lookup"><span data-stu-id="0e05e-127">Because the mapping is logical (not physical, to a specific file), routing provides great flexibility in how you define the URLs for your site.</span></span>

## <a name="how-routing-works"></a><span data-ttu-id="0e05e-128">Funzionamento del Routing</span><span class="sxs-lookup"><span data-stu-id="0e05e-128">How Routing Works</span></span>

<span data-ttu-id="0e05e-129">Quando ASP.NET elabora una richiesta, legge l'URL per determinare come eseguirne il routing.</span><span class="sxs-lookup"><span data-stu-id="0e05e-129">When ASP.NET processes a request, it reads the URL to determine how to route it.</span></span> <span data-ttu-id="0e05e-130">ASP.NET tenta di abbinare singoli segmenti dell'URL per i file su disco, procedendo da sinistra verso destra.</span><span class="sxs-lookup"><span data-stu-id="0e05e-130">ASP.NET tries to match individual segments of the URL to files on disk, going from left to right.</span></span> <span data-ttu-id="0e05e-131">Se non esiste una corrispondenza, qualsiasi elemento rimanenti nell'URL viene passato alla pagina *le informazioni sul percorso*.</span><span class="sxs-lookup"><span data-stu-id="0e05e-131">If there's a match, anything remaining in the URL is passed to the page as *path information*.</span></span>

<span data-ttu-id="0e05e-132">Si supponga che un utente effettua una richiesta con questo URL:</span><span class="sxs-lookup"><span data-stu-id="0e05e-132">Imagine that someone makes a request using this URL:</span></span>

`http://www.contoso.com/a/b/c`

<span data-ttu-id="0e05e-133">La ricerca procede come segue:</span><span class="sxs-lookup"><span data-stu-id="0e05e-133">The search goes like this:</span></span>

1. <span data-ttu-id="0e05e-134">È presente un file con il percorso e il nome della */a/b/c.cshtml*?</span><span class="sxs-lookup"><span data-stu-id="0e05e-134">Is there a file with the path and name of */a/b/c.cshtml*?</span></span> <span data-ttu-id="0e05e-135">In questo caso, eseguire tale pagina e non passarle alcuna informazione.</span><span class="sxs-lookup"><span data-stu-id="0e05e-135">If so, run that page and pass no information to it.</span></span> <span data-ttu-id="0e05e-136">In caso contrario...</span><span class="sxs-lookup"><span data-stu-id="0e05e-136">Otherwise ...</span></span>
2. <span data-ttu-id="0e05e-137">È presente un file con il percorso e il nome della */a/b.cshtml*?</span><span class="sxs-lookup"><span data-stu-id="0e05e-137">Is there a file with the path and name of */a/b.cshtml*?</span></span> <span data-ttu-id="0e05e-138">Se quindi, eseguire tale pagina e passare il valore `c` ad esso.</span><span class="sxs-lookup"><span data-stu-id="0e05e-138">If so, run that page and pass the value `c` to it.</span></span> <span data-ttu-id="0e05e-139">In caso contrario...</span><span class="sxs-lookup"><span data-stu-id="0e05e-139">Otherwise …</span></span>
3. <span data-ttu-id="0e05e-140">È presente un file con il percorso e il nome della */a.cshtml*?</span><span class="sxs-lookup"><span data-stu-id="0e05e-140">Is there a file with the path and name of */a.cshtml*?</span></span> <span data-ttu-id="0e05e-141">Se quindi, eseguire tale pagina e passare il valore `b/c` ad esso.</span><span class="sxs-lookup"><span data-stu-id="0e05e-141">If so, run that page and pass the value `b/c` to it.</span></span>

<span data-ttu-id="0e05e-142">Se la ricerca ha trovato non esatta corrisponda *cshtml* file nelle rispettive cartelle specificate, ASP.NET continuano a cercare questi file, a sua volta:</span><span class="sxs-lookup"><span data-stu-id="0e05e-142">If the search found no exact matches for *.cshtml* files in their specified folders, ASP.NET continues looking for these files in turn:</span></span>

1. <span data-ttu-id="0e05e-143">*/a/b/c/default.cshtml* (senza informazioni sul percorso).</span><span class="sxs-lookup"><span data-stu-id="0e05e-143">*/a/b/c/default.cshtml* (no path information).</span></span>
2. <span data-ttu-id="0e05e-144">*/a/b/c/index.cshtml* (senza informazioni sul percorso).</span><span class="sxs-lookup"><span data-stu-id="0e05e-144">*/a/b/c/index.cshtml* (no path information).</span></span>

> [!NOTE]
> <span data-ttu-id="0e05e-145">Per maggiore chiarezza, le richieste di pagine specifiche (vale a dire, le richieste che includono il *. cshtml* estensione del nome file) funzionare esattamente come si aspetterebbe.</span><span class="sxs-lookup"><span data-stu-id="0e05e-145">To be clear, requests for specific pages (that is, requests that include the *.cshtml* filename extension) work just like you'd expect.</span></span> <span data-ttu-id="0e05e-146">Una richiesta simile `http://www.contoso.com/a/b.cshtml` eseguiranno la pagina *b.cshtml* correttamente.</span><span class="sxs-lookup"><span data-stu-id="0e05e-146">A request like `http://www.contoso.com/a/b.cshtml` will run the page *b.cshtml* just fine.</span></span>


<span data-ttu-id="0e05e-147">All'interno di una pagina, è possibile ottenere le informazioni sui percorsi tramite la pagina `UrlData` proprietà, ovvero un dizionario.</span><span class="sxs-lookup"><span data-stu-id="0e05e-147">Inside a page, you can get the path information via the page's `UrlData` property, which is a dictionary.</span></span> <span data-ttu-id="0e05e-148">Si supponga di avere un file denominato *ViewCustomers.cshtml* e il sito riceve questa richiesta:</span><span class="sxs-lookup"><span data-stu-id="0e05e-148">Imagine that you have a file named *ViewCustomers.cshtml* and your site gets this request:</span></span>

`http://mysite.com/myWebSite/ViewCustomers/1000`

<span data-ttu-id="0e05e-149">Come descritto nelle regole precedenti, la richiesta verrà inviata a una pagina.</span><span class="sxs-lookup"><span data-stu-id="0e05e-149">As described in the rules above, the request will go to your page.</span></span> <span data-ttu-id="0e05e-150">All'interno della pagina, è possibile usare codice simile al seguente per ottenere e visualizzare le informazioni sul percorso (in questo caso, il valore &quot;1000&quot;):</span><span class="sxs-lookup"><span data-stu-id="0e05e-150">Inside the page, you can use code like the following to get and display the path information (in this case, the value &quot;1000&quot;):</span></span>

[!code-html[Main](creating-readable-urls-in-aspnet-web-pages-sites/samples/sample1.html)]

> [!NOTE]
> <span data-ttu-id="0e05e-151">Poiché il routing non prevede l'utilizzo di nomi di file completi, è possibile che ambiguità nel caso di pagine che presentano lo stesso nome ma con una diversa estensioni di file (ad esempio, *MyPage.cshtml* e *MyPage.html*) .</span><span class="sxs-lookup"><span data-stu-id="0e05e-151">Because routing doesn't involve complete file names, there can be ambiguity if you have pages that have the same name but different file-name extensions (for example, *MyPage.cshtml* and *MyPage.html*).</span></span> <span data-ttu-id="0e05e-152">Per evitare problemi con il routing, è consigliabile verificare che non hai le pagine nel sito i cui nomi si differenziano solo per l'estensione.</span><span class="sxs-lookup"><span data-stu-id="0e05e-152">In order to avoid problems with routing, it's best to make sure that you don't have pages in your site whose names differ only in their extension.</span></span>


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="0e05e-153">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="0e05e-153">Additional Resources</span></span>

<span data-ttu-id="0e05e-154">[WebMatrix - URL UrlData e Routing per SEO](http://www.mikesdotnetting.com/Article/165/WebMatrix-URLs-UrlData-and-Routing-for-SEO).</span><span class="sxs-lookup"><span data-stu-id="0e05e-154">[WebMatrix - URLs, UrlData and Routing for SEO](http://www.mikesdotnetting.com/Article/165/WebMatrix-URLs-UrlData-and-Routing-for-SEO).</span></span> <span data-ttu-id="0e05e-155">In questo intervento sul blog di Mike Brind fornisce alcuni dettagli aggiuntivi sul funzionamento del routing in ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="0e05e-155">This blog entry by Mike Brind provides some additional details on how routing works in ASP.NET Web Pages.</span></span>
