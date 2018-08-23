---
uid: web-pages/overview/performance-and-traffic/14-analyzing-traffic
title: Rilevamento delle informazioni relative ai visitatori (Analitica) per un ASP.NET Web Pages (Razor) del sito | Microsoft Docs
author: tfitzmac
description: Dopo che è stato utilizzato il sito Web sarà, è possibile analizzare il traffico dei siti Web.
ms.author: riande
ms.date: 02/17/2014
ms.assetid: 360bc6e1-84c5-4b8e-a84c-ea48ab807aa4
msc.legacyurl: /web-pages/overview/performance-and-traffic/14-analyzing-traffic
msc.type: authoredcontent
ms.openlocfilehash: aabe3177ba9479bfafafe81e1ea99a58f29d5271
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836478"
---
<a name="tracking-visitor-information-analytics-for-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="d9f01-103">Informazioni di rilevamento del visitatore (Analitica) per un sito di ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="d9f01-103">Tracking Visitor Information (Analytics) for an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="d9f01-104">da [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="d9f01-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="d9f01-105">Questo articolo descrive come usare un file di supporto per aggiungere analitica di sito Web alle pagine in un sito Web ASP.NET Web Pages (Razor).</span><span class="sxs-lookup"><span data-stu-id="d9f01-105">This article describes how to use a helper to add website analytics to pages in an ASP.NET Web Pages (Razor) website.</span></span>
> 
> <span data-ttu-id="d9f01-106">Che cosa si apprenderà come:</span><span class="sxs-lookup"><span data-stu-id="d9f01-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="d9f01-107">Come inviare le informazioni sul traffico del sito Web a un provider di analitica.</span><span class="sxs-lookup"><span data-stu-id="d9f01-107">How to send information about your website traffic to an analytics provider.</span></span>
> 
> <span data-ttu-id="d9f01-108">Queste sono le funzionalità introdotte nell'articolo di programmazione ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="d9f01-108">These are the ASP.NET programming features introduced in the article:</span></span>
> 
> - <span data-ttu-id="d9f01-109">Il `Analytics` helper.</span><span class="sxs-lookup"><span data-stu-id="d9f01-109">The `Analytics` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="d9f01-110">Versioni del software utilizzate nell'esercitazione</span><span class="sxs-lookup"><span data-stu-id="d9f01-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="d9f01-111">ASP.NET Web Pages (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="d9f01-111">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="d9f01-112">ASP.NET Web Helpers Library (pacchetto NuGet)</span><span class="sxs-lookup"><span data-stu-id="d9f01-112">ASP.NET Web Helpers Library (NuGet package)</span></span>


<span data-ttu-id="d9f01-113">Analitica è un termine generale per la tecnologia che misura il traffico nel sito Web in modo che è possibile comprendere come le persone usano il sito.</span><span class="sxs-lookup"><span data-stu-id="d9f01-113">Analytics is a general term for technology that measures traffic on your website so you can understand how people use the site.</span></span> <span data-ttu-id="d9f01-114">Molti servizi di analitica sono disponibili, inclusi i servizi di Google, Yahoo, StatCounter e ad altri utenti.</span><span class="sxs-lookup"><span data-stu-id="d9f01-114">Many analytics services are available, including services from Google, Yahoo, StatCounter, and others.</span></span>

<span data-ttu-id="d9f01-115">L'analitica modo funziona è che si iscriversi a un account con il provider di analitica, in cui si registra il sito che si vuole tenere traccia. Il provider invia un frammento di codice JavaScript che include un ID o il rilevamento del codice per l'account.</span><span class="sxs-lookup"><span data-stu-id="d9f01-115">The way analytics works is that you sign up for an account with the analytics provider, where you register the site that you want to track. The provider sends you a snippet of JavaScript code that includes an ID or tracking code for your account.</span></span> <span data-ttu-id="d9f01-116">Aggiungere il frammento JavaScript alle pagine web per il sito che si vuole tenere traccia. (È in genere aggiungere il frammento di analitica a una pagina layout o nel piè di pagina o altro markup HTML visualizzato in ogni pagina nel sito.) Quando gli utenti richiedono una pagina che contiene uno di questi frammenti di codice JavaScript, il frammento di codice invia informazioni relative alla pagina corrente per il provider di analitica, che registra informazioni dettagliate diverse relative alla pagina.</span><span class="sxs-lookup"><span data-stu-id="d9f01-116">You add the JavaScript snippet to the web pages on the site that you want to track. (You typically add the analytics snippet to a footer or layout page or other HTML markup that appears on every page in your site.) When users request a page that contains one of these JavaScript snippets, the snippet sends information about the current page to the analytics provider, who records various details about the page.</span></span>

