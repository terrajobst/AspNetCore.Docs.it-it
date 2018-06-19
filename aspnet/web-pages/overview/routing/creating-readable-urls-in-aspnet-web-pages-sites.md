---
uid: web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
title: Creazione di URL leggibili in ASP.NET Web Pages siti (Razor) | Documenti Microsoft
author: tfitzmac
description: Questo articolo descrive routing in un sito Web ASP.NET Web Pages (Razor) e come è possibile utilizzare gli URL che risultano più leggibile e migliori per SEO. Sarà necessario...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: a8aac1ac-89de-4415-afe0-97a41c6423d2
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: 7858b7cbd6dccafb2867ed9a1d102561e211e435
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/10/2017
ms.locfileid: "26529750"
---
<a name="creating-readable-urls-in-aspnet-web-pages-razor-sites"></a><span data-ttu-id="42f49-104">Creazione di URL leggibili in siti Web di ASP.NET di pagine (Razor)</span><span class="sxs-lookup"><span data-stu-id="42f49-104">Creating Readable URLs in ASP.NET Web Pages (Razor) Sites</span></span>
====================
<span data-ttu-id="42f49-105">da [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="42f49-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="42f49-106">Questo articolo descrive routing in un sito Web ASP.NET Web Pages (Razor) e come è possibile utilizzare gli URL che risultano più leggibile e migliori per SEO.</span><span class="sxs-lookup"><span data-stu-id="42f49-106">This article describes routing in an ASP.NET Web Pages (Razor) website, and how this lets you use URLs that are more readable and better for SEO.</span></span>
> 
> <span data-ttu-id="42f49-107">Illustra quanto segue:</span><span class="sxs-lookup"><span data-stu-id="42f49-107">What you'll learn:</span></span>
> 
> - <span data-ttu-id="42f49-108">Utilizzo di ASP.NET routing per l'utilizzo di più leggibili e possono essere cercati gli URL.</span><span class="sxs-lookup"><span data-stu-id="42f49-108">How ASP.NET uses routing to let you use more readable and searchable URLs.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="42f49-109">Versioni del software utilizzate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="42f49-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="42f49-110">Pagine Web ASP.NET (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="42f49-110">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="42f49-111">In questa esercitazione si integra inoltre con ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="42f49-111">This tutorial also works with ASP.NET Web Pages 2.</span></span>


## <a name="about-routing"></a><span data-ttu-id="42f49-112">Sul Routing</span><span class="sxs-lookup"><span data-stu-id="42f49-112">About Routing</span></span>

<span data-ttu-id="42f49-113">Gli URL per le pagine del sito possono avere un impatto sulla modalità anche il sito.</span><span class="sxs-lookup"><span data-stu-id="42f49-113">The URLs for the pages in your site can have an impact on how well the site works.</span></span> <span data-ttu-id="42f49-114">Un URL che &quot;descrittivo&quot; può rendere più semplice per gli utenti di utilizzare il sito.</span><span class="sxs-lookup"><span data-stu-id="42f49-114">A URL that's &quot;friendly&quot; can make it easier for people to use the site.</span></span> <span data-ttu-id="42f49-115">Consente inoltre di aiutare con l'ottimizzazione motore di ricerca (SEO) per il sito.</span><span class="sxs-lookup"><span data-stu-id="42f49-115">It can also help with search-engine optimization (SEO) for the site.</span></span> <span data-ttu-id="42f49-116">I siti Web ASP.NET includono la possibilità di utilizzare automaticamente gli URL brevi.</span><span class="sxs-lookup"><span data-stu-id="42f49-116">ASP.NET websites include the ability to use friendly URLs automatically.</span></span>

<span data-ttu-id="42f49-117">ASP.NET consente di creare URL che descrivono le azioni dell'utente anziché fare riferimento solo a un file nel server.</span><span class="sxs-lookup"><span data-stu-id="42f49-117">ASP.NET lets you create meaningful URLs that describe user actions instead of just pointing to a file on the server.</span></span> <span data-ttu-id="42f49-118">Considerare queste URL per un blog fittizio:</span><span class="sxs-lookup"><span data-stu-id="42f49-118">Consider these URLs for a fictional blog:</span></span>

- `http://www.contoso.com/Blog/blog.cshtml?categories=hardware`
- `http://www.contoso.com//Blog/blog.cshtml?startdate=2009-11-01&enddate=2009-11-30`

<span data-ttu-id="42f49-119">Confrontare tali URL riportate di seguito:</span><span class="sxs-lookup"><span data-stu-id="42f49-119">Compare those URLs to the following ones:</span></span>

- `http://www.contoso.com/Blog/categories/hardware/`
- `http://www.contoso.com/Blog/2009/November`

<span data-ttu-id="42f49-120">Nella prima coppia, un utente necessario conoscere il blog viene visualizzato utilizzando il *blog.cshtml* pagina e sarà necessario costruire una stringa di query che ottiene l'intervallo di categoria o data destra.</span><span class="sxs-lookup"><span data-stu-id="42f49-120">In the first pair, a user would have to know that the blog is displayed using the *blog.cshtml* page, and would then have to construct a query string that gets the right category or date range.</span></span> <span data-ttu-id="42f49-121">Il secondo set di esempi è molto più semplice da comprendere e creare.</span><span class="sxs-lookup"><span data-stu-id="42f49-121">The second set of examples is much easier to comprehend and create.</span></span>

<span data-ttu-id="42f49-122">Gli URL per il primo esempio viene inoltre fare riferimento direttamente a un file specifico (*blog.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="42f49-122">The URLs for the first example also point directly to a specific file (*blog.cshtml*).</span></span> <span data-ttu-id="42f49-123">Se per qualche motivo il blog sono stato spostato in un'altra cartella nel server o se il blog sono state riscritte per utilizzare una pagina diversa, i collegamenti sarebbe errati.</span><span class="sxs-lookup"><span data-stu-id="42f49-123">If for some reason the blog were moved to another folder on the server, or if the blog were rewritten to use a different page, the links would be wrong.</span></span> <span data-ttu-id="42f49-124">Il secondo set di URL non fa riferimento a una pagina specifica, pertanto anche se viene modificata l'implementazione di blog o il percorso, gli URL ancora sarebbe validi.</span><span class="sxs-lookup"><span data-stu-id="42f49-124">The second set of URLs doesn't point to a specific page, so even if the blog implementation or location changes, the URLs would still be valid.</span></span>

<span data-ttu-id="42f49-125">In ASP.NET Web Pages, è possibile creare più URL come quelli negli esempi precedenti poiché ASP.NET usa *routing*.</span><span class="sxs-lookup"><span data-stu-id="42f49-125">In ASP.NET Web Pages, you can create friendlier URLs like those in the above examples because ASP.NET uses *routing*.</span></span> <span data-ttu-id="42f49-126">Routing crea mapping logico da un URL a una pagina (o pagine) che può soddisfare la richiesta.</span><span class="sxs-lookup"><span data-stu-id="42f49-126">Routing creates logical mapping from a URL to a page (or pages) that can fulfill the request.</span></span> <span data-ttu-id="42f49-127">Poiché il mapping è logico (non fisica, a un file specifico), il routing fornisce una notevole flessibilità, in cui viene illustrato come definire gli URL per il sito.</span><span class="sxs-lookup"><span data-stu-id="42f49-127">Because the mapping is logical (not physical, to a specific file), routing provides great flexibility in how you define the URLs for your site.</span></span>

## <a name="how-routing-works"></a><span data-ttu-id="42f49-128">Il funzionamento del Routing</span><span class="sxs-lookup"><span data-stu-id="42f49-128">How Routing Works</span></span>

<span data-ttu-id="42f49-129">Quando ASP.NET elabora una richiesta, viene anche letto l'URL per determinare come eseguirne il routing.</span><span class="sxs-lookup"><span data-stu-id="42f49-129">When ASP.NET processes a request, it reads the URL to determine how to route it.</span></span> <span data-ttu-id="42f49-130">ASP.NET tenta di abbinare i singoli segmenti di URL di file su disco, da sinistra verso destra.</span><span class="sxs-lookup"><span data-stu-id="42f49-130">ASP.NET tries to match individual segments of the URL to files on disk, going from left to right.</span></span> <span data-ttu-id="42f49-131">Se viene trovata una corrispondenza, qualsiasi elemento rimanenti nell'URL viene passato alla pagina come *informazioni sul percorso*.</span><span class="sxs-lookup"><span data-stu-id="42f49-131">If there's a match, anything remaining in the URL is passed to the page as *path information*.</span></span>

<span data-ttu-id="42f49-132">Si supponga che un utente effettua una richiesta con questo URL:</span><span class="sxs-lookup"><span data-stu-id="42f49-132">Imagine that someone makes a request using this URL:</span></span>

`http://www.contoso.com/a/b/c`

<span data-ttu-id="42f49-133">La ricerca procede come segue:</span><span class="sxs-lookup"><span data-stu-id="42f49-133">The search goes like this:</span></span>

1. <span data-ttu-id="42f49-134">È presente un file con il nome e percorso */a/b/c.cshtml*?</span><span class="sxs-lookup"><span data-stu-id="42f49-134">Is there a file with the path and name of */a/b/c.cshtml*?</span></span> <span data-ttu-id="42f49-135">In questo caso, eseguire la pagina e non passarle alcuna informazione.</span><span class="sxs-lookup"><span data-stu-id="42f49-135">If so, run that page and pass no information to it.</span></span> <span data-ttu-id="42f49-136">In caso contrario...</span><span class="sxs-lookup"><span data-stu-id="42f49-136">Otherwise ...</span></span>
2. <span data-ttu-id="42f49-137">È presente un file con il nome e percorso */a/b.cshtml*?</span><span class="sxs-lookup"><span data-stu-id="42f49-137">Is there a file with the path and name of */a/b.cshtml*?</span></span> <span data-ttu-id="42f49-138">Se in tal caso, eseguire la pagina e passare il valore `c` a esso.</span><span class="sxs-lookup"><span data-stu-id="42f49-138">If so, run that page and pass the value `c` to it.</span></span> <span data-ttu-id="42f49-139">In caso contrario...</span><span class="sxs-lookup"><span data-stu-id="42f49-139">Otherwise …</span></span>
3. <span data-ttu-id="42f49-140">È presente un file con il nome e percorso */a.cshtml*?</span><span class="sxs-lookup"><span data-stu-id="42f49-140">Is there a file with the path and name of */a.cshtml*?</span></span> <span data-ttu-id="42f49-141">Se in tal caso, eseguire la pagina e passare il valore `b/c` a esso.</span><span class="sxs-lookup"><span data-stu-id="42f49-141">If so, run that page and pass the value `b/c` to it.</span></span>

<span data-ttu-id="42f49-142">Se la ricerca trovata esatta non corrispondente per *. cshtml* file nelle rispettive cartelle specificate, ASP.NET continua la ricerca per questi file, a sua volta:</span><span class="sxs-lookup"><span data-stu-id="42f49-142">If the search found no exact matches for *.cshtml* files in their specified folders, ASP.NET continues looking for these files in turn:</span></span>

1. <span data-ttu-id="42f49-143">*/a/b/c/default.cshtml* (senza informazioni sul percorso).</span><span class="sxs-lookup"><span data-stu-id="42f49-143">*/a/b/c/default.cshtml* (no path information).</span></span>
2. <span data-ttu-id="42f49-144">*/a/b/c/index.cshtml* (senza informazioni sul percorso).</span><span class="sxs-lookup"><span data-stu-id="42f49-144">*/a/b/c/index.cshtml* (no path information).</span></span>

> [!NOTE]
> <span data-ttu-id="42f49-145">Per essere chiaro, le richieste di pagine specifiche (vale a dire le richieste che includono il *. cshtml* estensione) funziona esattamente come ci si aspetta.</span><span class="sxs-lookup"><span data-stu-id="42f49-145">To be clear, requests for specific pages (that is, requests that include the *.cshtml* filename extension) work just like you'd expect.</span></span> <span data-ttu-id="42f49-146">Ad esempio una richiesta `http://www.contoso.com/a/b.cshtml` eseguirà la pagina *b.cshtml* correttamente.</span><span class="sxs-lookup"><span data-stu-id="42f49-146">A request like `http://www.contoso.com/a/b.cshtml` will run the page *b.cshtml* just fine.</span></span>


<span data-ttu-id="42f49-147">All'interno di una pagina, è possibile ottenere le informazioni sul percorso tramite la pagina `UrlData` proprietà, che è un dizionario.</span><span class="sxs-lookup"><span data-stu-id="42f49-147">Inside a page, you can get the path information via the page's `UrlData` property, which is a dictionary.</span></span> <span data-ttu-id="42f49-148">Si supponga di disporre di un file denominato *ViewCustomers.cshtml* e il sito riceve la richiesta:</span><span class="sxs-lookup"><span data-stu-id="42f49-148">Imagine that you have a file named *ViewCustomers.cshtml* and your site gets this request:</span></span>

`http://mysite.com/myWebSite/ViewCustomers/1000`

<span data-ttu-id="42f49-149">Come descritto nelle regole di precedenza, la richiesta verrà inviata a una pagina.</span><span class="sxs-lookup"><span data-stu-id="42f49-149">As described in the rules above, the request will go to your page.</span></span> <span data-ttu-id="42f49-150">All'interno della pagina, è possibile utilizzare codice simile al seguente per ottenere e visualizzare le informazioni sul percorso (in questo caso, il valore &quot;1000&quot;):</span><span class="sxs-lookup"><span data-stu-id="42f49-150">Inside the page, you can use code like the following to get and display the path information (in this case, the value &quot;1000&quot;):</span></span>

[!code-html[Main](creating-readable-urls-in-aspnet-web-pages-sites/samples/sample1.html)]

> [!NOTE]
> <span data-ttu-id="42f49-151">Poiché il routing non implica i nomi di file completo, può esserci delle ambiguità nel caso di pagine che presentano lo stesso nome ma diverse estensioni di file (ad esempio, *MyPage.cshtml* e *MyPage.html*) .</span><span class="sxs-lookup"><span data-stu-id="42f49-151">Because routing doesn't involve complete file names, there can be ambiguity if you have pages that have the same name but different file-name extensions (for example, *MyPage.cshtml* and *MyPage.html*).</span></span> <span data-ttu-id="42f49-152">Per evitare problemi con il routing, è consigliabile verificare che non si hanno le pagine del sito i cui nomi differiscono solo per l'estensione.</span><span class="sxs-lookup"><span data-stu-id="42f49-152">In order to avoid problems with routing, it's best to make sure that you don't have pages in your site whose names differ only in their extension.</span></span>


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="42f49-153">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="42f49-153">Additional Resources</span></span>

<span data-ttu-id="42f49-154">[WebMatrix - URL, UrlData e Routing per SEO](http://www.mikesdotnetting.com/Article/165/WebMatrix-URLs-UrlData-and-Routing-for-SEO).</span><span class="sxs-lookup"><span data-stu-id="42f49-154">[WebMatrix - URLs, UrlData and Routing for SEO](http://www.mikesdotnetting.com/Article/165/WebMatrix-URLs-UrlData-and-Routing-for-SEO).</span></span> <span data-ttu-id="42f49-155">Questo post di blog da Mike Brind fornisce alcuni dettagli aggiuntivi sul funzionamento del routing in ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="42f49-155">This blog entry by Mike Brind provides some additional details on how routing works in ASP.NET Web Pages.</span></span>