<span data-ttu-id="d9f01-117">Quando si desidera esaminare le statistiche del sito, accedere al sito Web del provider analitica.</span><span class="sxs-lookup"><span data-stu-id="d9f01-117">When you want to have a look at your site statistics, you log into the analytics provider's website.</span></span> <span data-ttu-id="d9f01-118">È quindi possibile visualizzare tutti i tipi di report sul sito, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="d9f01-118">You can then view all sorts of reports about your site, like:</span></span>

- <span data-ttu-id="d9f01-119">Il numero di visualizzazioni di pagina per le singole pagine.</span><span class="sxs-lookup"><span data-stu-id="d9f01-119">The number of page views for individual pages.</span></span> <span data-ttu-id="d9f01-120">Ciò indica (approssimativamente) quante persone hanno sta visitando il sito e le pagine nel sito sono quelli più diffusi.</span><span class="sxs-lookup"><span data-stu-id="d9f01-120">This tells you (roughly) how many people are visiting the site, and which pages on your site are the most popular.</span></span>
- <span data-ttu-id="d9f01-121">Le persone quanto tempo impiegano in pagine specifiche.</span><span class="sxs-lookup"><span data-stu-id="d9f01-121">How long people spend on specific pages.</span></span> <span data-ttu-id="d9f01-122">Ciò può indicare elementi come se la home page mantiene gli interessi delle persone.</span><span class="sxs-lookup"><span data-stu-id="d9f01-122">This can tell you things like whether your home page is keeping people's interest.</span></span>
- <span data-ttu-id="d9f01-123">Le persone siti presenti prima che visita il sito.</span><span class="sxs-lookup"><span data-stu-id="d9f01-123">What sites people were on before they visited your site.</span></span> <span data-ttu-id="d9f01-124">Questo aiuta a capire se il traffico proviene da collegamenti, da ricerche e così via.</span><span class="sxs-lookup"><span data-stu-id="d9f01-124">This helps you understand whether your traffic is coming from links, from searches, and so on.</span></span>
- <span data-ttu-id="d9f01-125">Quando gli utenti visitano il sito e i tempi di permanenza.</span><span class="sxs-lookup"><span data-stu-id="d9f01-125">When people visit your site and how long they stay.</span></span>
- <span data-ttu-id="d9f01-126">In quali paesi sono visitatori.</span><span class="sxs-lookup"><span data-stu-id="d9f01-126">What countries your visitors are from.</span></span>
- <span data-ttu-id="d9f01-127">Quali browser e sistemi operativi usano visitatori.</span><span class="sxs-lookup"><span data-stu-id="d9f01-127">What browsers and operating systems your visitors are using.</span></span>

    ![Ch14traffic-1](14-analyzing-traffic/_static/image1.jpg)

## <a name="using-a-helper-to-add-analytics-to-a-page"></a><span data-ttu-id="d9f01-129">Uso di un Helper per aggiungere Analitica a una pagina</span><span class="sxs-lookup"><span data-stu-id="d9f01-129">Using a Helper to Add Analytics to a Page</span></span>

<span data-ttu-id="d9f01-130">Pagine Web ASP.NET include alcuni helper analitica (`Analytics.GetGoogleHtml`, `Analytics.GetYahooHtml`, e `Analytics.GetStatCounterHtml`) che rendono più semplice gestire i frammenti di codice JavaScript utilizzati per analitica.</span><span class="sxs-lookup"><span data-stu-id="d9f01-130">ASP.NET Web Pages includes several analytics helpers (`Analytics.GetGoogleHtml`, `Analytics.GetYahooHtml`, and `Analytics.GetStatCounterHtml`) that make it easy to manage the JavaScript snippets used for analytics.</span></span> <span data-ttu-id="d9f01-131">Invece di consiste nel capire come e dove inserire il codice JavaScript, tutto ciò che devi fare è aggiungere l'helper a una pagina.</span><span class="sxs-lookup"><span data-stu-id="d9f01-131">Instead of figuring out how and where to put the JavaScript code, all you have to do is add the helper to a page.</span></span> <span data-ttu-id="d9f01-132">Le uniche informazioni da specificare sono il nome dell'account, ID o codice di rilevamento.</span><span class="sxs-lookup"><span data-stu-id="d9f01-132">The only information you need to provide is your account name, ID, or tracking code.</span></span> <span data-ttu-id="d9f01-133">(Per StatCounter, è anche necessario specificare alcuni valori aggiuntivi.)</span><span class="sxs-lookup"><span data-stu-id="d9f01-133">(For StatCounter, you also have to provide a few additional values.)</span></span>

<span data-ttu-id="d9f01-134">In questa procedura si creerà una pagina di layout che utilizza il `GetGoogleHtml` helper.</span><span class="sxs-lookup"><span data-stu-id="d9f01-134">In this procedure, you'll create a layout page that uses the `GetGoogleHtml` helper.</span></span> <span data-ttu-id="d9f01-135">Se hai già un account con uno degli altri provider di analitica, è possibile usare invece l'account e apportare delle lievi modifiche necessarie.</span><span class="sxs-lookup"><span data-stu-id="d9f01-135">If you already have an account with one of the other analytics providers, you can use that account instead and make slight adjustments as needed.</span></span>

> [!NOTE]
> <span data-ttu-id="d9f01-136">Quando si crea un account di analitica, registrare l'URL del sito che si desidera essere tracciati.</span><span class="sxs-lookup"><span data-stu-id="d9f01-136">When you create an analytics account, you register the URL of the site that you want to be tracking.</span></span> <span data-ttu-id="d9f01-137">Se si sta testando tutto nel computer locale, non sarà necessario registrare effettivo del traffico (il solo traffico è si), in modo non sarà in grado di registrare e visualizzare le statistiche del sito.</span><span class="sxs-lookup"><span data-stu-id="d9f01-137">If you're testing everything on your local computer, you won't be tracking actual traffic (the only traffic is you), so you won't be able to record and view site statistics.</span></span> <span data-ttu-id="d9f01-138">Ma questa procedura viene illustrato come aggiungere un helper di analitica a una pagina.</span><span class="sxs-lookup"><span data-stu-id="d9f01-138">But this procedure shows how you add an analytics helper to a page.</span></span> <span data-ttu-id="d9f01-139">Quando si pubblica il sito, il sito live verrà inviate informazioni al provider di analitica.</span><span class="sxs-lookup"><span data-stu-id="d9f01-139">When you publish your site, the live site will send information to your analytics provider.</span></span>


1. <span data-ttu-id="d9f01-140">Aggiungere ASP.NET Web Helpers Library nel sito Web come descritto in [installazione di helper in un sito con pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), se non si già aggiunti.</span><span class="sxs-lookup"><span data-stu-id="d9f01-140">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already added it.</span></span>
2. <span data-ttu-id="d9f01-141">Creare un account con Google Analitica e registrare il nome dell'account.</span><span class="sxs-lookup"><span data-stu-id="d9f01-141">Create an account with Google Analytics and record the account name.</span></span>
3. <span data-ttu-id="d9f01-142">Creare una pagina di layout denominata *Analytics.cshtml* e aggiungere il markup seguente:</span><span class="sxs-lookup"><span data-stu-id="d9f01-142">Create a layout page named *Analytics.cshtml* and add the following markup:</span></span>

    [!code-cshtml[Main](14-analyzing-traffic/samples/sample1.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="d9f01-143">È necessario inserire la chiamata ai `Analytics` helper nel corpo della pagina web (prima di `</body>` tag).</span><span class="sxs-lookup"><span data-stu-id="d9f01-143">You must place the call to the `Analytics` helper in the body of your web page (before the `</body>` tag).</span></span> <span data-ttu-id="d9f01-144">In caso contrario, il browser non verrà eseguito lo script.</span><span class="sxs-lookup"><span data-stu-id="d9f01-144">Otherwise, the browser will not run the script.</span></span>

    <span data-ttu-id="d9f01-145">Se si usa un provider di analitica diverso, usare uno degli helper seguenti:</span><span class="sxs-lookup"><span data-stu-id="d9f01-145">If you're using a different analytics provider, use one of the following helpers instead:</span></span>

    - <span data-ttu-id="d9f01-146">(Yahoo) `@Analytics.GetYahooHtml("myaccount")`</span><span class="sxs-lookup"><span data-stu-id="d9f01-146">(Yahoo) `@Analytics.GetYahooHtml("myaccount")`</span></span>
    - <span data-ttu-id="d9f01-147">(StatCounter) `@Analytics.GetStatCounterHtml("project", "security")`</span><span class="sxs-lookup"><span data-stu-id="d9f01-147">(StatCounter) `@Analytics.GetStatCounterHtml("project", "security")`</span></span>
4. <span data-ttu-id="d9f01-148">Sostituire `myaccount` con il nome dell'account, ID o codice di rilevamento creato nel passaggio 1.</span><span class="sxs-lookup"><span data-stu-id="d9f01-148">Replace `myaccount` with the name of the account, ID, or tracking code that you created in step 1.</span></span>
5. <span data-ttu-id="d9f01-149">Eseguire la pagina nel browser.</span><span class="sxs-lookup"><span data-stu-id="d9f01-149">Run the page in the browser.</span></span> <span data-ttu-id="d9f01-150">(Assicurarsi che sia selezionata la pagina nel **file** dell'area di lavoro prima dell'esecuzione.)</span><span class="sxs-lookup"><span data-stu-id="d9f01-150">(Make sure the page is selected in the **Files** workspace before you run it.)</span></span>
6. <span data-ttu-id="d9f01-151">Nel browser, visualizzare l'origine della pagina.</span><span class="sxs-lookup"><span data-stu-id="d9f01-151">In the browser, view the page source.</span></span> <span data-ttu-id="d9f01-152">Sarà in grado di visualizzare il codice sottoposto a rendering analitica:</span><span class="sxs-lookup"><span data-stu-id="d9f01-152">You'll be able to see the rendered analytics code:</span></span>

    [!code-html[Main](14-analyzing-traffic/samples/sample2.html)]
7. <span data-ttu-id="d9f01-153">Accedere al sito di Google Analitica ed esaminare le statistiche per il sito.</span><span class="sxs-lookup"><span data-stu-id="d9f01-153">Log onto the Google Analytics site and examine the statistics for your site.</span></span> <span data-ttu-id="d9f01-154">Se si esegue la pagina in un sito live, viene visualizzata una voce che registra la visita la pagina.</span><span class="sxs-lookup"><span data-stu-id="d9f01-154">If you're running the page on a live site, you see an entry that logs the visit to your page.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="d9f01-155">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="d9f01-155">Additional Resources</span></span>

- [<span data-ttu-id="d9f01-156">Sito di Google Analitica</span><span class="sxs-lookup"><span data-stu-id="d9f01-156">Google Analytics site</span></span>](https://www.google.com/analytics/)
- [<span data-ttu-id="d9f01-157">Yahoo! Sito Web Analytics</span><span class="sxs-lookup"><span data-stu-id="d9f01-157">Yahoo! Web Analytics site</span></span>](http://help.yahoo.com/l/us/yahoo/ywa/)
- [<span data-ttu-id="d9f01-158">Sito StatCounter</span><span class="sxs-lookup"><span data-stu-id="d9f01-158">StatCounter site</span></span>](http://statcounter.com/)
